java cLab Setup
This lab assignment exercise will be conducted in your own PC/ laptop using VIRTUAL machines. The images 
of required VMs can be downloaded with the following links.
• Kali (Attacker’s Machine, ~2GB): https://www.offensive-security.com/kali-linux-vm-vmware virtualbox-image-download/
• Windows 2000 (Victim1, ~450MB): https://mycuhk my.sharepoint.com/:f:/g/personal/1155155509_link_cuhk_edu_hk/EjWaVW0nm1VKtlLJ_YrruYMBJj
RVuYHfW7VhTDtiIqrx9A?e=81JM9V
Download password: ierg4130
• Lubuntu (Victim2, ~500MB): https://sourceforge.net/projects/osboxes/files/v/vm/33-L bu/14.04/14.04.3/144364.7z/download
The images we provided are for VMWare, so we recommended you using VMWare Player to load them.
• For Windows, you can install VMWare Player (available at
https://softwareupdate.vmware.com/cds/vmw-desktop/ws/
) to switch on all three virtual machines. 
• For MacOS, you can install VMWare Fusion Player (available at 
https://softwareupdate.vmware.com/cds/vmw-desktop/fusion/
). You may need to register a free personal license first.
After installation of the VMWare Player, please load three downloaded VM images into it. This can 
be done by simply drag-and-drop the .vmx files into VMWare.
You may also want to rename these VMs by right-clicking on their names on the left panel.
By default, we assign (2G, 2G, 256M} memory to (Kali, Lubuntu, Win2K). If your computer has 
limited memory space (<8G), you may want to adjust the virtual memory assignment. For 
example, you can reduce Lubuntu’s memory to 512M. We recommend assigning 2G to Kali.
Then, launch all three VMs and keep them running. You should be able to see login screen for 
each of them. Below is a screenshot of the VMWare Workstation on Windows after the setup.
Overview
In this lab exercise, you are going to examine some cyber hacking scenarios and counter-measures in the 
Internet. They include:
● Password sniffing
● Redirection attack
● Hacking exercise
The network topology used in this laboratory session is shown below. The machines are installed as virtual 
machines and they are connected as a network. 
 
You will mainly operate on Attacker’s machine (marked [Kali]). Some operations need to be done 
on Victim2’s machine (marked [Lubuntu]). You don’t need to log into Victim1 (Windows 2000)
during the whole lab.
Machines’ account information (username/password) are listed below:
- Attacker (Kali): kali/kali
- Victim1 (Lubuntu): osboxes.org/osboxes.org
Lab Preparation:
Before the following tasks, please find the IP addresses of the three virtual machines and they will be used 
afterwards. 
Hints: you may use ifconfig or run nmap or arp in Kali (You can search these terms online for more 
information)
1. Password Sniffing Experiment [15 Marks] 
You should only operate on [Kali] to complete the task
In this experiment, we will examine the HTTP User Authentication schemes [1] and see the levels of 
protection as well as the various security handshake mechanisms provided by these schemes. 
Using the Mozilla Firefox web browser, browse to the following web pages:
● http://ierg4130.minetest.land/a1.php
username: ierg4130 password:ierg4130_basic
● http://ierg4130.minetest.land/a2.php
username: ierg4130 password:ierg4130_digest
Use Wireshark [3] to capture the packets that are exchanged between the web browser and the web 
Attacker
Kali
Victim1
Windows 2000
Victim2
Lubuntu
server. To run Wireshark, open a terminal and type the following command: wireshark. After starting 
the tool, you shall see the application as follow:
Analyze the contents of the captured packets and answer the following questions:
Q1-1 [Kali] Using Wireshark, can you find out the username and password that were used to login to 
the two web pages?
Q1-2 [Kali] After the web server has verified the username/password, and sends back the webpage, is 
the content of the webpage encrypted?
Q1-3 [Kali] Identify the type of authentication mechanisms used in the two webpages and explain 
their differences.
2. ARP Poisoning Experiment [25 Marks]
Address Request Protocol (ARP) is a routing protocol for translating destination IP addresses to the 
MAC address of the network interface. ARP poisoning or ARP spoofing is an attack to remap the default 
router IP address to the attacker’s MAC address. By doing this, attacker can force all LAN traffics 
passing through to his own machine. Thus, he can be the man in the middle (MITM). 
Q2-1 [Lubuntu] Check the IP to MAC binding with the command arp -a? Include a screenshot in your 
report. What is the IP and MAC of the router and the attacker’s machine? (Hint: you can use the 
command route to find out router’s IP address)
Q2-2 [Kali] Ettercap is a MITM attacking tool. Search it online for tutorials of Ettercap or read [4] to learn 
its basic usage. Open in the Kali and conduct ARP poisoning attack to sniff traffics from/to Victim2. Take 
screenshots and describe the steps you took. Note that you can choose to use either the graphical 
interface or command line of Ettercap.
Q2-3 [Lubuntu] Repeat Q2-1 again. Can you see the difference in the output of arp -a? Take 
screenshots and explain what happened in your report.
Q2-4 On [Lubuntu], visit the two URLs in Q1. At the same time, on [Kali], use Wireshark to monitor 
traffics as you did in Q1. Can you see the requests and credentials sent from Victim2 (Lubuntu) on 
Attacker’s machine? Include screenshots in the report.
3. DNS Spoofing Experiment [25 Marks] 
Before getting data from a web service, we need to request DNS server for the IP address from the 
domain address of that web service. Client will then connect to the web server via returned IP address. 
DNS Spoofing is an attack to send the client with the malicious IP address so that they will connect to 
the malicious website instead of the legitimate one. 
In this session, we, as the attacker, will create our own fake website and use DNS spoofing to attack 
Victim2. We will also examine the mechanisms to distinguish between real and fake websites. 
We assume our target website is the official page of IE website: https://www.ie.cuhk.edu.hk/
[Kali] Let’s first create our fake webpage(https://www.hkbea-cyberbanking.com.) by cloning the bank
webpage on the attacker’s machine.
Q3-1 [Kali] Execute the follo代 写program、Python/c++
代做程序编程语言wing commands(without the “$”) line by line in the terminal to setup a web 
server on the attacker’s machine:
$ sudo a2enmod ssl
$ cd /etc/apache2/sites-available
$ sudo a2ensite 000-default
$ sudo a2ensite default-ssl 
$ sudo service apache2 restart
Q3-2 [Kali] Clone the target webpage into our web server’s folder with the command: 
sudo wget -k 'https://www.hkbea-cyberbanking.com/ibk/auth/web/login?Lang=Eng' -O 
/var/www/html/index.html
Now open a browser in Kali and visit http://localhost/. You should see a clone of the 
cyberbanking login page. Include a screenshot in the report.
Q3-3 [Kali] Check the IP address of the www.ie.cuhk.edu.hk using dig. What is its IP and IP of DNS 
server?
Q3-4 [Kali] Start Netwox and choose tool 105. Use this tool to spoof the DNS request of 
www.ie.cuhk.edu.hk with IP of attacker and name of authority server be ns1.ie.cuhk.edu.hk with IP of 
attacker.
Include the command used in the report. Also include screenshot of dig to prove the attack is 
successful.
Q3-5 Switch to [Lubuntu], visit www.ie.cuhk.edu.hk in Firefox browser. You need to clear all private 
data stored in the browser before re accessing the webpage. The browser should post a phishing website 
of the IE webpage. Include a screenshot of the webpage. State one method to prove that the webpage 
is not the real webpage.
Q3-6 [kali] Switch off the Netwox. Suggest one possible defense to this attack.
4. Hacking Experiment [35 Marks]
You should operate on [Kali] to complete the task
In this experiment, we will use different software attacking tools to perform network mapping and 
reconnaissance on victim machine to determine open ports and vulnerabilities, and then break into the 
victim machine. The victim machine is victim1. The attacking tools that will be used in this exercise are
● Nmap [7][8]
● Legion [6]
● Metasploit [9] [10]
Refer to the URLs provided in the Reference section for detailed usage and tutorials of the above 
attacking tools. The software has been installed in the attacker machine. 
For Nmap, you can run it by this command: “nmap”.
For Legion, you can find and run it in Kali’s menu.
For Metasploit, you can start by running “msfconsole” in the terminal.
The commands set and more details on the use of the metasploit console application can be referred to 
[11].
Tasks:
Try all means to break into Victim1’s machine (windows 2000) from Attacker’s machine (Kali).
After successfully breaking into the victim machine, you will need to search for a secret file in its 
filesystem, which is named "ierg4130.txt". Please include a screenshot of how/where you find it and the 
secret's content.
Q4-1 [Kali] Please provide the verification code in the report.
In addition, you have to describe the steps that you took to break into the victim machine, including the 
exploits and payloads that you used (including those you tried but failed to hack into). 
Q4-2 [Kali] Does the scanner or nmap report any vulnerabilities of the system? If so, list one of them
with their CVE-IDs, descriptions, and severities.
Bonus—Buffer Overflow Attack [35 Marks] 
To begin this experiment, students are required to: 
(1) learn about some advanced concepts in buffer overflow by reading the classical article:
“Aleph One, Smashing The Stack For Fun And Profit, http://insecure.org/stf/smashstack.html”
(2) run a virtual image in their own PC with the following files:
(i) RedHat 9 Image (285MB) – https://mycuhk my.sharepoint.com/:f:/g/personal/1155155509_link_cuhk_edu_hk/EiHHvCjhM1xArcOffif8R1oBzHUZgN
UzuRlog_5NcQnFYw?e=N1iACC
Download Password: ierg4130
You need username and password of course webpage to download the files.
After booting up the provided virtual PC, student should login using:
Username: hacker Password: hacker
which is for an account with no root privilege.
(You may need to understand the article in part 1 before doing it.)
I.Making shellcode:
In the folder /home/hacker/shellcode, there are three files code{0,1,3}.c. 
(1) Compile the code{0,1}.c and use gdb to view their assembly codes. Compare their assembly 
codes. What is the assembly code(s) of “name[0] = ‘/bin/sh’;”?
How long is the assembly code(s) in bytes? Please include the hex representation of this assembly 
code(s).
(2) Try to run code1. Can you get root privilege? Why or why not?
II.Understanding the exploit program:
In the folder /home/hacker, there is a file exp3.c which is the exploit program.
There are also binaries of two potentially vulnerable programs vul{1,2} and their corresponding 
source code files called vul{1,2}.c. We have already granted these programs escalated root 
privileges even when they are executed by a normal user like you.
(1) View the content of exp3.c and locate the shellcode. Why we need to include “setuid(0)” in 
the shellcode? 
(2) What is the meaning of the “addr” inside the code?
(3) Explain how to exploit a vulnerable program with exp3.c. Draw the diagram of stack allocation 
to help explain the idea. (You may use the similar diagram in tutorial for help.)
III.Exploiting the vulnerability:
For each potentially vulnerable program vul{1,2}, answer the following question:
Is the program exploitable, i.e. gaining root access? If yes, include the commands used and a 
screenshot of successful exploitation in report. If not, explain why it is not exploitable.
References
[1] J. Franks, P. Hallam-Baker, J. Hostetler, S. Lawrence, P. Leach, A. Luotonen, L. Stewart, RFC2617, HTTP 
Authentication: Basic and Digest Access Authentication, 1999. http://www.ietf.org/rfc/rfc2617.txt
[2] Authentication, Authorization, and Access Control http://httpd.apache.org/docs/1.3/howto/auth.html
[3] Wireshark network protocol analyzer, http://www.wireshark.org
[4] Ettercap Manual, https://miloserdov.org/?p=1772#1
[5] Ettercap for DNS Spoofing, https://null-byte.wonderhowto.com/how-to/tutorial-dns-spoofing-
0167796/
[6] Legion, https://govanguard.com/legion/
[7] Network Mapper, http://nmap.org/
[8] Nmap, http://en.wikipedia.org/wiki/Nmap
[9] The Metasploit Project official website, http://www.metasploit.com
[10] Metasploit project, http://en.wikipedia.org/wiki/Metasploit
[11] Metasploit User Guide, https://metasploit.help.rapid7.com/docs/manual-exploitation

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
