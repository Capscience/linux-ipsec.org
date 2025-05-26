Title: 2022 Linux IPsec workshop, London (3 - 5 November)
Date: 2025-05-26
Summary: The 2022 Linux IPsec workshop took place from 3 - 5 November, right before IETF 115 in London, UK.

## Workshop Notes
- AES-GCM in combination with ESN is NIST compient! There is a packet limit for 4 byte ICVs with aes-gcm 2^32 packets.
- Now, discussion about rekey timers: time SA ﬁrst used, and recently added, time SA last used. Much discussion about who should do what: Kernel vs IKE and which criteria for notifications. Maybe we need an eBPF hook in the statistics hook.
- Article on nftable flow-offload [https://thermalcircle.de/doku.php?id=blog:linux:flowtables_1_a_netfilter_nftables_fastpath](https://thermalcircle.de/doku.php?id=blog:linux:flowtables_1_a_netfilter_nftables_fastpath)
- Banana Pi hardware support nftable offload.
- pCPU Draft: Fallback SA violates IPsec architectural document: One SA is more special than the other SA. Remove this requirement fallback SA.
  - CH: reconsider lock less hand off a packet to another cpu. would solvefallback SA.
  - CH: Use one cpu as sorter to distribute packet.
  - TK: Steady state all necessary cpus should have the SA. Optimize for that.
- BEET mode draft: Mention fragmentation issue. One solution is reassemble the ipv4 packet before encapsulation ike negotiation should say ﬁnal narrowed TSi and TSr should one host ie. /32 and /128
- Mellanox driver put the packet data next to descriptor. when you fetch descriptor you get 64 bytes prefetched.
- ESPING: using tfs as aliveness check. TFS if it support constant bitrate, then in a "burst-mode" variation using a very slow constant bit rate tunnel with congestion control enabled gets you a liveness check but with on demand operation. Intial patchset would not support constant bit rate or burst mode.
- IPV4 can unconditionally set DF bit on the ESP4 packet. It is deﬁned in IPsec architectural documents.
- Pablo's patch set to [https://lwn.net/Articles/746776/](https://lwn.net/Articles/746776/)
