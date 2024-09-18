# SOC Automation Lab

Description:
------------
This lab is designed to simulate a Security Operations Center (SOC) environment with a focus on automating key tasks and processes. It includes scripts and tools for automating threat detection, incident response, and log analysis, leveraging technologies such as PowerShell, VirtualBox, Sysmon, and Bash. The lab also integrates with various security platforms and APIs to streamline workflows, reduce manual effort, and enhance the efficiency of SOC operations. This project is aimed at both learning and demonstrating the implementation of automation in a SOC setting, showcasing how automation can significantly improve security posture and operational effectiveness.

<img src="https://github.com/user-attachments/assets/ebe31b15-6f0f-472f-b616-a7dcd0e7926d" width="400" height="300"/>

Steps:

-------
1. Download & install VirtualBox  <br> <br> <img src="https://github.com/user-attachments/assets/3b083dca-300c-4673-8530-cb0ebadc350a" width="400" height="300"/>

2. Download the Windows Media Creation Tool, which will allow you to create an ISO file <br> <br> <img src="https://github.com/user-attachments/assets/95dd5f95-d6fd-483b-936c-6deaecc97dbe" width="400" height="300"/>


3. Return to VirtualBox and create a new virtual machine, make sure to use the previously downloaded Windows ISO file as the ISO image in VirtualBox
4. Power on the virtual machine and wait for Windows to install  <br> <br> <img src="https://github.com/user-attachments/assets/a8d42c1b-a895-41ee-bf6f-041a713960a5" width="400" height="300"/>

In the virtual machine:

5. Download Sysmon from [the Sysinternals website](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon).  <br> <br> <img src="https://github.com/user-attachments/assets/cbc1f019-d348-442d-ba22-4860f61b2f61" width="300" height="200" />

6. Save the SysmonConfig.xml file from https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml. <br> <br> <img src="https://github.com/user-attachments/assets/9d2adabe-90ce-441e-a019-9f95e8e5a7e5" width="400" height="300" />


7. In powershell, navigate to the Sysmon directory and install Sysmon with the xml file, make sure the config file and unzipped sysmon are in the same folder. Command: .\sysmon64.exe -i sysmonconfig.xml <br> <br> <img src="https://github.com/user-attachments/assets/f458dfb2-0db3-43af-ad0c-3e571c2e9da4" width="400" height="300" />

8. Create a Digital Ocean Droplet by logging into your account and selecting Ubuntu as the operating system with 8GB of RAM and 160GB of storage. <br> <br> <img src="https://github.com/user-attachments/assets/a52668c7-7a53-4a0a-a615-8dc655a7b67c" width="400" height="300" />

9. Create a firewall to avoid spam. On the left of Digital Ocean > Manage > Networking > Firewalls > Create Firewall.

10. In Digital Ocean, return to your droplet and select "Networking" and scroll down to add the firewall you just created.

11. Return to your droplet and select "Access" and then "Launch droplet console". <br><br> <img src="https://github.com/user-attachments/assets/7da8ce84-c3fd-48ba-9eee-7803b35674d6" width="400" height="300" />

12. Once you have SSH'ed into your virtual machine begin performing upgrades and updates by typing the command: "apt-get update && apt-get upgrade -y". Let it run and install and update what it needs to. A pink/purple screens will appear, press enter on it to complete this step.
13. Next run a curl command on the machine to install Wazuh, the command will be: "curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh && sudo bash ./wazuh-install.sh -a". Once it has installed, make sure you save the username and password it gives you as this will be necessary.
14. Copy your public IP address from your Digital Ocean droplet and go into a new browser and enter https://[your IP address], you will the be presented with the following screen where you must enter your wazuh username and password. <br> <br> <img src="https://github.com/user-attachments/assets/f217f82c-71aa-4611-9e50-22b402125658" width="400" height="300" alt="WazuhDashboard"/>

15. Next you will create another droplet for theHive using the same steps. Launch the droplet and follow the steps laid out at [The Hive Installation](https://docs.strangebee.com/thehive/installation/step-by-step-installation-guide/#java-virtual-machine) to install the necessary components. These will include, theHive itself, Java, Cassandra and ElasticSearch
16. Next type in nano /etc/cassandra/cassandra.yaml which will redirect you to a page that looks like this: <br><br> <img src="https://github.com/user-attachments/assets/606df1aa-dcea-4ef8-a403-c0da005dda99" width="400" height="300" />
17. Press ctrl+w which will bring up a search bar. Seach "listen" and scroll until you see "listen_address" which by default should be localhost, change this to the IP address of your "theHive" server
18. Press ctrl+w wagain. Seach "rpc_address" and scroll until you see "rpc_address" which by default should be localhost, change this to the IP address of your "theHive" server
19. Press ctrl+w again. Seach "seed_provider" until you see "seed_provider" which by default should be localhost, change this to the IP address of your "theHive" server but make sure to leave the ":7000" at the end of the IP address
20. Next hit ctrl+x to save it and then press "y"
21. The next step will be to stop the cassandra service by entering the command "systemctl stop cassandra.service". You also need to remove old files by typing in the command "rm -rf /var/lib/cassandra/*"
22. Now that you have removed the old files you can start the service by entering "systemctl start cassandra.service". Next, make sure cassandra is running by typing "systemctl status cassandra.service" and you should see this: <br> <br> <img src="https://github.com/user-attachments/assets/3eb35c70-fbb1-4783-a6e5-333916aed9c8" width="400" height="300"/>
23. Next we will need to configure elastic search by entering the following; "nano /etc/elasticsearch/elasticsearch.yml" which will bring up the following: <br><br> <img src="https://github.com/user-attachments/assets/ea583a44-f3b2-4954-91b5-9b48649c1748" width="400" height="300"/>

24. Next remove the comment for the cluster name and rename your cluster, I named mine thehive for consistency. You also need to remove the comment for "node.name" but you can leave the node as is.
25. Continue scrolling down until you get to "network.host", uncomment it and change the IP address to the public address of your hive server. Press ctrl+x to save your changes
26. Once you're finished configuring, enter the following commands: "systemctl start elasticsearch", "systemctl enable elasticsearch", "systemctl status elasticsearch.service". These commands will start Elasticsearch and allow it to run. You should be presented with the following output: <br> <br> <img src="https://github.com/user-attachments/assets/ba580438-40be-4f10-bb9e-7f00b05866dd" width="400" height="300"/>
27. 







# Conclusion

In this lab, I successfully built and configured a comprehensive Security Operations Center (SOC) automation environment. By integrating tools such as Sysmon, Wazuh and TheHive, I created a robust system capable of automating key SOC tasks including threat detection, incident response, and log analysis.

The process began with setting up VirtualBox and installing Windows, followed by the deployment of Sysmon to monitor system activity. This was complemented by creating a Digital Ocean Droplet to host Wazuh and TheHive, which are critical for security event management and incident response. By forwarding Sysmon logs to Wazuh via Filebeat, I ensured that security events from the Windows machine were captured and analyzed effectively.

The automation aspect was a major focus, with Bash and Python scripts developed to query ElasticSearch APIs and trigger incident response actions in TheHive. This setup demonstrated how automated workflows can enhance SOC operations, making the detection and response to security incidents more efficient.

Testing the setup with real-world scenarios, such as Nmap scans, validated the effectiveness of the automation and integration. The creation of dashboards in Elastic further allowed for the visualization of logs and alerts, ensuring that the entire SOC automation workflow functioned as intended.

This project not only enhanced my technical skills in SOC automation but also provided practical experience in integrating and configuring security tools. The knowledge gained through this lab is crucial for any SOC analyst role, where the ability to automate and streamline security operations is essential for maintaining a strong security posture.

Tools Used:
----------
wazuh - A free and open-source integrated Security Information and Event Management (SIEM) and Extended Detection and Response (XDR) soloution.  
VirtualBox - A free and open-source software that allows you to create and run virtual machines.  
SysMon - A windows system service that logs detailed information about system activity.  
TheHive - An open-source Security Incident Response Platform (SIRP) designed to help manage and respond to security incidents.  
Digital Ocean - A cloud infrastructure provider offering scalable virtual servers (Droplets) and managed services.
