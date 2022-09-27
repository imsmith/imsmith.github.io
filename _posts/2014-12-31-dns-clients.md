---
layout: post
title: 'I want my DNS clients to be smarter'
date: '2014-12-31 00:00Z'
categories: post
---
# I want my dns client to be smarter

Where we have this today:  

```conf
options edns0
options Inet6
nameserver 10.10.1.200
nameserver 10.10.1.32
domain example.com
```


I want to have this:

```conf
nameserver 2600:3c03::f03c:91ff:fe70:e776 {
	   priority 99
	   options dnssec
	   options ipsec   IPSECKEY   secure.example.net
}
nameserver 50.116.51.216 { 
	   priority 100
	   options dnssec
	   options ipsec   IPSECKEY   secure.example.net
}
domain example.com {
       options edns0
       options dane
       options interface tun0
       nameserver 192.168.11.1 {
       		  priority 20
       		  options dnssec 
       		  options dtls    CERT     secure.example.com 
       }
       nameserver 192.168.10.1 {
       		  priority 10
       		  options dnssec 
       		  options ipsec   IPSECKEY secure.example.com
       }
}
acl alt-tlds {
       match /usr/local/share/tld/alt
       options dnssec
       options ipsec
       options interface tor0
}
acl icann-tlds {
       match /usr/local/share/tld/icann
       options dnssec
}       
acl vpn-tlds {
       match /usr/local/share/tld/vpn
       options interface tun0     
}
acl public-wifi {
       no match /usr/local/share/ssid/trusted	       
       fetch-url http://nsa.gov
}
       
policy dns-lookups {
       01 public-wifi
       10 vpn-tlds
       20 example.com
       30 2600:3c03::f03c:91ff:fe70:e776
       40 50.116.51.216
       50 alt-tlds
       60 icann-tlds
}

nameserver dns-lookups
```
