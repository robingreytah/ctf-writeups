# Simple CTF

Hello fellow hackers, it's time to capture the flag! So grab a cup of your favourite coffee and let's dive right in!

This room is a beginner level CTF created by [0xSeth](https://tryhackme.com/p/MrSeth6797). The purpose of this writeup is to guide you through the process of completing this room rather than providing you with answers. If you like this writeup feel free to share and star the repo, and follow me on [Twitter](https://twitter.com/robingreytah), [GitHub](https://github.com/robingreytah), [Twitch](https://www.twitch.tv/robingreytah), [LinkedIn](https://www.linkedin.com/in/robin-greytah/), [YouTube](https://www.youtube.com/channel/UCX1ZdYGUCpZP6smtyhra6bQ).

## Enumeration

### nmap

The first thing we do right after deploying our machine is start a network scan as these usually take quite some time. For this purpose we decide to use nmap:

```sh
nmap -sC -sC -sV -vv -oN nmapScan.txt 10.10.122.134
```

> **Note**
> If you're still confused by nmap flags go checkout [this cheatsheet](https://www.stationx.net/nmap-cheat-sheet/).

<details>
  <summary>The output of the nmap scan is presented below.</summary>

  ```sh
PORT     STATE SERVICE REASON  VERSION
21/tcp   open  ftp     syn-ack vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.18.117.205
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
2222/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
  ```
</details>

Our nmap scan taught us three valuable pieces of information about our target machine:

- port 21 FTP is open, anonymous FTP login is allowed
- port 80 HTTP is open running Apache 2.4.18
- port 2222 SSH is open (which is quite unusual on such a high port)

## Exploration

### FTP

From the nmap scan we now that anonymous FTP login is allowed so let's try to login. After a bit of digging around we come across a file called `ForMitch.txt`. This file however does not teach us much. Let's keep going!

```sh
ftp 10.10.122.134
anonymous
ls
cd pub
get ForMitch.txt
exit
cat ForMitch.txt
```

### HTTP

Let's use `gobuster` to scan directories and files on the web server.

```sh
gobuster dir -u http://10.10.122.134/ -w /usr/share/dirb/wordlists/small.txt -t 20
```



## Exploitation