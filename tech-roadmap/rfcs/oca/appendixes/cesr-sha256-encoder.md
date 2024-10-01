# Appendix - CESR SHA-256 Encoder

This document proposes an implementation to encode SHA-256 digest in the CESR format.

>[!WARNING]
> The canonicalizition step is necessary, as the order of JSON properties can not be guaranteed otherwise. JCS (RFC8785) sets a specification to canonicalize JSON.

```javascript
// Generate CESR encoded SHA-256 digest
// Code only works for digest with fixed length!
// see original function _infil() https://github.com/WebOfTrust/cesr-ts/tree/development/src/matter.ts#L387
  
const { Buffer } = require('node:buffer');
const crypto = require('node:crypto');
const canonicalize = require('canonicalize');
  
const data ={"data": "value"};
const canonicalizedData = canonicalize(data)
const raw = crypto.createHash('sha256').update(canonicalizedData).digest();
const padSize = (3 - (raw.length % 3)) % 3; // compute necessary lead size bytes. A SHA-256 digest's length will always be 32 which means that the padSize will be 1.
  
const code = "I" // SHA2_256 digest CESR metadata
const codeSize = code.length; // SHA2_256 digest CESR metadata
  
// prepad the SHA-256 digest
const bytes = new Uint8Array(padSize + raw.length);
for (let i = 0; i < padSize; i++) {
    bytes[i] = 0;
}
for (let i = 0; i < raw.length; i++) {
    const odx = i + padSize;
    bytes[odx] = raw[i];
}
  
// Transform padded SHA-256 bytes to Base64, Remove character 'A' (6 out of the 8 padding bits) and add CESR metadata code in front
console.log(
    code +
    Buffer.from(bytes)
        .toString('base64url')
        .slice(codeSize % 4)
);
```
