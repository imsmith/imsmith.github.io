---
layout: post
title: 'IP Protocol Disaggregation'
date: '2017-03-30 00:00Z'
categories: post
---
# IP Protocol Disaggregation
The Internet Protocol is firmly in Layer 3 of the OSI Networking Model, and sometimes Layer 3 is all you need.  The IPv4 Protocol field and the IPV6 Next-Header field tell us what the Layer 4 protocol is going to be.  Most of these aren't something that you need to send to an up-the-stack service because you can just put them in a list and handle them very early in packet processing.  IANA maintains the allocation of the 255 possible values in these headers.

The protocols in the IANA list have some opportunities to optimize how we handle them. The list is broken out into several categories in the table below.

| Status                     | Recommendation            | List         |
|----------------------------|---------------------------|--------------|
| Not maintained by IETF     | Do not forward    | Group A   |
| Historic and Experimental  | Do not forward     | Group B   |
| Standards and Reservations | Forward            | Group C   |
| IPv4-only protocols        | Enforce AF requirement    | Group C.1 |
| IPv6-only protocols        | Enforce AF requirement    | Group C.2 |
| IPv6 Headers               | Treat as secondary header | Group C.3 |
| Specific enhancements      | Increase DAG information  | Group C.4 |

## We can make broad generalizations about many of these protocols.

-   Our first cut may be to simply say that some protocols aren’t going to be handled by an up-the-stack service. This can mean that we forward them without prejudice, that we send them down a path where there are acls but not a listener or flow entry, or it may mean we simply drop them.
-   Our second cut may be to say that in that set of protocols we do send to an up-the-stack service, for some protocols, they will never present themselves in a particular AF – protocols that are IPv4-only or IPv6-only – and drop the offending request without sending it to the up-the-stack service.
-   Our third cut may be to say that in that set of protocols we do send to our up-the-stack service, we can ignore protocols that should only exist as IPv6 next header field values, as they are Extension Headers. If we see them on an IPv6 datagram and there isn’t an Extension Header, we may consider them malformed.
-   Finally, in that set of protocols we do send to an up-the-stack service, there is a subset of protocols for which we may want to be more explicit in our DAG hash inputs because they do offer opportunities to be more entropic than just ip.src-ip.dst gives us.

### Group A. Protocols not maintained by IETF

*Do you ever need to see these protocols above Layer 3?*

| IP Protocol Number | Hex  | Protocol Name | Protocol Long Name                                                                         |
|--------------------|------|---------------|--------------------------------------------------------------------------------------------|
| 7                  | 0x07 | CBT           | Core Based Trees                                                                           |
| 10                 | 0x0A | BBN-RCC-MON   |                                                                                            |
| 12                 | 0x0C | PUP           | PARC Universal Packet                                                                      |
| 13                 | 0x0D | ARGUS         |                                                                                            |
| 14                 | 0x0E | EMCON         | Emissions Control                                                                          |
| 15                 | 0x0F | XNET          | Cross Net Debugger                                                                         |
| 16                 | 0x10 | CHAOS         | Chaosnet Protocol                                                                          |
| 18                 | 0x12 | MUX           | Multiplexing Protocol                                                                      |
| 19                 | 0x13 | DCN-MEAS      | DCN Measurement Subsystem                                                                  |
| 21                 | 0x15 | PRM           | Packet Radio Measurement                                                                   |
| 22                 | 0x16 | XNS-IDP       | XEROX Network Systems Protocol Family Internet Datagram Protocol                           |
| 23                 | 0x17 | TRUNK-1       |                                                                                            |
| 24                 | 0x18 | TRUNK-2       |                                                                                            |
| 25                 | 0x19 | LEAF-1        |                                                                                            |
| 26                 | 0x1A | LEAF-2        |                                                                                            |
| 31                 | 0x1F | MFE-NSP       | Magnetic Fusion Energy Network Services Protocol                                           |
| 32                 | 0x20 | MERIT-INP     | Michigan Educational Research Information Triad Computing Network Internetworking Protocol |
| 34                 | 0x22 | 3PC           | Third Party Connect Protocol                                                               |
| 35                 | 0x23 | IDPR          | Inter-Domain Policy Routing Protocol                                                       |
| 36                 | 0x24 | XTP           | Express Transport Protocol                                                                 |
| 37                 | 0x25 | DDP           | Datagram Delivery Protocol                                                                 |
| 38                 | 0x26 | IDPR-CMTP     | Inter-Domain Policy Routing Protocol Control Message Transport Protocol                    |
| 39                 | 0x27 | TP++          | Transmission Protocol ++                                                                   |
| 40                 | 0x28 | IL            | Inter Link Protocol                                                                        |
| 42                 | 0x2A | SDRP          | Source Demand Routing Protocol                                                             |
| 45                 | 0x2D | IDRP          | Inter-Domain Routing Protocol                                                              |
| 49                 | 0x31 | BNA           |                                                                                            |
| 52                 | 0x34 | I-NLSP        | Integrated Network Layer Security for TCP and UDP with Bigger Addresses                    |
| 53                 | 0x35 | SWIPE         | IP with Encryption                                                                         |
| 55                 | 0x37 | MOBILE        | IP Mobility                                                                                |
| 56                 | 0x38 | TLSP          | Transport Layer Security using Kryptonet key Management                                    |
| 57                 | 0x39 | SKIP          | Simple Key Management Protocol                                                             |
| 62                 | 0x3E | CFTP          | Covert File Transfer Protocol                                                              |
| 64                 | 0x40 | SAT-EXPAK     | SATNET and Backroom experiment control program                                             |
| 65                 | 0x41 | KRYPTOLAN     |                                                                                            |
| 66                 | 0x42 | RVD           | MIT Remote Virtual Disk Protocol                                                           |
| 67                 | 0x43 | IPPC          | Internet Pluribus Packet Core                                                              |
| 69                 | 0x45 | SAT-MON       | SATNET monitoring                                                                          |
| 70                 | 0x46 | VISA          | VISA protocol                                                                              |
| 71                 | 0x47 | IPCU          | Internt Packet Core Utility                                                                |
| 72                 | 0x48 | CPNX          | Computer Protocol Network Executive                                                        |
| 73                 | 0x49 | CPHB          | Computer Protocol Heart Beat                                                               |
| 74                 | 0x4A | WSN           | Wang Span Network                                                                          |
| 75                 | 0x4B | PVP           | Packet Video Protocol                                                                      |
| 76                 | 0x4C | BR-SAT-MON    | Backroom SATNET monitor                                                                    |
| 77                 | 0x4D | SUN-ND        | Sun Neighbor Discovery                                                                     |
| 78                 | 0x4E | WB-MON        | Wideband Monitor                                                                           |
| 79                 | 0x4F | WB-EXPAK      | Wideband experiment control program                                                        |
| 80                 | 0x50 | ISO-IP        | ISO Internet Protocol                                                                      |
| 83                 | 0x53 | VINES         | Banyan VINES                                                                               |
| 84                 | 0x54 | TTP           | Transaction Transport Protocol                                                             |
| 84                 | 0x54 | IPTM          | Internet Protocol Traffic Manager                                                          |
| 86                 | 0x56 | DGP           | Dissimilar Gateway Protocol                                                                |
| 87                 | 0x57 | TCF           |                                                                                            |
| 90                 | 0x5A | Sprite-RPC    | Sprite Remote Procedure Call                                                               |
| 91                 | 0x5B | LARP          | Locus Address Resolution Protocol                                                          |
| 92                 | 0x5C | MTP           | Multicast Transport Protocol                                                               |
| 93                 | 0x5D | AX.25         | AX.25 Frames                                                                               |
| 95                 | 0x5F | MICP          | Mobile Internetworking Control Protocol                                                    |
| 96                 | 0x60 | SCC-SP        | Semaphore Communications Security Protocol                                                 |
| 100                | 0x64 | GMTP          |                                                                                            |
| 101                | 0x65 | IFMP          | Ipsilon Flow Management Protocol                                                           |
| 102                | 0x66 | PNNI          | Private Network-to-Network Interface over IP                                               |
| 104                | 0x68 | ARIS          | Aggregate Route-Based IP Switching                                                         |
| 105                | 0x69 | SCPS          | Space Communications Protocol                                                              |
| 106                | 0x6A | QNX           | QNX Native Networking                                                                      |
| 107                | 0x6B | A/N           | Active IP                                                                                  |
| 109                | 0x6D | SNP           | Sitara Networks Protocol                                                                   |
| 110                | 0x6E | Compaq-Peer   | Compaq Peer Protocol                                                                       |
| 111                | 0x6F | IPX-in-IP     | Internetwork Packet Exchange in IP                                                         |
| 116                | 0x74 | DDX           | D-II Data Exchange                                                                         |
| 117                | 0x75 | IATP          | Interactive Agent Transfer Protocol                                                        |
| 118                | 0x76 | STP           | Schedule Transfer Protocol                                                                 |
| 119                | 0x77 | SRP           | SpectraLink Radio Protocol                                                                 |
| 120                | 0x78 | UTI           | Universal Transport Interface                                                              |
| 121                | 0x79 | SMP           | Simple Message Protocol                                                                    |
| 122                | 0x7A | SM            | Simple Message Protocol                                                                    |
| 123                | 0x7B | PTP           | Performance Transparency Protocol                                                          |
| 125                | 0x7D | FIRE          | Flexible Intra-AS Routing Environment                                                      |
| 126                | 0x7E | CRTP          | Combat Radio Transport Protocol                                                            |
| 127                | 0x7F | CRUDP         | Combat Radio User Datagram                                                                 |
| 128                | 0x80 | SSCOPMCE      |                                                                                            |
| 129                | 0x81 | IPLT          |                                                                                            |
| 130                | 0x82 | SPS           | Secure Packet Shield                                                                       |
| 131                | 0x83 | PIPE          | Private IP Encapsulation within IP                                                         |

## Group B. Historic and Experimental Protocols

*Do you ever need to see these protocols above Layer 3 if you aren't actually using them?*

| IP Protocol Number | Hex  | Protocol Name   | Protocol Long Name                                                |
|--------------------|------|-----------------|-------------------------------------------------------------------|
| 3                  | 0x03 | GGP             | Gateway to Gateway Protocol                                       |
| 5                  | 0x05 | ST              | Stream Protocol                                                   |
| 8                  | 0x08 | EGP             | Exterior Gateway Protocol                                         |
| 11                 | 0x0B | NVP-II          | Network Voice Protocol                                            |
| 20                 | 0x14 | HMP             | Host Monitoring Protocol                                          |
| 27                 | 0x1B | RDP             | Reliable Data Protocol                                            |
| 28                 | 0x1C | IRTP            | Internet Reliable Transaction Protocol                            |
| 29                 | 0x1D | ISO-TP4         | ISO Transport Protocol Class 4                                    |
| 30                 | 0x1E | NETBLT          | Bulk Data Transfer Protocol                                       |
| 48                 | 0x30 | DSR             | Dynamic Source Routing Protocol                                   |
| 54                 | 0x36 | NARP            | Non-broadcast Multiple Access network address resolution protocol |
| 81                 | 0x51 | VMTP            | Versatile Message Transaction Protocol                            |
| 82                 | 0x52 | SECURE-VMTP     | Secure Versatile Message Transaction Protocol                     |
| 98                 | 0x62 | ENCAP           | Encapsulation Header                                              |
| 113                | 0x71 | PGM             | Pragmatic General Multicast Reliable Transport Protocol           |
| 124                | 0x7C | IS-IS over IPv4 |                                                                   |

## Group C. Protocols standardized by IETF and IANA reservations

*You should want to see these protocols if you are doing things at Layer 4 or Layer 7*

| IP Protocol Number | Hex       | Protocol Name                 | Protocol Long Name                                                   |
|--------------------|-----------|-------------------------------|----------------------------------------------------------------------|
| 0                  | 0x00      | HOPOPT                        | IPv6 Hop-By-Hop Option                                               |
| 1                  | 0x01      | ICMP                          | Internet Control Message                                             |
| 2                  | 0x02      | IGMP                          | Internet Group Management                                            |
| 4                  | 0x04      | IP-in-IP                      | IPv4 encapsulation                                                   |
| 6                  | 0x06      | TCP                           | Transmission Control Protocol                                        |
| 9                  | 0x09      | IGP                           | any private interior gateway                                         |
| 17                 | 0x11      | UDP                           | User Datagram Protocol                                               |
| 33                 | 0x21      | DCCP                          | Datagram Congestion Control Protocol                                 |
| 41                 | 0x29      | IPv6                          | IPv6 encapsulation                                                   |
| 43                 | 0x2B      | IPv6-Route                    | IPv6 Routing Header                                                  |
| 44                 | 0x2C      | IPv6-Frag                     | Fragment Header for IPv6                                             |
| 46                 | 0x2E      | RSVP                          | Reservation Protocol                                                 |
| 47                 | 0x2F      | GRE                           | Generic Routing Encapsulation                                        |
| 50                 | 0x32      | ESP                           | Encapsulating Security Protocol                                      |
| 51                 | 0x33      | AH                            | Authentication Header                                                |
| 58                 | 0x3A      | IPv6-ICMP                     | Internet Control Message                                             |
| 59                 | 0x3B      | IPv6-NoNxt                    | No Next Header                                                       |
| 60                 | 0x3C      | IPv6-Opts                     | Destination Options                                                  |
| 61                 | 0x3D      | Any Host Internal Protocol    |                                                                      |
| 63                 | 0x3F      | Any Local Network             |                                                                      |
| 68                 | 0x44      | Any Distributed File system   |                                                                      |
| 89                 | 0x59      | OSPF                          | Open Shortest Path First                                             |
| 94                 | 0x5E      | IPIP                          |                                                                      |
| 99                 | 0x63      | Any private encryption scheme |                                                                      |
| 103                | 0x67      | PIM                           | Protocol Independent Multicast                                       |
| 108                | 0x6C      | IPComp                        | IP Payload Compression Protocol                                      |
| 112                | 0x70      | VRRP                          | Virtual Router Redundancy Protocol                                   |
| 114                | 0x72      | Any 0-hop protocol            |                                                                      |
| 115                | 0x73      | L2TP                          | Layer-2 Tunneling Protocol                                           |
| 132                | 0x84      | SCTP                          | Stream Control Transmission Protocol                                 |
| 133                | 0x85      | FC                            | Fibre Channel                                                        |
| 134                | 0x86      | RSVP-E2E-IGNORE               |                                                                      |
| 135                | 0x87      | Mobility Header               | Mobility Header                                                      |
| 136                | 0x88      | UDPLite                       | Lightweight User Datagram Protocol                                   |
| 137                | 0x89      | MPLS-in-IP                    | Multiprotocol Label Switching in IP or Generic Routing Encapsulation |
| 138                | 0x8A      | manet                         | MANET protocols                                                      |
| 139                | 0x8B      | HIP                           | Host Identity Protocol                                               |
| 140                | 0x8C      | Shim6                         | Level 3 Multihoming Shim Protocol for IPv6                           |
| 141                | 0x8D      | WESP                          | Wrapped Encapsulating Security Protocol                              |
| 142                | 0x8E      | ROHC                          | Robust Header Compression                                            |
| 255                | 0xFF      | Reserved                      |                                                                      |
| 143 \~ 252         | 0x8F-0xFC | Unassigned                    |                                                                      |
| 253 \~ 254         | 0xFD-0xFE | Experimentation               |                                                                      |

### Group C.1 Protocols that are explicitly IPv4-only

*If they are found in an IPv6 header, it is malformed.*

| IP Protocol Number | Hex  | Protocol Name | Protocol Long Name       |
|--------------------|------|---------------|--------------------------|
| 1                  | 0x01 | ICMP          | Internet Control Message |
| 4                  | 0x04 | IP-in-IP      | IPv4 encapsulation       |
| 94                 | 0x5E | IPIP          |                          |

### Group C.2 Protocols that are explicitly IPv6-only

*If they are found in an IPv4 header, it are malformed.*

| IP Protocol Number | Hex  | Protocol Name   | Protocol Long Name                         |
|--------------------|------|-----------------|--------------------------------------------|
| 0                  | 0x00 | HOPOPT          | IPv6 Hop-By-Hop Option                     |
| 41                 | 0x29 | IPv6            | IPv6 encapsulation                         |
| 43                 | 0x2B | IPv6-Route      | IPv6 Routing Header                        |
| 44                 | 0x2C | IPv6-Frag       | Fragment Header for IPv6                   |
| 58                 | 0x3A | IPv6-ICMP       | Internet Control Message                   |
| 59                 | 0x3B | IPv6-NoNxt      | No Next Header                             |
| 60                 | 0x3C | IPv6-Opts       | Destination Options                        |
| 135                | 0x87 | Mobility Header | Mobility Header                            |
| 140                | 0x8C | Shim6           | Level 3 Multihoming Shim Protocol for IPv6 |

### Group C.3 Protocols that are Extension Headers in IPv6

*These are not L4 protocols, they are additional information in the L3 header that could be used for identifying a flow.*

| IP Protocol Number | Hex  | Protocol Name   | Protocol Long Name                         |
|--------------------|------|-----------------|--------------------------------------------|
| 0                  | 0x00 | HOPOPT          | IPv6 Hop-By-Hop Option                     |
| 41                 | 0x29 | IPv6            | IPv6 Encapsulation                         |
| 43                 | 0x2B | IPv6-Route      | IPv6 Routing Header                        |
| 44                 | 0x2C | IPv6-Frag       | Fragment Header for IPv6                   |
| 50                 | 0x32 | ESP             | Encapsulating Security Protocol            |
| 51                 | 0x33 | AH              | Authentication Header                      |
| 60                 | 0x3C | IPv6-Opts       | Destination Options                        |
| 134                | 0x86 | RSVP-E2E-IGNORE |                                            |
| 135                | 0x87 | Mobility Header | Mobility Header                            |
| 139                | 0x8B | HIP             | Host Identity Protocol                     |
| 140                | 0x8C | Shim6           | Level 3 Multihoming Shim Protocol for IPv6 |

### Group C.4 Specific Enhancements

*These protocols have fixed information fields that can help identify a flow at Layer 4.*

| IP Protocol Number | Protocol Name   | Protocol Long Name                                                   | Fixed location field(s) we could use but are not                                                                                            |
|--------------------|-----------------|----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| 1                  | ICMP            | Internet Control Message                                             | per-type decision                                                                                                                           |
| 2                  | IGMP            | Internet Group Management                                            | group address(32-63)                                                                                                                        |
| 3                  | GGP             | Gateway to Gateway Protocol                                          | sequence number(16-31)                                                                                                                      |
| 4                  | IP-in-IP        | IPv4 encapsulation                                                   | encapsulated src.ip();  dst.ip()                                                                                                            |
| 6                  | TCP             | Transmission Control Protocol                                        | src.port(0-15);  dst.port(16-32)                                                                                                            |
| 17                 | UDP             | User Datagram Protocol                                               | src.port(0-15);  dst.port(16-31)                                                                                                            |
| 27                 | RDP             | Reliable Data Protocol                                               | src.port(16-23);  dst.port(24-31)                                                                                                           |
| 28                 | IRTP            | Internet Reliable Transaction Protocol                               | port(8-15)                                                                                                                                  |
| 30                 | NETBLT          | Bulk Data Transfer Protocol                                          | local.port(48-63);  foreign.port(64-79);  connection Unique ID(96-127)                                                                      |
| 33                 | DCCP            | Datagram Congestion Control Protocol                                 | src.port(0-15);  dst.port(16-31)                                                                                                            |
| 41                 | IPv6            | IPv6 encapsulation                                                   | encapsulated src.IP();  dst.ip()                                                                                                            |
| 47                 | GRE             | Generic Routing Encapsulation                                        | if bit(2)=1, then key(64-95)                                                                                                                |
| 48                 | DSR             | Dynamic Source Routing Protocol                                      | per-option decision                                                                                                                         |
| 50                 | ESP             | Encapsulating Security Protocol                                      | spi(0-31)                                                                                                                                   |
| 51                 | AH              | Authentication Header                                                | spi(32-63)                                                                                                                                  |
| 54                 | NARP            | Non-broadcast Multiple Access network address resolution protocol    | src.addr(64-95);  dst.addr(96-127)                                                                                                          |
| 81                 | VMTP            | Versatile Message Transaction Protocol                               | serverEntityID(0-31)                                                                                                                        |
| 82                 | SECURE-VMTP     | Secure Versatile Message Transaction Protocol                        | serverEntityID(0-31)                                                                                                                        |
| 88                 | EIGRP           | Enhanced Interior Gateway Routing Protocol                           | VRID(128-143);  ASN(144-160)                                                                                                                |
| 89                 | OSPF            | Open Shortest Path First                                             | routerID(32-63);  areaID(64-95);  InstanceID(144-152)                                                                                       |
| 94                 | IPIP            |                                                                      | encapsulated payload src.ip(); encapsulated payload dst.ip()                                                                                |
| 97                 | ETHERIP         | Ethernet within IP encapsulation                                     | encapsulated payload src.eth(0-47);  encapsulated payload dst.eth(48-95)                                                                    |
| 98                 | ENCAP           | Encapsulation Header                                                 | flowID(32-63)                                                                                                                               |
| 115                | L2TP            | Layer-2 Tunneling Protocol                                           | sessionID(0-31)                                                                                                                             |
| 131                | PIPE            | Private IP Encapsulation within IP                                   | vpn-oui(8-31);  vpn-index(32-63)                                                                                                            |
| 132                | SCTP            | Stream Control Transmission Protocol                                 | src.port(0-15);  dst.port(16-31)                                                                                                            |
| 134                | RSVP-E2E-IGNORE |                                                                      | IPv4 Session Address(0-31);  IPv6 Session Address(0-63)                                                                                     |
| 136                | UDPLite         | Lightweight User Datagram Protocol                                   | src.port(0-15);  dst.port(16-31)                                                                                                            |
| 137                | MPLS-in-IP      | Multiprotocol Label Switching in IP or Generic Routing Encapsulation | 1st label(0-31)                                                                                                                             |
| 139                | HIP             | Host Identity Protocol                                               | sender HIT(64-96);  receiver HIT(97-128)                                                                                                    |
| 141                | WESP            | Wrapped Encapsulating Security Protocol                              | if encrypted payload bit is "0", then ESP payload can be inspected;  possible to use L4-7 payload DAG as indicated by ESP Next Header value |
| 142                | ROHC            | Robust Header Compression                                            | DAG as ESP                                                                                                                                  |
