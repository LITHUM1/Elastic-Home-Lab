# WAZUH-Home-Lab
home lab for WAZUH Stack Security Information and Event Management (SIEM) using the WAZUH Web portal and a Kali Linux VM

tools we gonna use in our home lab 
1) VMware
2) WAZUH
3) Kali VM

------------------------------------------

EEnvironment Setup
Deploying Wazuh Server VM
To get started we’re going to deploy the Wazuh Server locally on a VMware. 
Read more [documentation](https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html)

Starting off we going to download the Wazuh server image OVA 
![Download](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Download-Wazuh-server-image.png)

After downloading and launching it using VMware, we wait a couple of minutes for it.
After waiting for it to start we log in usinmg default credentials, we see the terminal below indicates taht the deployment of Wazuh is successful


![terminal](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/up-and-running.png)


You can find the ip of the machine by running command "ip a"
Now what we have the ip, we enter the ip we got into browser to access the portal 

![portal](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Wazuh-portal.png)

and default credentails are admin:admin 
Wazuh main dashborad


--------------------------------------
Deploying Wazuh Agents

Now we need 2 VMs 1 windows and 1 kali.

We need to install Wazuh agent on them, to collect telemery from the agents and give us visibility into those hosts.

![win-agent](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Win-Agent.png)

![kali-agent](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Kali-agent.png)

As we can see both Wazuh Agents are up and running

![wazuh-agents](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/Wazuh-agents.png)


----------------------
Stress Testing our agents

Now we have deployed 2 Wazuh Agents that are sending their logs/checking in to the Wazuh Server.
Let's try to trigger a malicious alert in the Wazuh Server web console - by manaul testing.

Manul Testing 

To start let's try some basic to see of Wazuh alerts us when a user installs Metasploit Framework.

![win-install](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/installing-metasploit-windows.png)

Now in Kali VM we gonna do a port scan through metasploit

![metasploit](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/metasploit-kali.png)

searching for port scan and launching TCP port 

![port-scan](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/port-scanner.png)

Now we see that Wazuh detected some activity from either installing Windows VM or running a port scan from Kali VM.

Installing the framework generated no risk event neither our port scan from kali VM 

Let's try snother hopefully noisier test.
On our Windows VM let's create a user then add that user to local administrator group.
We can do that with the following commands on Windows VM:

1) net user <name-of-user> <password> /add
2) net localgroup Administrator <name-of-user> /add

![win-commands](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/adding-user.png)

Now we get a hit after running commands.

![risk](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/report-Wazuh.png)

Immediately after clicking on the “2” we see the logs in their SIEM module.
We can quickly identify which agent is affected, the MITRE ATT&CK technique, and a brief description of the activity seen.

------------------------

Automated Testing

Now that we’ve done some manual testing let's try using an Endpoint Detection and Response (EDR)
Testing script that will perform a few MITRE ATT&CK techniques but the payload is the Windows calculator.
The script can be found here - credit to op7ic for creating it.

Running the script opens calc.exe

![cal.exe](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/cal-script.png)

Once the script is installed we unzip the file and run “runtests.bat” with ./runtests.bat.
Below we can see about 15 minutes after it started running Windows Defender is generating alerts and its “payload” of calc.exe keeps dropping on the desktop.

On our dashboard MITRE ATT&CK
![mitre](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/wazuh-mitre-attack.png)

Logging into the Wazuh Server over HTTPS we didn't get any new high severity alerts like we did when creating and adding a new user.
However in the Wazuh MITRE ATT&CK dashboard we can see a ton of activity from the script.

![event](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/wazuh-events.png)

Under the “Top tactics” pie chart in the top right lets click into “Persistence”.
This brings us into their SIEM query editor. Additionally you can put on more filters to drill down or filter out noise.

![event-1t078](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/event-t1078.png)

Expanding on the results an event log showing a new service being created.

![explanding](https://raw.githubusercontent.com//LITHUM1/Wazuh-Home-Lab/main/Assets/script.png)

This was a Simple Home-Lab using Wazuh





















