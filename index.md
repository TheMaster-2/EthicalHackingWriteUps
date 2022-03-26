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

Nnce VM booted, I had to log in and run (creds are provided ib a text file byt TCM)
```markdown
DHClient
```
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









