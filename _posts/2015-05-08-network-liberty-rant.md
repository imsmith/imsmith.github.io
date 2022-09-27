---
layout: post
title: 'The network isn't a monolith, it is an organism'
date: '2014-12-31 00:00Z'
categories: post
---
# The network isn't a monolith, it is an organism.

It shouldn't be owned by a single entity.

The physical cables and the data-link (mostly Ethernet these days) should be owned by one entity and this should be designed and provisioned for symmetric data rates and volumes.

The network layer (mostly the Internet Protocol these days) should be free to open market competition.

The transport layer (things like TCP and UDP) should NOT be owned by the network layer; this breaks the end-to-end principle, creates many of the reasons why the goodput != theoretical medium throughput, and it prone to abuse.  The transport layer belongs to no one - it is the commons of the Internet.

First order access.  Taken from the First Order Retrievability principle espoused by Adam Savage, that the tool should be accessible without having to move anything else.  The First Order Access principle is that the service should be accessible without having to use anything else.  WiFi access points that require you to "click through" a terms of use agreement in order to first order access to the Domain Name System and the Network Time Protocol should be free to open market competiton.  I should not be forced to use a DNS server chosen for me, ever.  Same with NTP.  If you need to assign me a bootstrap DNS server to get out of your network, you are doing it wrong.  If you need to assign a split horizon DNS server as part of a VPN, that should be ok.

This:
   
```conf
nameserver 198.51.100.1
nameserver 192.0.2.53
nameserver 203.0.113.53
nameserver 2001:db8::53/64
```

Could be this:

```conf
nameserver 198.51.100.1    99
nameserver 192.0.2.53	   49
nameserver 203.0.113.53    19
nameserver 2001:db8::53/64 09
```

Prioritization matters when you are picking your own DNS server and keeping it.  This says use `2001:db8::53/64` first, and if it isn't available, use something else.

But it really should be this:

```conf
search example.com {
       nameserver 198.51.100.1     99
}
search example.net {
       nameserver 192.0.2.53       49
       nameserver 198.51.100.1     99
}
search * {
       nameserver 203.0.113.53     19
       nameserver 2001:db8::53/64  09
}
```

Context matters more - when you are really doing it right, you are telling the client to use _this particular server for this particular set of domains_. 

Then you can do this:

```conf
search icann_tlds {
       nameserver 198.51.100.1:53 expect { dnssec } desire { tls dane } 99
}
search alt_tlds {
       nameserver 198.51.100.1:5353 expect { tls dane dnssec } 89   
}
search vpn_domain {
       nameserver 192.0.2.53:53 expect { tls dane dnssec } 49
}
search my_dns {
       nameserver 203.0.113.53:53 expect { ipsec dane dnssec } desire { dccp sctp }  19
       nameserver 2001:db8::53/64 expect { ipsec dane dnssec } desire { dccp sctp }  09
}
```

Here context + priority + policy allows the client to be in control of _how it consumes centralized services outside its control_.
  

## First-order access to Certificate Authority services should be free to open market competition.

The current ca-bundle.crt is unmanagable. The current trust model of the CA system is broken (mostly because of profiteering, not engineering, but there are some engineering issues, too ).  Browsers don't support DANE or OCSP, thus, no clientside validation of trust exists outside of the ca-bundle and intentionally installed server certs.  This makes the self-signed certificate and explicit trust - and therefore personal webs of trust - unscalable.  Given the depencence upon TLS by the Web, this is an untenable situation.

I want something like this: 

```conf
secure ca-bundle {
       file          ca-bundle.crt 
       dane	     try { guard }
       ocsp	     try { alert }
}
secure wot-bundle {
       file          wot-bundle.crt
       dane	     yes
       ocsp	     try { alert }	 	
}
secure my-ca {
       ca-server     keyserver.example.com  
       ca-pub-key    keyserver.example.com.crt
       ca-client-key me@keyserver.example.com.crt
       dane	     yes
       ocsp	     yes
}
```
Again, the desire here is to allow the client to be in control of _how it consumes centralized services outside its control_.

But we can be even more granular by combining name resolution and certificate validation:

```conf
search icann_tlds {
       nameserver 198.51.100.1:53 expect { dnssec } desire { tls dane } 99
       secure ca-bundle {
             file          ca-bundle.crt 
       	     dane	   try { guard }
       	     ocsp  	   try { alert }
       }
}
search alt_tlds {
       nameserver 198.51.100.1:5353 expect { tls dane dnssec } 89   
       secure wot-bundle {
             file          wot-bundle.crt
       	     dane	   yes
       	     ocsp	   try { alert }	 	
       }
}
search vpn_domain {
       nameserver 192.0.2.53:53 expect { tls dane dnssec } 49
       secure vpn-ca {
       	     pub-key       vpn.example.com.crt 
	     dane	   yes
	     ocsp	   yes
       }
}
search my_dns {
       nameserver 203.0.113.53:53 expect { ipsec dane dnssec } desire { dccp sctp }  19
       nameserver 2001:db8::53/64 expect { ipsec dane dnssec } desire { dccp sctp }  09
       secure my-ca {
             ca-server     keyserver.example.com  
       	     ca-pub-key    keyserver.example.com.crt
       	     ca-client-key me@keyserver.example.com.crt
       	     dane	   yes
       	     ocsp	   yes
       }
}
```

This says to match the requested domain to one of the four lists of domains - most specific to least specific - then use the nameservers listed in order of priority - lowest to highest - and use the ca-bundles, public certificates, key servers, and validation mechanisms listed to validate not only the name resolution, but the subsequent data connections as well.