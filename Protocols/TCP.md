TCP guarantees in-order, reliable, error-free delivery that is fully duplexable and has flow/congestion control. It’s pipelined, too!

TCP creates reliable data transfer on top of IP’s unreliability using
- ACKs
- Checksum
- Sequence Num
- Timer
- Pipelines
- Sliding Window

TCP uses a retrans timer for the oldest unACKed segment

TCP sets a seq number, ACK goes to number of correctly received bytes e.g.
42 + 8 -> ACK 50 -> 50+20 -> ACK 70 and so on

The receiver is able to piece together what’s in order like so
1. Segment arrives at receiver
	1. If in order, deliver to app and check to see if we have “future” segments available. Send a cumulative ACK
	2. if a future segment, buffer and return the last cumulative ACK
	3. if a duplicate, return duplicate ack
	4. if corrupt, duplicate ack

## Some known ports

| Port Number | Protocol |
| --- | ---|
| 53 | DNS |
| 80 | HTTP |
| 88 | kerberos |
| 135 | msrpc |
| 139 | netBIOS |
| 389 | ldap |
| 443 | HTTPS |
| 445 | SMB |
| 464 | kpasswd5 |
| 593 | http-rpc-epmap |
| 3268 | globalcat ldap |
| 3269 | ldap-ssl |
| 3389 | rdp |


#tcp