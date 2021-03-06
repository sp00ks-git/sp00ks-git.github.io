---
title:  "HAT - Hashcat Automation Tool - Features"
date:   2021-08-04 19:35:00 +0000
categories: [tools, hat]
tags: [tools, cracking, hat]
---

[HAT] was built out of the need to quickly automate multiple different attacks with a higher likelihood of success based on known information about the hash or by testing what users are more likely to do, especially in the case of AD NTLM hashes.

Firstly make sure you update the wordlist directory to where you wordlists are and create the following directory's within.

The directory structure that HAT expects by deafult is set out below (of course you can just amend the code to your own needs)

* /opt/worliststs/rockyou.txt   
* /opt/wordlists/1GB-4GB/   
* /opt/wordlists/4GB+/   
* /opt/wordlists/english-words/   
* /opt/wordlists/merged_list/   


Menus
=====

(0) Input OS - Singular Hash - (MD5, NTLM, NetNTLMv1/2, Kerberos)   
(1) Input OS - Hashes From File - (MD5, NTLM, NetNTLMv1/2, Kerberos)   
(2) Input Hash Dump & filter out usernames - e.g. useful for NTDS.dit   
(3) Input Wireless - Capture From File - (WPA-EAPOL-PBKDF2) {*.cap, *.hccapx}   
(4) Run Statistical Analysis on l00t   
(5) Display l00t - Full Dump   
(6) Display l00t - Key Information Summary (BETA)   
(7) Display Hashes Uploaded - Full Dump   


(0) Input OS - Singular Hash   
----------------------------
This purpose of this menu is when you have found a single hash that you want to crack. A copy / paste style input. The file will be added into a file with the same filename and corresponding pot file.

(1) Input OS - Hashes From File
-------------------------------
The purpose of this menu is when you have one or more hashes in a file and want to crack against a file.

(2) Input Hash Dump & filter out usernames
------------------------------------------
This menu is useful during AD audits / engagements where you have machine accounts, accounts with a ($) as well as user accounts. Usually you dont want to attempt to crack machine accounts as they are by design long and complex. This menu will filter out all the machine accounts for you and only try to crack user accounts. A crack file will be created with a suffix of '_users' denoting this is users only.

(3) Input Wireless - Capture From File
--------------------------------------
This menu is useful when you have a .cap or .hccapx file that you want to crack. HAT will convert this for you using the PMKID method. This method is preferable when supported as it is more easily retrievable to use the Pairwise Master Key Identifier (PMKID) from a router using WPA/WPA2 security, which can then be used to crack the wireless password of the router. While previous WPA/WPA2 cracking methods required an attacker to wait for a user to login to a wireless network and capture a full authentication handshake, this new method only requires a single frame which the attacker can request from the AP because it is a regular part of the protocol. A file with the corresponding name and capture is created along with the pot file.

(4) Run Statistical Analysis on l00t
------------------------------------
This menu simples shows you stats about the passwords you have found. useful if you quickly want to understand patterns in the passwords you've already cracked to help in finding more.

(5) Display l00t - Full Dump
----------------------------
A dump of the passwords found form a given pot file

(6) Display l00t - Key Information Summary (BETA)   
This menu was designed to simply show the username and passwords found and remove all of information for quickly visualizing passwords.

(7) Display Hashes Uploaded - Full Dump
---------------------------------------
This menu will simple show the hashes that have been uploaded.


MENU Examples
=============

Hash Upload Menu
----------------
![img-description](/images/hash-menu-1.png)
_This menu displays the files currently in the uploads directory_ 


![img-description](/images/Multi_Hash_Cracking_Menu.png)
_Main Hash Cracking Menu Displaying Various Cracking Options_

![img-description](/images/loot_directory.png)
_Image L00t Directory Alphanumerically Organised By Name_

[HAT]: https://github.com/sp00ks-git/hat

