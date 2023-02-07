# [Overpass](https://tryhackme.com/room/overpass)

![password cracking](../../img/passwordCracking.webp)

Hello fellow hackers, time to capture the flag :triangular_flag_on_post:! This room was built by [NinjaJc01](https://tryhackme.com/p/NinjaJc01) and consists in cracking a password manager. I am writing this article as I progress along the room. It will therefore include my thought process, dead ends, good as well as bad decisions. If you enjoy this kind of content .

> **Quote of the Day**
>
> No man has the right to be an amateur in the matter of physical training. It is a shame for a man to grow old without seeing the beauty and strength of which his body is capable.
> 
> Socrates

## nmap scan

The first thing we do right after deploying the machine it kick off an nmap scan. 

```sh
nmap -sC -sV -sS -vv -oN nmapScan.txt 10.10.151.212
```

> **Note**
>  a good nmap cheat sheet can be found [here](https://www.stationx.net/nmap-cheat-sheet/)

<details>
  <summary>The nmap scan result can be found below.</summary>

  ```sh
  Starting Nmap 7.60 ( https://nmap.org ) at 2023-02-07 08:09 GMT
NSE: Loaded 146 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:09
Completed NSE at 08:09, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:09
Completed NSE at 08:09, 0.00s elapsed
Initiating ARP Ping Scan at 08:09
Scanning 10.10.151.212 [1 port]
Completed ARP Ping Scan at 08:09, 0.22s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 08:09
Completed Parallel DNS resolution of 1 host. at 08:09, 0.00s elapsed
Initiating SYN Stealth Scan at 08:09
Scanning ip-10-10-151-212.eu-west-1.compute.internal (10.10.151.212) [1000 ports]
Discovered open port 80/tcp on 10.10.151.212
Discovered open port 22/tcp on 10.10.151.212
Completed SYN Stealth Scan at 08:09, 1.25s elapsed (1000 total ports)
Initiating Service scan at 08:09
Scanning 2 services on ip-10-10-151-212.eu-west-1.compute.internal (10.10.151.212)
Completed Service scan at 08:09, 11.06s elapsed (2 services on 1 host)
NSE: Script scanning 10.10.151.212.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:09
Completed NSE at 08:09, 0.09s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:09
Completed NSE at 08:09, 0.00s elapsed
Nmap scan report for ip-10-10-151-212.eu-west-1.compute.internal (10.10.151.212)
Host is up, received arp-response (0.0014s latency).
Scanned at 2023-02-07 08:09:24 GMT for 13s
Not shown: 998 closed ports
Reason: 998 resets
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDLYC7Hj7oNzKiSsLVMdxw3VZFyoPeS/qKWID8x9IWY71z3FfPijiU7h9IPC+9C+kkHPiled/u3cVUVHHe7NS68fdN1+LipJxVRJ4o3IgiT8mZ7RPar6wpKVey6kubr8JAvZWLxIH6JNB16t66gjUt3AHVf2kmjn0y8cljJuWRCJRo9xpOjGtUtNJqSjJ8T0vGIxWTV/sWwAOZ0/TYQAqiBESX+GrLkXokkcBXlxj0NV+r5t+Oeu/QdKxh3x99T9VYnbgNPJdHX4YxCvaEwNQBwy46515eBYCE05TKA2rQP8VTZjrZAXh7aE0aICEnp6pow6KQUAZr/6vJtfsX+Amn3
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMyyGnzRvzTYZnN1N4EflyLfWvtDU0MN/L+O4GvqKqkwShe5DFEWeIMuzxjhE0AW+LH4uJUVdoC0985Gy3z9zQU=
|   256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (EdDSA)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINwiYH+1GSirMK5KY0d3m7Zfgsr/ff1CP6p14fPa7JOR
80/tcp open  http    syn-ack ttl 64 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-favicon: Unknown favicon MD5: 0D4315E5A0B066CEFD5B216C8362564B
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Overpass
MAC Address: 02:0C:8A:DF:AA:25 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:09
Completed NSE at 08:09, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:09
Completed NSE at 08:09, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.27 seconds
           Raw packets sent: 1002 (44.072KB) | Rcvd: 1002 (40.076KB)
```

</details>

From the nmap scan we can see that two ports are open: port 22 running SSH and port 80 running HTTP.