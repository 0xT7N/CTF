# CTF challenge name : Hammered
at the first we have to know something about what is the inside this logs 
<br> 1- /var/log/auth.log – Contains system authorization information, including user logins and authentication machinsm that were used, successful and failed attemps.
<br> 2- /var/log/apache/access.log is a log file that stores information about incoming requests
<br> 3- '/var/log/dmesg' file provides a snapshot of the kernel messages at boot time 
<br> 4- var/log/kern.log: stores information from the Ubuntu Linux kernel
<br> 4- /var/log/daemon  Daemon logs contains information about events related running the Linux operation.
<br> 5- /var/log/apt/term.log: store records of actions such as package installations and updates. , history of apt command
<br> 
<br>
![18](https://github.com/0xT7N/CTF/assets/75274517/f4db070a-9fa8-4b05-9051-5d9644e7e3b3)

# Q1 Which service did the attackers use to gain access to the system?
<br> the only log file will help us to know which service attacker used for gain access (auth.log) 
<br> because it Contains system authorization information, including user logins and authentication machinsm that were used.
<br> we will use this command to gain more info about it
<br> **cat auth.log | grep "Failed"**
<br> the attacker was been using brute force attack to gain access using **ssh service** 
![1](https://github.com/0xT7N/CTF/assets/75274517/de61e35a-d09e-476a-961c-b067dd30a177)

# Q2 What is the operating system version of the targeted system?
<br> in this **Q** we can recognition about the operating system with two ways 
<br> **first one** we will check dmesg file **( dmesg' file provides a snapshot of the kernel messages at boot time )**
<br> with command line **cat dmesg | less**
<br> we will see the the operating system version is **4.2.4-1ubuntu3**
![2](https://github.com/0xT7N/CTF/assets/75274517/ab8c46e9-d882-4deb-b2d5-f8464c95810a)
<br> **second one** we will check **kern.log**
<br> with command line **cat kern.log | less**
<br> we will see the the operating system version is **4.2.4-1ubuntu3**
![3](https://github.com/0xT7N/CTF/assets/75274517/72dd7471-dca1-49d6-9e0f-a16a58e45f05)

# Q3 What is the name of the compromised account?
<br> in this **Q** we need to know which accound was compromised so only log file will help us is **auth.log** because it 
<br>Contains system authorization information, including user logins and authentication machinsm that were used. 
<br> we will use command line **cat auth.log | grep 'Accepted password'** 
<br> so **root** account was compromised 
<br> how we detect it if we go back to this command **cat auth.log | grep "Failed"** 
<br> we will see the ip **219.150.161.20** 
<br>At Apr 19 05:38:37 gain Failed password more than one time that mean the attacker was been using brute force attack to gain access until 
<br> did it and compromised root account At Apr 19 05:55:20
![4](https://github.com/0xT7N/CTF/assets/75274517/39fbe868-933b-4d51-9f80-257c72d4d581) ![5](https://github.com/0xT7N/CTF/assets/75274517/6faeda17-cf32-4373-9f75-c0d7df791d23)

# Q4 Consider that each unique IP represents a different attacker. How many attackers were able to get access to the system?
<br> we need to know how many attackers were able to get access to the system so we need to know how many 
<br> ip get **Failed password for root** Then get **Accepted password for root**
<br> with comparing to commands we will get the number of attackers were able to get access to the system
<br> cat auth.log | grep 'Failed password for root' | awk '{print $11}' | sort | uniq -c
<br> cat auth.log | grep 'Accepted password for root' | awk '{print $11}' | sort | uniq -c
<br> we will see **6** 
![7](https://github.com/0xT7N/CTF/assets/75274517/a4000c6c-a0ab-48b3-aa3e-973b56447325)
![8](https://github.com/0xT7N/CTF/assets/75274517/d835b7c4-f4cb-4383-b12e-88953c57b970)
# Q5 Which attacker's IP address successfully logged into the system the most number of times
![19](https://github.com/0xT7N/CTF/assets/75274517/fea4215c-c0dc-4ccf-851f-bdb2d799c953)

# Q6 How many requests were sent to the Apache Server?
<br> to know how many requsests were sent to the apache server
<br> we will find it inside access.log because it is  **stores information about incoming requests**
<br> **wc -l** will count number of lines then we get number of requestes 
<br> command line **wc -l www-access.log**
<br>
![9](https://github.com/0xT7N/CTF/assets/75274517/f4ef7ebf-62a7-41ac-aceb-26c693be69e4)
# Q7 How many rules have been added to the firewall?
<br> Iptables is a standard firewall included in most Linux distributions by default.
<br> command line **cat auth.log | grep 'iptable'**
![10](https://github.com/0xT7N/CTF/assets/75274517/a9911f4a-ae42-44e2-96c7-5f170e200953)
 # Q8 One of the downloaded files to the target system is a scanning tool. Provide the tool name.
 <br> will help us in this **Q** to know which tool downloaded is **term.log** **( store records of actions such as package installations and updates.)**
<br> we will use this command **cat term.log | grep -A 5 -B 5 'install'**
<br> **nmap** is most popular scanning tools
![11](https://github.com/0xT7N/CTF/assets/75274517/dd6b0a54-a989-4412-af0c-d09a8a9b4649)

# Q9 When was the last login from the attacker with IP 219.150.161.20? Format: MM/DD/YYYY HH:MM:SS AM
<br> using this command **cat auth.log | grep 'Accepted password for root'** will know last login
<br> but we will not see the year get it from **auth.log properties**
![12](https://github.com/0xT7N/CTF/assets/75274517/40a70a82-2376-4003-a100-9740ec7e31cb)
![13](https://github.com/0xT7N/CTF/assets/75274517/2e6f48d3-74a6-4427-8382-89fa6f7cc454)
# Q10 The database displayed two warning messages, provide the most important and dangerous one.
<br> data about database will stored in **daemon.log** **(Daemon logs contains information about events related running the Linux operation.)**
<br> with command line **cat daemon.log  grep 'WARNING'**
![20](https://github.com/0xT7N/CTF/assets/75274517/1e2400a1-eed8-41ef-b807-83240e725b35)
# Q11 Multiple accounts were created on the target system. Which one was created on Apr 26 04:43:15?
<br> we will search in auth.log about new user with this command  **cat auth.log | grep 'new user' | grep 'Apr 26 04:43:1'**
![22](https://github.com/0xT7N/CTF/assets/75274517/fe1cb1fa-7603-4aa7-8e03-870c0a3135f5)

# Q12 Few attackers were using a proxy to run their scans. What is the corresponding user-agent used by this proxy?
<br>The HTTP CONNECT method starts two-way communications with the requested resource. It can be used to open a tunnel.
![17](https://github.com/0xT7N/CTF/assets/75274517/720ac577-e857-4f6b-a584-b1053036e848)

<br> command line **cat www-access.log | grep 'CONNECT'** 
<br>   ans  **pxyscand/2.1**
![16](https://github.com/0xT7N/CTF/assets/75274517/c34815dd-d407-4c8d-8f40-f038350d503f)



