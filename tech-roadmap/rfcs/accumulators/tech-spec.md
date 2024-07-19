# BLS Accumulator - Technical Specification

Status: Draft<br/>
Authors: J. Niestroj

## Public parameters
This specification assumes that BLS12-381 is used as the underlying curve.

G1, G2:          subgroups of the curve<br/>
GT:              subgroup of the multiplicative group of a field extension<br/>
e:               G1 x G2 -> GT: bilinear pairing map<br/>
Identity_GT:     identity element for the GT subgroup<br/>

## Interface

The interface is split between a non-adaptively accumulator [KB21] and an adaptively accumulator [VB20].
The non-adaptively accumulator is only usable in cases where it's not needed to readd an element to the accumulator.

### Non-Adaptively Accumulator

#### Create Accumulator

See Create Accumulator in Adaptively Accumulator.

#### Remove Batch

See Remove Batch in Adaptively Accumulator.

#### Calculate Delta

```
delta = CalculateDelta(SK, remove_elements)

Inputs:

- SK, secret key of the accumulator
- remove_elements, array of scalar elements that were removed in this delta

Outputs:

- delta

Deserialization:

1. (d_1, ..., d_L) = remove_elements

Procedure:

1. v_d = ∑^{m}_{s=1}{ ∏ 1..s {yD_1 + alpha}^-1 ∏ 1 ..s-1 {yD_m - x} }
2. return {remove_elements, v_d}
```

#### Calculate Witness

See Calculate Witness in Adaptively Accumulator.

#### Verify Witness

See Verify Witness in Adaptively Accumulator.

#### Update Witness

```
updated_witness = UpdateWitness(value, witness, accumulator_update)

Inputs:

- value, Value for which this witness is created
- witness, Current Witness value
- accumulator_updated, Output of the CalculateDelta function

Outputs:

- witness, Updated witness

Deserialization:

1. (remove_elements, omegas) = accumulator_update
2. (d_1, ..., d_K) = remove_elements

Procedure:

1. d_d = (d_1 + value) * ... * (d_K + value)
2. d_d = d_d.invert()
3. if d_d == zero, return error
4. poly = Polynomial(omegas)
5. v = poly.evaluate(value)
6. witness -= v
7. witness *= d_d
8. return witness
```

#### Generate Proof

See Generate Proof in Adaptively Accumulator.

#### Finalize Proof

See Finalize Proof in Adaptively Accumulator

#### Verify Proof

See Verify Proof in Adaptively Accumulator.

### Adaptively Accumulator

#### Create Accumulator

```
acc = CreateAccumulator()

Outputs:

- empty accumulator

Procedure:

1. random = GenerateRandom()
2. acc = random
3. return acc
```

#### Add Batch

```
acc = AddBatch(SK, accumulator, elements)

Inputs:

- SK, secret key of the accumulator
- accumulator, Current accumulator value
- elements, array of scalar elements to add to the accumulator

Outputs:

- updated accumulator

Deserialization:

1. (e_1, ..., e_L) = elements

Procedure:

1. e = (e_1 + SK) * ... * (e_L + SK)
2. acc = acc * e
3. return acc
```

#### Remove Batch

```
acc = RemoveBatch(SK, accumulator, elements)

Inputs:

- SK, secret key of the accumulator
- accumulator, Current accumulator value
- elements, array of elements to remove from the accumulator

Outputs:

- updated accumulator

Deserialization:

1. (e_1, ..., e_L) = elements

Procedure:

1. e = (e_1 + SK) * .... * (e_L + SK)
2. acc = acc * e.invert()
3. return acc
```

#### Calculate Delta

```
delta = CalculateDelta(SK, add_elements, remove_elements)

Inputs:

- SK, secret key of the accumulator
- add_elements, array of scalar elements that were added in this delta
- remove_elements, array of scalar elements that were removed in this delta

Outputs:

- delta

Deserialization:

1. (a_1, ..., a_L) = add_elements
2. (d_1, ..., d_L) = remove_elements

Procedure:

1. a = (a_1 + SK) * ... * (a_L + SK)
2. v_d = ∑^{m}_{s=1}{ ∏ 1..s {yD_1 + alpha}^-1 ∏ 1 ..s-1 {yD_m - x} }
3. v_d *= a
4. v_a = ∑^n_{s=1}{ ∏ 1..s-1 {yA_1 + alpha} ∏ s+1..n {yA_m - x} }
5. v_a -= v_d
6. return {add_elements, remove_elements, v_a}
```

#### Calculate Witness

```
witness = CalculateWitness(SK, accumulator, value)

Inputs:

- SK, secret key of the accumulator
- accumulator, Current accumulator value
- value, Value for which this witness is created

Outputs:

- witness

Procedure:

1. r = (sk + value).invert()
2. w = accumulator * r
3. return w
```

#### Verify Witness

```
valid = VerifyWitness(PK, witness, value, accumulator)

Inputs:

- PK, public key of the accumulator
- witness, Witness to verify
- value, Value for which this witness should be verified
- accumulator, Current accumulator value

Outputs:

- valid, boolean if witness is correct or not

Procedure:

1. p = G2Generator * value + PK
2. if e(witness, p) + e(accumulator, G2Generator.negate()) != IDENTITY_GT, return false
3. return true
```

#### Update Witness

```
updated_witness = UpdateWitness(value, witness, accumulator_update)

Inputs:

- value, Value for which this witness is created
- witness, Current Witness value
- accumulator_updated, Output of the CalculateDelta function

Outputs:

- witness, Updated witness

Deserialization:

1. (add_elements, remove_elements, omegas) = accumulator_update
2. (a_1, ..., a_L) = add_elements
3. (d_1, ..., d_K) = remove_elements

Procedure:

1. d_d = (d_1 + value) * ... * (d_K + value)
2. d_d = d_d.invert()
3. if d_d == zero, return error
4. d_a = (a_1 + value) * ... * (a_L + value)
5. d_a *= d_d
6. poly = Polynomial(omegas)
7. v = poly.evaluate(value)
8. v *= d_d
9. witness *= d_a
10. witness += v
11. return witness
```

#### Generate Proof

```
prepared_proof = GenerateProof(witness, value, accumulator, value_blinding)

Inputs:

- witness, Witness for which the proof should be generated
- value, Value of the witness
- accumulator, Current accumulator value
- value_blinding, *Optional* random value to blind the value of the witness

Outputs:

- prepared_proof, Prepared Proof of membership

Deserialization:

1. (r, alpha) = calculate_random_values(2)
2. beta = value_blinding || calculate_random_values(1)

Procedure:

1. a_bar = witness * r
2. b_bar = (accumulator - witness * value) * r
3. u = alpha * accumulator + beta * a_bar
7. return {alpha, beta, r, value, a_bar, b_bar, u}
```

#### Finalize Proof

```
proof = FinalizeProof(prepared_proof)

Inputs:

- prepared_proof, Output of the GenerateProof method

Outputs:

- proof, proof of membership

Deserialization:

1. (alpha, beta, r, value, a_bar, b_bar, u) = prepared_proof

Procedure:

1. challenge = calculate_challenge()
2. s = alpha + r * challenge
3. t = beta - value * challenge
4. return {a_bar, b_bar, s, t, challenge}
```

#### Verify Proof

```
valid = VerifyProof(proof, pk, accumulator)

Inputs:

- proof, Generated proof by the holder
- pk, Public Key of the Accumulator
- accumulator, Current value of the accumulator

Outputs:

- valid, Boolean if the proof is valid or not

Deserialization:

1. (a_bar, b_bar, s, t, challenge) = proof

Procedure:

1. pair = e(a_bar, pk) - e(b_bar, G2Generator)
2. u = s * accumulator + t * a_bar - challenge * b_bar
3. challenge_proof = calculate_challenge()
4. if pair != IDENTITY_GT, return false
5. if challenge != challenge_proof, return false
6. return true
```

## References

[NG05] Nguyen, L., "Accumulators from Bilinear Pairings and Applications to ID-based Ring Signatures and Group Membership Revocation", <https://eprint.iacr.org/2005/123.pdf><br/>
[VB20] Giuseppe, V. and A. Biryukov, "Dynamic Universal Accumulator with Batch Update over Bilinear Groups", <https://eprint.iacr.org/2020/777.pdf><br/>
[KB21] Karantaidou, I. and F. Baldimtsi, "Efficient Constructions of Pairing Based Accumulators", <https://ieeexplore.ieee.org/abstract/document/9505229><br/>
[TZ23] Tessaro, T. and C. Zhu, "Revisiting BBS Signatures", <https://eprint.iacr.org/2023/275><br/>
