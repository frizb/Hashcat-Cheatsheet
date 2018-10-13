# Hashcat-Cheatsheet
Hashcat Cheatsheet for OSCP\


## Identify Hashes

`hash-identifier`

## Using hashcat
Create a .hash file with all the hashes you want to crack puthasheshere.hash: $1$O3JMY.Tw$AdLnLjQ/5jXF9.MTp3gHv/

Hashcat example cracking Linux md5crypt passwords $1$ using rockyou:  

`hashcat --force -m 500 -a 0 -o found1.txt --remove puthasheshere.hash /usr/share/wordlists/rockyou.txt`

Hashcat example cracking Wordpress passwords using rockyou:  
`hashcat --force -m 400 -a 0 -o found1.txt --remove wphash.hash /usr/share/wordlists/rockyou.txt`

Sample Hashes
http://openwall.info/wiki/john/sample-hashes


## Cracking Linux Hashes - /etc/shadow file  

 500 | md5crypt $1$, MD5(Unix)                          | Operating-Systems
3200 | bcrypt $2*$, Blowfish(Unix)                      | Operating-Systems
7400 | sha256crypt $5$, SHA256(Unix)                    | Operating-Systems
1800 | sha512crypt $6$, SHA512(Unix)                    | Operating-Systems

## Cracking Windows Hashes  

3000 | LM                                               | Operating-Systems
1000 | NTLM                                             | Operating-Systems

## Cracking Common Application Hashes  

  900 | MD4                                              | Raw Hash
    0 | MD5                                              | Raw Hash
 5100 | Half MD5                                         | Raw Hash
  100 | SHA1                                             | Raw Hash
10800 | SHA-384                                          | Raw Hash
 1400 | SHA-256                                          | Raw Hash
 1700 | SHA-512                                          | Raw Hash


## To crack linux hashes you must first unshadow them

`unshadow passwd-file.txt shadow-file.txt`  

`unshadow passwd-file.txt shadow-file.txt > unshadowed.txt`  
