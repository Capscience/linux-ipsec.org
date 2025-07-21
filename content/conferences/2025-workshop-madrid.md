Title: 2025 Linux IPsec workshop, Madrid (17 - 18 July)
Date: 2025-07-19
Summary: The 2025 Linux IPsec workshop took place on July 17th - 18th in Madrid, directly before the IETF Meeting 123.

The 2025 Linux IPsec workshop was held on July 17th - 18th in the scorching hot Madrid,
directly before the IETF Meeting 123.
The workshop began on thrusday morning with the welcome speech by Steffen Klassert, who organized the event.

## Presentation Notes and Slides

### [Recent Developments in strongSwan]({static}/slides/2025/brunner-strongswan-developments.pdf) by Tobias Brunner
Tobias presented an overview of new features added in recent strongSwan releases.

These new features include:

- Narrowing with Trap Policies
- Per-CPU SAs
- AGGFRAG Mode
- Regular Expressions
- EAP-Identity Matching

### [IKEv2 Signature Authentication using ML-DSA]({static}/slides/2025/steffen-pqc-auth-for-ikev2.pdf) by Andreas Steffen
Andreas presented about the current state of the PQC authentication standards, such as ML-DSA, SLH-DSA and FN-DSA. The presentation included the preliminary strongSwan implementation of IKEv2 signature authentication using ML-DSA.

### [Group IPsec SAs and key management]({static}/slides/2025/smyslov-g-ikev2.pdf) by Valery Smyslov
Valery presented about securing IP multicast using G-IKEv2.
The G-IKEv2 document has been in development for more than 15 years, and is expeted to be published as an RFC soon.


### [Handling IPsec traffic with nftables based firewall]({static}/slides/2025/soini-handling-ipsec-with-foomuuri.pdf) by Tuomo Soini
Tuomo gave a presentation about a recent nftables firewall project, Foomuuri. The main focus of the presentation was on how IPsec secured traffic is handled in Foomuuri.


### [The Autonomic Control Plane (ACP)]({static}/slides/2025/richardson-whatsacp.pdf) and [Minerva Connect]({static}/slides/2025/richardson-minerva.pdf) by Michael Richardson
Michael held a presentation about the Autonomic Control Plane (ACP).
The presentation included some basics of ACP, as well as some insights to Michael's solution called Minerva.
The architecture and challenges in implementation were discussed.

### [Enterprise Networking with IPsec and Dynamic Routing]({static}/slides/2025/enterprise-networking-with-ipsec-and-dynamic-routing.pdf) by Simo Soini
Simo presented his plans for an experiment on enterprise networking using IPsec for securing the traffic and dynamic routing between tunnels.


### [IPsec Performance Tests]({static}/slides/2025/tschofenig-jansen-ipsec-performance-test.pdf) by Hannes Tschofenig and Kai Jansen
Hannes and Kai presented their IPsec performance tests with strongSwan and two 100GB Mellanox NICs.
The discussion about the results and IPsec performance optimization in general continued during the IETF 123 Hackathon.


### [Diet-ESP and Integration of IPsec]({static}/slides/2025/migault-diet-esp.pdf) by Daniel Migault
Daniel presented SeCPRI, an IPsec based security layer for eCPRI, which uses Diet-ESP for compressing the encrypted messages.

### [Enhanced ESP]({static}/slides/2025/klassert-enhanced-esp.pdf) by Steffen Klassert
Steffen presented an overview about the EESP protocol design.
There was a lot of discussion concerning the requirements and specifics of the protocol.

Related links:

- [Google's reason for telemetry use case for crypto offset](https://netdevconf.info/0x18/docs/netdev-0x18-paper43-talk-slides/Introduction%20to%20Falcon%20Reliable%20Transport.pdf)


### [IKEv2 negotiation for EESP]({static}/slides/2025/brunner-eesp-ikev2.pdf) by Tobias Brunner
The second presentation by Tobias was about IKEv2 negotiation for the EESP protocol Steffen Klassert presented about.
The focus was on the slight differences between negotiation for ESP versus EESP.


### [EESP Stateless Encryption Scheme]({static}/slides/2025/xialiang-eesp-stateless-encryption.pdf) by Frank Xialiang


### [Industry Trends in HPC/AI workloads and ESP-like Protocols]({static}/slides/2025/) by Anthony Anthony
Anthony gave an overview about industry trends in high-performance computing and AI.

Related links:

- [Netdev 0x18 Falcon Talk 2024](https://netdevconf.info/0x18/docs/netdev-0x18-paper43-talk-slides/Introduction%20to%20Falcon%20Reliable%20Transport.pdf)
- [UET Specification 1.0 June 2025](https://ultraethernet.org/wp-content/uploads/sites/20/2025/06/UE-Specification-6.11.25.pdf)
- [AI Networking Summary Netdev 0x19](https://netdevconf.info/0x19/sessions/bof/networking-for-ai-bof.html)



### [Stateless encryption]({static}/slides/2025/smyslov-stateless-encryption.pdf) by Valery Smyslov
Valery's second presentation was about stateless encryption,
and more specifically about how much security can be sacrificed in favor of performance.


### Linux XFRM Code Path by Anthony Anthony
Anthony gave a presentation about the XFRM code in Linux kernel.
The XFRM code is challenging to understand by just reading the code, and this started some fruitful conversation about how to better understand and learn this code.
Inspired by this discussion, Christian Hopps gave a short demonstration about semantic code navigation using Language Servers.
Language servers can communicate with code editors (such as Emacs, Neovim or VSCode) using language server protocol.

Related links:

- [Keynote from Usenix-25/OSDI 25](https://www.usenix.org/conference/atc25/presentation/berger)


### IPsec Interoperability discussion by Michael Richardson
Michael lead a discussion about interoperability between different IPsec implementations.
There was an idea about an event for testing interoperability between as many IPsec implementations as possible.


### [Handling fragments]({static}/slides/2025/kivinen-handling-fragments.pdf) by Tero Kivinen
Tero presented about handling incoming fragments in IPsec, and the three methods for this as listed in [RFC4301](https://datatracker.ietf.org/doc/html/rfc4301).


### Future of the IPsec Workshop and Closing by Steffen Klassert
The 2025 IPsec Workshop was finished with some final words about the workshop and it's future from the organizer, Steffen Klassert. Huge thanks to Steffen for organizing the workshop, and to everyone who attended!
