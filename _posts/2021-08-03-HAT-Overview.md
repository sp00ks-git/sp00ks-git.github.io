---
title:  "HAT - Hashcat Automation Tool"
date:   2021-08-03 21:21:20 +0000
categories: [tools, hat]
tags: [tools, cracking, hat]
comments: true
math: true

---

An automated Hashcat tool for common wordlists and rules to speed up the process of cracking hashes during engagements. HAT is simply a wrapper for [hashcat] (with a few extra features), however I take no credit for that superb tool.

Support for Linux and Windows from the respective repos.

Link For [Linux](https://github.com/sp00ks-git/hat)   
Link For [Windows](https://github.com/sp00ks-git/hat-windows)


All Hashes Supported by Hashcat are supported by HAT if you know the HashMode:

Examples Include:

NTLMv2 (NTHASH) -> NetNTLMv1 -> NetNTLMv2 -> MD5 -> SHA-512 -> RC4-HMAC-MD5 (Kerberoasting)


Features:

1. Straight Wordlist testing from publicy known breaches (dependant on your wordlists)   
2. Straight Wordlists using the Oxford Dictionary incrmementing through various combinations   
3.  Common Rule sets used in corporate environments   
4.  Smart ordering of compromised hashes alphabetically in (Username::Domain:Hash:Password) format.   
5.  Visual hash cracking status showing you how many hashes you have left to crack   
6.  Cewl Integration for finding specific words common to the business not found in dictionaries or breached lists   
7.  Rsmangler Integration for finding permutations of a specific word that the firm might be using. (includes incrementing various combinations on either side)   
8.  The directory structure that HAT expects is upto you, the default is:

-> /opt/worliststs/rockyou.txt   
-> /opt/wordlists/1GB-4GB/   
-> /opt/wordlists/4GB+/   
-> /opt/wordlists/english-words/   
-> /opt/wordlists/merged_list/   

Suggested Wordlists download links (HTTP) - working as of 14/10/2019 (maybe out of date now but some should still be working)   


* [rockyou] - https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt (~14,300,000 words)   
* [rocktastic12a] - http://www.mediafire.com/file/9tf3n2d45tgktq1/Rocktastic12a.7z/file (1.37GB - Compressed)   
* [dictionary_words] - https://github.com/dwyl/english-words/blob/master/words.txt (~466,000 words)   
* [PsycOPacKv2] - http://storage.aircrack-ng.org/users/PsycO/PsycOPacKv2.rar (1.4GB)   
* [sp00ks_merged_file_uniq] - https://download.g0tmi1k.com/wordlists/large/sp00ks_merged_file_uniq.7z (2.7 GB - Compressed)   
* [crackstation_human] - https://crackstation.net/files/crackstation-human-only.txt.gz (4.2 GB)   
* [crackstation_main] - https://download.g0tmi1k.com/wordlists/large/crackstation.txt.gz (4.5 GB)   
* [10_million_combos] - https://download.g0tmi1k.com/wordlists/large/10-million-combos.zip (8.8 GB)   
* [18_in_1] - https://download.g0tmi1k.com/wordlists/large/36.4GB-18_in_1.lst.7z (48.4 GB)   
* [b0n3z] - https://download.g0tmi1k.com/wordlists/large/b0n3z-wordlist-sorted-something.tar.gz (165 GB)   
* [sheez] - http://download1568.mediafire.com/yuh4jmehecwg/8oazhwqzexid771/WordlistBySheez_v8.7z (166.17 GB)   
* [hashkiller] - http://hashkiller.io/downloads/hashkiller-dict-2020-01-26.7z   

[rockyou]: https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt (~14,300,000 words)   
[rocktastic12a]: http://www.mediafire.com/file/9tf3n2d45tgktq1/Rocktastic12a.7z/file (1.37GB - Compressed)   
[dictionary_words]: https://github.com/dwyl/english-words/blob/master/words.txt (~466,000 words)   
[PsycOPacKv2]: http://storage.aircrack-ng.org/users/PsycO/PsycOPacKv2.rar (1.4GB)   
[sp00ks_merged_file_uniq]: https://download.g0tmi1k.com/wordlists/large/sp00ks_merged_file_uniq.7z (2.7 GB - Compressed)   
[crackstation_human]: https://crackstation.net/files/crackstation-human-only.txt.gz (4.2 GB)   
[crackstation_main]: https://download.g0tmi1k.com/wordlists/large/crackstation.txt.gz (4.5 GB)   
[10_million_combos]: https://download.g0tmi1k.com/wordlists/large/10-million-combos.zip (8.8 GB)   
[18_in_1]: https://download.g0tmi1k.com/wordlists/large/36.4GB-18_in_1.lst.7z (48.4 GB)   
[b0n3z]: https://download.g0tmi1k.com/wordlists/large/b0n3z-wordlist-sorted-something.tar.gz (165 GB)   
[sheez]: http://download1568.mediafire.com/yuh4jmehecwg/8oazhwqzexid771/WordlistBySheez_v8.7z (166.17 GB)   
[hashkiller]: http://hashkiller.io/downloads/hashkiller-dict-2020-01-26.7z   

Thanks to:

Cewl - [digininja_cewl] @digininja   
Passphrases - [initstring] @initstring   
Rsmangler - [digininja_rsmangler] @digininja   

[digininja_cewl]: https://github.com/digininja/CeWL   
[digininja_rsmangler]: https://github.com/digininja/RSMangler   
[initstring]: https://github.com/initstring/passphrase-wordlist   
[hashcat]: https://hashcat.net/hashcat   

