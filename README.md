# htb-meta

### Initial Scans:
nmap -sV -sC

![image](https://user-images.githubusercontent.com/19478700/171474499-8e5e8458-fea4-4d04-bfae-b43e19555e81.png)


### Directory Enumeration
No directories found using:
gobuster dir --url artcorp.htb -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 25  > gobuster_dir.txt

### Sub-Domain Enum
#### Used dns proxy
https://github.com/hubdotcom/marlon-tools/blob/master/tools/dnsproxy/dnsproxy.py
#### Found dev01 subdmain using:
gobuster dns -t 30 -w /usr/share/wordlists/subdomains-top1million-110000.txt -d artcorp.htb

![image](https://user-images.githubusercontent.com/19478700/171474657-f3b2b867-f1c6-4050-b99c-144ec9675b7c.png)

### Exploit time
#### looks like service may be running exiftool to get the metadata
// Server is Devian + php
//May contain CVE-2021-22204-exiftool
//Used tool to create malicious jpg
https://github.com/convisolabs/CVE-2021-22204-exiftool
//now we have access to the machine as www-data user
![image](https://user-images.githubusercontent.com/19478700/171474715-069534b9-3577-496c-aa75-062972d334cc.png)



