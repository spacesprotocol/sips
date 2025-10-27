### Given john@doe and nothing else, how do we determine if it's valid and if so return associated meta-data?


```javascript

// Parse the handle into subname and top-level space
let (subname, space) = handle.split('@');

// Check if the space is a valid space using space-cli getspace
space-cli getspace '@'+space

// If the result is null, then the space is not valid
if (result === null) {
    return null;
}

// Otherwise, parse out the covenant.data as a hex string

// Process the hex string per SIPXX

// The first byte is the version byte

// While there is more bytes to process
    // Get the next byte as the type and following length byte
    // Get the value as the next length bytes of data
    // Switch on the type and process the value accordingly
    // If the type is not recognized, return null
    // If the value is valid, interpret the value accordingly
    // Case type of:
        // 0x01: Subname
        // 0x02: Space
        // 0x03: Data
        // 0x04: Metadata
        // 0x05: Signature
        // 0x06: Public Key
        // 0x07: Private Key
        // 0x08: Hash
        // 0x09: Address


// If the hex string is not valid, return null
return null;

```