## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/TheMaster-2/EthicalHackingWriteUps/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

Test

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/TheMaster-2/EthicalHackingWriteUps/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

Machine 1
Dev by TCM Academy

This is my first write up

This Vulnerable by design VM is provided by TCM Acamdemy as part of the Practical Ethical Hacking - The Complete Course

I had to change Network cards to NAT, also for some reason this VM was dual honed, however, this is not relevant for this VM. 

Nnce VM booted, I had to log in and run (creds are provided in a text file byt TCM)
```markdown
DHClient
```
**Enumeration**

Then run Netdiscover to get target IP, for me.
```markdown
sudo netdiscover -r 192.168.22.0/24
```
We find our target VM
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
- |_http-server-header: Apache/2.4.38 (Debian)
- |_http-title: Bolt - Installation error
- 111/tcp   open  rpcbind  2-4 (RPC #100000)
- 2049/tcp  open  nfs_acl  3 (RPC #100227)
- 8080/tcp  open  http     Apache httpd 2.4.38 ((Debian))
-	|_http-server-header: Apache/2.4.38 (Debian)
-	|_Methods supported:CONNECTION
-	|_http-title: PHP 7.3.27-1~deb10u1 - phpinfo()
-
- Running: Linux 4.X|5.X
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

Ran FFUF to parse web folders in two tabs
```markdown
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ http://192.168.22.136/FUZZ
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ http://192.168.22.136:8080/FUZZ
```

So now I checked NFS
```markdown
Showmount -e 192.168.22.136 for nfs
```

mkdir in mnt i called it nfs
```markdown
cd mnt
mkdir nfs
sudo mount -t nfs 192.168.22.136:/srv/nfs /mnt/nfs -o nolock
```

in nfs floder we find a file called save.zip
![image](https://user-images.githubusercontent.com/66864342/160244210-cb28624a-b755-4284-8f4d-2ebd4efa4ec8.png)



Discovered the following web folders

![image](https://user-images.githubusercontent.com/66864342/160243845-7911f7ae-a700-4cbd-991a-4e48fd81a3cb.png)

opened config.yaml and we Find a usern & password
![image](https://user-images.githubusercontent.com/66864342/160244148-5dcb3f37-b215-47f1-9b77-6e9fafc0847c.png)






