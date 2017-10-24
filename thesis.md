# Thesis - Automation of router configuration

Description text here

* Automation of router configuration
* Name: Joram Puumala
* E-Mail: a1500947@myy.haaga-helia.fi
* Supervisor: Tero Karvinen
* Haaga-Helia University of Applied Sciences
* Keywords: python, configuration, router, automation, excel

- - -

# Table of Contents

* 1 Introduction
    * 1.1 Background
    * 1.2 About this thesis
    * 1.3 Goals of the study
* 2 Router configuration
	* 2.1 Configuration management
	* 2.2 Web Scraping
	* 2.3 Command line configuration
	* 2.4 Data manipulation with Regular Expressions
	* 2.5 Data conversion
* 3 Current state
	* The current process of router configuration
	* The current process of updating Excel
* 4 Automating the process
	* 4.1 Functions and configuration order
	* 4.2 Initializing SSH connection to router
	* 4.3 Fetching router's serial number and MAC address
	* 4.4 Restoring router's configuration
	* 4.5 Changing SNMP name
	* 4.6 Adding user modules
	* 4.7 Changing root password
	* 4.8 Getting backup file
	* 4.9 Catching errors
	* 4.10 Updating Excel
	* 4.11 Integration
* 5 Results
* 6 Future development
* 7 References
* 8 Appendix

# 1 Introduction

## 1.1 Background

Router orders are getting bigger, new customers and services are stirring the soup. NDC Networks is facing a serious problem, they are running out of resources. Router configuration manually is no longer an option.

NDC Networks is a small Finnish company based in Espoo. The company is known for its expertise in networks, Virtual Private Network (VPN) management and router configuration management. Routers that specialists at NDC Networks configure are mostly mobile routers. 

Different mobile routers do exist, but the basic idea is that it can be connected to a mobile network using a traditional Subscriber Identity Module (SIM) card and is capable of changing its point of attachment to the Internet, moving from one link to another link. Because of the possible different features and demands though, configurations must be unique for each customer (Ernst & Lach, 2007, 5).

## 1.2 About this thesis

## 1.3 Goals of the study

Main goal of this study is to find out how to decrease used time and configuration errors in router configuration.

Research questions are:

- What is the best technology/technique to configure a router in this case and why?
- How much time can be saved, automation vs hand?
- How many fewer errors/misconfigurations will occur?

- - -

# 2 Router configuration, management and techniques

Routers can be configured in different ways, depending on model and manufacturer. Some routers have a fancy Web interface, while other routers can be configured only by using text-based command line. Router configuration using a Web interface is usually pretty straightforward, since Web interfaces are designed so that even average Joes have some clue how to configure a router. Router configuration on a command line can be little trickier. Not only because some knowledge of how the command line works is needed, but because commands may change radically between different router operating systems. For example, Cisco, which is dominating router market with share of 55.1% , has its own Cisco IOS operating systems with unique commands. Then again, Huawei's routers run their own operating system, which means different commands apply when configuring the routers (IDC, 2017). 


> ![ciscohuawei](img/comparison.png)

> Fig. 1 - Some basic command comparison - Cisco vs Huawei


Since routers can be configured using a Web interface or a command line, the configuration process can be automated. Most vendors also allow scripting on the command line, so this is a giant first step towards automation. Sometimes that is not enough. There may be something that cannot be done or added via a configuration/script file, for example some modules need to be added by hand. This means manual labor and additional time, so the best shot is to automate it using external methods. There are different ways to automate such tasks. One way to do it is over an SSH connection, using Python for example. It is also possible to create an automation tool/robot that uses router's Web user interface. The latter technique is called Web scraping (Heydon & Najork, 1999).



## 2.1 Configuration management

Configuration management is a process to handle changes to a system over its life span, and keep it running maintaining its integrity. One of the most important things in configuration management is automation, as it is used to make a system to reach its desirable state. Automation has many benefits over manual configuration, since manual configuration practices are limited in many ways. For example, manual configuration is costly, time-consuming and unscalable. Now imagine having tens of thousands of network elements, and applying a new configuration to every single one by hand. It would be pretty much impossible and eat way too much resources. Also, manual configuration is prone to misinterpretations and errors. Engineering guidelines can be ambiguous, sometimes even imprecise and this leads to multiple interpretations. Many configuration management tools/systems exist. One should be chosen according to demands that has to be met. Examples of such systems could be Puppet, SolarWinds and SmartWorx Hub (Enck et al., 2007) (Heidi, 2016).

## 2.2 Web scraping

Extracting patterned data from web pages in the Internet is called Web scraping. There are different uses for Web scraping, one major use is for businesses to track pricing activities of their competitors. Using Web scraping techniques time and labor can be saved in massive amounts. Web scraping can prove itself useful in the realm of research as well. Web scraping is one way to create an automation tool. It requires a Web user interface for router configuration though (Haddaway, 2015).

The basic idea behind a Web scraping framework is that it establishes communication with a user defined Web page using the HTTP protocol, which is a stateless text-based Internet protocol designated to coordinate the request-response transactions between a client and a Web server, in which the client is typically a Web browser. The "User-Agent" header also plays a big role, because it tells the server whether it is trying to be accessed by a robot or a browser. Once the Web scraping framework has retrieved the HTML documents using GET method, contents of interest can be extracted. Because extracting the contents of interest is relevant, regular expressions alone or with a combination of additional logic prove to be powerful and thus are widely adapted. Alternative methods include selector-based languages such as XPath and the CSS selector syntax (Glez-Peña et al., 2014).


> ![CSSselector](img/cssselector.png)

> Fig. 2 - Copying CSS selector


Web scraping has some downsides too. It is slower than a simple HTTP request to a web server, because browser waits until the whole Web page is loaded before it allows you to access its elements. What is more, the browser generates more network traffic, which is because of the supplementary files being loaded such as image files, JavaScript and CSS, yet they usually don't prove to be useful. One of the biggest issues is that Web pages do change. This may break your code and you have to fix it to make it work again.

## 2.3 Command line configuration

Configuring a router via command line is the "traditional way", as it is the primary user interface still in use today. Every router has the command line option, but not every router has a Web user interface, which is one of the many reasons the command line is still preferred. Router configuration on a command line is basically just running a string of commands to change the way a router behaves. Configuration commands usually differ depending on manufacturer (Cisco, 2013).

As it was mentioned earlier, most manufacturers' routers has the option to write configuration scripts. Cisco is a good example. 
Cisco IOS scripting with Tcl is a popular thing, since it would make no sense to run commands one by one. To be able to write scripts for Cisco IOS, one needs to be familiar with Cisco IOS command line commands and Tcl programming. With a Linux based router, Bourne Shell (sh) or Bourne again Shell (bash) may be present, which allows so called Shell Scripting (Cisco, 2014).

Sometimes something external may be needed. For example, some configuration files and modules need to be transferred to router. Maybe some settings that couldn't be included inside the configuration file/script. A dream situation is that a configuration management system manages it later, unfortunately that's not always the case, and it is always a good practice to make all configurations that are needed as early as possible. Luckily, most routers ship with SSH (configurable or configured), and external scripting can be used over an SSH connection.

- - -

# 3 Current state

There are currently lots of problems in NDC's router configuration. The greatest problem is that everything is done manually using router's graphical Web user interface. After having finished router configuration in the web GUI, Excel documents need to be updated with information such as router's serialnumber, MAC address, IP address and model, also manually.


> ![Web Interface](img/smartflexgui.png)

> Fig. 3 - Routers' Web Interface


## 3.1 The current process of router configuration

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

This is the current process of manual router configuration. The process includes lots of clicking and browsing to files, which obviously takes time. Also, chances are that the person congifuring the router does something wrong and the process has to be started over. Even worse, an inperceptible mistake during the process happens and it has to be debugged and fixed later.

## 3.2 The current process of updating Excel

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

# 4 Automating the process

Configuring thousands of routers manually is time consuming and tedious. Humans also tend to make mistakes. The best way to get rid of possible misconfigurations and speed up the process is to automate it. Let computer do all the work. The automation program that will be written for NDC's case, is going to be designed particularly for Advantech B+B's mobile routers, which are running a Linux operating system with embedded BusyBox software. Even though this automation program is designed for Advantech's mobile routers, the idea is that it can be used for other Linux based routers, with only minor changes.

> ![smartflex](img/smartflex.png)

> Fig. 5 - Advantech B+B's SmartFlex mobile router

> ![smartstart](img/smartstart.png)

> Fig. 6 - Advantech B+B's SmartStart LTE mobile router

The program will use a command line configuration technique over an SSH connection, which it initiates when the program is started. Language of choice is Python (3.5.2), because of its versatility, efficiency and simplicity. Ideally, the program can be run on different operating systems, such as different Linux flavors and Windows versions. The program could also be made using a Web scraping framework, such as Selenium. This option can be deemed little dirty and code fragile, as it can break with minor changes to the Web interface. So, it is a better idea to stick with command line.

## 4.1 Functions and configuration order

Before writing the actual code, it is important to know what has to be written and in which order. For example, router's new configuration file has to be in place before changing its SNMP name, because the new configuration file will overwrite SNMP settings including SNMP name.

Order of functions:
- 1. Initializing SSH connection
- 2. Fetching router's serial number and MAC address
- 3. Put new configuration file into router and run it
- 4. Change SNMP name
- 5. Add user modules
- 6. Change password
- 7. Download backup
- 8. Update excel

- Additionally, for each task a functionality will be written to confirm the success of configuration. 

## 4.2 Initializing SSH connection to router

Firstly, a connection needs to be established between a configuring computer and a router. The computer and the router are connected with an ethernet cable. So, the first step is to write a code snippet that initializes the connection over SSH. Python module "paramiko" will be used, which is a non-native module but can easily be installed using pip (instructions in Appendix).

```python
import paramiko

#default IP for the routers is always the same
router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"

#defining the SSH connection
ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)
```

The command "ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())" is important here because it automatically deals with host keys. To demonstrate what happens without this line, it will be removed for a test run.

> ![missinghost](img/missingkey.png)

> Fig. 7 - Running the program without "ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())" line

As it can be seen, "paramiko.ssh_exception.SSHException" was raised. This is because there is a missing host key. Now running the original program should produce different result, because it knows how to deal with the host key.

> ![sshconnection](img/sshconn.png)

> Fig. 8 - No exceptions are raised this time

This time the program runs without any errors. This means the SSH connection was succesfully created.

## 4.3 Fetching router's serial number and MAC address

In order to fetch a router's serial number, the first thing is to know how to find it inside the router. Fortunately, command "status -v sys" exists. This command prints too much information though, so grep and awk can be used to get just what is needed.

> ![systemstat](img/statussys.png)

> Fig. 9 - status -v sys command run on router's command line

Now concatenating grep and awk to the command. First thing is to grep for "Serial Number" to get the correct line, after which awk's print function can be used to print the correct column. The fixed command is as follows, "status -v sys |grep "Serial Number" |awk '{print $4}'".

> ![serialno](img/awked.png)

> Fig. 10 - This time, only serial number is printed out to standard output

Knowing how to get router's serial number on the command line, it is time to integrate it with the python program.


```python
import paramiko

#function runs command inside cmd variable on router's command line
#standard output is read and the function returns serial number
def get_serial():
	cmd = "status -v sys |grep \"Serial Number\" |awk '{print $4}'"
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
	outp = ssh_stdout.readlines()
	serial = outp[0].strip()
	return serial

router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)

#these two lines below are just to confirm that the function
#works as expected.
serial = get_serial()
print(serial)

ssh.close()
```

First the command that prints router's serial number is put into cmd variable. When the command is run on router's command line, all of its output will be stored in ssh_stdout variable. Variable outp is used to store values of ssh_stdout in a tuple, after which the first item that is the serial number is chosen. 


> ![serialno2](img/get_serial.png)

> Fig. 11 - Serial number is returned


It works as expected. Also, now it can be confirmed that an SSH connection was succesfully established between the computer and the router. Another very similar function needs to be created, but this time MAC address of eth0 port will be returned. The MAC address of router's eth0 port can be found running "ifconfig eth0" command. This again, gives too much additional information, so  grep and awk will be used again.


> ![ifconfig](img/ifconfigeth0.png)

> Fig. 12 - Router's eth0 interface

Concatenation of grep and awk to the original command will do the job; "ifconfig eth0 |grep "HWaddr" |awk '{print $5}'".


> ![onlymac](img/awked2.png)

> Fig. 13 - Only MAC address of eth0 interface is printed out on the screen this time

So now a Python function that fetches router's MAC address can be written. Because the function will not differ that much from the get_serial() function, it can be copied with minor changes.


```python
import paramiko

def get_mac():
	cmd = "ifconfig eth0 |grep \"HWaddr\" |awk '{print $5}'"
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
	outp = ssh_stdout.readlines()
	mac = outp[0].strip()
	return mac
	
router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)

mac = get_mac()
print(mac)

ssh.close()
```

> ![onlymac2](img/pymac.png)

> Fig. 14 - Function get_mac() returns MAC address of router's eth0 interface

This time, when the Python program was run, MAC address was returned as expected. Now there are two working functions that fetch two values of high importancy. Both of the values will be used later in the program, for example, SNMP name will be changed to router's serial number. 

## 4.4 Restoring router's configuration

This is the first phase in which actual changes to router's current configuration are made. Every single router has its own unique configuration file. What makes the file unique are certificates and its VPN IP address. The VPN IP address is also used in the filename when it is created by NDC's server. For example, a typical configuration filename could be "customer_10.240.254.cfg". Now because the filename is unique for every router, it would be a bad idea to put it inside the Python program. It would be time consuming and additional work to change it everytime before the program is run. So, instead of putting the filename inside the code, it will be given as a parameter, so that one number in the file name can easily be changed before re-running the program. To give parameters to a python program, "sys" module needs to be imported.

The syntax for the actual restore command inside the router is as simple as "restore <filename>". But before anything can be restored, the file has to be transferred to the router. Once the file is in the router, "restore <filename>" can be run. It is also a good practice to make sure that the command ran succesfully. If it did, "Configuration succesfully updated." will be printed to standard output. Now, it is possible to use this information to check whether everything went well or awry.


> ![restore](img/restorecfg.png)

> Fig. 15 - "Configuration succesfully updated." indicates success

Now everything is pretty straightforward, so it can be put into the Python program.


```python
import paramiko
import sys

#takes one parameter, which should be configuration file
#sftp is used to transfer the file to router
#check is made if restore command was succesfully run, return OK or FAILED
def restore_cfg(restore_file):
	orig = restore_f
	dest = "/root/" + restore_file
	cmd = "restore " + dest
	sftp = ssh.open_sftp()
	sftp.put(orig, dest)
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
	success = "Configuration successfully updated."
	outp = ssh_stdout.readlines()
	status = outp[0].strip()
        if status == success:
                status_msg = "OK"
        else:
                status_msg = "FAILED"
        return status_msg

#first parameter given to program is restore_file
router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"
restore_file = sys.argv[1]

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)

status = router_cfg(restore_file)
print(status)

ssh.close()
```

> ![restorestatus](img/restorestatus.png)

> Fig. 16 - In the Python program, "OK" indicates success

In this case, the program returns "OK" message which translates into "Configuration succesfully updated.". First the file is transferred to the router's /root/ directory, after which the command "restore testcfg_10.240.254.cfg" is run. The status message is caught and later used to determine whether the command was succesful or not.

## 4.5 Changing SNMP name

SNMP settings are changed when the new configuration file is put into use. You may wonder why couldn't the SNMP name be updated to the configuration file just like the other settings? This is because SNMP name needs to be router's serial number and one doesn't know the serial number yet when the configuration files are created. It would be difficult to put this information into the configuration file, and wreak havoc if serial numbers and files get mixed up.

SNMP name configuration can be found under /etc in settings.snmp file.


> ![settingssnmp](img/catsnmp.png)

> Fig. 17 - A snippet of settings inside /etc/settings.snmp


Probably the best tool available to make this kind of change is "sed". "sed" is a really powerful tool escpecially for text editing and data mining.   


> ![sed](img/sedded.png)

> Fig. 18 - sed makes magic happen


"sed -i 's/SNMP_NAME=.*/SNMP_NAME=testname/' /etc/settings.snmp" (stop cursive*) was run. As it can be seen, SNMP name changed to "testname". Now with the real case, the only difference is that the router's serial number has to be used instead.


```python
import paramiko
import sys

def get_serial():
	cmd = "status -v sys |grep \"Serial Number\" |awk '{print $4}'"
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
	outp = ssh_stdout.readlines()
	serial = outp[0].strip()
	return serial

#because there's no output in the command, another command is run to check the new SNMP name
def change_snmp(serial):
	cmd = "sed -i 's/SNMP_NAME=.*/SNMP_NAME=" + str(serial) + "/' /etc/settings.snmp"
	check = "sed -n 's/SNMP_NAME=//p' /etc/settings.snmp"
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(check)
	outp = ssh_stdout.readlines()
        serialcheck = outp[0].strip()
        if str(serialcheck) == str(serial):
                state = "OK"
        else:
                state = "FAILED"
        return state

router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)

serial = get_serial()
status = change_snmp(serial)
print(status)

ssh.close()
```



> ![pythonsnmp](img/pythonsnmp.png)

> Fig. 19 - Succesfully changed SNMP name


As it can be seen again, SNMP name has changed. This time the Python program first called get_serial() function to get router's serial number, after which it called change_snmp() function with one argument which was the serial number. Changing SNMP name is not rocket science, but there's always a possibility that something goes wrong. This is the reason why even the simplest change should be verified.

## 4.6 Adding user modules

User modules are third party programs than can be added to routers. User modules are located under /opt directory. User modules are added as .tgz files, which will be decompressed and put into /opt directory. Before decompression is possible, user modules need to be transferred to router. For this, the code for restore_cfg() function can be reused with some changes.


```python
import paramiko
import sys

#sftp is used to transfer user module to router
#.tgz is decompressed to /opt
#check is made to make sure the user module exists
#return OK or FAILED
def add_um(user_m, m_name):
        orig = user_m
        dest = "/opt/" + user_m
        cmd = "tar -xzf " + dest + " -C /opt/"
	check = "if [ -d \"/opt/" + m_name + "\" ];then echo OK;else echo NOT;fi"
        sftp = ssh.open_sftp()
        sftp.put(orig, dest)
        ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(check)
	outp = ssh_stdout.readlines()
        status = outp[0].strip()
        if status == "OK":
                status_msg = "OK"
        else:
                status_msg = "FAILED"
        return status_msg

	

router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"
user_m1 = "pinger.v3.tgz"
user_m1_name = "pinger"

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)

status = add_um(user_m1):
print(status)

ssh.close()
```


> ![umverify](img/umverify.png)

> Fig. 20 - Program returns "OK" status message indicating success


> ![pinger](img/pinger.png)

> Fig. 21 - Pinger module can be seen under /opt now

Again, Python program returns "OK" message, so in other words the pinger module has been succesfully transferred to the router. Now this function can be reused as many times as needed to add more user modules.


## 4.7 Changing root password

Having passwords in clear text is always questionable, especially when the root account is concerned. In this case, it was agreed that it is okay as long as only authorized people in NDC have read and write permissions to the program. 

To change root's password without interacting, echo and chpasswd commands combined can be used. The whole command would be "echo 'root:<password>' |chpasswd". If the password was actually changed, status message "Password for 'root' changed" is printed to standard output, which can be caught again and used later to determine whether root's password change was a success or not.



```python
import paramiko
import sys

def change_pw(passwd):
	cmd = "echo 'root:"+ str(passwd) + "' |chpasswd"
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
	outp = ssh_stdout.readlines()
	status = outp[0].strip()
	success = "Password for 'root' changed"
	if status == success:
		status_msg = "OK"
	else:
		status_msg = "FAILED"
	return status_msg

router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"
new_passwd = "Str0ngerandl0nger!-"

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)

status = change_pwd(new_passwd)
print(status)

ssh.close()
```


> ![passchange](img/passchange.png)

> Fig. 22 - Status message "OK" indicates that the root password was succesfully changed


The root user's password was succesfully changed. Now the actual configuration part of the process is ready, but this is not the end yet. A backup file has to be downloaded and the excel file needs to be updated.

## 4.8 Getting backup file

Now that every configuration is verified, a backup file of the current configuration is needed, it will be stored in NDC's server. The reason why it is needed, is simply because restoring router configuration becomes easy in case if something goes awry. This is part of configuration management. There's command "backup" that can be used to create the backup file. Command "backup" itself just prints router's current configuration to standard output, but it can easily be redirected to a file.


```python
import paramiko
import sys

#takes one argument, configuration filename
#original filename will be preceded by "bckup"
def get_backup(filename):
	bu_file = "bckup" + filename
	cmd = "backup > " + bu_file
	orig = "/root/" + bufile
	ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command(cmd)
        sftp = ssh.open_sftp()
        sftp.get(orig, bu_file)

router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"
restore_file = sys.argv[1]

ssh = paramiko.SSHClient()
ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh.connect(router_dflt_ip, username=uname, password=passwd)

get_backup(restore_file)

ssh.close()
```

> ![backuo](img/backup.png)

> Fig. 23 - Backup file can be seen in the directory


This time, variable bu_file that contains the actual backup filename is used as the destination address of the file. This means the backup file will be saved under the same directory with the Python program. The next step is to put all the functions and code together. What is really important here, some checks need to be made, such as does every single file exists in the same directory with the Python program.

## 4.9 Catching errors

There are many potential causes of errors. The idea is to get rid of most of them. This makes misuse of the program harder. 

Firstly, the Python program takes one parameter, an error will be raised if there are no parameters at all or more than one.


> ![indexerror](img/indexerror.png)

> Fig. 24 - No parameters to the script caused IndexError


Perfect, now the cause of error is known, so it can be prevented in the program! Also, there are bunch of files defined in the code, so making sure the files really exists in the directory is necessary.

> ![filenotfound](img/notfound.png)

> Fig. 25 - Missing file caused FileNotFoundError


The user module "pinger.v3.tgz" was changed to "pinger.v4.tgz" in the code. Because there is no such a file, FileNotFoundError is raised. Now this can also easily be prevented in the program.

There are still two probable errors that can easily be spotted by just quickly looking at the code. Both are related to the SSH connection. Firstly, when the SSH authentication happens, incorrect credentials will cause an error.


> ![autherror](img/autherror.png)

> Fig. 26 - Wrong credentials caused an error


The username "root" was changed to "root1" in the code. Because the user doesn't exists, the credentials are deemed incorrect. This caused paramiko's own AuthenticationException exception to be raised. This will be prevented soon as well! The last probable cause of an error that can easily be spotted is incorrectly configured network settings. The Python program will keep on trying to connect to the router. Because the network settings are incorrect, the router cannot be found and connection is never established.


> ![timeout](img/timeout.png)

> Fig. 27 - TimeoutError occured


TimeoutError was caused by misconfigured network settings. Anyway, it took a really long time before the exception was raised. Now, it is important to remember that this configuration happens via ethernet cable. Anything more than five seconds indicates that there's something wrong with network settings. Luckily, it is possible to set user defined timeout. Timeout of five seconds should be enough, but it is a good practice to add some room, so timeout will be set to 15 seconds.


> ![timeout15](img/timeout15.png)

> Fig. 28 - This time timeout was caused by socket


This time TimeoutError exception was not raised. The error was "socket.timeout". To be able to catch this error, module "socket" needs to be imported. Otherwise, NameError will be raised when catching "socket.timeout". Now that it is known what needs to be caught, it is time to strenghten the code and add more logic.


```python
import paramiko
import sys
import socket
import os

router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"
new_passwd = "Str0ngerandl0nger!-"
user_m1 = "pinger.v3.tgz"
user_m1_name = "pinger"
user_m2 = "hmpclient.v2.tgz"
user_m2_name = "hmpclient"

#checking that there's one parameter given to the program
try:
	restore_file = sys.argv[1]
except IndexError:
	print("Usage: python autoconfig.py <cfg_file>")
	sys.exit()

#checking if necessary files exist in current working directory
if not os.path.exists(user_m1):
	print("Unable to find user module " + user_m1 + " in current working directory.")
	sys.exit()
	
if not os.path.exists(user_m2):
	print("Unable to find user module " + user_m2 + " in current working directory.")
	sys.exit()
	
if not os.path.exists(restore_file):
	print("Unable to find cfg file " + restore_file + " in current working directory.")
	sys.exit()

#timeout is set to 15 seconds
try:
	ssh = paramiko.SSHClient()
	ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
	ssh.connect(router_dflt_ip, username=uname, password=passwd, timeout=15)
#catching misconfigured network settings
except socket.timeout:
	print("Connection timed out. Check your network settings")
	sys.exit()
#catching incorrect credentials
except paramiko.ssh_exception.AuthenticationException:
	print("Authentication error. Check your credentials")
	sys.exit()

serial = get_serial()
mac = get_mac()

print("Serial number: " + str(serial))
print("MAC address: " + mac + "\n")
print("Starting configuration daemon...")

print("Sending configuration file...")
while True:
        restore_status = restore_cfg(restore_file)
        if restore_status == "OK":
                break

print("Changing SNMP name...")
while True:
	snmp_status = change_snmp(serial)
	if snmp_status == "OK":
		break

print("Adding user module " + user_m1 + "...")
while True:
	userm_status1 = add_um(user_m1, user_m1_name)
	if userm_status1 == "OK":
		break

print("Adding user module " + user_m2 + "...")
while True:
	userm_status2 = add_um(user_m2, user_m2_name)
	if userm_status2 == "OK":
		break

print("Changing root's password...\n")
while True:
	pw_status = change_pw(new_passwd)
	if pw_status == "OK":
		break

#creating fancy easily spottable summary
print("Verification summary")
print("----------------------------------")
print("|Configuration file restored: " + restore_status + "|")
print("|SNMP Changed: " + snmp_status + "|")
print("|User module " + user_m1 + ": " + userm_status1 + "|")
print("|User module " + user_m2 + ": " + userm_status2 + "|")
print("|Password changed: " + pw_status + "|")
print("----------------------------------\n")
print("Saving backup configuration file as bckup " + restore_file + "...\n")
get_backup(restore_file)
print("Done. The router can be unplugged now!")

ssh.close()
```


> ![exceptions](img/allerrors.png)

> Fig. 29 - Exceptions were caught correctly


As it can be seen, catching errors works as expected.

> ![confsuccess](img/confsuccess.png)

> Fig. 30 - The program ran succesfully


Perfect! The router is configured now. The idea behind the while loops are that the program will keep on trying until it succeeds in a task. There's of course a risk that for some unknown reason it never succeeds and will get stuck in an infinite loop. Some more logic could be added. For example, the program could try three times and if it doesn't succeed, it returns "FAILED" and skips to the next task.

## 4.10 Updating Excel

Now when the router is configured, some information needs to be added to an Excel file. Below is an excel template, which is identical to the original one, just without any data.

> ![excel2](img/excel2.png)

> Fig. 31 - An empty excel template

As it was written earlier, the data which needs to be updated to the excel file is as follows:

- VPN IP address
- Network mask
- Serial Number
- MAC address
- Model
- Date
- Reference

There are also some additional cells, which will be left empty. Those will be updated later if needed. Writing to an excel file with Python is pretty straightforward. In this case, a non-native module "openpyxl" will be used. First, it is time to write the code and test it with test values. If everything goes according to plan, the code can and will be integrated with the router configuration automation program.

```python
import openpyxl

def update_excel():
        #opening excel workbook
        wb = openpyxl.load_workbook(filename = excelfile)

        #opening the first sheet
        sheets = wb.sheetnames
        ws = wb[sheets[0]]

        count = 1
        i = 0

	#titles are located under these columns, on the same line
        letters = ["A", "B", "C", "D", "E", "F", "J"]
        while 1:
                total = letters[i] + str(count)
                valuecheck = ws[total].value
		
		#if a cell is empty, write new value
                if not valuecheck:
                        if letters[i] == "A":
                                ws[total] = fullip
                                print("VPN IP updated to excel")
                        elif letters[i] == "B":
                                ws[total] = mask
                                print("Network mask updated to excel")
                        elif letters[i] == "C":
                                ws[total] = ser
                                print("Serial number updated to excel")
                        elif letters[i] == "D":
                                ws[total] = maci
                                print("MAC address updated to excel")
                        elif letters[i] == "E":
                                ws[total] = model
                                print("Model updated to excel")
                        elif letters[i] == "F":
                                ws[total] = date
                                print("Date updated to excel")
                        elif letters[i] == "J":
                                ws[total] = reference
                                print("Reference updated to excel")
                                break
                        count = 1
                        i += 1
                count += 1

        wb.save(filename = excelfile)

fullip = sys.argv[1]
mask = sys.argv[2]
ser = sys.argv[3]
maci = sys.argv[4]
model = sys.argv[5]
date = sys.argv[6]
reference = sys.argv[7]

excelfile = "customer_router_information.xlsx"

print("Updating excel...\n")
update_excel()

```


> ![exceltest](img/exceltest.png)

> Fig. 32 - Test values given as parameters



> ![exceltest2](img/exceltest2.png)

> Fig. 33 - Data is correctly written to the excel file


Okay perfect, it works! The code itself is very straightforward. Locations of title cells are known, every title is on line one, only columns differ. For example, SerialNo which represents serial number, can be found at line one and column C (1C). Now when that is known, the code checks if the cell below is empty. If it is empty, the new value is written into it. If it is not empty, it keeps checking the cells below until it finds an empty one and writes into it. In this test, test values were used and they were given as parameters to the program. Now in the real case when this code is integrated with the router configuration automation program, some of the values can be hardcoded inside the code, some will be provided by the automation program and only few has to be provided as parameters.


## 4.11 Integration

Integration of these two programs is quite simple. Few changes need to be made though. First of all, the fewer parameters that has to be provided, the better. Exact model and reference are something that cannot be found inside the router, which is the reason why the information needs to be given as parameters. Luckily for one batch, both usually stay the same so re-running the program is fast. Also, earlier the configuration filename was provided as a paremeter, to make it simpler, only the IP address part needs to be provided, for example 10.240.254. This way it can easily be used by update_excel() function and also it is easy to turn it into a filename inside the program.

For clarity, only essential parts are showed below. The actual code in its entirety can be found in Appendix.

```python
import paramiko
import sys
import socket
import os
import openpyxl
import time

def get_serial():
	...

def get_mac():
	...

def restore_cfg(restore_file):
	...

def change_snmp(serial):
	...

def add_um(user_m, m_name):
	...

def change_pw(passwd):
	...

def get_backup(filename):
	...
	
def update_excel(ser, maci):
	...
	
#checking that there are three parameter given to the program
try:
	model = sys.argv[1]
	reference = sys.argv[2]
	vpnip = sys.argv[3]
except IndexError:
	print("Usage: python autoconfig.py <model> <reference> <first 3 of VPNIP/xxx.xxx.xxx>")
	sys.exit()
	
router_dflt_ip = "192.168.1.1"
uname = "root"
passwd = "Password3xample-"
new_passwd = "Str0ngerandl0nger!-"
user_m1 = "pinger.v3.tgz"
user_m1_name = "pinger"
user_m2 = "hmpclient.v2.tgz"
user_m2_name = "hmpclient"	
restore_file = "testcfg_" + vpnip + ".cfg"
excelfile = "customer_router_information.xlsx"

#information for update_excel()
mask = "255.255.255.0"
fullip = vpnip + ".1"
date = time.strftime("%d/%m/%Y")

serial = get_serial()
mac = get_mac()

#checkin if excel file exists in current directory
if not os.path.exists(excelfile):
        print("Unable to find excel file " + excelfile + " in current working directory.")
        sys.exit()

#----------other error catching code----------

#----------configuration and print statements----------

update_excel(serial, mac)
```

> ![finalresult](img/finalresult.png)

> Fig. 34 - Router succesfully configured and data written to the excel file

> ![finalresult2](img/finalresult2.png)

> Fig. 35 - Everything seems to be in their places


Everything works as expected! So, now there's the fully functional code and it can be used in testing and later in production.
- - -

# 5 Results

- - -

# 6 Future development

- - -

# 7 References

Cisco. (2013). Configuration Guide; Chapter: Using the Command-Line Interface.
	Retrieved from https://www.cisco.com/c/en/us/td/docs/ios/12_2/configfun/configuration/guide/ffun_c/fcf001.html
	
Cisco. (2014). Cisco IOS Scripting with TCL Configuration Guide, Cisco IOS Release 15M&T.
	Retrieved from https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ios_tcl/configuration/15-mt/ios-tcl-15-mt-book/nm-script-tcl.html

Enck, McDaniel, Sen, Psebos, sspoerel, albert, sanjay, aiello. (2007). Configuration Management at Massive Scale:
	System Design and Experience. 
	Retrieved from https://www.usenix.org/legacy/event/usenix07/tech/full_papers/enck/enck_html/

Ernst, T. & Lach, H-Y. (2007). Network Mobility Support Terminology.
	Retrieved from https://tools.ietf.org/pdf/rfc4885.pdf
	
Glez-Peña, D., Lourenço, A., López-Fernández H., Reboiro-Jato M., Fdez-Riverola F.; Web scraping technologies in an API world, 		Briefings in Bioinformatics, Volume 15, Issue 5, 1 September 2014, Pages 788–797. 
	Retrieved from https://doi.org/10.1093/bib/bbt026 
	
Haddaway, N. (2015). The Use of Web-scraping Software in Searching for Grey Literature. Grey Journal. 11. 186-190.
	Retrieved from 
	https://www.researchgate.net/publication/282658358_The_Use_of_Webscraping_Software_in_Searching_for_Grey_Literature

Heidi, E., (24.03.2016). An Introduction to Configuration Management. 
	Retrieved from https://www.digitalocean.com/community/tutorials/an-introduction-to-configuration-management

Heydon, A. & Nojark, M. (1999). Mercator: A scalable, extensible Web crawler. 
	Retrieved from http://www.bagualu.net/linux/crawler.pdf 

IDC. (2017). IDC's Worldwide Quarterly Ethernet Switch and Router Trackers Show Steady Growth for Q1 2017; Results Bode Well for 	Year Ahead. Retrieved from https://www.idc.com/getdoc.jsp?containerId=prUS42757317

- - -

# 8 Appendix
