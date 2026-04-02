Investigating a Windows machine that has been previously compromised.

When I started the Windows machine, I saw that Windows is connecting to an IP adress following below.</br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/a1cdb6f68529b910a808ddf183beb3283839a188/03%20Investigating%20Windows/src/1-additional.png) </br>

Also, when I exited the connection terminal first, what I did was find who I am. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/a1cdb6f68529b910a808ddf183beb3283839a188/03%20Investigating%20Windows/src/2.png) </br>

First, when investigating the OS, I needed to know which version of Windows it is. Here it says Windows Server 2016. </br> 

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/d335215b1db61ff0f40e1d60a92e49946a6a276d/03%20Investigating%20Windows/src/3.png) </br>

Here I saw that only 2 accounts have logged on, and the last one that was logged on is the Administrator (It was me) and I also see that John was last logged on in 2019 (suspicious). </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/d335215b1db61ff0f40e1d60a92e49946a6a276d/03%20Investigating%20Windows/src/4.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/d335215b1db61ff0f40e1d60a92e49946a6a276d/03%20Investigating%20Windows/src/5.png) </br>

Also, here we see the users who have administrator privileges. Those are Jenny and the guest beside the user Administrator. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/d335215b1db61ff0f40e1d60a92e49946a6a276d/03%20Investigating%20Windows/src/6.png) </br>

Now, name of the scheduled tasks that are malicious, here I get the Idea which is the Windows OS to see the scheduled tasks that are not in the Microsoft route. Many of them are malicious. One of them is "Clean system info", also flashupdate22 is where additional 22 is customised because he has an original file called "flashupdate" and "falshupdate" where there is a typo. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/d335215b1db61ff0f40e1d60a92e49946a6a276d/03%20Investigating%20Windows/src/7.png) </br>

For falshupdate I see -WindowStyle hidden, also "update windows" in Internet Explorer is very obvious because WindowsUpdate is in Windows\temp\...  

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/43a88a3f033e40a2fd313de917637340354fe3aa/03%20Investigating%20Windows/src/ad-1.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/43a88a3f033e40a2fd313de917637340354fe3aa/03%20Investigating%20Windows/src/ad-2.png) </br>

Here I found 2 ways to identify the file that was trying to run daily. (nc.ps1) </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/73d266148d9e7da426125e285ff1d11acd4c2272/03%20Investigating%20Windows/src/8.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/73d266148d9e7da426125e285ff1d11acd4c2272/03%20Investigating%20Windows/src/ad-3.png) </br>

So next thing I did is found the port where this file (nc.ps1) was listening. </br>
In the next pictures, is where I tried to find it in many ways. </br>
First tried to find the route of the nc.ps1 file (c:\TMP\) </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/9.png) </br>

Tried to find it in the content of the file "nc", see for patterns because -p is an alias for port, and netstat is used to see where the machine is connected to. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/9.2.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/9.3.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/9.4.png) </br>

Helpless at this point, but I remembered that the scheduled task "Clean file system" has an action nc.ps1, so I opened the scheduled task. Opened task "Clean file system" and found the port in the description. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/10.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/11.png) </br>

Also, from the PowerShell command terminal. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/ad-3.png) </br>

Now I see if Jenny has the last logon. Never. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/12.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/13.png) </br>

During the compromise, Windows first assigns special privileges to a new logon here. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/14.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/15.png) </br>

So I know that he was able to go on this machine. Now I need to know how did he get through, so I go where is nc.ps1 in the TMP folder and see a weird name with .exe extension. This is the most popular open-source post-exploitation tool that extracts plaintext passwords, password hashes, and PIN codes directly from Windows system memory. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/16.png) </br>

Attackers external control and command servers' IP, in the following pattern of an IP adresses but with no success. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/18.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/19.png) </br>

I found it in DNS cache, how i know this is the right IP adress? Basically is without a name. If it is legitimate, it would have a name. [76.32.97.132]

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/20.png) </br>

The extension name of the shell is uploaded via the server's website. Is .jsp extension. I found it on wwwroot, which is designed for everyone who can access an IP address and a route. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/21.png) </br>

Last port that the attacker oppened is 1337. </br>

![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/22.png) </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/23.png) </br>

DNS poisoning, google.com site was targeted  </br>
![image_alt](https://github.com/adin991/Penetration-testing-CTF/blob/50959e1713e8cca2b74266de32b48aa7887a96a3/03%20Investigating%20Windows/src/24.png) </br>



