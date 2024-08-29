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













