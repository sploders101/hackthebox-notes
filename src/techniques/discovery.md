# Discovery

There are many techniques for discovery. This section contains a small list of
the most common/useful ones.


## Banner grabbing

Banner grabbing is a common technique for identifying which service is on a
port. The service can sometimes also identify its name, variant, and features.
Banner grabbing can be performed by simply opening a connection to the service
and listening to what it says. No data is sent to the service.

Netcat example:
```
┌─[shaunkeys@parrot]─[~/ptnotes]
└──╼ $nc 127.0.0.1 22
SSH-2.0-OpenSSH_10.0p2 Debian-7
```

Nmap example:
```
┌─[shaunkeys@parrot]─[~/ptnotes]
└──╼ $nmap -sV -p 22 --script banner 127.0.0.1
Starting Nmap 7.95 ( https://nmap.org ) at 2026-02-16 09:22 MST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000060s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 10.0p2 Debian 7 (protocol 2.0)
|_banner: SSH-2.0-OpenSSH_10.0p2 Debian-7
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.17 seconds
```

## Web Enumeration

Web enumeration encompasses a variety of techniques used to identify software,
versions, and vulnerabilities in web servers.

### Common Files

Some common files can provide an indication as to where you can find other
parts of the service. These can help identify weak points.

* sitemap.xml
* robots.txt
