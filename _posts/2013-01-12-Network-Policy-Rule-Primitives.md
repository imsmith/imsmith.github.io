---
layout: post
title: 'Network policy rule primatives'
date: '2013-01-12 00:00Z'
categories: post
---
# Network Policy Rule Primitives

## Matches

|    | name                            | matching criteria                        | example / description                                                              |
|----|---------------------------------|------------------------------------------|------------------------------------------------------------------------------------|
| 1  | patching                        | ingress interface                        | send everything from interface 1/3 to interface 1/6                                |
| 2  | bridging                        | ingress interface                        |                                                                                    |
| 3  | switching                       | source mac                               |                                                                                    |
| 4  | switching                       | destination mac                          |                                                                                    |
| 5  | switching                       | source vlan                              |                                                                                    |
| 6  | switching                       | destination vlan                         |                                                                                    |
| 7  | routing                         | source ip                                |                                                                                    |
| 8  | routing                         | destination ip                           |                                                                                    |
| 9  | policy based route (rfc 1104.5) | EtherType                                |                                                                                    |
| 10 | policy based route (rfc 1104.5) | vlan priority                            |                                                                                    |
| 11 | policy based route (rfc 1104.5) | IP protocol                              | ipv4 "protocol"; ipv6 "next header"                                                |
| 12 | policy based route (rfc 1104.5) | IPv4 DSCP                                |                                                                                    |
| 13 | policy based route (rfc 1104.5) | IPv6 Traffic Class                       |                                                                                    |
| 14 | policy based route (rfc 1104.5) | ARP code                                 |                                                                                    |
| 15 | policy based route (rfc 1104.5) | ICMP type                                |                                                                                    |
| 16 | policy based route (rfc 1104.5) | ICMP code                                |                                                                                    |
| 17 | policy based route (rfc 1104.5) | network 2-tuple                          | src.ip, dst.ip                                                                     |
| 18 | policy based route (rfc 1104.5) | network 3-tuple                          | ip.proto, src.ip, dst.ip                                                           |
| 19 | policy based route (rfc 1104.5) | network 4-tuple                          | src.ip, src.port, dst.ip, dst.port                                                 |
| 20 | policy based route (rfc 1104.5) | network 5-tuple                          | ip.proto, src.ip, src.port, dst.ip, dst.port                                       |
| 21 | policy based route (rfc 1104.5) | network 6-tuple                          | vlan.id, src.ip, src.port, dst.ip, dst.port, vlan.id                               |
| 22 | policy based route (rfc 1104.5) | network 7-tuple                          | ip.proto, vlan.id, src.ip, src.port, dst.ip, dst.port, vlan.id                     |
| 23 | policy based route (rfc 1104.5) | network 8-tuple                          | vlan.id, src.ip, src.port, xlat.ip, xlat.port, dst.ip, dst.port, vlan.id           |
| 24 | policy based route (rfc 1104.5) | network 9-tuple                          | ip.proto, vlan.id, src.ip, src.port, xlat.ip, xlat.port, dst.ip, dst.port, vlan.id |
| 25 | flow forwarding (rfc 1104.6)    | network flow id                          | network protocol header, eg. IPv6 Flow_Label                                       |
| 26 | flow forwarding (rfc 1104.6)    | transport flow id                        | transport protocol header, eg., TCP option, SCTP stream ID                         |
| 27 | flow forwarding (rfc 1104.6)    | sdn flow id                              | eg., network 7-tuple hash, avp \> "flow_id", "[uuid]"                              |
| 28 | flow forwarding (rfc 1104.6)    | subscriber                               | avp \> "flow id", "subscriber id"                                                  |
| 29 | flow forwarding (rfc 1104.6)    | application (L7) protocol                | transport protocol header, eg TCP option, SCTP payload protocol ID                 |
| 30 | flow forwarding (rfc 1104.6)    | named application                        | avp \> "flow id", "application id"                                                 |
| 31 | flow forwarding (rfc 1104.6)    | network session                          | network protocol header, eg., IPv6 option, avp \> "session id", "flow id"          |
| 32 | flow forwarding (rfc 1104.6)    | transport session                        | transport protocol header, eg., TLS sessionID, VxLAN VNI, SCTP hostname address    |
| 33 | flow forwarding (rfc 1104.6)    | application session                      | application protocol header, eg. HTTP cookie, GTP tunnel ID                        |
| 34 | flow forwarding (rfc 1104.6)    | top of stack label                       | mpls label                                                                         |
| 35 | flow forwarding (rfc 1104.6)    | ordered sequence of labels               | mpls label sequence                                                                |
| 36 | meta-switching                  | irule binary Y/N result                  |                                                                                    |
| 37 | meta-switching                  | bad reputation                           |                                                                                    |
| 38 | meta-switching                  | reputation grade                         | eg., "red"                                                                         |
| 39 | meta-switching                  | risk score                               | eg., IPViking IPQ                                                                  |
| 40 | meta-switching                  | path score                               | eg., OPSF shortest path tree                                                       |
| 41 | meta-switching                  | path state                               | eg., BGP descision engine                                                          |
| 42 | meta-switching                  | certificate info                         | eg., x.509 Subject Alternative Names                                               |
| 43 | meta-switching                  | node / instance availability metric      | eg., LTM dynamic ratio LB                                                          |
| 44 | meta-switching                  | sevice / application availability metric | eg., GTM dynamic ratio LB                                                          |
| 45 | meta-switching                  | environmental suitability metric         | eg., cost-based LB; datacenter temp LB                                             |
| 46 | meta-switching                  | time/date/day                            |                                                                                    |
| 47 | meta-switching                  | geoip/geoloc                             |                                                                                    |
| 48 | meta-switching                  | device-type/OS/useragent                 |                                                                                    |
| 49 | meta-switching                  | under-attack                             |                                                                                    |
| 50 | meta-switching                  | part of an attack                        |                                                                                    |

## Actions

|    | name      | instruction   | example /description                                                                                                                                   |
|----|-----------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1  | filtering | accept        | permit to forwarding engine                                                                                                                            |
| 2  | filtering | reject        | deny with reject response                                                                                                                              |
| 3  | filtering | redirect      | deny with redirect response                                                                                                                            |
| 4  | filtering | drop          | deny with no response                                                                                                                                  |
| 5  | mapping   | translate     | translate address (DNAT, SNAT, NAT, etc)                                                                                                               |
| 6  | mapping   | transform     | translate between protocol (NAT64, UDP-TCP, TCP-SCTP, etc.)                                                                                            |
| 7  | mapping   | classify      | apply a classification taxonomy to the packet or flow                                                                                                  |
| 8  | mapping   | cluster       | group the packet or flow together with other like objects                                                                                              |
| 9  | mapping   | persist       | create a decision-free next hop entry for subsequent frames or flows                                                                                   |
| 10 | push-pop  | stack         | strip or add label to stack of labels                                                                                                                  |
| 11 | push-pop  | encapsulate   | decapsulate interior traffic; [egress] encapsulate interior traffic                                                                                    |
| 12 | push-pop  | replace       | rewrite existing information with new information                                                                                                      |
| 13 | queue     | insert        | insert the frame or flow into a queue                                                                                                                  |
| 14 | queue     | remove        | remove the frame or flow from a queue                                                                                                                  |
| 15 | queue     | prioritize    | assign a priority to the frame or flow for negotiating the queue                                                                                       |
| 16 | copy      | mirror        | 1:1 copy of entire frame or flow                                                                                                                       |
| 17 | copy      | log           | record occurance of frame or flow with description                                                                                                     |
| 18 | copy      | amplify       | 1:many copy of payload (with translation as required)                                                                                                  |
| 19 | count     | frame         | increment counts with frame as the base                                                                                                                |
| 20 | count     | flow          | increment counts with flow as the base                                                                                                                 |
| 21 | count     | rate          | record telemetrics on the rate of data and the quality of the transmission                                                                             |
| 22 | proxy     | half          | terminate the frame or flow to the local socket, no corresponding egress                                                                               |
| 23 | proxy     | full          | terminate the frame of flow to the local socket with corresponding egres                                                                               |
| 24 | proxy     | tri           | terminate the frame or flow to the local socket, egress-and-return to Man-in-the-Middle socket, then egress in correspondence to the original ingress  |
| 25 | proxy     | chain         | as tri, but with n Man-in-the-Middle operations                                                                                                        |
| 26 | forward   | nexthop       | perform a next hop selection and direct frame or flow to selected destination                                                                          |
| 27 | forward   | shape         | apply shaping policy to the flow                                                                                                                       |
| 28 | forward   | slipstream    | insert frame or packet or message into existing flow                                                                                                   |
| 29 | forward   | transparent   | perform a next hop selection and direct frame or flow to selected destination without modifying headers                                                |
| 30 | meta      | script        | execute a data plane irule                                                                                                                             |
| 31 | meta      | consult       | perform a blocking out of band query                                                                                                                   |
| 32 | meta      | lookup        | perform a non-blocking out of band query                                                                                                               |
| 33 | meta      | config-change | execute a control plane irule                                                                                                                          |
| 34 | meta      | replay        | replay a request frame, packet, or message through the decision engine                                                                                 |
