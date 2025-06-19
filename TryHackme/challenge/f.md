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


