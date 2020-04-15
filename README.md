# Hashcat-Cheatsheet
Hashcat Cheatsheet for OSCP
https://hashcat.net/wiki/doku.php?id=hashcat

## Identify Hashes

`hash-identifier`

Example Hashes: https://hashcat.net/wiki/doku.php?id=example_hashes

## MAX POWER!
I have found that I can squeeze some more power out of my hash cracking by adding these parameters:
```
--force -O -w 4 --opencl-device-types 1,2
```
These will force Hashcat to use the CUDA GPU interface which is buggy but provides more performance (–force) , will Optimize for 32 characters or less passwords (-O) and will set the workload to "Insane" (-w 4) which is supposed to make your computer effectively unusable during the cracking process.
Finally "--opencl-device-types 1,2 " will force HashCat to use BOTH the GPU and the CPU to handle the cracking.

## Using hashcat and a dictionary
Create a .hash file with all the hashes you want to crack puthasheshere.hash: $1$O3JMY.Tw$AdLnLjQ/5jXF9.MTp3gHv/

Hashcat example cracking Linux md5crypt passwords $1$ using rockyou:  

`hashcat --force -m 500 -a 0 -o found1.txt --remove puthasheshere.hash /usr/share/wordlists/rockyou.txt`

Hashcat example cracking Wordpress passwords using rockyou:  
`hashcat --force -m 400 -a 0 -o found1.txt --remove wphash.hash /usr/share/wordlists/rockyou.txt`

Sample Hashes
http://openwall.info/wiki/john/sample-hashes

## HashCat One Rule to Rule them All  
Not So Secure has built a custom rule that I have had luck with in the past:  
https://www.notsosecure.com/one-rule-to-rule-them-all/  
The rule can be downloaded from their Github site:  
https://github.com/NotSoSecure/password_cracking_rules  

I typically drop OneRuleToRuleThemAll.rule into the rules subfolder and run it like this from my windows box (based on the notsosecure article):
```
hashcat64.exe --force -m300 --status -w3 -o found.txt --remove --potfile-disable -r rules\OneRuleToRuleThemAll.rule hash.txt rockyou.txt
```

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


## Cracking Commmon Database Hash Formats
|     ID | Description                                      | Type | Example Hash |
|--------|--------------------------------------------------|------|--------------|
|     12 | PostgreSQL                                       | Database Server |  a6343a68d964ca596d9752250d54bb8a:postgres |
|    131 | MSSQL (2000)                                     | Database Server | 0x01002702560500000000000000000000000000000000000000008db43dd9b1972a636ad0c7d4b8c515cb8ce46578 |
|    132 | MSSQL (2005)                                     | Database Server | 	0x010018102152f8f28c8499d8ef263c53f8be369d799f931b2fbe |
|   1731 | MSSQL (2012, 2014)                               | Database Server |  0x02000102030434ea1b17802fd95ea6316bd61d2c94622ca3812793e8fb1672487b5c904a45a31b2ab4a78890d563d2fcf5663e46fe797d71550494be50cf4915d3f4d55ec375 |
|    200 | MySQL323                                         | Database Server | 7196759210defdc0 |
|    300 | MySQL4.1/MySQL5                                  | Database Server | fcf7c1b8749cf99d88e5f34271d636178fb5d130 |
|   3100 | Oracle H: Type (Oracle 7+)                       | Database Server | 7A963A529D2E3229:3682427524 |
|    112 | Oracle S: Type (Oracle 11+)                      | Database Server | ac5f1e62d21fd0529428b84d42e8955b04966703:38445748184477378130 | 
|  12300 | Oracle T: Type (Oracle 12+)                      | Database Server | 78281A9C0CF626BD05EFC4F41B515B61D6C4D95A250CD4A605CA0EF97168D670EBCB5673B6F5A2FB9CC4E0C0101E659C0C4E3B9B3BEDA846CD15508E88685A2334141655046766111066420254008225 |
|   8000 | Sybase ASE                                       | Database Server | 0xc00778168388631428230545ed2c976790af96768afa0806fe6c0da3b28f3e132137eac56f9bad027ea2 |

## Cracking NTLM hashes 

After grabbing or dumping the NTDS.dit and SYSTEM registry hive or dumping LSASS memory from a Windows box, you will often end up with NTLM hashes.  

| Path | Description  |
| ----- | ------------|
| C:\Windows\NTDS\ntds.dit |  Active Directory database | 
| C:\Windows\System32\config\SYSTEM |  Registry hive containing the key used to encrypt hashes |

And using Impacket to dump the hashes
```
impacket-secretsdump -system SYSTEM -ntds ntds.dit -hashes lmhash:nthash LOCAL -outputfile ntlm-extract
```
You can crack the NTLM hash dump usign the following hashcat syntax:
```
hashcat64 -m 1000 -a 0 -w 4 --force --opencl-device-types 1,2 -O d:\hashsample.hash "d:\WORDLISTS\realuniq.lst" -r OneRuleToRuleThemAll.rule
```
*Benchmark using a Nvidia 2060 GTX:*
Speed: 7000 MH/s
Recovery Rate: 12.47%
Elapsed Time: 2 Hours 35 Minutes

## Cracking Hashes from Kerboroasting - KRB5TGS

A service principal name (SPN) is a unique identifier of a service instance. SPNs are used by Kerberos authentication to associate a service instance with a service logon account. This allows a client application to request that the service authenticate an account even if the client does not have the account name.
KRB5TGS - Kerberoasting Service Accounts that use SPN Once you have identified a Kerberoastable service account (Bloodhound? Powershell Empire? - likely a MS SQL Server Service Account), any AD user can request a krb5tgs hash from it which can be used to crack the password.

Based on my benchmarking, KRB5TGS cracking is 28 times slower than NTLM.

Hashcat supports multiple versions of the KRB5TGS hash which can easily be identified by the number between the dollar signs in the hash itself.

* 13100 - Type 23       - $krb5tgs$23$
* 19600 - Type 17       - $krb5tgs$17$
* 19700 - Type 18       - $krb5tgs$18$
* 18200 - ASREP Type 23 - $krb5asrep$23$

KRB5TGS Type 23 - Crackstation humans only word list with OneRuleToRuleThemAll mutations rule list.
```
hashcat64 -m 13100 -a 0 -w 4 --force --opencl-device-types 1,2 -O d:\krb5tgs.hash d:\WORDLISTS\realhuman_phill.txt -r OneRuleToRuleThemAll.rule	
```
*Benchmark using a Nvidia 2060 GTX:*
Speed: 250 MH/s
Elapsed Time: 9 Minutes


## Cracking NTLMv2 Hashes from a Packet Capture
You may be asked to recover a password from an SMB authentication (NTLMv2) from a Packet Capture.
The following is a 9-step process for formatting the hash correctly to do this.
https://research.801labs.org/cracking-an-ntlmv2-hash/

## To crack linux hashes you must first unshadow them

`unshadow passwd-file.txt shadow-file.txt`  

`unshadow passwd-file.txt shadow-file.txt > unshadowed.txt`  

## Crack a zip password
`zip2john Zipfile.zip | cut -d ':' -f 2 > hashes.txt`  
`hashcat -a 0 -m 13600 hashes.txt /usr/share/wordlists/rockyou.txt`  

Hashcat appears to have issues with some zip hash formats generated from zip2john.  You can fix this by editing the zip hash contents to align with the example zip hash format found on the hash cat example page:
`$zip2$*0*3*0*b5d2b7bf57ad5e86a55c400509c672bd*d218*0**ca3d736d03a34165cfa9*$/zip2$`  

John seems to accept a wider range of zip formats for cracking.

## PRINCE Password Generation
PRINCE (PRobability INfinite Chained Elements) is a hashcat utility for randomly generating probable passwords:
```
pp64.bin --pw-min=8 < dict.txt | head -20 shuf dict.txt | pp64.bin --pw-min=8 | head -20
```
Reference:  
https://github.com/hashcat/princeprocessor

## Purple Rain
Purple Rain attack uses a combination of Prince, a dictionary and random Mutation rules to dynamicaly create infinite combinations of passwords.

```
shuf dict.txt | pp64.bin --pw-min=8 | hashcat -a 0 -m #type -w 4 -O hashes.txt -g 300000
```
Reference:  
https://www.netmux.com/blog/purple-rain-attack



