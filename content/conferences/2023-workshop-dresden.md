Title: 2023 Linux IPsec/Netfilter workshop, Dresden (11 - 14 September)
Date: 2025-05-26
Summary: The 2023 Linux IPsec/Netfilter workshop took place 11 - 14 September in Dresden, Germany.

## Topics
### IPsec
#### Nathan
1. Intro and update on IPsec in Android
2. Challenges and learnings from IPsec commercialization on mobile (power, performance, and reliability)
    - Replay window issues esp on cellular
    - Keepalive power issues and possible alternatives?
3. Improving IPv6 ESP support (problem statements, current thoughts, brainstorming w/ the group on each)
    - ESP ping
    - ESP keepalive
    - Flow labels performance & privacy
    - DSCP performance & privacy
4. Compatibility and signalling
    - Dynamic update problems
    - MOBIKE and UDP encap (incl dynamic encap+non-encap receive)

#### Tobias
- Future of strongSwan's high-availability solution and patches for CLUSTERIP now that the latter has been removed from the kernel.
- XFRM_MSG_MIGRATE vs. strongSwan, change the current command or add new one(s)

#### Andreas
X.509 Certificate \[Re-\]Enrollment using stronsSwan pki tool

#### Steffen
- ESP protocol problems
- NFT/xfrm packet bulking infrastructure (with Pablo)
- Socket policies
- What is needed for an IPsec reference implementation

#### Pablo
NFT ﬂowtable forwarding fast path (with Steffen)

#### Thomas
Implications of NAT-mapping changes with high line speeds. 

#### Chris
- Quick intro to IP-TFS if needed    
- IPTFS Linux xfrm implementation.

#### Anthony
ESPPing Encrypted: how send and receive on Linux (I guess tangential to Nathan's ESPPing and ESPKeep alive).

Depreciating Unsed xfrm/ipsec features PF_KEY, UDP_ENCAP_ESPINUDP_NON_IKE
- Are there more you would like to Depreciate?

Merging pCPU to Kernel : minimal recap on Technology. More on can merge this sooner than later in Linux Kerenel. To catch up on tech details 
- [https://datatracker.ietf.org/doc/draft-ietf-ipsecme-multi-sa-performance/](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-multi-sa-performance/)
- [https://datatracker.ietf.org/meeting/110/materials/slides-110-ipsecme-ikev2-multi-sa-performance-00](https://datatracker.ietf.org/meeting/110/materials/slides-110-ipsecme-ikev2-multi-sa-performance-00)

BEET Mode (Trying bring new Internet Draft to IETF 118, Nov 2023) : details from initial version of the document)

### Netfilter
#### Daniel
bpf+netfilter integration

#### Quentin
bpfilter, and its relation to iptables and nftables.

#### Florian
- Bridge Netfilter+conntrack: Conntrack race conditions with L2 (conntrack-on-bridge)
- Various problems with netfilter and nftables in kernel and userspace
- nftables fuzzer integration
- Introspection aka 'who is dropping my packets':
- Flowtable offload helpers for XDP
- chainloop detection


#### Pablo
- flowtable
- C test API
- a trip to bugzilla
- pending backlog in patchwork
- register tracking infrastructure: zeroing and coalescing
- Flowtable HW offload + transaction semantics
- (lib)nftables api
- 'type ipv4_addr : ct helper' issue
- userspace nft cache for tracing
