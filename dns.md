# Domain Name System (DNS)

DNS is comprised logically of Domains but physically of Zones

- DNS is Application layer protocol
- DNS uses TCP for Zone transfer and UDP for name(dns queries)

## Terminologies
```
Recursive DNS Server
Name Server(Authoritative Server)
DNS Record
Root DNS Servers
TLD Server
Record Types
Namespace
Zone
Zone Transfer
Domain
Domain Registrars
Registry
```

## Recursive DNS Server
Recursive DNS Server communicates with several other DNS servers to hunt down an ip address and return it to the client.
The Google DNS server (8.8.8.8) is in fact the recursive dns server hosted by Google.
### Public Recursive DNS Servers
```
Google: 8.8.8.8
Cloudflare: 1.1.1.1
Cloudflare: 1.0.0.1
Google: 8.8.8.8
Google: 8.8.4.4
Quad9: 9.9.9.9
Quad9: 149.112.112.112
```
