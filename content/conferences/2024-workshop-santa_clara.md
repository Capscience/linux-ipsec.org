Title: 2024 Linux IPsec workshop, Santa Clara (15 - 16 July)
Date: 2025-05-26
Summary: The 2024 Linux IPsec workshop was held 15 - 16 July in Santa Clara, California, colocated with the Netdev 0x18 conference.

## Topics
###  Steffen
* ESP problem statement
* Wrapped ESP v2
   * [https://datatracker.ietf.org/doc/draft-klassert-ipsecme-wespv2/](https://datatracker.ietf.org/doc/draft-klassert-ipsecme-wespv2/)
* (PSP)
* Combined Tunnel-Interfaces (e.g. xfrm and gre/vxlan)

### Chris Hopps
* IP-TFS Update
* IP-TFS Upcoming Work (constant send rate w/ congestion control)

### Yan Yan
* Android Kernel Networking Unit Tests (30 min.)

### Feng Wang
* XFRM interface identifier in packet offload mode (30 min.)

### Antony
* Encrypted ESP Ping
   * [https://datatracker.ietf.org/doc/draft-antony-ipsecme-encrypted-esp-ping/](https://datatracker.ietf.org/doc/draft-antony-ipsecme-encrypted-esp-ping/)


## Notes
Android Kernel Networking Unit Tests example: [https://android-review.googlesource.com/c/kernel/tests/+/1668886](https://android-review.googlesource.com/c/kernel/tests/+/1668886)
[https://lore.kernel.org/netdev/20220119000014.1745223-2-evitayan@google.com/](https://lore.kernel.org/netdev/20220119000014.1745223-2-evitayan@google.com/)

Packet spraying, Falcon: use multiple paths out of order receiing is ok. e.g  matrices data like that

### Notes from the PSP + WESPv2 Talks on Tuesday morning
* Widely deplayed.
   * 10M+ connections
   * 100K+ connections/sec can be established
   * 100usec key derivation

Mutltipath 

No state of SA in the NIC., a.k.a. no SADB, no state.

Cryptoffset 

Outer UDP DST is not used for RSS entropy


Outer UDP SRC port used for entropy/ECMP : source avoids well known ports.



Fixed offset: pointed 

sample active probe



Version (4 bits):  algorithms in every packet  because stateless decrypting



KDF: NIST adversied function

200 M/sec key derivattion 



TX:  Key comes from the socket. Not stored in the NIC

Device keys:  spi 0 and 1

Rekey is time bytes. 

IV is time on the device, from PTP clock. 



WESPv2
* need to be fixed offset hardware parsable fields

Flow identfier: Hardware flags need to be at a fixed location.



Crypt offset: only need L3 header a d not intented expose user payload
