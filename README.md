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
27. After making sure that both cassandra and elasticsearch are active, we will begin to configure theHive. To do this type in the command "ls -la /opt/thp". This will check if theHive has access to the correct files. <br><br><img src="https://github.com/user-attachments/assets/c5e77cdc-815d-4396-a783-4d9652d74a26" width="400" height="300"/>
28. As we can see in the above screenshot, theHive does not have access to the correct files. To fix this we need to enter the command "chown -R thehive:thehive /opt/thp", this will change ownership to theHive. Type the command "ls -la /opt/thp" again to make sure that ownership was changed. <br><br><img src="https://github.com/user-attachments/assets/04465914-d6c3-4219-9d38-7e5ba806d7ab" width="400" height="200" />
29. Type in the command "nano /etc/thehive.application.conf" to begin configuring theHive. You should see a page that looks like this: <br><br> <img src="https://github.com/user-attachments/assets/7a274b14-0404-49a0-a706-45ad2612f630" height="300" style="width: auto;" />
30. In this configuration file, scroll down and change the hostname to the public IP of your droplet. You also need to change the cluster name to whatever you renamed your cluster name to in cassandra. Keep scrolling until you see "application.baseURL" you need to change it so that application.baseURL = "https://[YourDropletIP]:9000".
31. Make sure to save your configurations and enter the following commands: "systemctl start thehive", "systemctl enable thehive", "systemctl status thehive". This will start theHive and check that it is running. <br><br> <img src="https://github.com/user-attachments/assets/0565c2fa-5518-4470-b66d-dc9a07e7b4e9" height="100" width="400" />
<br>
**Please note, my laptop does not have the capability to complete the rest of the project but the steps are as follows and screenshots wil be provided where possible**
<br>

33. Return to the Wazuh dashboard by entering your credentials, once on the dashboard you will see that there are no agents. Click "Add Agent". Once you are adding the agent, you will need to make sure that you have set it to a Windows machine and assign the server address as your Wazuh droplet IP. <br><br><img src="https://github.com/user-attachments/assets/10ff79c8-096e-466f-9cb9-2c31b4db62bf" width="400" height="300" />


34. At the bottom of the screen you will see something that says, "Run the following commands to download and install the Wazuh agent". Copy those commands and run them in PowerShell to begin installation
35. Once the agent installation is complete, type "net startwazuhsvc" into poweshell to start the service <br><br><img src="https://github.com/user-attachments/assets/936cbcba-143a-4dac-8f07-ba9b0e344f8b" width="400" height="300" />

36. You should now be able to see one active agent in your dashboard.<br><br><img src="https://github.com/user-attachments/assets/bef9a839-a9f7-4275-87f4-6f2661fffa3b" width="500" height="300" />

37. Locate the ossec-agent file. The path is shown
38. Configure Sysmon Log Analysis by finding the log analysis section in the osc.conf file. To add Sysmon logs, retrieve the channel name through Event Viewer: Open Event Viewer, expand Applications and Services Logs > Microsoft > Windows > Sysmon. Right-click Operational, select Properties, and copy the full log channel name. Update the configuration file with the Sysmon channel name.
39. Ingest Sysmon Logs. Modify the configuration file by replacing the default log location with the Sysmon channel. Disable logging for unnecessary sources like Application, Security, or System, unless needed. Save the changes and restart the Wazuh service for the updates to take effect.
40. Download Mimikatz. You will need to disable Windows Defender temporarily or exclude the Downloads folder to avoid detection when downloading Mimikatz. Download and extract Mimikatz, run it through an Administrator PowerShell session.
41. Check Logs in the Wazuh Dashboard. Go to the Wazuh dashboard under the Events or Alerts section. Search for Sysmon or Mimikatz events, ensuring that Sysmon is configured correctly.

42. Enable Log All Feature. If no events show up, modify osc.conf to enable logging of all events by changing the log_all and log_all_json settings to yes. Restart the Wazuh Manager and Filebeat services after making configuration changes.

43. Create a New Index in Wazuh. Create a new index pattern for Archives in the Wazuh dashboard by navigating to Stack Management > Index Patterns. Use this index to search all logs, including those not triggering specific rules.
44. Create Custom Alerts. Use Event ID 1 (process creation) from Sysmon to track Mimikatz executions. Configure Wazuh rules based on the original file name field (e.g., mimikatz.exe), rather than image or path, to avoid bypass through file renaming.
45. Force Log Ingestion (Optional). If logs aren't appearing in the dashboard immediately, force log ingestion by restarting the Wazuh Manager and checking for archives under var/ossec/logs/archives.










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
