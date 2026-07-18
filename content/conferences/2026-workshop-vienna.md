Title: 2026 Linux IPsec workshop, Vienna (16 - 17 July)
Date: 2026-06-02
Summary: The 2026 Linux IPsec workshop was held in Vienna from 16th to 17th of July.

The 2026 Linux IPsec workshop was held in Vienna from July 16th to 17th.

![Group picture]({static}/images/workshop_2026_group_picture.jpg)

## Presentation Notes and Slides

### VPN Measurements on two host systems by Hannes Tschofenig and Paul Wouters

Hannes and Paul have been testing VPN performance with IPsec and Wireguard on a
two host system. The hosts were equipped with Connectix 7 NICs.

### [VPN Performance Experiment]({static}/slides/2026/soini-vpn-performance-experiment.pdf) by Simo Soini

For his Master's thesis Simo has been experimenting with traffic forwarded via
VPN using IPsec, IP-TFS, Wireguard and OpenVPN. Some of the interesting findings
included IP-TFS performance being noticeably behind that of regular IPsec,
Generic Receive Offload (GRO) having huge effect on IPsec performance, and
IPsec performance being inconsistent when using two-way traffic.

### [IPsec SA Migration in the Linux Kernel]({static}/slides/2026/brunner-sa-migration.pdf) by Tobias Brunner

Tobias presented the issues with XFRM_MSG_MIGRATE, namely not being able to
update the IP addresses of IPsec SAs. Starting from Linux kernel version 7.2
strongSwan is using the new XFRM_MSG_MIGRATE_STATE which allows more flexible
migrations.

### [XFRM Migrate Mark Blues]({static}/slides/2026/antony-xfrm-migrate-mark-blues.pdf) by Anthony Anthony

### [PMTU detection during SA establishment and during MOBIKE]({static}/slides/2026/harold-ipsec-mobility-challenges-with-linux.pdf) by Nathan Harold

Nathan presented some problems encountered when using IPsec in

### [The Libreswan Project Road Map]({static}/slides/2026/wouters-libreswan-roadmap-2026.pdf) by Tuomo Soini and Paul Wouters

Libreswan has had a slow year when it comes to the number of releases.
Paul and Tuomo went through the main contents of those releases, explaining
AI security audit findings, fixed CVEs and new features added.
They talked about features currently being worked on, including three Google
Summer of Code projects.

Paul also brought up his colleague's experimental project rewriting Libreswan
in Rust using AI. Though the project is not serious the initial test results
were better than expected, as PSK interoperability tests with Libreswan were
successful.

### Deprecations in the Linux IPsec subsystem by Sabrina Dubroca

Sabrina presented a list of items in the Linux IPsec subsystem to discuss their
deprecation. A few of the items were deemed ready to deprecate, while others
need more research about wether or not they are still being used.

### [ESP-Ping Implementation]({static}/slides/2026/antony-esp-ping-slides.pdf) by Anthony Anthony

Anthony talked about his Linux kernel prototype implementation of ESP ping.

### IKEv2 Group assisted Key Establishment by Steffen Klassert

Group assisted key establishment was discussed last year in Madrid. Steffen presented
his and Anthony's work on an [initial draft](https://github.com/antonyantony/ikev2-kms/blob/main/ikev2-kms.org).
There was a long discussion about the use case, security guarantees and overlap
with existing protocols. There was no clear consensus reached, and more discussion
is needed.

### [Using MLS with G-IKEv2]({static}/slides/2026/smyslov-g-ikev2-mls.pdf) by Valery Smyslov

Valery presented a [recent draft](https://datatracker.ietf.org/doc/draft-kohbrok-ipsecme-mls-gike/)
about extending G-IKEv2 with the [MLS](https://datatracker.ietf.org/doc/rfc9420/)
protocol.

### [Multi-Authentication in IKEv2 with Post-quantum Security]({static}/slides/2026/wang-short-version-of-multi-authentication-in-ikev2.pdf) by Guilin Wang

### [Optimized rekeys and the next]({static}/slides/2026/pan-optimized-rekey.pdf) by William Panwei

William brought the status of [draft-ietf-ipsecme-ikev2-sa-ts-payloads-opt](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-ikev2-sa-ts-payloads-opt/)
up for discussion. It was deemed that along with [draft-ietf-ipsecme-child-pfs-info](https://datatracker.ietf.org/doc/draft-ietf-ipsecme-child-pfs-info/)
it is ready and should be pushed forward in the ipsecme working group session.

### [KEM-based authentication in IKEv2]({static}/slides/2026/smyslov-ikev2-authkem.pdf) by Valery Smyslov

### Documentation/networking/xfrm*.rst by Michael Richardson

Improving the documentation of XFRM has been long overdue. Michael offers a proposal
about how to reorganize the file structure and go through existing documentation
which has parts that are outdated and / or unhelpful.

### The netlink UAPI by Sabrina Dubroca

### [Netfilter's flowtable updates]({static}/slides/2026/ayuso-flowtable-updates.pdf) by Pablo Neira Ayuso

### [Using IKEv2 for Key Management for PSP]({static}/slides/2026/smyslov-ikev2-psp.pdf) by Valery Smyslov

As it is IKEv2 can't be used to handle key management for the PSP Security
Protocol by Google. Valery explained how IKEv2 could be expanded to work
with PSP, described in [draft-smyslov-ipsecme-ikev2-psp](https://datatracker.ietf.org/doc/draft-smyslov-ipsecme-ikev2-psp/).

### Diet-ESP for 5G Fronthaul by Daniel Migault

Daniel presented his work on running diet-ESP for 5G fronthaul, which has very strict
latency requirements and a need for a specific number of packets per second.

### Future Work with IP-TFS by Christian Hopps
