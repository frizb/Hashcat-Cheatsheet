# Hashcat-Cheatsheet
Hashcat Cheatsheet for OSCP
https://hashcat.net/wiki/doku.php?id=hashcat

## Identify Hashes

`hash-identifier`

Example Hashes: https://hashcat.net/wiki/doku.php?id=example_hashes

## Using hashcat and a dictionary
Create a .hash file with all the hashes you want to crack puthasheshere.hash: $1$O3JMY.Tw$AdLnLjQ/5jXF9.MTp3gHv/

Hashcat example cracking Linux md5crypt passwords $1$ using rockyou:  

`hashcat --force -m 500 -a 0 -o found1.txt --remove puthasheshere.hash /usr/share/wordlists/rockyou.txt`

Hashcat example cracking Wordpress passwords using rockyou:  
`hashcat --force -m 400 -a 0 -o found1.txt --remove wphash.hash /usr/share/wordlists/rockyou.txt`

Sample Hashes
http://openwall.info/wiki/john/sample-hashes

## Using hashcat bruteforcing 
```
predefined charsets
?l = abcdefghijklmnopqrstuvwxyz
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
?d = 0123456789
?s = «space»!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
?a = ?l?u?d?s
?b = 0x00 - 0xff
```

?l?d?u is the same as:  
?ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789  
  
Brute force all passwords length 1-8 with possible characters A-Z a-z 0-9   
`hashcat64 -m 500 hashes.txt -a  3  ?1?1?1?1?1?1?1?1 --increment -1 ?l?d?u`  

## Cracking Linux Hashes - /etc/shadow file  

| ID | Description                                      | Type |
|----|--------------------------------------------------|------|
| 500 | md5crypt $1$, MD5(Unix)                          | Operating-Systems|
|200 | bcrypt $2*$, Blowfish(Unix)                      | Operating-Systems|
|400 | sha256crypt $5$, SHA256(Unix)                    | Operating-Systems|
|1800 | sha512crypt $6$, SHA512(Unix)                    | Operating-Systems|

## Cracking Windows Hashes  

| ID | Description                                      | Type |
|----|--------------------------------------------------|------|
|3000 | LM                                               | Operating-Systems|
|1000 | NTLM                                             | Operating-Systems|

## Cracking Common Application Hashes  

| ID | Description                                      | Type |
|----|--------------------------------------------------|------|
| 900 | MD4                                              | Raw Hash|
|   0 | MD5                                              | Raw Hash|
|5100 | Half MD5                                         | Raw Hash|
|  100 | SHA1                                             | Raw Hash|
|10800 | SHA-384                                          | Raw Hash|
| 1400 | SHA-256                                          | Raw Hash|
| 1700 | SHA-512                                          | Raw Hash|

## Cracking Common File Password Protections
| ID | Description                                      | Type |
|----|--------------------------------------------------|------|
|  11600 | 7-Zip                                            | Archives|
|  12500 | RAR3-hp                                          | Archives|
|  13000 | RAR5                                             | Archives|
|  13200 | AxCrypt                                          | Archives|
|  13300 | AxCrypt in-memory SHA1                           | Archives|
|  13600 | WinZip                                           | Archives|
|   9700 | MS Office <= 2003 $0/$1, MD5 + RC4               | Documents|
|   9710 | MS Office <= 2003 $0/$1, MD5 + RC4, collider #1  | Documents|
|   9720 | MS Office <= 2003 $0/$1, MD5 + RC4, collider #2  | Documents|
|   9800 | MS Office <= 2003 $3/$4, SHA1 + RC4              | Documents|
|   9810 | MS Office <= 2003 $3, SHA1 + RC4, collider #1    | Documents|
|   9820 | MS Office <= 2003 $3, SHA1 + RC4, collider #2    | Documents|
|   9400 | MS Office 2007                                   | Documents|
|   9500 | MS Office 2010                                   | Documents|
|   9600 | MS Office 2013                                   | Documents|
|  10400 | PDF 1.1 - 1.3 (Acrobat 2 - 4)                    | Documents|
|  10410 | PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #1       | Documents|
|  10420 | PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #2       | Documents|
|  10500 | PDF 1.4 - 1.6 (Acrobat 5 - 8)                    | Documents|
|  10600 | PDF 1.7 Level 3 (Acrobat 9)                      | Documents|
|  10700 | PDF 1.7 Level 8 (Acrobat 10 - 11)                | Documents|
|  16200 | Apple Secure Notes                               | Documents|

## To crack linux hashes you must first unshadow them

`unshadow passwd-file.txt shadow-file.txt`  

`unshadow passwd-file.txt shadow-file.txt > unshadowed.txt`  

## Crack a zip password
`zip2john Zipfile.zip | cut -d ':' -f 2 > hashes.txt`  
`hashcat -a 0 -m 13600 hashes.txt /usr/share/wordlists/rockyou.txt`  

Hashcat appears to have issues with some zip hash formats generated from zip2john.  You can fix this by editing the zip hash contents to align with the example zip hash format found on the hash cat example page:
`$zip2$*0*3*0*b5d2b7bf57ad5e86a55c400509c672bd*d218*0**ca3d736d03a34165cfa9*$/zip2$`  

John seems to accept a wider range of zip formats for cracking.
