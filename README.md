# eBPF-https: web application firewall (WAF)
eBPF-https is an open source web application firewall (WAF) for Apache and Nginx built using eBPF.
[![Build Status](https://drone.grafana.net/api/badges/grafana/beyla/status.svg?ref=refs/heads/main)](https://ebpf-security.github.io/navihtml/ebpf-dump.html)

## Feature
eBPF is a revolutionary technology that can  hooks OpenSSL's SSL_read and SSL_write functions through uprobe, and directly obtains HTTPS plaintext request data.
* SSL/TLS plaintext capture, support openssl\libressl\boringssl\gnutls\nspr(nss) libraries
* Enables web application defenders to gain visibility into HTTP(S) traffic 
* Deployment without downtime in production environment
* No certificate required

## Detection Rules
* Malicious Web vulnerability scanning
* Database SQL injection
* Cross-site scripting attack (XSS)
* CC & DDOS protection
* Password brute force cracking
* Dangerous file upload detection
* Illegal URL/file access
* Compatible with some OWASP ModSecurity regular rules
* eBPF technology and extremely high performance
* Unsupervised machine learning, autonomous generation of adversarial rules

## Requirements
Security monitoring purposes, It runs on/requires Linux Kernel >= 5.10 such as the following platforms:
* Ubuntu 22.04+
* Fedora 33+
* RHEL 9.0+
* Debian 12+
* Rocky Linux 9.0+
* OpenSUSE 15+
* ...

## Building & Running
```console
# Ubuntu
sudo apt-get install -y make gcc libelf-dev

# RHEL
sudo yum install -y make gcc elfutils-libelf-devel

$ make

$ ./ebhttps
   OpenSSL path: /lib64/libssl.so.3
   ebhttps start ok...sql injection attack example:  curl or wget https://www.google.com/?id=123' or 1='1
   2025-11-28 13:21:08   [*ALERT*]  [942100] [GET /] STR:"123 or 1=1" Matched, SQL Injection Attack Detected via libinjection 
```
Loading eBPF program  requires root privileges 

## Attack test
```
  1. Use nginx HTTPS server to test
  rules/main.rule loads a SQL injection detection rule by default. You can access https://serverip/select.html?testsql=delete * from test
  Or run the vulnerability scanner nikto of the Kali system: ./nikto -host serverip -ssl -port 443 -C all

  2. You can also use wget (CentOS/Fedora/Ubuntu) or curl (debian) to test SQL injection or XSS:
  wget https://www.google.com/?id=123' or 1='1
  wget https://www.google.com/?id=<script>alert(1);</script>
  wget https://www.google.com/?select.html?testsql=delete * from test
  If it is invalid, please check the dynamic link library printed by ldd /usr/bin/wget and make sure it is consistent with the OpenSSL library (libss.so.x) path displayed by ebhttps.

  3. To test DDOS attack detection, you can use wrk and other tools to test in the same environment.
  wrk -c 25 -t 25 -d 10 https://127.0.0.1/
```
## Contact Us
* Mail to `ebpf-sec@hotmail.com`
Before moving on, please consider giving us a GitHub star ⭐️. Thank you!

## License
This project is licensed under the terms of the
[MIT license](/LICENSE).
