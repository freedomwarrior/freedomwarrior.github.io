#### WireGuard server side configure

Put this in `/etc/wireguard/wg0.conf`:
```
[Interface]
Address = 10.5.0.1/24
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o enp4s0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o enp4s0 -j MASQUERADE
ListenPort = 51820
PrivateKey = <server private key>

[Peer]
PublicKey = AzEeDj6QK/QKYaTffJ2ONqSyXWCUktTO0xKYBp8AM1k=
AllowedIPs = 10.5.0.2/32

[Peer]
PublicKey = bAuSLgHIsvMWHSUVgI5ZbNp6FlOS91WV5vDAevEjZCk=
AllowedIPs = 10.5.0.3/32
```
  `[Interface]` - server side configure block 
  - `Address = 10.5.0.1/24` - inside tunnel net
  - `PostUp` and `PostDown ` firewall configure
  - `ListenPort ` - what port server should listen for new connections
  - `PrivateKey ` - servers private key, generated via `wg genkey`
 
  `[Peer]` - peer configureblock
  - `PublicKey` - peer public key, generated via `wg genkey`
  - `AllowedIPs` - peer ip-address 
 
 #### WireGuard client side configure
 
 `/etc/wireguard/wg0.conf`:
 ```
[Interface]
PrivateKey = <client private key>
Address = 10.5.0.2/24

[Peer]
PublicKey = <server public key>
Endpoint = <server public ip>:1194
AllowedIPs = 0.0.0.0/0
 ```
all the packets destined to `AllowedIPs` will be encrypted with `PublicKey` and sent to `Endpoint`.
