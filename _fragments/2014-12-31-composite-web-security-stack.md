---
layout: post
title: 'network security flags'
date: '2014-12-31 00:00Z'
categories: post
---
# Network security flags
Computer networks are insecure by default, and there isn't really a way to measure how secure they are as you start to add security functions.  Here's an idea for doing that:

Are you using any of these?

| Flag | Protocol|
|---|---|
|0|DNSSec|
|1|IPSec AH|
|2|IPSec ESP|
|3|IPSECKEY|
|4|TLS|
|5|OCSP|
|6|TLSA/DANE|

Fewer than 3 bits is insecure, period.

| |Code|Definition|
|---|---|---|
|0+2+3|Green|SECURE: VERIFIED CREDENTIALS|
|0+4+6|Green|SECURE: VERIFIED CREDENTIALS|
|1+2+3|Green|SECURE: VERIFIED CREDENTIALS|
|1+3|Yellow|INSECURE: MEDIUM STRENGTH|
|2+3|Yellow|INSECURE: MEDIUM STRENGTH|
|4+5|Yellow|INSECURE: MEDIUM STRENGTH|
|1|Orange|INSECURE: LOW STRENGTH|
|2|Orange|INSECURE: LOW STRENGTH|
|4|Orange|INSECURE: LOW STRENGTH|
|none|Red|UNSECURED|
