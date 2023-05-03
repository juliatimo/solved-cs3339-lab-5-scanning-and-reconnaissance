Download Link: https://assignmentchef.com/product/solved-cs3339-lab-5-scanning-and-reconnaissance
<br>
<h1>Introduction</h1>

<strong> </strong>The key to successfully exploit or intrude a remote system is about the information you have. The first step for penetration is the scanning and reconnaissance. In this lab, you will learn how to use tools to scan and retrieve information from a targeting system. You will be using <em>nmap </em>and <em>OpenVAS </em>to scan a vulnerable machine and identify exploits that can be used to attack it. We will use two Linux virtual machines: One is a Kali Linux with <em>nmap </em>and <em>OpenVAS</em>; and the other one is intentionally vulnerable Linux. We will use the <em>nmap </em>and <em>OpenVAS </em>on Kali Linux to scan the vulnerable Linux machine.







<h2><strong>Software Requirements </strong></h2>

<strong> </strong>

<ul>

 <li>VirtualBox <a href="https://www.virtualbox.org/wiki/Downloads">https://www.virtualbox.org/wiki/Downloads</a></li>

</ul>




<ul>

 <li>The Kali Linux, Penetration Testing Distribution from LAB 1</li>

</ul>




<ul>

 <li>Metasploitable2: Vulnerable Linux Platform <u>https://s2.smu.edu/~rtumac/cs3339/Lab3/Metasploitable2-Linux.ova</u> <a href="https://sourceforge.net/projects/metasploitable/files/Metasploitable2/">http://sourceforge.net/projects/metasploitable/files/Metasploitable2/</a></li>

</ul>




<ul>

 <li>nmap: the Network Mapper – Free Security Scanner <a href="https://nmap.org/">https://nmap.org/</a></li>

</ul>




<ul>

 <li>OpenVAS: Open Vulnerability Assessment System <a href="http://www.openvas.org/index.htm">http://www.openvas.org/index.htm</a></li>

</ul>

<h1>Starting the Lab 3 Virtual Machines</h1>

We need to use two VMs for this lab: the Kali Linux and the Metasploitable2-Linux. First, import the new Kali Linux VM and start the machine.




Login the Kali Linux with username root, and password CS3339. Below is the screen snapshot after login

Then, import and start up the <strong>Metasploitble2-Linux</strong> virtual machine. This is an intentionally vulnerable Linux VM that you will attack against.




Log into the virtual machine with username, msfadmin, and password msfperunaadmin.










After you log into the VM, you will see the screen below.




<h1>Finding the IP Address of the Attacking Target</h1>

<strong> </strong>

For the purpose of this lab, it uses Metasploitable2-Linux as the attacking target. First, we need to find the host IP address of the target to launch a scan. You can use the command “ifconfig” (ipconfig is the windows equivalent). This command allows you to find all the connected interfaces and network cards.

Go to the Metasploitable2-Linux VM, and execute the following command

<h2>$ ifconfig</h2>

<em> </em>

<em> </em>

<em> </em>

From the screenshot above, we can see that the IP address of the network interface, eth0, is <strong>172.16.108.172</strong>. This is the IP address for the target that you will use later in this lab. When you work on the lab in the classroom, you will get a different IP address for your Metaploitable2-Linux VM. Note that this is not a public IP but we can access it within the subset.













<h1>Scanning the Target Using nmap</h1>

<strong> </strong>

<strong>nmap </strong>(“Network Mapper”) is an open source tool for network exploration and security auditing. Though it was designed to rapidly scan large networks, we use it for scanning the target host in this lab.







Since nmap has been installed on the Kali Linux, we can just launch the scanning in the terminal by typing the following command:

<em>$ nmap -T4 172.16.108.172 </em><strong>nmap </strong>is the execution command; option <strong>-T4 </strong>means faster execution; and

<strong>172.16.108.172 </strong>is the IP address of the target. As mentioned, you will have a different

IP    address    when    working    on    this    with    the    VMs    in    the classroom.







The screenshot above shows a quick scan of the target machine using <strong>nmap</strong>. We can see that there are many open ports and services on the target system including FTP, SSH, HTTP, and MySQL. These services may contain vulnerabilities that you can exploit.

<strong>nmap </strong>provides many useful functions that we can use. You can find more information from the man page of <strong>nmap </strong>

From this link: <a href="https://linux.die.net/man/1/nmap">http://linux.die.net/man/1/nmap</a>

Or execute the following command in a terminal:

<h2>$ man nmap</h2>

<em> </em>

<em>  </em>

The screenshot above shows the man page of <strong>nmap</strong>.

<h1>Vulnerability Scanning Using OpenVAS</h1>

<strong> </strong>

OpenVAS is an open-source framework of several services and tools offering a comprehensive and powerful vulnerability scanning and vulnerability management solution. If OpenVAS is not already installed, you can follow these steps:

<h2><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="02706d6d764269636e6b">[email protected]</a>:~# apt update <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="bac8d5d5cefad1dbd6d3">[email protected]</a>:~# apt install openvas <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e5978a8a91a58e84898c">[email protected]</a>:~# openvas-setup</h2>

<strong>REMEMBER THE PASSWORD given to you when you run openvas-setup. If you don’t remember the password, you can reset it by running the following command: </strong>

<em><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="74061b1b00341f15181d">[email protected]</a>:~# openvasmd –user=admin –new-password=new-super-secure-pass </em>




You can run the following command to check if the OpenVAS manager, scanner, and GSAD services are listening:

<h2><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="8dffe2e2f9cde6ece1e4">[email protected]</a>:~# netstat –antp</h2>

Otherwise, just start the services by executing the following command <em><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="d5a7babaa195beb4b9bc">[email protected]</a>:~# openvas-start</em>

<h2><strong>Connecting to the OpenVAS Web Interface </strong></h2>

<strong> </strong>

<strong> </strong>

Go to the Kali Linux, and open the browser




Then, go to <u>https://127.0.0.1:9392</u> and accept the self-signed SSL certificate.







Input the username as admin, and the password given to you when you ran <strong>openvassetup</strong>

The following screenshot is the homepage of OpenVAS. Navigate to Scans -&gt; Tasks. Then, open the Task Wizard (screenshot on next page). Type the IP address of the target in the Task Wizard, and press “Start Scan”. It will do the following for you:

<ol>

 <li>Create a new Target with default Port List</li>

 <li>Create a new Task using this target with default Scan Configuration</li>

 <li>Start this scan task right away</li>

 <li>Switch the view to reload every 30 seconds so you can lean back and watch the scan progress</li>

</ol>













After finishing the scanning, you can look at the reports (Scans -&gt; Reports) as shown in the screenshot below.
















<h1>Assignments for the Lab 3</h1>

<ol>

 <li>Read the lab instructions above and finish all the tasks.</li>

 <li>Use nmap to scan the target and find the software version of the OS and the running services (post a screenshot).</li>

 <li>Use OpenVAS to find two vulnerabilities of the target, and briefly describe them. Post a screenshot with the list of vulnerabilities found by OpenVAS.</li>

</ol>




<h1>Happy Scanning</h1>