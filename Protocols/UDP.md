UDP is a barebones protocol. It only guarantees that a message is *sent*, not that the message arrives. 
UDP does not guarantee in-order or reliable delivery, but is useful for situations where no state is needed, small packets are in use, and congestion control doesn’t matter.
## UDP Datagram
![[E078BF9F-5D04-4328-8A28-B3D03AF4FC16.jpeg]]

## UDP Checksum

#udp