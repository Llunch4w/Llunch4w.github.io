---
title: Python绝技
date: 2020-12-09 10:55:56
tags: Python Hack
---

{% centerquote %} ^ _ ^ {% endcenterquote %}
<!-- more -->

# 前言

《Python绝技--运用Python称为顶级黑客》是 TJ.O'Connor 编著的一本利用 Python 进行网络攻防的书籍。一次偶然路过还书车看到这本书，觉得可以深入了解一下，于是就拿来看了。看了一章后，决定开启这篇博客，以便在之后的日子里一边阅读一边记录。做这个决定的原因有这么几个：
1. 正所谓‘好记性不如烂键盘’，记录大概有利于当前理解与日后查阅
2. 在看完书中的第一章后，大概发现书中使用的 python 版本是 2.7，而我 Windows 主机上安装的 python 是3.6版本的。而且不知是不是系统的原因，书中 linux 系统下 python 自带的 crypt 库在 Windows 下的 python 中似乎不是自带的，我恐怕之后还会有这样因为环境不统一而产生的诸多问题。我不希望在环境统一上耗费太多的心力，于是决定将使用平台从本机上的命令行迁移到实验楼中。因为不配本机环境，所以代码自然不必要以文件形式存在本机上了，不能运行的代码并无太大价值。所以将在实验楼中写下的代码都存在本篇博客的代码段中，实验结果以图片的形式存在本篇博客的图片中，
3. 最后立一个Flag：希望我不要鸽，好好读书记笔记。也希望这本书不要让我失望，是一本值得坚持的书。


# 入门

## 程序1：Unix口令破解机

这个程序是一款二十年前的Unix破解程序，对于今天的Unix系统已经不适用，但是其思想在今天仍然有一定的借鉴意义。

**曾经的Unix系统的 /etc/passwd 文件样例**
```bash
# UserName:PassWord:UserID:GroupID:User Info:User Directory:Command/Shell
victim:HX9LLTdc/jiDE:503:100:Iama Victim:/home/victim:/bin/sh
root:DFNFxgW7C05fo:504:100:Markus Hess:/root:/bin/bash

# 但今天的 Unix 系统中，/etc/passwd中密码字段均用 x 字符代替，真正的密文存储在 /etc/shalow 中
# 另外，今天的 Unix 系统能使用更多安全的hash算法，比如SHA-512
```

但是对于20年前 Unix 系统中用于加密密码的hash算法，可以使用 python 中的crypt库进行模拟，这个库中只有一个函数，就是crypt，用法如下：
```python
crypt(word,salt) -> string
# word ：需要加密的明文
# salt : 长度为2的字符串
```

**准备文件**
```
# password.txt
victim:HX9LLTdc/jiDE:503:100:Iama Victim:/home/victim:/bin/sh
root:DFNFxgW7C05fo:504:100:Markus Hess:/root:/bin/bash

# dictionary.txt，包含一个字符串：egg
```

**Unix密码破解原理**
- dictionary.txt中包含常见密码的明文
- 对于password.txt中每个用户的密码密文
  - 使 salt = 密文的头两个字符
  - 使 word 依次 = dictionary.txt 中每个字符串
  - 如果 crypt(word,salt) 得到的值和密码密文相同，则说明破解成功，word 字段就是密码明文


**程序源码**
```python
import crypt

def testPass(cryptPass):
	salt = cryptPass[0:2]
	dictFile = open('dictionary.txt','r')
	for word in dictFile.readlines():
		word = word.strip('\n')
		cryptWord = crypt.crypt(word,salt)
		if(cryptWord == cryptPass):
			print "[+] Found password :" + word + "\n"
			return
		print "[-] password not found.\n"

def main():
	passFile = open('passwords.txt')
	for line in passFile.readlines():
		if ":" in line:
			print(line)
			user = line.split(':')[0]
			cryptPass = line.split(':')[1].strip()
			print "[*] Cracking password for: " + user
			testPass(cryptPass)

if __name__ == "__main__":
	main()
```

