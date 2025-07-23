---
{"dg-publish":true,"permalink":"/思/hackmyvm disguise/","tags":["渗透","oscp","#hackmyvm"]}
---


子域名

```
wfuzz -c -w /usr/share/wfuzz/wordlist/general/big.txt -u http://disguise.hmv -H 'host:FUZZ.disguise.hmv' --hl 890

```

