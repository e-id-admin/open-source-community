# BLS Accumulator - Specification

Status: Draft<br/>
Authors: J. Niestroj

## Abstract

This specification describes a privacy-preserving mechanism for publishing status information such as revocation of Verifiable Credentials using cryptographic accumulators.

## Introduction

### What is an accumulator

*Caution: This is only a simplified explanation*

An accumulator is the product of multiplying numbers together.

`a * b * c = d`

In this example `d` would be the accumulator. It is the product of `a * b * c`. 
In reality these values are long numbers that are also multiplied with the secret key of the issuer. This prevents that someone can calculate the accumulator without knowing the secret key.

### What is a witness

A witness is the product of all values except the value that this witness is created for.

If we use the accumulator `a * b * c = d` and we want to create a witness for the value of `a` we would multiply `b * c` to get the witness.
This witness can be used to prove that your value is inside the accumulator. This works as `a * (b * c) = a * b * c`.

A witness is only valid for the accumulator value it was created on. If you want to verify a witness with a new/old accumulator value, the verification will fail. With an invalid witness it's not possible to create a valid proof.

### What is a delta

A delta will be published by the issuer to enable holders to update their witnesses. If the accumulator value changes (e.g. a value was deleted), the witness value is no longer correct (the witness now contains a value that is no longer in the accumulator).

A delta contains all added/removed elements and an array of omegas. These omegas are needed for the holder to update his witness.

## Accumulator Types

### Non-Adaptively Accumulator

A non-adaptively accumulator is a variant of an accumulator where no initial adding is needed and readding is also not possible. The accumulator value only changes on deletion. This has the advantage that no delta needs to be published if something is added to the accumulator.

This type is only viable for status purposes which are not reversible. (e.g. revocation)

### Adaptively Accumulator

An adaptively accumulator supports addition and deletion. On both operations a delta needs to be published.

If an element is added to the accumulator the accumulator value changes and all witnesses need to be updated.

This type is suitable for status purposes which are reversible. (e.g. suspension)

## W3C VC Data Model

### Accumulator Entry

| Property | Description |
| -------- | ----------- |
| type | type property is one of BLSNonAdaptivelyAccumulator, BLSAdaptivelyAccumulator |
| statusPurpose | Defines the purpose of this accumulator |
| value | Defines the value of this credential that it has in the accumulator |
| accumulatorURL | Defines the URL to get the accumulator/deltas |
| accumulatorPublicKey | Defines the URI to the public key of the accumulator |
| accumulatorWitness | Witness as octets encoded in base64 |
| accumulatorWitnessUpdateURL | *OPTIONAL* Defines the endpoint to update the witness of the holder |

Example

```json
{
    "@context": ["https://www.w3.org/ns/credentials/v2"],
    "type": ["VerifiableCredential", "DriversLicenseCredential"],
    "issuer": "did:example:issuer",
    "validFrom": "2024-06-17T07:30:00Z",
    "credentialSubject": {
        "firstName": "John",
        "lastName": "Doe",
        "isAllowedToDrive": "true"
    },
    "credentialStatus": [
        {
            "type": "BLSNonAdaptivelyAccumulator",
            "statusPurpose": "revocation",
            "value": "10",
            "accumulatorURL": "https://www.example.com/1234",
            "accumulatorPublicKey": "did:example:issuer#key-3",
            "accumulatorWitness": "M...NzU=",
            "accumulatorWitnessUpdateURL": "https://www.example.com/1234/update"
        },
        {
            "type": "BLSAdaptivelyAccumulator",
            "statusPurpose": "suspension",
            "value": "8",
            "accumulatorURL": "https://www.example.com/5678",
            "accumulatorPublicKey": "did:example:issuer#key-4",
            "accumulatorWitness": "N...yDU=",
            "accumulatorWitnessUpdateURL": "https://www.example.com/5678/update"
        }
    ]
}
```

### Presentation

#### CredentialStatus

Only the following properties of the initial credentialStatus must be disclosed to the Verifier. If additional properties are disclosed there is the risk that the presentation is/becomes linkable.

| Property | Description |
| -------- | ----------- |
| type | type property is one of BLSNonAdaptivelyAccumulator, BLSAdaptivelyAccumulator |
| statusPurpose | Defines the purpose of this accumulator |
| accumulatorURL | Defines the URL to get the accumulator/deltas |
| accumulatorPublicKey | Defines the public key of the accumulator |

#### Proof

| Property | Description |
| -------- | ----------- |
| accumulatorURL | Defines for which accumulator this proof is |
| accumulatorProof | Given proof by the holder as base64 encoded octets |
| type | type property must be BLSAccumulatorMembership |

Example

```json
{
    "@context": ["https://www.w3.org/ns/credentials/v2"],
    "type": ["VerifiablePresentation", "ExamplePresentation"],
    "verifiableCredential": [
        {
            "@context": ["https://www.w3.org/ns/credentials/v2"],
            "type": ["VerifiableCredential", "DriversLicenseCredential"],
            "issuer": "did:example:issuer",
            "validFrom": "2024-06-17T07:30:00Z",
            "credentialSubject": {
                "firstName": "John",
                "lastName": "Doe"
            },
            "credentialStatus": [
                {
                    "type": "BLSNonAdaptivelyAccumulator",
                    "statusPurpose": "revocation",
                    "accumulatorURL": "https://www.example.com/1234",
                    "accumulatorPublicKey": "did:example:issuer#key-3"
                },
                {
                    "type": "BLSAdaptivelyAccumulator",
                    "statusPurpose": "suspension",
                    "accumulatorURL": "https://www.example.com/5678",
                    "accumulatorPublicKey": "did:example:issuer#key-4"
                }
            ]
        }
    ]
    "proof": [
        {
            "type": "BLSAccumulatorMembership",
            "accumulatorProof": "N...MzQ=",
            "accumulatorURL": "https://www.example.com/1234"
        },
        {
            "type": "BLSAccumulatorMembership",
            "accumulatorProof": "M...zNA==",
            "accumulatorURL": "https://www.example.com/5678"
        }
    ]   
}
```

## Models

### Witness

#### Membership Witness

The witness is sent in octets and base64 encoded.

| Property | Description |
| -------- | ----------- |
| value | The value of the witness |
| accumulator | accumulator this witness is based on |

### Delta

The delta will be sent in octets and encoded in base64.

| Property | Description |
| -------- | ----------- |
| additions | Array of added values |
| deletions | Array of deleted values |
| omegas | Array of omegas |

### Membership Proof

See details in tech spec.

## Algorithms

### Get Accumulator

The endpoint to get the current accumulator is defined by the accumulatorURL in the credential.

#### Request

GET *accumulatorURL*

#### Response

The response contains the current accumulator.

| Attribute | Description |
| --------- | ----------- |
| accumulator | Current accumulator in octets encoded as base64 |

### Get Deltas

The endpoint to get the deltas is defined by the accumulatorURL in the credential plus the /deltas suffix.

So if the accumulatorURL is https://www.example.com/1234 then the endpoint to get the deltas would be https://www.example.com/1234/deltas.

The holder must send the accumulator his witness is based on to get only the relevant deltas for him.

#### Request

GET *accumulatorURL*/deltas

Query parameters

| Parameter | Description |
| --------- | ----------- |
| lastAccumulator | The current accumulator of the witness |

#### Response

The response contains the deltas between the current accumulator and the last accumulator (sent by the holder).

| Attribute | Description |
| --------- | ----------- |
| deltas | Array of deltas as octets in base64 |
| currentAccumulator | Id of the current Accumulator |


Example

```json
{
    "deltas": [
        "12345", "123456", "1234"
    ],
    "currentAccumulator": "c8fab6e5-27a8-42c4-b2b4-a49902898a65"
}
```

### Update Witness

The Holder can update his witness by getting back to the issuer. This is especially helpful if the holder hasn't updated his witness for a while. With this approach we can skip the expensive update of the witness with all the deltas.

To update the witness the holder will send a POST request to the `accumulatorWitnessUpdateURL` endpoint (defined in the credential). The holder will send a presentation of his credential with the disclosed credentialStatus of the witness he wants to update.

#### Request

POST *accumulatorWitnessUpdateURL*

Body

VC Presentation with the disclosed credentialStatus.

#### Response

New witness in the format that is defined in [Membership Witness](#membership-witness).

```json
{
    "witness": "12345"
}
```

## Privacy Considerations

### Updating Witness

For the update of the witness the holder needs to get back to the issuer. Only the issuer can issue a new witness.
This is a call-to-home. That is the reason why this action should only be done if the delta between the holders' witness and the current accumulator is too large.


With that the calculation time on the holder should be skipped if the update of the witness would take too long.

### Linkability

If the holder follows this spec the verifier is not able to link the holder based on his accumulator status information. There could be other reasons why the holder is still linkable. It can be either by content of the VC or by using a linkable credential format/signature scheme (e.g. SD-JWT). If followed correctly Accumulators are one of the best privacy focused status mechanisms.

## Other use cases

### Trackability

In some use cases it could be useful that the verifier can track the status of the credential. So, the verifier can recheck by himself if e.g. a credential is revoked and he doesn't need to contact the holder for that. This flow is not possible with accumulators. It would be only possible if the holder shares his witness and private value and that shouldn't be done.
Instead of trying to make accumulators trackable it could be useful to add another status mechanism to the credential (e.g. Status List). Then the verifier can choose which mechanism he wants, and the holder can hide the not relevant information. With that approach (and a unlinkable signature/format) it should be possible to also have a solution for the trackable use case.

## Security considerations

### Accumulator initialization

The accumulator initialization should be done with already some elements in the accumulator. Best case would be if the accumulator is already initialized with the size of the maximum entries size. First positive aspect of that is, that the adding of elements is no longer needed. This prevents the delta creation if the issuer wants to add a new credential. The other aspect is that the holder could calculate the secret key of the issuer if he has a witness and only one element is inside the accumulator.

### Binding to a credential

If a witness is not stored in the hardware (or the value for the witness) the witness can be stolen or copied. This means that another holder would be able to generate a valid proof.
As this is a problem the holder should additionally proof the equalness between the value of the witness and the value in the credential. That means that the holder also needs a valid credential with the same value (that was signed by the issuer)
If this binding is used it's also not a problem if the witness would be public. As you still need a valid credential to really use the witness.

To do that binding we use the same random value to blind the witness value in the Membership and in the BBS proof. Also, the challenge of the BBS proof and the membership proof are combined to generate the final proofs.

#### Example flow

**Holder**
```
1. create random value
2. generate bbs proof with generated value as a blinding for the holder private value
3. generate membership proof with generated value as a blinding for the private value
4. calculate bbs proof challenge
5. calculate membership proof challenge
6. finalize bbs proof with both challenges
7. finale membership proof with both challenges
```

**Verifier**
```
1. calculate bbs proof challenge
2. calculate membership proof challenge
3. verify bbs proof with both challenges
4. verify membership proof with both challenges
5. verify if membership.t is equal to m^_j of private value (in bbs proof)
```
