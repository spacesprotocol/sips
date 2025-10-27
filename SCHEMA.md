# VTLV Schema Specification

## Overview

This document defines a Version-Type-Length-Value (VTLV) binary encoding scheme for storing multiple data types in a single payload. The schema supports up to 255 different data types and allows for protocol versioning.

## Format Structure

```
[Version: 1 byte][Type: 1 byte][Length: 1 byte][Value: N bytes]
```

- **Version (1 byte)**: Protocol version (0-255)
- **Type (1 byte)**: Data type identifier (0-255) 
- **Length (1 byte)**: Length of value payload (0-255 bytes)
- **Value (N bytes)**: Actual data payload

## Data Types

| Type | Name | Description | Example Value |
|------|------|-------------|---------------|
| 0x00 | Owner URI | RPC Interface | `http://192.168.1.254:8080/v1/services/getspace` |
| 0x01 | Owner Web | Info Website | `https://spacesops.com` |
| 0x02 | Nostr Pubkey | 64 hex digits (32 bytes) | `040e739ce127b6d77c34ea12e10245b72742a26c67ce0575c3b0add38dc297b4282` |
| 0x03 | Nostr Relay | WebSocket relay (IP, DNS, Spaces) | `wss://relay@primal,wss://relay.primal.net,ws://194.195.222.47:4848/` |
| 0x04 | Pubky.app Pubkey | Pubky.app identifier | `pk:7fmjpcuuzf54hw18bsgi3zihzyh4awseeuq5tmojefaezjbd64cy` |
| 0x05 | Decentralized ID | DID identifier (68 bytes hex) | `did:btc1:k1q0rnnwf657vuu8trztlczvlmphjgc6q598h79cm6sp7c4fgqh0fkc0vzd9u:` |
| 0x06 | DNS A Record | IPv4/IPv6 address as hex | IPv4/IPv6 hex representation |
| 0x07 | DNS CNAME | Canonical name (DNS, Spaces) | `www.example.com`, `www@example` |
| 0x08 | DNS SMTP | SMTP server address | `in1-smtp.messagingengine.com` |
| 0x09 | DNS TXT | Arbitrary ASCII text | Any ASCII string |
| 0x0A | Bitcoin Address | Bitcoin wallet address | `bc1pjeda67ewjtms6p20nk3udt6c5wwk90zzdlhd3dx73r8ynzsm07nqwxggmu` |
| 0x0B | Ethereum Address | Ethereum wallet address | `0xC3P0F36260817d1c78C471406BdE482177a1935071` |
| 0x0C-0xFF | Reserved | Unused types | Reserved for future use |

## Payload Examples

### Example 1: Nostr Pubkey + DID
```
Version: 0x01
Type: 0x00, Length: 0x40, Value: 040e739ce127b6d77c34ea12e10245b72742a26c67ce0575c3b0add38dc297b4282
Type: 0x04, Length: 0x88, Value: 3886469643A627463313A6B317130726E6E77663635377675753874727A746C637A766C6D70686A67633671353938683739636D3673703763346667716830666B6330767A6439753A
```

### Example 2: Nostr Pubkey + 2 Relays + Bitcoin Address
```
Version: 0x01
Type: 0x00, Length: 0x40, Value: 040e739ce127b6d77c34ea12e10245b72742a26c67ce0575c3b0add38dc297b4282
Type: 0x01, Length: 0x16, Value: 72656C61794064616D7573
Type: 0x02, Length: 0x23, Value: 72656C61792E7072696D616C2E6E6574
Type: 0x09, Length: 0x54, Value: 62633171717067656379353433767A636770666A64746D746C7A3672357563367472676668686D336D6E
```

## Implementation Notes

1. **Version Field**: Currently set to 0x01. Increment for protocol changes.
2. **Type Field**: Limited to 0-255 (0x00-0xFF) as specified.
3. **Length Field**: Limited to 0-255 bytes per value payload.
4. **Multiple Records**: Multiple VTLV records can be concatenated in sequence.
5. **Endianness**: All multi-byte fields use big-endian byte order.
6. **Padding**: No padding between records - records are tightly packed.

## Parsing Algorithm

```javascript
function parseVTLV(buffer) {
    const records = [];
    let offset = 0;
    
    while (offset < buffer.length) {
        if (offset + 3 > buffer.length) break; // Need at least V+T+L
        
        const version = buffer[offset++];
        const type = buffer[offset++];
        const length = buffer[offset++];
        
        if (offset + length > buffer.length) break; // Not enough data
        
        const value = buffer.slice(offset, offset + length);
        offset += length;
        
        records.push({ version, type, length, value });
    }
    
    return records;
}
```

## Error Handling

- **Invalid Length**: If length field exceeds remaining buffer, skip record
- **Unknown Type**: Parser should preserve unknown types for future compatibility
- **Version Mismatch**: Parser should handle multiple versions gracefully
- **Truncated Data**: Detect and report incomplete records
