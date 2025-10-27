I was thinking about this some more and what might be some desired things to put in rawhex.  Here's my thoughts so far.

0 = Nostr Pubkey in Hex 64 hex digits 0x40
040e739ce127b6d77c34ea12e10245b72742a26c67ce0575c3b0add38dc297b4282

1 = Nostr Relay[ws] (IP, DNS, Spaces) relay.primal.net, relay.primal.net, relay@primal  ex: ws://194.195.222.47:4848/
2 = Nostr Relay[wss] (IP, DNS, Spaces) relay.primal.net, relay.primal.net, relay@primal ex: wss://relay.primal.net/

3 = Pubky.app Pubkey = 7fmjpcuuzf54hw18bsgi3zihzyh4awseeuq5tmojefaezjbd64cy
37666D6A706375757A6635346877313862736769337A69687A7968346177736565757135746D6F6A656661657A6A626436346379

4 = Decentralized ID example = did:btc1 identifier Hex 136 hex digits 0x88
did:btc1:k1q0rnnwf657vuu8trztlczvlmphjgc6q598h79cm6sp7c4fgqh0fkc0vzd9u:
3886469643A627463313A6B317130726E6E77663635377675753874727A746C637A766C6D70686A67633671353938683739636D3673703763346667716830666B6330767A6439753A

5 = DNS A (IPV4 or IPV6 as hex)  
6 = DNS CNAME (DNS, Spaces) www.example.com, www@example, @otherspace, www.example.spaces
7 = DNS SMTP (IP, DNS, Spaces) in1-smtp.messagingengine.com 
8 = DNS TXT (Arbitrary ASCII)
9 = Bitcoin wallet address ex: bc1pjeda67ewjtms6p20nk3udt6c5wwk90zzdlhd3dx73r8ynzsm07nqwxggmu
A = Ethereum wallet address ex: 0xC3P0F36260817d1c78C471406BdE482177a1935071
B = Unused 11
C = Unused 12
D = Unused 13
E = Unused 14
F = Unused 15

Here's an example data payload that would contain both a Nostr pubkey and a did:btc1 identifier.

3+64+3+136=206 bytes to hold 2 data types(Nostr Pubkey + did)
040e739ce127b6d77c34ea12e10245b72742a26c67ce0575c3b0add38dc297b42823886469643A627463313A6B317130726E6E77663635377675753874727A746C637A766C6D70686A67633671353938683739636D3673703763346667716830666B6330767A6439753A

Here's an example data payload that would contain both a Nostr pubkey and 2 relays and a Bitcoin address

3+64+3+22+3+35+3+84=217 bytes to hold 4 data elements(Nostr Pubkey + relay@damus + relay.primal.net + bc1p...ggmu )
 64
040e739ce127b6d77c34ea12e10245b72742a26c67ce0575c3b0add38dc297b4282 \
 22
1  72656C61794064616D7573 \ relay@damus (recursive lookup -> ws://192.168.1.254/)
 35
2  72656C61792E7072696D616C2E6E6574 \ relay.primal.net
 84
9  62633171717067656379353433767A636770666A64746D746C7A3672357563367472676668686D336D6E \Bech32? p2wpkh bc1qqpgecy543vzcgpfjdtmtlz6r5uc6trgfhhm3mn
