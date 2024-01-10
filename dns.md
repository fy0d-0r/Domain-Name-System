# Domain Name System (DNS)

DNS is comprised logically of Domains but physically of Zones

- DNS is Application layer protocol
- DNS uses TCP for Zone transfer and UDP for name(dns queries)

## Terminologies
```
Recursive DNS Server
Name Server(Authoritative Server)
DNS Record
Zone File
Root DNS Servers
TLD Server
DNS Record
Record Types
Namespace
Zone
Zone Transfer
Domain
Resource Record (RR)
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

## DNS Record
- DNS records hold the information about which IP addresses match which domains
- DNS records all come with a TTL (Time To Live) value. This value is a number represented in seconds that the response should be saved for locally until you have to look it up again.
- DNS records are stored in the zone file

## Zone File
- Zone file is a text file that describes a DNS zone.
- A DNS zone is a subset, often a single domain, of the hierarchical domain name structure of the DNS.
- The zone file contains mappings between domain names and IP addresses and other resources, organized in the form of text representations of resource records (RR)
```
$TTL	86400 ; 24 hours could have been written as 24h or 1d
; $TTL used for all RRs without explicit TTL value
$ORIGIN example.com.
@  1D  IN  SOA ns1.example.com. hostmaster.example.com. (
			      2002022401 ; serial
			      3H ; refresh
			      15 ; retry
			      1w ; expire
			      3h ; nxdomain ttl
			     )
       IN  NS     ns1.example.com. ; in the domain
       IN  NS     ns2.smokeyjoe.com. ; external to domain
       IN  MX  10 mail.another.com. ; external mail provider
; server host definitions
ns1    IN  A      192.168.0.1  ;name server definition     
www    IN  A      192.168.0.2  ;web server definition
ftp    IN  CNAME  www.example.com.  ;ftp server definition
; non server domain hosts
bill   IN  A      192.168.0.3
fred   IN  A      192.168.0.4 
```

## Zone
