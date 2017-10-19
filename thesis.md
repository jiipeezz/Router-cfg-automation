# Thesis - Automation of router configuration

Description text here

* Automation of router configuration
* Name: Joram Puumala
* E-Mail: a1500947@myy.haaga-helia.fi
* Supervisor: Tero Karvinen
* Haaga-Helia University of Applied Sciences
* Keywords: pytnon, vpn, configuration, router, automation

- - -

# Table of Contents

* 1 Introduction
    * 1.1 Background
    * 1.2 About this thesis
    * 1.3 Goals of the study
* 2 Router configuration
	* 2.1 Configuration management
		* 2.1.1 Configuration Management Systems
	* 2.2 Web Scraping
	* 2.3 Command line configuration
	* 2.4 Data manipulation with Regular Expressions
	* 2.5 Data conversion
* 3 Current state
* 4 Automating the process
	* 4.1 Automatic configuration
	* 4.2 Data extraction
	* 4.3 Data conversion
	* 4.4 Integration
* 5 Results
* 6 Future development
* 7 References
* 8 Appendix

# Introduction

## Background

Router orders are getting bigger, new customers and services are stirring the soup. NDC Networks is facing a serious problem, they are running out of resources. Router configuration by hand is no longer an option.

NDC Networks is a small Finnish company based in Espoo. The company is known for its expertise in networks, Virtual Private Network (VPN) management and router configuration. Routers that technicians at NDC Networks configure are mostly mobile routers. Different mobile routers do exist, but the basic idea is that it can be connected to mobile network using a traditional Subscriber Identity Module (SIM) card and is capable of changing its point of attachment to the Internet, moving from one link to another link (Ernst & Lach, 5). Because of the possible different features and demands though, configurations must be unique for each customer.

## About this thesis

## Goals of the study

Main goal of this study is to find out how to decrease used time and configuration errors in router configuration.

Research questions are:

- What is the best technology/technique to configure a router in this case and why?
- How much time can be saved, automation vs hand?
- How many fewer errors/misconfigurations will occur?

- - -

# Router configuration

Routers can be configured in different ways, depending on its model and manufacturer. Some routers have a fancy Web interface, while other routers can be configured only using text-based command line. Router configuration using a Web interface is usually pretty straightforward, since Web interfaces are designed so that even average Joes have some clue how to configure a router. Router configuration using a command line can be little trickier. Not only because some knowledge of how command line works is needed, but because commands may change radically between different router operating systems. For example, Cisco, which is dominating router market with share of 55.1% (IDC, 2017), has its own Cisco IOS operating systems with unique commands. Then again for instance, Huawei's routers run their own operating system, which means different commands apply when configuring the routers. 


> ![ciscohuawei](img/comparison.png)

> Fig. 1 - Some basic command comparison - Cisco vs Huawei


Since routers can be configured using a Web interface or a command line, the configuration process can also be automated. Vendors like Cisco allow scripting on the command line, so this is a giant first step towards automation. 



## Configuration management

## Web scraping

Extracting patterned data from web pages in the Internet is called Web scraping. There are different uses for Web scraping, one major use is for businesses to track pricing activities of their competitors. Using Web scraping techniques time and labor can be saved in massive amounts. Web scraping can prove itself useful in the realm of research as well (Haddaway, 1-2).

The basic idea behind a Web scraping framework is that it establishes communication with user defined Web page using the HTTP protocol, which is a stateless text-based Internet protocol designated to coordinate the request-response transactions between a client and a Web server, in which the client is typically a Web browser. The "User-Agent" header also plays a big role, because it tells the server whether it is trying to be accessed by a robot or a browser. Once the Web scraping framework has retrieved the HTML documents using GET method, contents of interest can be extracted. Because extracting the contents of interest is relevant, regular expressions alone or with a combination of additional logic prove to be powerful and thus are widely adapted. Alternative methods include selector-based languages such as XPath and the CSS selector syntax (Glez-Peña, ?).


> ![CSSselector](img/cssselector.png)

> Fig. 2 - Copying CSS selector - one of the most used selector-based languages


Web scraping has some downsides too. It is slower than a simple HTTP request to a web server, because browser waits until the whole Web page is loaded before it allows you to access its elements. What is more, the browser generates more network traffic, which is because of the supplementary files being loaded such as image files, JavaScript and CSS, yet they usually don't prove to be useful. One of the biggest issues is that Web pages do change. This may break your code and you have to fix it to make it work again.

## Command line configuration

## Data manipulation with Regular Expressions

## Data conversion

- - -

# Current state

There are currently lots of problems in NDC's router configuration. The greatest problem is that everything is done by hand using router's graphical web user interface. After having finished router configuration in the web GUI, Excel documents need to be updated with information such as router's serialnumber, MAC address, IP address and model, also manually by hand.


> ![Web Interface](img/smartflexgui.png)

> Fig. 3 - Routers' Web Interface


## The current process of router configuration:

1. Plug in the router
2. Browse to its default IP address
3. Log in using username and password
4. Click Restore Configuration
5. Click Add Configuration and browse to the right file
6. Click Add or Update
7. After confirmation takes you to another page, click Back
8. Click Services
9. Click SNMP under Services
10. Change SNMP name
11. Click Apply
12. Click Back
13. Click Change password
14. Type a new password twice
15. Click Apply
16. Click Back
17. Click User Modules
- Currently two user modules are added
18. Click Add new and browse to the right file
19. Click Add or Update
20. Click Back
21. Repeat 18.
22. Repeat 19.
23. Repeat 20.

This is the current process of router configuration by hand. The process includes lots of clicking and browsing, which obviously takes time. Also, chances are that the user does something wrong and it had to be debugged and fixed.

## The current process of updating Excel

1. Write router's VPN IP address
2. Write VPN IP address' netmask
3. Write router's serialnumber
4. Write router's MAC address
5. Write router's model
6. Write current date
7. Write sales reference

The Excel has to be updated after a router is configured, so this process is a part of the whole process.


> ![Excelfile](img/excelinfo.png)

> Fig. 4 - Excel file in which the information above is stored


- - -

# Automating the process

Configuring thousands of routers by hand is time consuming and tedious. We humans also make mistakes. The best way to get rid of possible misconfigurations and speed up the process is to automate it. Let computer do all the work. The automation program that will be written is going to be designed particularly for Advantech B+B's mobile routers, which are running a Linux operating system with BusyBox software embedded in it. Even though this automation program is designed for Advantech's mobile routers, the idea is that it can be used for other routers running a Linux operating system as well, with only minor changes.

> ![smartflex](img/smartflex.png)

> Fig. 5 - Advantech B+B's SmartFlex mobile router

> ![smartstart](img/smartstart.png)

> Fig. 6 - Advantech B+B's SmartStart LTE mobile router

The program will use a command line configuration technique over an SSH connection, which it initiates when the program is started. Language of choice is Python (3.5.2), because of its versatility, efficiency and simplicity. It will be a cross-platform program, which means it can be run in more than one operating system. Let's kick the tires!

## Functions and configuration order

Before we start writing the actual code, it is important that we know what we have to write and in which order. For example, router's new configuration file has to be in place before changing its SNMP name, because the new configuration file will overwrite SNMP settings including SNMP name.

Order of functions:
- 1. Initializing SSH connection
- 2. Fetching router's serial number and MAC address
- 3. Put new configuration file into router and run it
- 4. Change SNMP name
- 5. Add user modules
- 6. Change password

- Additionally, for each task we will write a function that confirms the success of configuration

## Initializing SSH connection to router

Firstly, we need to have a connection between our computer and the router we are going to configure. The router and our computer are connected with an ethernet cable. So, let's write the code that initializes the connection over SSH. We will use "paramiko" module for Python, which is a non-native module but can easily be installed using pip (instructions in Appendix).

```python
import paramiko

router_dflt_ip = "192.168.1.1" #default IP for the routers is always the same
uname = "root"
passwd = "Password3xample-"

ssh = paramiko.SSHClient()	#we define the ssh connection
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())	#this is to prevent program from crashing 
ssh.connect(router_dflt_ip, username=uname, password=passwd)	#we establish the connection between our computer and router
```

The command "ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())" is important here because it automatically deals with host keys and saves us from manual labour. Let's first remove the line and run the program.

> ![missinghost](img/missingkey.png)

> Fig. 7 - Running the program without "ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())" line

As we can see, "paramiko.ssh_exception.SSHException" error is raised. This is because we have a missing host key. Now let's run the original program.

> ![sshconnection](img/sshconn.png)

> Fig. 8 - No errors are raised this time

This time the program runs without raising any errors. We can suppose that the SSH connection was succesfully created. Next let's create a function that fetches the router's serial number, this will confirm that the connection is really working as expected.

## Fetching router's serial number and MAC address

To fetch a router's serial number, we need to know how to find it first inside the router. After messing around little on SmartFlex's command line, the serial number is found using "status -v sys" command. This command gives us way too much information though, so we are going to use grep and awk to get just what we want.

> ![systemstat](img/statussys.png)

> Fig. 9 - status -v sys command run on router's command line

Now let's add something to the command. First let's grep for Serial Number to get the correct line, after which we use awk's print function to print the correct column. The fixed command looks like this now "status -v sys |grep "Serial Number" |awk '{print $4}'".

> ![serialno](img/awked.png)

> Fig. 10 - This time, only serial number is printed out

Now that we know how to get router's serial number, let's write the function in Python.


```python
import paramiko

def get_serial():
	cmd = "status -v sys |grep \"Serial Number\" |awk '{print $4}'"	#command that prints serial number to standard output
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)	#executed command's output can be read from ssh_stdout
	outp = ssh_stdout.readlines()	#storing ssh_stdout in outp variable
	serial = outp[0].strip()	#serial number is the only item we have in tuple, and the tuple starts at 0
	return serial

router_dflt_ip = "192.168.1.1" #default IP for the routers is always the same
uname = "root"
passwd = "Password3xample-"

ssh = paramiko.SSHClient()	#we define the ssh connection
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())	#this is to prevent program from crashing 
ssh.connect(router_dflt_ip, username=uname, password=passwd)	#we establish the connection between our computer and router

serial = get_serial()	#these two lines are just to confirm that the function
print(serial)		#works as expected.
```

First we put the command that prints router's serial number into cmd variable. When the command runs, all of its output will be stored in ssh_stdout variable. Variable outp is used to store values of ssh_stdout in a tuple, after which we choose the first and only item that is the serial number. 


> ![serialno2](img/get_serial.png)

> Fig. 11 - Serial number is returned


It works as expected. Also, now it can be confirmed that we succesfully establish an SSH connection between our computer and router. Let's move on and create a very similar function, but this time MAC address of eth0 port will be returned. The MAC address of router's eth0 port can be found running "ifconfig eth0" command. This again, gives us too much additional information we want to get rid of, so we use grep and awk again.


> ![ifconfig](img/ifconfigeth0.png)

> Fig. 12 - Router's eth0 interface

Let's concatenate grep and awk to the original command The following command will do the job; "ifconfig eth0 |grep "HWaddr" |awk '{print $5}'".


> ![onlymac](img/awked2.png)

> Fig. 13 - Only MAC address of eth0 interface is printed out on the screen this time

So now we can proceed to write a Python function that fetches router's MAC address. Because the function will not differ that much from the get_serial() function, we can copy it and make minor changes.


```python
import paramiko

def get_mac():
	cmd = "ifconfig eth0 |grep \"HWaddr\" |awk '{print $5}'"	#command that prints mac address to standard output
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)	#executed command's output can be read from ssh_stdout
	outp = ssh_stdout.readlines()	#storing ssh_stdout in outp variable
	mac = outp[0].strip()	#mac address is the only item we have in tuple, and the tuple starts at 0
	return mac
	
router_dflt_ip = "192.168.1.1" #default IP for the routers is always the same
uname = "root"
passwd = "Password3xample-"

ssh = paramiko.SSHClient()	#we define the ssh connection
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())	#this is to prevent program from crashing 
ssh.connect(router_dflt_ip, username=uname, password=passwd)	#we establish the connection between our computer and router

mac = get_mac()	#these two lines are just to confirm that the function
print(mac)	#works as expected.
```

> ![onlymac2](img/pymac.png)

> Fig. 14 - Function get_mac() returns MAC address of router's eth0 interface

This time, when the Python program is run, MAC address is returned as expected. Now we have two working functions that import two values of high importancy. For example, later SNMP name will be changed to router's serial number and both values will be written to Excel file.

## Restoring router's configuration

This is the first phase in which the actual changes to router's current configuration are made. Every single router has its own unique configuration file. What makes the file unique are certificates and its VPN IP address. The VPN IP address is also used in the filename when it is created by NDC's server. For example, a typical configuration filename could be "customer_10.240.250.cfg". Now because the filename is unique for every router, it is a bad idea to put it inside the Python program. It would be time consuming and additional work to change it everytime before the program is run. So, instead of putting the filename inside the code, it will be given as a parameter, so that one number can easily be changed before re-running the program. To achieve this, "sys" module needs to be imported.

The syntax for the actual restore command inside the router is as simple as "restore <filename>". But before anything can be restored, the file has to be transferred to the router. Once the file is in the router, "restore <filename>" can be run. It is also a good practice to make sure that the command ran succesfully. If it did, "Configuration succesfully updated." will be printed to standard output. Now, it is possible to use this information to check whether everything went well or awry.


> ![restore](img/restorecfg.png)

> Fig. 15 - "Configuration succesfully updated." indicates success

Now all is pretty straightforward, so it can be put into the Python program.


```python
import paramiko
import sys

def restore_cfg(restore_file):
	orig = restore_f	#this is for clarity, file's origin (current dir)
	dest = "/root/" + restore_file	#destination of the file in router
	cmd = "restore " + dest	#restore command
	sftp = ssh.open_sftp()	#define sftp (secure version of ftp)
	sftp.put(orig, dest)	#put the file into router's /root/ dir
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd) #status message will be caught 
	success = "Configuration successfully updated."
	outp = ssh_stdout.readlines()
	status = outp[0].strip() #status message will be here
        if status == success: #if status message = "Configuration successfully updated."
                status_msg = "OK"
        else:
                status_msg = "FAILED"
        return status_msg

router_dflt_ip = "192.168.1.1"	#default IP for the routers is always the same
uname = "root"
passwd = "Password3xample-"
restore_file = sys.argv[1]	#first parameter given to program

ssh = paramiko.SSHClient()	#we define the ssh connection
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())	#this is to prevent program from crashing 
ssh.connect(router_dflt_ip, username=uname, password=passwd)	#we establish the connection between our computer and router

status = router_cfg(restore_file)
print(status)
```

> ![restorestatus](img/restorestatus.png)

> Fig. 15 - In the Python program, "OK" indicates success

In this case, the program returns "OK" message which translates into "Configuration succesfully updated.". First the file is transferred to the router's /root/ directory, after which the command "restore testcfg_10.240.254.cfg" is run. The status message is caught and later used to determine whether the command was succesful or not.

## Changing SNMP name

## Automatic configuration

## Data extraction

## Data Conversion

## Integration

- - -

# Results

- - -

# Future development

- - -

# References

- - -

# Appendix
