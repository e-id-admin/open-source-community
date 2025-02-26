# Swiss e-ID and trust infrastructure: Trust protocol based on VCs

Status: Draft<br/>
Version: 0.1<br/>

## Introduction

This document defines the publicly consumable technical specification of the trust protocol, is based on VCs and was created as an initial concept for the Swiss trust infrastructure. 
The aim is to provide a straight-forward solution allowing a governing body (trust issuer) to confirm the identity of issuers and verifiers.

The trust protocol is based on verifiable credentials (VCs), which are signed by a particular trust issuer to authenticate the corresponding statement about a subject. In the context of the trust protocol, those VCs are referred to as **trust statements**.

## Trust Statements

Trust Statements are designed to work regardless of the VC format but they rely on a few features the format **MUST** support:

- Identification and verification of the trust statement issuer
- Identification and verification of the trust statement subject
- Revocation mechanism for the other actors to validate the current state of the trust statement
- Mechanism to differentiate trust statement types
- Mechanism to define the life-cycle of a trust statement
- Mechanism to include additional attributes in the trust statement

### SD-JWT VC Representation

Trust statements **MUST NOT** use any form of device binding, as all data are designed to be public.
Trust statements must encode Type Metadata directly (see section-6.3.5 at [SD-JWT-based Verifiable Credentials](https://www.ietf.org/archive/id/draft-ietf-oauth-sd-jwt-vc-04.html#section-6.3.5)).

The SD-JWT VC JOSE Header **MUST** have the following parameters:
| Parameter  | Description                                                                   |
| -----------| ------------------------------------------------------------------------------|
| `typ`      | **MUST** be `vc+sd-jwt`                                                       |
| `alg`      | **MUST** be `ES256`                                                           |
| `kid`      | URI to the key used by the trust statement issuer to sign the trust statement |


The SD-JWT VC claims **MUST** have the following properties:
| Claim Name | Claim Value Description                   |
| -----------| ------------------------------------------|
| `vct`      | Type id of trust statement                |
| `iss`      | URI of the trust statement issuer         |
| `sub`      | URI of the trust statement subject        |
| `iat`      | Issuance time                             |
| `status`   | Status list revocation entry              |

The SD-JWT VC claims **MAY** have the following properties:
| Claim Name | Claim Value Description                   |
| -----------| ------------------------------------------|
| `nbf`      | Start of validity time                    |
| `exp`      | Expiry of validity time                   |

A trust statement **MUST** be considered inactive when:
- `nbf` is higher than the current epoch
- `exp` is lower or equal to the current epoch
- `status` resolves to invalid

### W3C VCDM 2.0 Representation

Trust statements **MUST NOT** use any form of device binding, as all data are designed to be public.

The W3C VCDM 2.0 representation **MUST** have the following properties:
| Properties            | Description                                                           |
| ----------------------| ----------------------------------------------------------------------|
| `type`                | Type id of trust statement and **MUST** include `VerifiableCredential`|
| `issuer`              | URI of the trust statement issuer                                     |
| `credentialSuject.id` | URI of the trust statement subject                                    |
| `issuedAt`            | Issuance time and **MUST** be a [XMLSCHEMA11-2](https://www.w3.org/TR/xmlschema11-2/#dateTime) dateTimeStamp string value representing the date and time. |
| `credentialStatus`    | Status list revocation entry                                          |

The W3C VCDM 2.0 representation **MAY** have the following properties:
| Properties            | Description                                                           |
| ----------------------| ----------------------------------------------------------------------|
| `validFrom`           | Start of validity time                                                |
| `validUntil`          | Expiry of validity time                                               |

A trust statement **MUST** be considered inactive when:
- `validFrom` is higher than the current datetime
- `validUntil` is lower or equal to the current datetime
- `credentialStatus` resolves to invalid

### Trust Statement Types

Trust statements are defined by a type which **MAY** use additional claims in addition to aforementioned ones.

A trust statement type is a continuous string consisting of the following three connected parts:
1. The literal string `TrustStatement`, which is a fixed value
2. A name reflecting the purpose of the trust statement
3. The version of the trust statement leading with a `V` followed by an integer starting at `1` (incrementing by 1 with every version)

The following example is a trust statement about the identity of a subject.

```
                  |- Purpose of trust statement
              /------\
TrustStatementMetadataV1
\------------/        \-/
       |- Fixed value  |- Version of trust statement
```

Additional trust statements **MAY** be specified. 

#### TrustStatementMetadataV1
The `TrustStatementMetadataV1` type defines a set of rules and values which are validated by the trust statement issuer and **MUST** confirm the trust statement subject is who they claim to be.

The metadata trust statement **MUST** extend the basic trust statement claims described above with the following specific claims:
| Claim Name | Claim Value Description                       |
| -----------| ----------------------------------------------|
| `vct` for SD&minus;JWT&nbsp;VC<br/>`type` for W3C&nbsp;VCDM&nbsp;2.0 | **MUST** be `TrustStatementMetadataV1`        |
| `orgName` | Object that contains the validated entity names in multiple languages.<br/>The key **MUST** be a language identifier tag in accordance with RFC5646.<br/> The value **MUST** be the validated entity name in that language. |
| `logoUri`  | (Optional) Object that contains the validated entity logo URI in multiple languages.<br/>The key **MUST** be a language identifier tag in accordance with RFC5646.<br/> The value **MUST** be a Data URL (RFC2397). |
| `prefLang` | (Optional) Preferred Language for the content. <br/>The value **MUST** be a language identifier tag in accordance with RFC5646. |


Additional claims **MAY** be defined and used in conjunction with the claims above.

**Example of a metadata trust statement in SD-JWT VC**

Decoded SD-JWT VC
```
{
    "typ": "vc+sd-jwt",
    "alg": "ES256",
    "kid": "did:example:issuer#key-1"
}
.
{
    "vct": "TrustStatementMetadataV1",
    "iss": "did:example:issuer",
    "sub": "did:example:subject",
    "iat": 1690360968,
    "nbf": 1721896968,
    "exp": 1753432968,
    "status": "...",
    "orgName": {
      "en": "John Smith's Smithery",
      "de-CH": "John Smith's Schmiderei"
    },
    "logoUri": {
      "en": "data:image/png;base64,..."
    },
    "prefLang": "en"
}
.
<SIGNATURE>
```

Encoded SD-JWT VC
```
eyJ0eXAiOiJ2YytzZC1qd3QiLCJhbGciOiJFUzI1NiIsImtpZCI6ImRpZDpleGFtcGxlOmlzc3VlciNrZXktMSJ9.eyJ2Y3QiOiJUcnVzdFN0YXRlbWVudE1ldGFkYXRhVjEiLCJpc3MiOiJkaWQ6ZXhhbXBsZTppc3N1ZXIiLCJzdWIiOiJkaWQ6ZXhhbXBsZTpzdWJqZWN0IiwiaWF0IjoxNjkwMzYwOTY4LCJuYmYiOjE3MjE4OTY5NjgsImV4cCI6MTc1MzQzMjk2OCwic3RhdHVzIjoiLi4uIiwib3JnTmFtZSI6eyJlbiI6IkpvaG4gU21pdGgncyBTbWl0aGVyeSIsImRlLUNIIjoiSm9obiBTbWl0aCdzIFNjaG1pZGVyZWkifSwibG9nb1VyaSI6eyJlbiI6ImRhdGE6aW1hZ2UvcG5nO2Jhc2U2NCwuLi4ifSwicHJlZkxhbmciOiJlbiJ9.XC7W_55DR5Duk2iU2TVHi1rO1bUOmaUbhxbQX91dyx2oThblNVXDL9LhEVlWl8TgO0S9g8f5bdz3B0jhoOIklg
```

**Example of a metadata trust statement in W3C VCDM 2.0**

```
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2"
  ],
  "type": ["VerifiableCredential", "TrustStatementMetadataV1"],
  "issuer": "did:example:issuer",
  "issuedAt": "2010-01-01T00:00:00Z",
  "validFrom": "2010-01-01T00:00:00Z",
  "validUntil": "2010-01-01T00:00:00Z",
  "credentialSubject": {
    "id": "did:example:subject",
    "entityName": {
      "en": "John Smith's Smithery",
      "de-CH": "John Smith's Schmiderei"
    },
    "registryIds": [
      {
        "type": "UID",
        "value": "CHE-000.000.000"
      },
      {
        "type": "LEI",
        "value": "0A1B2C3D4E5F6G7H8J9I"
      }
    ],
    "logoUri": {
      "en": "data:image/png;base64,..."
    },
    "prefLang": "en"
  },
  "credentialStatus": {...},
  "proof" {...}
}

```

### Fetch Trust Statements List

This endpoint returns an array of currently active trust statements for a given URI if not otherwise stated by the parameters.

```
SCHEME://DOMAIN/api/v1/truststatements/{identifier_URI}?path_param=param_value
``` 

The endpoint has the following parameters:
| Type   | Name           | Data Type |Default | Description |
| -------|----------------|-----------|--------|-------------|
| path   | Identifier_URI | string    | -      | The URL encoded URI of the entity for which trust statements should be returned. |
| query  | filter_active  | bool      | true   | Active filter, if false also return trust statements which are currently not active. |
| query  | filter_format  | string    | -      | VC format filter, if applied only returns VCs that are in the selected format. Formats for verifiable credentials are defined in [OpenID4VCI credential format profiles](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID1.html#name-credential-format-profiles) |

**Example of a Trust Statement List**

Request
```
GET /api/v1/truststatements/did%3Aexample%3Asubject HTTP/1.1
Host: trust.example.com
```

Result
```
[  "eyJ0eXAiOiJ2YytzZC1qd3QiLCJhbGciOiJFUzI1NiIsImtpZCI6ImRpZDpleGFtcGxlOmlzc3VlciNrZXktMSJ9.eyJ2Y3QiOiJUcnVzdFN0YXRlbWVudE1ldGFkYXRhVjEiLCJpc3MiOiJkaWQ6ZXhhbXBsZTppc3N1ZXIiLCJzdWIiOiJkaWQ6ZXhhbXBsZTpzdWJqZWN0IiwiaWF0IjoxNjkwMzYwOTY4LCJuYmYiOjE3MjE4OTY5NjgsImV4cCI6MTc1MzQzMjk2OCwic3RhdHVzIjoiLi4uIiwib3JnTmFtZSI6eyJlbiI6IkpvaG4gU21pdGgncyBTbWl0aGVyeSIsImRlLUNIIjoiSm9obiBTbWl0aCdzIFNjaG1pZGVyZWkifSwibG9nb1VyaSI6eyJlbiI6ImRhdGE6aW1hZ2UvcG5nO2Jhc2U2NCwuLi4ifSwicHJlZkxhbmciOiJlbiJ9.XC7W_55DR5Duk2iU2TVHi1rO1bUOmaUbhxbQX91dyx2oThblNVXDL9LhEVlWl8TgO0S9g8f5bdz3B0jhoOIklg",
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2"
  ],
  "type": ["VerifiableCredential", "TrustStatementMetadataV1"],
  "issuer": "did:example:issuer",
  "issuedAt": "2010-01-01T00:00:00Z",
  "validFrom": "2010-01-01T00:00:00Z",
  "validUntil": "2010-01-01T00:00:00Z",
  "credentialSubject": {
    "id": "did:example:subject",
    "orgName": {
      "en": "John Smith's Smithery",
      "de-CH": "John Smith's Schmiderei"
    },
    "logoUri": {
      "en": "data:image/png;base64,..."
    },
    "prefLang": "en"
  },
  "credentialStatus": {...},
  "proof" {...}
}
]
``` 

## References

**OpenID4VCI credential format profiles**<br/>
https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0-ID1.html

**RFC2397**<br/>
https://datatracker.ietf.org/doc/html/rfc2397

**RFC5646**<br/>
https://datatracker.ietf.org/doc/rfc5646/

**SD-JWT VC**<br/>
https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/

**W3C VCDM 2.0**<br/>
https://www.w3.org/TR/vc-data-model-2.0/

**XMLSCHEMA11-2**<br/>
https://www.w3.org/TR/xmlschema11-2/#dateTime
