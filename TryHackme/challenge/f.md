![image](https://github.com/user-attachments/assets/4bdb49c4-6535-416f-ae4d-08427f93569e)

When we look at the description we understand that in this challenge we need escalate privilege that is get root level access.
![image](https://github.com/user-attachments/assets/42cea0b7-7041-4e28-ac99-d8118c64b370)

First question is about getting user.txt file so need to get access to the user account.
Step 1: We open the target machine.
![image](https://github.com/user-attachments/assets/0014600f-aebe-413d-a4ef-df414c0fe829)

First question is about getting user.txt file so need to get access to the user account.
Step 1: We open the target machine
![image](https://github.com/user-attachments/assets/d09deff1-c056-4d82-81f6-ac14a253367c)

Step 2: We click on c0ldd link to check what it is a twitter profile opens which seems of no use
![image](https://github.com/user-attachments/assets/e693a820-64cd-4f95-ab31-ab41c018842e)

Step 3: We now go back on first page and open page source file but there also we don’t find anything useful to get user access.

Step 4: We need to check ports open on the given machine we use nmap scan
nmap -p- --open -T4 -Pn <ip addresss>
![image](https://github.com/user-attachments/assets/7b8d7918-c660-4cae-b14d-c97cdea3e7ee)

We got info that 2 ports are up one on 80 and other on 4512

Step 5: To see detail about these port we run another command
nmap -sC -sV -p80,4512 <ip addresss>
![image](https://github.com/user-attachments/assets/56310508-e676-4dfa-80e8-7844d2f6bb0d) 
We now get detailed information about what is running and on which port 
80- http – apache with version 2.4.18
4512- ssh – openssh 7.2p2

Step 6: We use  gobuster to see hidden directories and information

![image](https://github.com/user-attachments/assets/6a37d61c-e8f1-4965-b0d1-d9faa62f2db3)
![image](https://github.com/user-attachments/assets/5b649b0c-6f28-47d1-ab68-bf398f10c854)

We got all this info among this we got hidden directory and we open it first.

Step 7: open http://ip-address/ hidden/

![image](https://github.com/user-attachments/assets/7f11547d-1ce0-4577-9c6f-e053e2ed0404)

When we open hidden directory we got this info but it is not clear enough we just feel C0ldd, Hugo, Philip , U-R-G-E-N-T and something related to password is there.We even check source file but we don’t get any meaningful information.
![image](https://github.com/user-attachments/assets/600718f1-6ee2-4984-a51a-82f9c0bb1518)

Step 8: open http://ip-address/ wp-admin/ page to check any info

![image](https://github.com/user-attachments/assets/46225569-6542-4dbc-852b-99a42eceb5c1)

Here we got login page we checked its source also we don’t get anything but this login page can be used we get any username and password , we check all other http links from gobuster command but they didn’t have any information

Step 9: We go back to first page, We just get this info by looking at the website and source page that it is wordpress website 

![image](https://github.com/user-attachments/assets/eb4a5c9c-68e9-46df-b55f-8cd7d0ec8a75)

we can use wpscan to scan in wordpress website
wpscan --url http://ipaddress --enumerate will scan the site and give us more information about the WordPress CMS.
How this works:
wpscan – The command to execute WPScan.
–url – Target URL.
–enumerate – Tells WPScan to scan the site to learn about plugins, themes, configs, users and other info.
![image](https://github.com/user-attachments/assets/8aa8b5cc-9362-4749-9fb1-6c7523b6c58e)
![image](https://github.com/user-attachments/assets/d97639b6-2877-4db6-bd58-fabe6082358a) 

We get information and identify users here like c0ldd, hugo, Philip, the cold in person

Step 10: Tried to check c0ldd is user or not by putting a default password passwd123. Got response as below

![image](https://github.com/user-attachments/assets/e07d9cd6-a464-422f-a894-f343910050ff)

It clearly shows username c0ldd is correct password is wrong we need to find the password. Similar response got for other users as well.
 
 
Step 10:
We can use WPScan to see if we can find passwords for these users and log in with that password 
Running the command:
wpscan --url http://10.10.170.248 --usernames philip,hugo,c0ldd --passwords /usr/share/wordlists/rockyou.txt 
This begins the brute force attack to see if we can find a password for these users.
How this works:
wpscan – The command to execute WPScan.
–url – Target URL.
–usernames – Users that we want to attack.
–passwords – List of passwords to use in the brute force attack. In this case we are using the famous rockyou.txt file.

 
This takes a lot time, I stopped it earlier as I found password for one user.
 
We found the password for cOldd now we will use it and check.
Step 11: Again open http://<ip-address>/wp-admin/ page and give username as C0ldd and password as 9876543210 and click login.
 
. 
 
We now have access to the dashboard. Here we can look around to get more information about our target.
Step 12: On left menu click on users and select all users and open that list
All users get displayed with their role as below.

We get to know that only c0ldd has administrator access rest are editor
Step 13: I explore further and see in appearance tab we have editor which is enabled. This is a great finding for us. We open it and see php files can be updated.
 
we can easily drop in PHP code to perform a reverse shell.
 (Note:-Anyone running a PHP site needs to disable this as it’s very easy to abuse)
Kali Linux has PHP Reverse Shell scripts located in /usr/share/webshells/php/. The file is named php-reverse-shell.php. 
 
Step 14: I copy this file in my present working directory. I copy in downloads
Running the command:
cp /usr/share/webshells/php/php-reverse-shell.php .
 
 







Step 15: Open it with a text editor and we need to update two values. The $ip and $port.
 
The $ip will be the IP Address of our attack machine kali linux. Since we are using the TryHackMe VPN, let’s look for tun0 and use that IP! We can find the IP Address with the command ip addr (which displays network interfaces).
 
 The $port will be our listening port for the reverse shell. Let’s use 1234 as it’s out of the range of the known ports.
 
To save the changes Ctrl+O enter ctrl+X

Step 16 : let us now start listener with Netcat.
Running the Command: nc -lvnp 1234 starts a Netcat listener
What this does
nc – Executes Netcat.
-l – Listen for a request.
-v – Verbose output.
-n – Do not use DNS.
-p – What port to listen on.
 
Now that we have the script set up and the listener going. We need to copy and paste it to the editor to get the reverse shell to fire. We also need to pick a page to use that we can navigate to in the web browser.
Step 17: I go on media on wordpress page and click on add new file and select php-reverese-shell-php.php file and open there but I get below message.
 
Step 18: I go to appearance and click on editor 
 
On right click on 404template to open 404 php file I find this is editable so I paste my php file code here and click on update file after which I get a message file successfully updated.
Step 19: I now open a wrong page like http://ip-address/p=3
It gives 404 not found error I go and check my listener now.
 
But there are no changes there.
Step 20: Click on plugins page and then click on add new plugin
 
 
Click on upload plugin
 
Click on browse and go in downloads and select the php file.
 
Click on install now.
 
You will get above error.
Step 21: Now search http://ip-address/wp-content/uploads
 
Click on 2025 folder , then click on 06/ folder.
 
Now click on latest php-reverse-shell3.php and check your  listener it appears as below
 
GREAT! we have successfully performed the reverse shell and got the shell running in our terminal.
Step 22: Now run  command whoami to know user.
 
Step 22: Now run  command cd /var/www/html and then ls to know all the files
 
Step 23: Now run command cat wp-config.html because config usually have credentials about database and that can be the password for ssh.
 
 
 
Here we got a password cybersecurity.
Step 24 : We now run ssh command in new terminal
Ssh port number is 4512
ssh -p 4512 c0ldd@ip
 
 
Step 25: Now give command ls and we see the user file and open it using cat 
 
We get our first flag and we copy and paste it.
Now we to find root.txt file.
 
Step 26:Now we tried to check do we have sudo rights or not 
 
Step 27: We have sudo rights so we use below command sudo vim -c ':!/bin/bash'
And after getting root access we give ls command and here we get the user.txt file which was our first flag
 
Now we go in root directory by cd /root and type ls here and we get our root.txt file.
 
We use cat to open this file and we get our second flag here.
 


