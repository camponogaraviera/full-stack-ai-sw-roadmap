<div align='center'>
  <h1> 3.5 System Design </h1>
  <h2> 3.5.1 URL/DNS/SSL </h2>
</div>

# Table of Contents

- [URL](#url)
- [Domain Name vs Domain Name System](#domain-name-vs-domain-name-system)
- [Round-robin DNS](#round-robin-dns)
- [GeoDNS](#geodns)
- [SSL vs TLS](#ssl-vs-tls)
- [SSL Pinning](#ssl-pinning)
- [Registry vs Registrar](#registry-vs-registrar)

---

# URL

URL structure: `protocol://` `domain_name` `.TLD` `:port` `/path` `?query_string` `#fragment_identifier`

Meaning:

- `Protocol`: could be FTP (File Transfer Protocol), SMTP (Simple Mail Transfer Protocol), HTTP (HyperText Transfer Protocol), or HTTPS (HyperText Transfer Protocol Secure).
- `Domain Name`: the domain name of the server hosting the application. This domain name gets resolved to an IP address via DNS.
- `TLD`: The top-level domain (.com, .net, .org, etc.).
- `Port (optional)`: specifies the connection endpoint on the server where the client should connect. It is an optional parameter, and if not specified, defaults to a well-known port for the given protocol (e.g., 80 for HTTP, 443 for HTTPS).
- `Path (optional)`: specifies the specific resource or endpoint to be served.
- `query_string (optional)`: contains the parameters for a query to be applied to the resource, usually in the form of key-value pairs.
- `fragment_identifier (optional)`: identifies a specific location/section within the resource by its ID or name attribute. Works as a bookmark/anchor point for fast access within the page document.

---

# Domain Name vs Domain Name System

## Domain Name

The domain name is the name (address) of the website.

Official Domain Name `Registries`:

- US:
  - https://www.enom.com/
  - https://www.verisign.com/
- BR: https://registro.br/

One can search for the ownership of a domain name at https://www.whois.com/whois/.

## Domain Name System (DNS)

DNS is a [name service](https://en.wikipedia.org/wiki/Directory_service) that works as a mapping by translating human-friendly hostnames (e.g., example.com) into IP/network addresses (e.g., 93.184.216.34 for IPv4) that computers understand. DNS is what connects your domain name (example.com) to your website's actual server (its IP address).

Domain Name System (DNS) records include:

- A: maps a hostname to a 32-bit IPv4 address.
- AAAA: maps a hostname to a 128-bit IPv6 address.
- CNAME: canonical name. Aliases one domain name to another domain name (not an address).
- MX: mail exchange. Names the host(s) accepting SMTP mail for the domain, each with a preference integer where lower is higher priority.
- TXT: arbitrary text attached to a name (e.g., SPF, DKIM).
- NS: delegates a zone to its authoritative nameservers.

The DNS protocol allows a single domain name to resolve to multiple server IP addresses. This happens when a domain owner configures multiple A records (for IPv4) or AAAA records (for IPv6) for the same domain name.

```bash
                    DNS
                     |
                example.com
                     |
        +------------+------------+
        |            |            |
        v            v            v
       IP1          IP2          IP3
     0.0.0.1      0.0.0.2      0.0.0.3
        |            |            |
        v            v            v
    Server A      Server B      Server C
```

---

# Round-robin DNS

[Round-robin DNS](https://en.wikipedia.org/wiki/Round-robin_DNS) is a technique that rotates the order of multiple IP addresses across DNS responses, helping distribute clients across multiple servers hosting the same service (e.g., a website). The simplest implementation returns a list of IP addresses per request, with clients often attempting to connect to the first IP in the list.

Drawbacks: Even distribution is [not guaranteed](https://www.cloudflare.com/learning/dns/glossary/round-robin-dns/) due to both DNS caching and client-side caching.

```bash
Round-robin DNS:

User1 requests example.com
DNS Response 1: IP1, IP2, IP3

User2 requests example.com
DNS Response 2: IP2, IP3, IP1

User3 requests example.com
DNS Response 3: IP3, IP1, IP2

User4 requests example.com
DNS Response 4: IP1, IP2, IP3
        .
        .
        .
```

---

# GeoDNS

An alternative to round-robin DNS is GeoDNS, where a domain name returns a different IP address depending on where the query appears to come from.

```bash
GeoDNS:

European user -> European data center
American user -> American data center
Asian user -> Asian data center
```

---

# SSL vs TLS

The HTTP protocol relies on the TLS protocol (previously SSL, [which is now deprecated](https://datatracker.ietf.org/doc/html/rfc7568)) to establish encrypted communication (HTTPS) between the client and the server.

- SSL: Is a protocol that enables encrypted communication between a client and a server.
- TLS: Is the successor of SSL.

To install an SSL/TLS certificate on a server or third-party provider, the following files are needed:

1. `certificate.crt`: This is your actual certificate issued specifically for your domain.
2. `ca_bundle.crt (certificate chain)`: This is the certificate chain that links your certificate to a trusted root authority. It usually contains one or more intermediate certificates.
3. `private.key`: This is the private key associated with your certificate. Keep this file secure, as it is used to establish a secure connection.

To acquire these files, you should obtain a public certificate from an external certificate authority (CA) that allows you to download both the certificate and private key. Some popular CA options include: [Let's Encrypt](https://letsencrypt.org/getting-started/), [SSL For Free](https://www.sslforfree.com), `ZeroSSL`, `Sectigo`, or other paid providers.

CNAME records (name and value) are typically used for domain verification via DNS validation, and they are not included in the certificate export (not part of the certificate itself).

---

# SSL Pinning

...

---

# Registry vs Registrar

- Domain Name Registry: Manages top-level domains (gTLDs or ccTLDs).
  - Verisign for `.com` generic TLD (gTLD).
  - Nic.br for `.br` country-code TLD (ccTLD).

- Domain Name Registrar: Interface with users to register domain names on their behalf with the registry.
  - [Registro.br](https://registro.br/) is both a domain registrar and the registry operator.
  - [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html) is a domain registrar and DNS provider.
  - [Cloudflare](https://www.cloudflare.com/) is a domain registrar and DNS provider.

---

# References

[1] https://www.cloudflare.com/learning/dns/glossary/round-robin-dns/

[2] https://aws.amazon.com/compare/the-difference-between-ssl-and-tls/
