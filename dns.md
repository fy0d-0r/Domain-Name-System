# Domain Name System (DNS)

DNS is comprised logically of Domains but physically of Zones

- DNS is Application layer protocol
- DNS uses TCP for Zone transfer and UDP for name(dns queries)

## Terminologies
```
Recursive DNS Server
Name Server(Authoritative Server)
Root DNS Servers
TLD Server
Zone File
Resource Record (RR)
DNS Record
Record Types
Namespace
Zone
Zone Transfer
Domain
Domain Registrars
Registry
```
## DNS Query Steps
![dns_query_steps](https://github.com/fy0d-0r/Domain-Name-System/blob/main/images/Screenshot-2022-10-17-at-5.27.27-PM.png)
```
Step 1: recursive server <-> Root DNS Server
Step 2: recursive server <-> TLD Server
Step 3: recursive server <-> Name Server
```


## Recursive DNS Server
Recursive DNS Server(DNS Resolver) communicates with several other DNS servers to hunt down an ip address and return it to the client.
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

## Root DNS Server
- Root DNS Servers: `https://www.iana.org/domains/root/servers`
- They are backbone of internet
- their job is to redirect you to the correct Top Level Domain Server

## Top Level Domain DNS Server
holds records for name servers
- gTLD (generic)
- ccTLD (country code)

## Name Server(Authoritative Server)
- Works on tcp port 53
- Names servers have "zone files" which are but text files that holds Resource Record informations
- Name server stores "DNS records" for a particular domain name
- A name server is a library, and "DNS records" are the catalog.
- You'll often find multiple nameservers for a domain name to act as a backup in case one goes down.
- name server sents dns records back to recursive server

## DNS Record (Resource Record)
- DNS records hold the information about which IP addresses match which domains
- DNS records all come with a TTL (Time To Live) value. This value is a number represented in seconds that the response should be saved for locally until you have to look it up again.
- DNS records are stored in the zone file

## Zone File
- Zone file is a text file that describes a DNS zone.
- A DNS zone is a subset, often a single domain, of the hierarchical domain name structure of the DNS.
- The zone file contains mappings between domain names and IP addresses and other resources, organized in the form of text representations of resource records (RR)

### Zone File Format
```
$TTL	86400		; $TTL used for all RRs without explicit TTL value
$ORIGIN example.com.	; designates the start of this zone file in the namespace
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
; @ represents zone origin
ns1    IN  A      192.168.0.1  ;name server definition     
www    IN  A      192.168.0.2  ;web server definition
ftp    IN  CNAME  www.example.com.  ;ftp server definition
; non server domain hosts
bill   IN  A      192.168.0.3
fred   IN  A      192.168.0.4 
```
- The format of a zone file is defined in RFC 1035 (section 5) and RFC 1034 (section 3.6.1).
- This format was originally used by the "Berkeley Internet Name Domain" (BIND) software package, but has been widely adopted by other DNS server software.

- A DNS zone file is constructed using Resource Records (RRs) and Directives. The text representation of these records are stored in zone files.
- A zone file is a sequence of line-oriented entries, each of which is either a directive or a text description that defines a single resource record (RR). An entry is composed of fields separated by any combination of white space (tabs and spaces), and ends at a line boundary except inside a quoted string field value or a pair of enclosing formatting parentheses. Any line may end with comment text preceded by a semicolon, and the file may also contain any number of blank lines.
- Entries may occur in any order in a zone file, with some exceptions.


Resource Records (RR)
- Resource Record is the unit of information entry in DNS zone files
- The first Resource Record must be the SOA (Start of Authority) record. The generic format is described below.
```
name	ttl	record class	record type	record data
```
ttl field in the above example is omitted since their ttl values are assigned implicitly by $TTL directive.
'@' represents the zone origin

Directives
- Directives are control entries that affect the rest of the zone file. The first field of a directive consists of a dollar sign followed by a keyword.
```
$ORIGIN		- $ORIGIN is followed by a domain name to be used as the origin for subsequent relative domain names.
$TTL		- is followed by a number to be used as the default TTL (time-to-live).
```

Record Class
- The record class field indicates the namespace of the record information
- The most commonly used namespace is that of the Internet, indicated by parameter IN


Record Types: 
- A - Returns a 32-bit IPv4 address, most commonly used to map hostnames to an IP address of the host
- AAAA - Returns a 128-bit IPv6 address, most commonly used to map hostnames to an IP address of the host.
- CNAME(canonical name) - points a domain name (an alias) to another domain(domain name to domain name)
- MX (returns Mail Exchange servers)
- TXT
- NS - specifies the authoritative DNS server for a domain
- PTR record - A pointer record provides a domain name for reverse lookup. It's the opposite of an "A" record as it provides the domain name linked to an IP address instead of the IP address for a domain.
- SOA (Start of Authority) - Specifies authoritative information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.
At minimum, the zone file must specify the Start of Authority (SOA) record with the name of the authoritative master name server for the zone and the email address of someone responsible for management of the name server

> NOTE: In the zone file, domain names that end with a full stop character (such as "example.com." in the above example) are fully qualified while those that do not end with a full stop are relative to the current origin (which is why www in the above example refers to `www.example.com`).

## Namespace
- the internet is a single DNS name space, within which all network devices with a DNS name can be resolved to a particular address
- namespaces: ".", ".com", ".net"
- The domain namespace of the internet is organized into a hierarchical layout of subdomains below the DNS root domain.(wikipedia)

## Zone
- an administrative sub-division of the DNS namespace

## Zone vs Domain
- The distinction between domains and zones is that domains provide a logical structure to the DNS name space while zones provide an administrative structure
- Domain name servers store information about part of the domain name space called a zone. The name server is authoritative for a particular zone. A single name server can be authoritative for many zones.
- A zone is simply a portion of a domain. For example, the Domain Microsoft.com may contain all of the data for Microsoft.com, Marketing.microsoft.com and Development.microsoft.com. However, the zone Microsoft.com contains only information for Microsoft.com and references to the authoritative name servers for the subdomains.
- The zone Microsoft.com can contain the data for subdomains of Microsoft.com if they have not been delegated to another server. For example, Marketing.microsoft.com may manage its own delegated zone. Development.microsoft.com may be managed by the parent, Microsoft.com.
- If there are no subdomains, then the zone and domain are essentially the same. In this case the zone contains all data for the domain
- ref:`https://stackoverflow.com/questions/22440582/difference-between-a-dns-zone-and-dns-domain`

![zone-domain](https://github.com/fy0d-0r/Domain-Name-System/blob/main/images/zone_domain.png)

## Zone Transfer
- copy contents of zone file on a primary dns server to secondary dns server
- Use AXFR Protocol
- replicate DNS records across DNS servers (avoid the need to edit information on multiple DNS servers, you can edit information on one server and use AXFR to copy information to other servers)
- DNS servers host zones. A DNS zone is a portion of the domain name space that is served by a DNS server. For example, example.com with all its subdomains may be a zone. However, second.example.com may also be a separate zone.

## Why only 13 root servers?
- because limitations of original DNS Infrastructure
- `https://securitytrails.com/blog/dns-root-servers`

## domain registrars
They provides domain name registrations to the general public
- GoDaddy
- NameCheap

## Registry
- registries are companies that manage Top Level Domains (TLDs)

## What does it mean to ‘own’ a domain name?
Although people often speak of buying and owning domain names, the truth is that registries own all of their domain names and registrars simply offer customers the opportunity to reserve those domain names for a limited amount of time.
The maximum reservation period for a domain name is ten years. A user can hold onto a domain name for longer than ten years, as registrars usually let them keep renewing their reservation indefinitely. But the user never truly owns the domain; they just lease it.

> NOTE: .local belongs to mDNS(Multicast DNS)

References
```
rfc:1035
rfc:1034
```
```
https://en.wikipedia.org/wiki/Zone_file
```
