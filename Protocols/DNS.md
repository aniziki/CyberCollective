Domain Name Service (DNS) is an Internet protocol historically served over Port 53 on [[UDP]]. DNS over [[TCP]] exists and is used in DNSSEC, but this is not widespread.

## DNS Resolution
DNS Resolution only works because of a hierarchical structure. Very few nameservers actually refer to the target domain in question. There is a significant amount of whizzing-about of packets (most at the top-level are malformed or spam).

### Process of Lookup


### Hierarchy
TLD -> ANS -> Recursive -> Client

### Recursive Resolvers


## DNS Structure
![](http://unixwiz.net/images/dns-query-packet.gif)

Note the query ID. It's randomized to make spoofing harder.
## DNS-Based Attacks
Several attacks (present and former) leverage DNS. These are described below with mitigations.
### [Kaminsky Attack](http://unixwiz.net/techtips/iguide-kaminsky-dns-vuln.html)
Careful design of requests can let bad actors specify their servers as the authoritative nameserver for a target domain. 
By varying the outgoing request port for recursive resolvers as well as the transaction ID, the search space is increased exponentially. DNSSEC can also act as a mitigating factor. [[IPv6]] may also be a mitigating factor, though I don't understand quite how.

### DNS Amplification Attacks
DNS Reflection is achieved by sending a request to a DNS resolver masquerading as the target server by placing the target's IP address in the sender field. DNS responses can be many times larger than the request, adding an amplifying factor to this traffic.

Employing internal filtering to prevent IP address spoofing can prevent networks from being the origin of reflection attacks.

## DNSSEC

#ipv4 #ipv6
#tcp #udp