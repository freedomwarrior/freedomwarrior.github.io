### WireGuar server side configure

Create `wg0.conf` with this content:
```
[Interface]
Address = 10.5.0.1/24
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o enp4s0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o enp4s0 -j MASQUERADE
ListenPort = 51820
PrivateKey = ********************************

[Peer]
PublicKey = AzEeDj6QK/QKYaTffJ2ONqSyXWCUktTO0xKYBp8AM1k=
AllowedIPs = 10.5.0.2/32

[Peer]
PublicKey = bAuSLgHIsvMWHSUVgI5ZbNp6FlOS91WV5vDAevEjZCk=
AllowedIPs = 10.5.0.3/32
```
 - `[Interface]` - server side configure block 
 - - `Address = 10.5.0.1/24` - inside tunnel net
 - - `PostUp` and `PostDown ` firewall configure
 - - `ListenPort ` - what port server should listen for new connections
 - - `PrivateKey ` - servers private key, generated via `wg genkey`
 ---
 - `[Peer]` - peer configureblock
 - - `PublicKey` - peer public key, generated via `wg genkey`
 - - `AllowedIPs` - peer ip-address 

