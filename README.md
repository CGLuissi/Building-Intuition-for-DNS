<p align="center" width="100%">
    <img width="33%" src="[https://i.stack.imgur.com/RJj4x.png](https://imgur.com/a/ras6XJH)">
</p>

<h1> Building Intuition for DNS </h1>
This tutorial showcases DNS and a few of its use cases.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- DNS

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>


NOTE:This tutorial is a continuation of the previous Active Directory Lab, and needs those steps to be taken to function correctly.


1.) To start off this experiment, we are going to create a type of DNS resource record called an A record. A records are used to create an association between an IPv4 address and a domain name. As an example, if you create an A record of "homepage" and map it to the IP address 192.168.1.1, then traffic will be delivered to that IP address when you ping  "homepage". If you are not signed into both DC-1 and Client-1 with the Username "mydomain.com\jane_admin", then do so now.

<img width="468" alt="DNS step 1" src="https://github.com/CGLuissi/Building-Intuition-for-DNS/assets/143234913/8ea0ea1b-8681-45e6-af50-b377b48cc313">


<img width="434" alt="DNS step 2" src="https://github.com/CGLuissi/Building-Intuition-for-DNS/assets/143234913/c31e0479-030c-4382-aa5c-5ac3e5a5c597">

From Client-1, open command prompt and attempt to ping "mainframe". When we ping mainframe, we can see that it fails, as there is no host (A record) by that name. In the DNS archives within server manager, we can see the A Records for DC-1 and Client-1 along with their IP addresses, but none for mainframe. We can also search for the mainframe by typing the command "nslookup mainframe" to try and open a DNS query into a given domain name or IP address. There is no IP address or domain associated with that name yet, so we will have to create an A Record for mainframe. Right click on empty space anywhere and select Create New Host (A or AAAA). 

<img width="221" alt="DNS Step 5" src="https://github.com/CGLuissi/Building-Intuition-for-DNS/assets/143234913/1125a4a1-60b5-40db-be19-f1e2ff242514">

Put mainframe as the name and DC-1's private IP as the IP.It will create, and when pinged or queried with nslookup, mainframe will return the IP address of DC-1. 


<img width="348" alt="DNS Step 6" src="https://github.com/CGLuissi/Building-Intuition-for-DNS/assets/143234913/e2384e45-b31e-4e21-ac5e-a9f83134aff5">

  
2.) All of the queries and commands we have made so far are stored as part of the "DNS Cache" that is used to speed up DNS queries by looking through stored results. We can observe the way that the DNS cache works by changing the IP of mainframe and attempting to ping it again. In DC-1, use server manager to change the mainframe IP to 8.8.8.8. Ping mainframe from Client-1 and notice that it still returns the IP address of 10.0.0.4. This is because the DNS cache has not been updated. You can erase and update the cache by "flushing" it. Type the command ipconfig /flushdns. Ping mainframe again and look at the address that appears. One important thing to note is that the "TTL", or Time-to-Live value shows how long an entry stays in the cache before being automatically erased.


<img width="298" alt="DNS Step 7" src="https://github.com/CGLuissi/Building-Intuition-for-DNS/assets/143234913/dd0474ca-7f7d-4fe0-bfef-733f1ad56aee">




<img width="338" alt="DNS Step 8" src="https://github.com/CGLuissi/Building-Intuition-for-DNS/assets/143234913/8ef15361-956d-4fce-b9b7-b4ce2d456f39">

  
3.) In the previous step, we created a hostname and mapped it to an IPv4 address; we are now going to create a CNAME (Canonical name) record and observe how it connects an alias or nickname to a domain. In DC-1, right click empty space in DNS Manager and create a new CNAME. In the fully  qualified domain name box, enter www.google.com. For the alias, enter "search". Use the command "Nslookup search" and look at the results. What this served to do was make it so that we can use the "search" CNAME record to route traffic to Google.com. This function is useful for allowing you to reach websites or domains through alternative means: "www.Google.com" is a viable way to reach "Google.com" as the "www." segment  serves as a CNAME record.

