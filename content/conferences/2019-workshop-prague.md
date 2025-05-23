Title: 2019 Linux IPsec workshop, Prague (18 - 20 March)
Date: 2025-03-13
Summary: Summary TODO

- Recap IPsec workshop 2018
- xfrm policy database
- IPsec full-offload
- Offloading the policy database into hardware
- post-quantum crypto for IKEv2
- Foundation meeting of the ‘IPsec and Network Security e.V.’
- Bonus adhoc kernel debugging section
- IPsec tunnel-mode integration in Android
- ESP over TCP (rfc8229)
    - netlink attribute for tcp encap on xfrm states: XFRMA\_ENCAP (like for udp)
    - IKETCP prefix: sent by userspace before enabling TCP\_ULP (connect() side), received by userspace (accept() socket) before enabling TCP\_ULP
    - non-ESP marker: will be passed to/from userspace (contrary to what the slides say). consistent with UDP encap behavior. kernel adds/strips the 2-byte length prefix to each IKE message.
- open questions
    - delete states then close socket, or close socket then delete states?
    - TCP queue tuning and congestion control
- Integrating IPsec and XDP
- IPsec performance (Steffen Klassert)
- Flow cache replacement (Florian Westphal)
- Fastpath for IPsec gateways using the flowtable infrastructure
- libreswan items/wish list and experienes with XFRMi
- Configuring the OS native IPSec stack from Python. What could go wrong?
- Implicit IV / ESP Header Compression
- IPsec with listified GRO
