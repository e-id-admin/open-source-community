# Visualization of Verifiable Credential with OCA - Specification

Status: Draft<br/>
Version: 0.1<br/>
Authors: Michel Sahli, Marco Stähli, Martina Kolpondinos, Jonas Niestroj

## Introduction

This document extends the [Overlays Capture Architecture (OCA)](https://oca.colossi.network/) by adding a technical specification for visualizing verifiable credentials (VCs) through a mobile wallet. The aim is to define a set of requirements and clarify the use and support of OCA functionalities that go beyond the core [OCA specifications 1.0.1](https://oca.colossi.network/specification/) by focusing on the context of VC visualization.

## Capture Base: JSON Pointers attribute mapping
- [JSON Pointers (RFC6901)](https://datatracker.ietf.org/doc/html/rfc6901) **MAY** be used instead of the plain attribute names. 
- Attributes that begin with a slash `/` **MUST** be interpreted as JSON Pointers.

A JSON Pointer is a string syntax that defines a path through a JSON document. It facilitates to fetch the value(s) of the specific key property (the Pointer points to) within that document, including values of a nested object. 

The simplest syntax proposes to point to an JSON property with a slash separated path like the following example:

```
/credentialSubject/firstname
``` 

Root of the JSON Pointers path is the root node of the JSON document.

**Example Capture Base with JSON Pointer**

```json
{
    "capture_base":{
        "type":"spec/capture_base/1.0",
        "classification":"",
        "digest":"IMFQWZ_xszfSOugAuYHmlhmQm3EUgJ_uk0S9ExISjqbc",
        "attributes":{
            "/credentialSubject/firstname":"Text",
            "/credentialSubject/address/street":"Text"
        }
    }
}
```

## Additional Overlays
The following Overlays are defined additionally to the ones defined in the core OCA specification to be able to visualization verifiable credentials.

### Aries Branding Overlay update proposal
The core OCA specification does not include a way to add visualization metadata. The [Hyperledger Aries project](https://github.com/hyperledger/aries-rfcs/blob/main/features/0755-oca-for-aries/README.md#aries-specific-branding-overlay) has addressed this gap by proposing a Branding Overlay which mimics the options of visualizing data element about the branding of a given verifiable credential.

This specification adds additional attributes to the Branding Overlay to further enhance the visualization of Verifiable Credentials:

- the Branding Overlay **MAY** use the `language` attribute from the core OCA specification.
- the Branding Overlay **MAY** use an additional `theme` attribute.

And the context of OCA Bundle, it adds the following constraints:

- the Branding Overlay **MUST** use the value `aries/overlays/branding/1.1` in the `type` attribute.
- the Branding Overlay **MUST** only use the embedded form of media in form of data URLs.
- The primary and secondary attributes of the Branding Overlay determine which attribute value from the Capture Base will be displayed. If the defined attribute does not exist in the Capture Base or in the Verifiable Credential, the value of the corresponding Branding Overlay attribute is displayed instead.

**Example of the Branding Overlay**

```json
{
    "type":"aries/overlays/branding/1.1",
    "capture_base":"IMFQWZ_xszfSOugAuYHmlhmQm3EUgJ_uk0S9ExISjqbc",
    "language":"en",
    "theme":"light",
    "logo":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==",
    "background_image":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==",
    "background_image_slice":"",
    "primary_background_color":"#003366",
    "secondary_background_color":"#003366",
    "primary_attribute":"firstname",
    "secondary_attribute":"lastname",
    "issued_date_attribute":"",
    "expiry_date_attribute":"",
}
```

> [!NOTE]
> It is up to the Wallet implementors to interpret the Branding Overlay attributes they need to implement their design und style guidelines.

### Cluster Ordering Overlay
The Cluster Ordering Overlay adds the possibility to enable the grouping and ordering of attributes defined in the OCA Capture Base. It allows VC Issuers to group more relevant attributes first and order technical based information like identifiers at the end.

- The `type` attribute's value **MUST** be `admin-ch/overlays/cluster_ordering/1.0`

In addition to the `capture_base`, `type` and `language` attributes (see Common attributes), the Cluster Ordering Overlay **MUST** include the following attribute:

- `cluster_order`
JSON object that contains a map of key-value pairs (String:Int) which defines the identifiers of the different clusters and their order.

- `attribute_cluster_order`
JSON object that contains JSON objects with a key labeled after a cluster identifier. Each cluster JSON object contains a map of key-value pairs (String:Int) which defines the order of the attributes within the cluster.

and `MAY` include

- `cluster_labels`
JSON object that contains a map of key-value pairs (String:String) which defines the language in which the attributes of the cluster identifiers are displayed. The label of a cluster without a defined cluster-identifier-label mapping is not visually rendered.

Attributes not declared in the Cluster Ordering Overlay **SHOULD** be visually displayed after the declared clusters and **MUST** not be hidden from the user. 

>[!NOTE]
> It is up to the Wallet implementers to define the order and cluster for non declared attributes.

**Cluster Odering Example**

Dataset
```json
{
    "id":"123456",
    "name":"Helvetia",
    "birthdate":"2000-01-01",
    "photo":"data:image/png;base64,...",
    "address":{
        "street":"Bundesplatz",
        "city":"Bern",
        "country":"Switzerland"
    }
}
```

OCA bundle
```json
{
    "capture_base":{
        "type":"spec/capture_base/1.0",
        "classification":"",
        "digest":"IDnzU7K8-ndOuuI1p81uO6zTnrpfXJx2DDFJXbmZZWwf",
        "attributes":{
            "id":"Text",
            "name":"Text",
            "birthdate":"DateTime",
            "photo":"Text",
            "/address/street":"Text",
            "/address/city":"Text",
            "/address/country":"Text"
        }
    },
    "overlays":[
        {
            "capture_base":"IDnzU7K8-ndOuuI1p81uO6zTnrpfXJx2DDFJXbmZZWwf",
            "type":"admin-ch/overlays/cluster_ordering/1.0",
            "language":"de",
            "cluster_order":{
                "main":1,
                "address":2,
                "additional":3
            },
            "cluster_labels":{
                "main":"Inhalt",
                "address":"Adresse",
                "additional":"Ergänzungen"
            },
            "attribute_cluster_order":{
                "main":{
                    "name":2,
                    "photo":1,
                    "birthdate":3
                },
                "address":{
                    "/address/street":1,
                    "/address/city":2,
                    "/address/country":3    
                },
                "additional":{
                    "id":1
                }
            }
        }
    ]
}
``` 

## Special type handling
In the core OCA specification, attributes in the Capture Base are defined through a data type. Those data types are not only used to understand the type of an attribute value but also to provide a specific way to interpret data in the Overlays.

Special types, such as data URLs, need to have a standardised representation so that they can be interpreted the same way by everyone.

### DateTime ISO8601
The DateTime type is represented with the following constraints:

- The DateTime attribute **MUST** be of type `DateTime` in the Capture Base.
- The DateTime **SHOULD** define the ISO8601 time format in the Format Overlay. If none is specified, the time format `YYYY-MM-DDTHH:mm:ssZ` **MUST** be assumed.
- The DateTime **MUST** define ISO8601 with the urn URI scheme in the Standard Overlay.

**Example OCA bundle with a DateTime attribute**

```json
{
    "capture_base":{
        "type":"spec/capture_base/1.0",
        "classification":"",
        "digest":"ICs3_W5ardFeXihcLHxy6hQFlyZyYDNNH-w9GsfolD-Z",
        "attributes":{
            "date":"DateTime"
        }
    },
    "overlays":[
        {
            "capture_base":"ICs3_W5ardFeXihcLHxy6hQFlyZyYDNNH-w9GsfolD-Z",
            "type":"spec/overlays/format/1.0",
            "attribute_formats":{
                "date":"YYYY-MM-DDTHH:mm:ssZ"
            }
        },
        {
            "capture_base":"ICs3_W5ardFeXihcLHxy6hQFlyZyYDNNH-w9GsfolD-Z",
            "type":"spec/overlays/standard/1.0",
            "attr_standards":{
                "date":"urn:iso:std:iso:8601"
            }
        }
    ]
}
```

### Data URLs
Data URL is a way to represent media like image in a compact form to be embedded into other formats. Data URLs are defined by the [RFC2397](https://datatracker.ietf.org/doc/html/rfc2397).

Data URLs are represented with the following constraints:

- The data URL attribute **MUST** be of type `Text` in the Capture Base
- The data URL image **MUST** define RFC2397 with the urn URI scheme in the Standard Overlay

**Example OCA bundle with an image Data URL**

```json
{
    "capture_base":{
        "type":"spec/capture_base/1.0",
        "classification":"",
        "digest":"IHfGgFAZcSIp707AG2DXr4zZHxXOwuoxckbHFIrJTEx4",
        "attributes":{
            "picture":"Text"
        }
    },
    "overlays":[
        {
            "capture_base":"IHfGgFAZcSIp707AG2DXr4zZHxXOwuoxckbHFIrJTEx4",
            "type":"spec/overlays/standard/1.0",
            "attr_standards":{
                "picture":"urn:ietf:rfc:2397"
            }
        }
    ]
}
```

## OCA reference in Verifiable Credentials
For the OCA to be usable, it needs to be resolvable by the Wallet and referenced inside the VC.

Additional VC formats **MAY** be specified and used as long as the Wallet is able to resolve the OCA bundle. 

### SD-JWT VC
When an Issuer desires to specify OCA rendering instructions for a Verifiable Credential in the `vc+sd-jwt` format, they **MUST** add a render property that uses the data model described below.

- `render`
JSON object containing an URI to metadata that supports the visualization of the SD-JWT VC

    - `type`
A string which represent the type of rendering. **MUST** be `OverlaysCaptureBundleV1`
    - `oca`
A URL that dereferences to an OCA bundle file with an associated `application/json` media type. The last URL path segment **MUST** be the OCA bundle filename which is the CESR encoded digest of the file's content and the `json` file extension.


The data model defined above is expressed next with the example of a Verifiable Credential in SD-JWT format

```json
{
  "vct":"https://example.com/credential/1.0",
  "firstname":"John",
  "lastname":"Smith",
  "render":{
    "type":"OverlaysCaptureBundleV1",
    "oca":"https://example.com/oca/IEY2Sow9DYS8cSAUN3ot95BRMVxzlCrwtkqBdpZE1kI8.json"
  }
}
``` 

### W3C VC
In a similar way, if an Issuer desires to specify OCA rendering instructions for a W3C Verifiable Credential, they **MUST** add a `renderMethod` property that uses the data model described below.

- `renderMethod`
JSON array containing URIs to metadata that help the visualisation of the W3C VC
    - `type`
A string which represent the type of rendering. **MUST** be `OverlaysCaptureBundleV1`
    - `id`
A URL that dereferences to an OCA bundle file with an associated `application/json` media type. The last URL path segment **MUST** be the OCA bundle filename which is the CESR encoded digest of the file's content and the `json` file extension.


The data model defined above is expressed next with the example of a W3C Verifiable Credential

```json
{
  "@context":[
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/vc/render-method/v1"
  ],
  "id":"http://example.com/credentials/3732",
  "type":["VerifiableCredential"],
  "credentialSubject":{
    "firstname":"John",
    "lastname":"Smith"
  },
  "renderMethod":[{
    "id":"https://example.com/oca/IEY2Sow9DYS8cSAUN3ot95BRMVxzlCrwtkqBdpZE1kI8.json",
    "type":"OverlaysCaptureBundleV1"
  }]
}
```

## Limitations
- OCA bundle **MUST** be a single json file
- OCA bundle **MUST** be named after the CESR encoded SHA-256 hash of the file content and finish with the json file extension.
- OCA bundle and the Capture Base **MUST** be canonicalized with [JCS (RFC8785)](https://datatracker.ietf.org/doc/html/rfc8785) before generating a digest.

## Implementation Guide
### CESR encoding
The OCA specification defines that the OCA file name and the digest inside the OCA bundle have to use the [CESR encoding](https://weboftrust.github.io/ietf-cesr/draft-ssmith-cesr.html).

CESR is an encoding format for text and binary data that has the unique property of text-binary concatenation composability (for context: A popular encoding format for binary is Base64).

For those interested, the composable concatenation is described in detail in the [CESR Specification](https://trustoverip.github.io/tswg-cesr-specification/#concatenation-composability-property). However, it is not necessary to understand how the encoding of digests works to proceed with this document.

CESR uses the Base64 transformation in it's process to encode binaries but the composability property only works when no Base64 padding is used (when you have padding in Base64 you can not add two binaries together!).

To avoid Base64 padding, the smallest common denominator between the number of bits in a byte and the information stored in a Base64 character (6 bits of information in a byte) has to be used.

The smallest common denominator of 8 and 6 is 24, so the smallest CESR unit is 24 bits. The essential information here is that the number of bits used for binary text encoding with CESR has to be divisible by 8 and 6.

Each CESR encoding also includes metadata over the content of the data. In the digest case, it will start with a letter which defines which digest algorithm was used.

### Generate CESR encoding flow with SHA-256
A SHA-256 digest has a size of 256 bits. It is divisible by 8 but not by 6. The next possible value would be 264 bits which are 33 bytes.

Hence, the CESR encoding will need 33 bytes to work which means that the SHA-256 hash needs a padding byte.

The CESR encoding flow works as described next and depicted in Fig. 1.:

1. Add a leading padding byte to the binary digest. (result = lead byte + binary digest)
2. Encode the result from step 1 in Base64
3. Remove the part of the Base64 encoded padding by removing character A (will remove 6 of the 8 lead padding bits)
4. Add the CESR metadata letter in front of the result of step 3 (uppercase i => I for SHA-256 digest)

![CESR explained](./cesr-explained.png)

A [CESR SHA-256 JavaScript implementation](./appendixes/cesr-sha256-encoder.md) can be found in the appendix.

### Creation example of an OCA Bundle
1. Prepare the capture_base and fill the digest with the diggest dummy defined by [CESR](https://weboftrust.github.io/ietf-cesr/draft-ssmith-cesr.html#section-4.2) (44 '#' for the SHA-256 digest).
    ```json
    {
        "type":"spec/capture_base/1.0",
        "classification":"",
        "digest":"############################################",
        "attributes":{
            "name":"Text"
        }
    }
    ``` 

2. Compute the CESR encoded SHA-256 digest with the code given above and put the value into the digest property. The output should match the following result.

    ```json
    {
        "type":"spec/capture_base/1.0",
        "classification":"",
        "digest":"IJ6mvVUywTrpQYvKx8UzWH2QX7SLZRLi23YlfqRsuJkP",
        "attributes":{
            "name":"Text"
        }
    }
    ``` 

3. Add the Capture Base and Overlays to the JSON and fill the reference to the Capture Base in each Overlay.

    ```json 
    {
        "capture_base":{
            "type":"spec/capture_base/1.0",
            "classification":"",
            "digest":"IJ6mvVUywTrpQYvKx8UzWH2QX7SLZRLi23YlfqRsuJkP",
            "attributes":{
                "name":"Text"
            }
        },
        "overlays":[
            {
                "capture_base":"IJ6mvVUywTrpQYvKx8UzWH2QX7SLZRLi23YlfqRsuJkP",
                "type":"spec/overlays/label/1.0",
                "language":"fr-CH",
                "attribute_labels":{
                    "name":"Nom"
                }
            }
        ]
    }

    ```
4. The core OCA specification computes a CESR encoded digest for each Overlay in the same way as in steps 1 and 2. This documentation skips the generation of the Overlay digests, as the additional digests add no value to a single file OCA bundle. 
5. Compute the CESR encoded SHA-256 digest of the whole file above to get the filename of the OCA bundle. The output should be

    ```
    INoKyVAAE497eTh30eNOTHC21PQl0LIYDys_CIyBfMzZ
    ```

After those steps a file named INoKyVAAE497eTh30eNOTHC21PQl0LIYDys_CIyBfMzZ.json with the following content is generated:

```json
{
    "capture_base":{
        "type":"spec/capture_base/1.0",
        "classification":"",
        "digest":"IJ6mvVUywTrpQYvKx8UzWH2QX7SLZRLi23YlfqRsuJkP",
        "attributes":{
            "name":"Text"
        }
    },
    "overlays":[
        {
            "capture_base":"IJ6mvVUywTrpQYvKx8UzWH2QX7SLZRLi23YlfqRsuJkP",
            "type":"spec/overlays/label/1.0",
            "language":"fr-CH",
            "attribute_labels":{
                "name":"Nom"
            }
        }
    ]
}
```

## References

**Aries Branding Overlay**<br/>
https://github.com/hyperledger/aries-rfcs/blob/main/features/0755-oca-for-aries/README.md#aries-specific-branding-overlay

**CESR**<br/>
https://weboftrust.github.io/ietf-cesr/draft-ssmith-cesr.html

**ISO8601**<br/>
https://www.iso.org/iso-8601-date-and-time-format.html

**Overlays Capture Architecture**<br/>
https://oca.colossi.network/specification/

**RFC2397**<br/>
https://datatracker.ietf.org/doc/html/rfc2397

**RFC6901**<br/>
https://datatracker.ietf.org/doc/html/rfc6901

**RFC8785**<br/>
https://datatracker.ietf.org/doc/html/rfc8785

**SD-JWT VC**<br/>
https://datatracker.ietf.org/doc/draft-ietf-oauth-sd-jwt-vc/

**W3C VC**<br/>
https://www.w3.org/TR/vc-data-model-2.0/
