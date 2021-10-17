---
title:  "BURP - Bit Flipping"
date:   2021-08-05 19:30:00 +0000
categories: [techniques, website testing]
tags: [burp, web explotion]
---

Bit Flipping
============

Bit Flipping is useful during web exploitation testing as is allows us to change 1 bit of data e.g. (a cookie) to see if we can work out what another users login is from the perspective of the cookie not the user ID.   

For example if we can create a user account that is "bdmin" we will receive a cookie after logging in for our session.   
 
If we flip 1 bit from the authorisation cookie we might get lucky and find the value for "admin"   

We might also find lots of different values like "@dmin" "$dmin" etc.. Also you will get different results each time its run so run it multiple times until you get lucky..   

As we can see in the below request we have logged in with the user 'bdmin' and have a cookie under the parameter of "auth"   

![img-description](/images/bit-flip-1.png)
_logged in with the user 'bdmin' and have a cookie under the parameter of "auth"_


Now we send that response to Intruder and change the Payload to "Bit flipper"   
Next change the value of "Format of original data" to "Literal value" this is very important!   

Now start the attack.   

We see a varied of responses and we will have to run this a few times to see one that is right.   

I have tried this 4 times before getting the result seen below.  Each one of the Lengths of 1351 bytes in this case has found 1 bit differences in usernames. in the below example we can see the username of "wdmin" was found with the cookie at request 63.   


![img-description](/images/bit-flip-2.png)
_ we can see the username of "wdmin" was found with the cookie at request 63._


We also notice at request 50 and 57 a larger length of response. On inspection we can see that this is in fact the user "admin" awesome! So now we have the auth code for the user "admin" we know this because of the line "You are currently logged in as admin!"   

![img-description](/images/bit-flip-3.png)
_We have the auth code for the user "admin" we know this because of the line "You are currently logged in as admin!"_


Now we can replace our cookie from our browser using your favourite cookie manger. i use "Cookies Manager +" in Firefox.   

![img-description](/images/bit-flip-4.png)
_We've now logged in as admin_

we have sucessfully logged in as admin.



