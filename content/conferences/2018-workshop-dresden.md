Title: 2018 Linux IPsec workshop, Dresden (26 - 28 March)
Date: 2018-03-28
Summary: The first Linux IPsec workshop was held 26 - 28 March 2018 in Dresden, Germany as a standalone event.

### From the Libreswan Team:

The Linux IPsec Summit has been very important to us in getting to know the kernel developers and to exchange information about features, use cases and limitations. We got a much better understanding of the kernel’s view of the IPsec/XFRM subsystem and already have some ideas on how we can improve the userland and use some of the XFRM features to the benefit of our users. We also got to share the typical use cases of our users to the kernel developers, so they have a better idea of how we use their code.

We had great conversations with the strongswan and android people about the IKE protocol and its oddities. We also learned a lot from the hardware people and how we can better use the support they are providing with XFRM.

Getting to know the faces behind the email addresses has given us more confidence to send contributions and work more closely with the XFRM and netfilter community.

Many thanks to Steffen Klassert and Secunet for arranging this IPsec
summit and thanks to all the developers that attended!

*Paul, Antony and Tuomo*

## Presentation Notes and Slides

### [Virtual xfrm interfaces]({static}/slides/2018/xfrm_interfaces-SK.pdf) by Steffen Klassert, secunet Security Networks AG

Steffen presented about a new approach for virtual xfrm (IPsec) interfaces to overcome the design limitations of the VTI interfaces. Some of the disadvantages of VTI interfaces are:

- VTI interfaces are L3 tunnels with configurable endpoints, but the tunnel endpoints are already determined by the IPsec SA. So it does not make much sense to configure tunnel endpoints at a VTI too.
- We need separate interfaces for IPv4 and IPv6 tunnels.
- We can have only one VTI with wildcard tunnel endpoints.
- VTI works just with tunnel mode SAs.

To overcome these issues a new design for XFRM interfaces  was
proposed:

- Should be a virtual interface that ensures IPsec transformation.
- No limitation on xfrm\_mode (tunnel, transport and beet).
- Should be possible to create multiple interfaces (e.g. to move to different namespaces).
- Interfaces should be configured with an interface ID that must match a (new) policy/SA lookup key.
- Should be possible to tunnel IPv4 and IPv6 through the same interface.
- Should be possible to use IPsec hardware offloads of the underlying interface.

RFC code that implements this was published before the workshop: [https://git.kernel.org/pub/scm/linux/kernel/git/klassert/linux-stk.git/log/?h=ipsec-next-xfrmi2](https://git.kernel.org/pub/scm/linux/kernel/git/klassert/linux-stk.git/log/?h=ipsec-next-xfrmi2) This code was reviewed and tested during the workshop. People agreed to continue this approach and to do more testing and review after the workshop.

### [Usecase and Requirements for new XFRMI a secunet’s point of view]({static}/slides/2018/xfrmi_usecase_lrk-CL.pdf) by Christian Langrock, secunet Security Networks AG

Christian presented about the secunet usecase of the xfrm interfaces. It turned out that the new approach meets all these usecase requirements.

### xfrm 32/64 bit compat layer by Florian Westphal

Florian Westphal presented and old effort to add a compatbility layer to XFRM to allow 32bit tasks to add and receive ipsec policies and states. This affects the x86_64 platform where 32bit binaries fail to talk to the ipsec subsystem because of alignment and differences in structure layouts. Although there was not much interest in this compat layer back in to 2010 this has changed a bit because Android is
now encountering this problem.

### IPsec offloading session by Don Skidmore / Boris Pismenny / Sowmini Varadhan / [Josh Hay]({static} ../slides/2018/MapUpdate-JH-DS.pdf) / Stephen Doyle

Boris Pismenny presented how to support encapsulations with [inline crypto offload]({static}/slides/2018/IPsec-full_offload-BP.pdf) (vlan, vxlan, bonding and teaming). Overall, the idea is to bind the virtual netdev to a physical netdev and expose the IPsec crypto offload capabilities and callbacks from the upper netdev.
The use of full [IPsec termination with Infiniband RoCEv2]({static}/slides/2018/RoCE-BP.pdf) by Boris Pismenny. Thoughts for [QUIC/XFRM offloads]({static}/slides/2018/quic_offload-DS-JH.pdf) were presented by Don Skidmore. Sowmini: QUIC group is already considering using a modified DTLS for this, please compare notes with them.


QUIC/DTLS [slides](https://datatracker.ietf.org/meeting/101/materials/slides-101-quic-stream-0-and-dtls-00)

Boris Pismenny presented various issues encountered by Mellanox with [handling IP fragments with offloads]({static}/slides/2018/Linux_Full_IPsec_Offload-BP.pdf). Steven Doyle concurred that Intel too has had to confront these challenges. Reference here is Appendix D of RFC 4301. The problem is “what to do when the IPsec offload engine encounters a fragment”. It can:

1. disable h/w offload and fall back to s/w offload (Paul: temporary blip in perf may be hard for admin to follow. This could be a DoS vector)
2. drop the fragment (Steffen: can we not just deal with this in slow path?)
3. just do s/w offload for the fragment (very hard to track seq# state etc between h/w and s/w ipsec)

Sowmini: [open questions about device/host terminated offloads]({static}/slides/2018/questions_virtualized_ipsec-SV.pdf)

- host terminated offloads: the scope of the SPI number space is within the VM, so two VMs may end up using the same SPI for tunnels. Offload needs to tracke both SPI and an unique identifier for the VM (VLAN and mac address)
- device terminated offload: where is IKE running, and what addresses is it using for the sockets to the IKE peer? If the device terminated offload is used in conjunction with some other underlay like VXLAN, then the cleanest design is to have IKE run on the hypervisor, and set up SA’s using the underlay IP address as the local address (in which case this becomes the same as IPsec offload on bare-metal, with the payload being an L2 or L3 packet from the VM). Other models (where there is no underlay) lead to several open questions and complexity, and are needless

### [ixgbe offload status, Intel FlowDirector]({static}/slides/2018/ixgbe_offload-Intel-FlowDirector-SN.pdf) by Shannon Nelson

- Intel’s Niantic 10Gbe has ipsec hardware offload that has been laying in wait since 2009, no fw updates needed in existing products
- recent patches to support ipsec offload pulled into v4.17
- performance is nearly line rate at around 9.1 Gbps in informal tests
- includes support for TSO and checksum offloads
- RSS and FlowDirector header parsing happens after inbound decryption
- Allows for multi-rx-queue processing, flow directing to proper CPUs
- some tunneled-ipsec reportedly available on Rx, not yet tested
- Niantic FlowDirector could be (ab)used for further ipsec-related flow handling
- odd things seen: occasional out-of-order TSO fragments; lower throughput on multi-threaded
   discussions mentioned both of these seen elsewhere
   multi-threaded issue may be related to xfrm table contention

### Status of IPsec in Android P. and discussions by Lorenzo Colitti / Nathan Harold

- [IPsec in Android]({static}/slides/2018/IPsec-in-android-LC-NH.pdf)
- [Networking In Your Pocket]({static}/slides/2018/Networking_In_Your_Pocket-LC-NH.pdf), How the Linux networking stack is made to work on Android devices

### [Populate From Packet]({static}/slides/2018/pfp-SV.pdf) by Sowmini Varadhan

Problem description: for clear traffic we get entropy/flow-hashing for ECMP and RSS from the TCP/UDP 4-tuple. With IPsec, you can get the same entropy using SPI in place of TCP/UDP port numbers but you would then need to ensure that each 4-tuple has a unique SPI. The challenge here is that TCP/UDP clients typically do an implicit bind
for the client port (let the kernel choose an ephemeral port) so you cannot set up the SA for the 4-tuple *before* connection comes up.

RFC 4301 proposes the “Populate From Packet” to solve this: when the data packet hits the xfrm layer and finds a SPD marked “XFRM_POLICY_PFP”, have the kernel send up an SADB_ACQUIRE to Pluto (or the IKE implementation) asking for an SA with the full 4-tuple. This upcall will be generated after we know the full 4-tuple (after connect() or sendmsg()). Needs a small patch in the kernel, and some changes to Pluto.

One concern here is that we may be hashing thousands of sockets across a few paths (e.g., 8, 16, or 32 paths) so we may want to put
an upper bound on the numbre of PFP SAs generated. Remedy: have a unable to cap the number of  PFP-SAs that pluto will generate e.g.,
allow admin to say “generate at most 64 4-tuple SAs for a PFP SPD, the 65th ACQUIRE will just generate a wildcard SA (*.<serverport>) as we do today

Sowmini/Paul to go and try this out over the next couple of weeks.

### GCM & FIPS 140-2 by Stephan Müller

### [Libreswan presentation and discussion session]({static}/slides/2018/IPsecSummit201803-PW.pdf) by Paul Wouters

[https://libreswan.org/wiki/Linux_IPsec_Summit_2018_wishlist](https://libreswan.org/wiki/Linux_IPsec_Summit_2018_wishlist)

### Post Quantum Crypto by Kai Martius

### [IPsec flowcache replacement]({static}/slides/2018/flowcache-SK.pdf), Netfilter flowtable by Steffen Klassert / Pablo Neira Ayuso

Steffen mentioned that we lost our fastpath policy/SA lookup with the removal of the flowcache. He asked about possible replacement solutions. One replacement proposal was to use a Radix Tree. Another one is the new Netfilter flowtable infrastructure, Pablo gave an introduction about that. The main question here would be how to configure this flowtable from within IPsec without the need of
an extra configuration step for the user.

### Netfilter forwarding fastpath by Pablo Neira Ayuso / Steffen Klassert

Pablo presented the idea on how to use the GRO layer to aggregate even UDP packets and how to bypass most of the stack in the forwarding case. This a followup on the ideas and PoC Steffen presented at the netfilter workshop last year. Pablo also explaind how this can be configured by using nftables.

After that, Steffen did a live demo of the forwarding fastpath to show that this basically doubles the UDP (and UDP tunneled in IPsec) throughput.

[RFC Code](https://git.kernel.org/pub/scm/linux/kernel/git/klassert/linux-stk.git/log/?h=net-next-nft-ffwd)

### [Strongswan Presentation and discussion session]({static}/slides/2018/strongSwan-AS-TB.pdf) by Andreas Steffen / Tobias Brunner

[https://wiki.strongswan.org/projects/strongswan/wiki/Linux_IPsec_Workshop_2018](https://wiki.strongswan.org/projects/strongswan/wiki/Linux_IPsec_Workshop_2018)

### crypto async callbacks by Hannes Frederic Sowa

With the current state of the kernel, the ipsec stack has no real control whether offloading takes place, a slow path can be used or the offloaded encryption/decryption is scheduled in an asynchronous callback. The reason for that is, that a lof of CPUs employ registers for doing such offloading, which are often not saved and restored by context switches (to keep the amount of data to be transferred for every context switch low). As a consequence, small system behavioral changes, like processes getting scheduled on the CPU where
the ipsec traffic is being received can drastically alter the performance outcome.

This talk presented the state of affair on different architectures (namely those, which have overloaded crypto routines in arch/*/crypto/) and showed their current use and problems. The x86 as well as the arm architecture have this behavior and such a discussion should be stimulated on how to avoid those patterns.

Possible ideas to remedy this are:

- dispatching ipsec crypto operations to cryptd or pdata
- employing threaded napi operations thus crypto engine runs in process context always
- possibility of manually saving and restoring the FPU state on stack with irq handling
- allowing xfrm api to configure behavior

Additionally the linux kernel doesn’t provide any kinds of counters or easily observable mechanism to detect this problem.

### State of IPsec testing and possible directions by Sabrina Dubroca

ipsec secpath matching with nftables by Florian Westphal

Florian presented work on making ipsec policy / secpath matching available in nftables. This discussion mainly revolved around useability, command line syntax and so on. Unlike iptables the nftables policy version will support lookups in sets. As result of this session the proof-of-concept work will see considerable changes to improve usability and reduce leakage of kernel implementation names and details into the nftables command line tools grammar.

### Improving GRO handling for ESP packets by Yossi Kuperman

### Exposing secpath to eBPF programs by Eyal Birger

Eyal Birger presented use cases for exposing the xfrm state to eBPF programs. One use case is to allow custom routing implementations where the selection criteria is based in part on incoming tunnel information. Another use case is custom statistics based on tunnel information.
QoS and IPsec by Thomas Egerer

Secunet customers are seeing the following problem:  if two packets going on an IPsec tunnel have different values of the DSCP in the IP ToS, the packets may get reordered (depending on the diffserv configuration). The receiving end of the IPsec tunnel then ends up dropping packets due to IPsec replay protection checks.

this is a potential problem for RoCE as well, where the DSCP is heavily used to give differential treatment to packet flows, and were packet loss is not tolerated well by RDMA

Possible solution is to remedy this using PFP- extend the selector to be “4-tuple + ToS” for triggering PFP based upcalls. Also needs changes everywhere (both in the IKE daemon and in the xfrm layer) to factor the ToS in each selector lookup of SPD/SADB

Some known limitations of this proposal: cannot handle the case where packet priority is carried in the 802.1q VLAN tag (in the 3 bit user-priority field). If the diffserv domain re-marks the IP DSCP, you can still have packet reordering and drops (the IP TOS is not protected by ipsec). If only one peer supports multiple DSCP values, how do we set up multiple IPsec SA’s per DSCP? (proposed solution – fall back to having one SA for all ToS values)

In spite of limitations, the idea of having SPI per ToS will help the common case at least, so will be explored as part of the PFP effort.

### [Future of PFKEY in the kernel, Configurable system default (allow/drop) if there is no matching policy, Crypto layer problems, Hardware GRO]({static}/slides/2018/misc_topics-SK.pdf) by Steffen Klassert

Future of PFKEY in the Kernel: The only feature that the \*swan implementations still use is the autoprobing of available
crypto algorithms because there is nothing like that in the netlink interface. So we agreed to implement an autoprobing in netlink and mark PFKEY as  deprecated.

Configurable system default (allow/drop) if there is no matching policy: We discussed the idea to have a configurable system default to allow or drop packets if there is no matching IPsec policy (current default is allow). This would be configurable per direction (input/output/forward). During discussion the idea came up to have a kernel commandline option to configure this.

Crypto layer problems: Steffen noted that performance optimizations of the network layer are often ‘eat up’ by the crypto layer, mostly because of (unnecessary) memcopy.

Hardware GRO: This was just a feature request to the hardware vendors because, for instance the forwarding fastpath could benefit from that.

### Future of the workshop and the Linux IPsec communtiy, Closing by Steffen Klassert

During this session, we discussed about the workshop itself, what was good/bad etc. The attendees agreed that the worshop was very fruitful and should be continued once a year. It was also discussed to found a Linux IPsec association to make the organization of the workshop easier and to get some communication infrastructure for the community.
