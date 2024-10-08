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

32. Return to the Wazuh dashboard by entering your credentials, once on the dashboard you will see that there are no agents. Click "Add Agent". Once you are adding the agent, you will need to make sure that you have set it to a Windows machine and assign the server address as your Wazuh droplet IP. <br><br><img src="https://github.com/user-attachments/assets/10ff79c8-096e-466f-9cb9-2c31b4db62bf" width="400" height="300" />


33. At the bottom of the screen you will see something that says, "Run the following commands to download and install the Wazuh agent". Copy those commands and run them in PowerShell to begin installation
34. Once the agent installation is complete, type "net startwazuhsvc" into poweshell to start the service <br><br><img src="https://github.com/user-attachments/assets/936cbcba-143a-4dac-8f07-ba9b0e344f8b" width="400" height="300" />

35. You should now be able to see one active agent in your dashboard.<br><br><img src="https://github.com/user-attachments/assets/bef9a839-a9f7-4275-87f4-6f2661fffa3b" width="500" height="300" />

36. Locate the ossec.conf file. The path is shown in the following image: <br> <br> <img src="https://github.com/user-attachments/assets/4d58e578-20e8-4006-84e2-beb5e8362df5" width="400" height="300" />


37. Configure Sysmon Log Analysis by finding the log analysis section in the ossec.conf file. To add Sysmon logs, retrieve the channel name through Event Viewer: Open Event Viewer, expand Applications and Services Logs > Microsoft > Windows > Sysmon. Right-click Operational, select Properties, and copy the full log channel name. Update the configuration file with the Sysmon channel name. <img src="https://github.com/user-attachments/assets/c99282a3-c73a-4445-bb51-00757fe4831c" width="400" height="300" />

38. In the 'services' application in your VM you will need to restart Wazuh to allow the changes to take effect.
39. Return to your Wazuh dashboard and on the menu click on threat intelligence > threat hunting. Once you're in the right menu type "sysmon" into the search bar and you should get something that looks like the following: <br><br> <img src="https://github.com/user-attachments/assets/6545679c-53a8-4d1e-947c-1bea109ec54c" width="400" height="300" />


40. Next we will download Mimikatz. To do this you will first need to exclude the Downloads folder to avoid detection when downloading Mimikatz.  <br><br> <img src="https://github.com/user-attachments/assets/7dd31629-b17a-4540-a237-c458c64711db" width="400" height="300" />

41. Download and extract [Mimikatz](https://github.com/gentilkiwi/mimikatz/releases/tag/2.2.0-20220919), your browser might try and block it. If it does just click on "download anyway" when you see that it might be unsafe.

42. Run it through an Administrator PowerShell session by changing to its directory in PowerShell and then enter .\mimikatz.exe <br> <br> <img src="https://github.com/user-attachments/assets/f4103d3a-8fe7-4355-bf45-056509118e92" width="400" height="300" />


43. Check Logs in the Wazuh Dashboard. Go to the Wazuh dashboard under the Events or Alerts section. Search for Sysmon or Mimikatz events, ensuring that Sysmon is configured correctly.

44. If no events show up, modify ossec.conf to enable logging of all events. Navigate to your Wazuh CLI and type in the command "cp /var/ossec/etc/ossec.conf ~/ossec-backup.conf", this command will create a backup file. Next type in the command "nano /var/ossec/etc/ossec.conf" which will bring up the following interface: <br><br> <img src="https://github.com/user-attachments/assets/422e959a-fc32-4b73-ab75-2f3b51795c67" width="400" height="300" />

45. Under `<Ossec Config>` look for `<logall>no</logall>` and `<logall_json>no</logall_json>` and change the "no" to a "yes" and save it.
46. Next we will need to restart the Wazuh manager by typing the command "systemctl restart wazuh-manager.service"
47. Check to make sure that the files were archived by entering the command "cd /var/ossec/logs/archive/", once that has run enter "ls", your output should display the following: <br><br> <img src="https://github.com/user-attachments/assets/cab7f39f-32b8-4892-8aa0-3829191b1b7f" width="300" height="200" />
48. To start ingesting these logs we need to configure the file. Type in the command "nano /etc/filebeat/filebeat.yml". Scroll down in the file and look for a line that says "archives: enabled: false", change the false to say "true" and save the changes. Once you are out of the file, enter the command "systemctl restart filebeat". <br><br> <img src="https://github.com/user-attachments/assets/541cf5fc-4c12-4c57-b9aa-6c6e150ca00e" width="400" height="300" />

49. Head back to your Wazuh dashboard and click on the menu icon, click on Dashboards management > Dashboard management > Index Patterns. We will need to create an index for archives, to do that click on "Create Index". Name your Index "wazuh-archives-*", click next and under Time field select "timestamp". Create the Index.
50. Next navigate to the "dicover" section in the menu by pressing the menu icon and the Explore > Discover. On the top left, select the drop down menu and select "wazuh-archives-*". You should see something similar to the following: <br><br> <img src="https://github.com/user-attachments/assets/b5544b3d-8301-453d-979d-d46ddf0937fb" width="400" height="300" />

51. Next you will need to add a new rule. In the menu search for "Rules" and then click "Custom Rules". In the Custom rules file add the following underneath what is already there: <br><br> <img src="https://github.com/user-attachments/assets/551664d5-0228-47bf-9e6f-9287f055739d" width="400" height="300" />

52. This rule update will allow us to detect any Mimikatz intrusions into the network.
53. Congratulations! You've successfully set up everything to detect Mimikatz intrusions and monitor Sysmon logs. In the next steps, we'll focus on automating alerts to notify us of these intrusions
54. Navigate to [**Shuffle**](https://shuffler.io/) and create a free account.
55. Create a new workflow and name it whatever you want. Once you have created the workflow, click on the bottom left of the page where it says, "triggers" and add "Webhook" as a trigger and copy the Webhook URI as we will need to add this to the Ossec file. <br><br><img src="https://github.com/user-attachments/assets/fb00dc9b-c419-4876-8970-5a9cfcfdac61" alt="WebHookURI" width="400" height="300">
56. Head back over to the Wazuh manager to begin adding shuffle. Type in the command "nano /var/ossec/etc/ossec.conf" and enter the following code, making sure to enter your own URL and rule ID: <br><br> <img src="https://github.com/user-attachments/assets/82ee7cf2-444d-4883-9eaa-b34903bcaa85" alt="WazuhOssec" width="400" height="300">
57. Once you have saved your changes, return to windows powershell and run Mimikatz again. This will make sure that a security event is generated.
58. Return to shuffle, click on your webhook and then click "start". Click on the icon of a person on the bottom of the page and then click "Test Workflow", you should see something similar to the following once you have clicked on the incident: <img src="https://github.com/user-attachments/assets/f0eb08f6-cee8-4068-95ec-834187f12135" alt="IncidentsInShuffle" width="400" height="300">
59. Create an account on VirusTotal, copy your API and return to Shuffle and search for VirusTotal. Add VirusTotal to your shuffle. In the VirusTotal icon, change the action to "Get a hash report" and then click on the button that says "Authenticate VirusTotal V3" and enter your API key. Save the workflow.

**Please note that this is where my system crashed as it cannot handle running theHive, Wazuh, and a Virtual Machine simultaneously, the steps to finish the project are as followed**

61. Log in to The Hive using the default credentials.
62. Create a new organization by clicking the plus button and naming it whatever you choose with any description.
63. Add users to the organization by clicking the plus button, specifying user details like login, name, profile, and permissions.
64. Set a password for the account and create an API key for the service account.
65. Log out of the administrative account and log in to the new organisation account to access the cases and alerts.
66. Configure Shuffle to work with The Hive by authenticating with the API key and specifying the URL of your Hive instance.
67. In Shuffle, select "create alert" under find action instead of querying the API.
68. Connect VirusTotal to The Hive to enable it to analyze Wazug alerts in The Hive.
69. Set the date field by connecting VirusTotal and executing arguments to include UTC time and a description.
70. Include relevant information like host, user, external link, flag, severity, source description, status, summary, and tags.
71. Save and run the workflow to test the integration with The Hive and VirusTotal.
72. Update the Cloud firewall to allow inbound traffic on Port 9000 for The Hive instance.
73. Rerun the workflow to ensure that the Hive actions are successful, creating alerts in The Hive.
74. Access The Hive to view the automatically created alert, containing detailed information about the detection.
75. Drag and drop the email application in Shuffle to configure sending alerts to email recipients.
76. Connect VirusTotal to the email application and add the recipient's email to receive alerts.
77. Input relevant alert details like subject, time, title, host, and user in the email configuration.
78. Save and rerun the workflow to trigger the email notification to the analyst.
79. Check the recipient's email for the alert and interact with the email to confirm actions (e.g., block the source IP).
80. Verify that the automated response works by monitoring the IP tables or relevant logs to ensure the IP is blocked successfully.

# Conclusion

By the completion of this project I have successfully set up a robust security infrastructure using VirtualBox, DigitalOcean, Wazuh, and TheHive. Starting with the creation of a virtual machine running Windows and the installation of essential monitoring tools like Sysmon, I laid the foundation for effective event logging. The Wazuh deployment on DigitalOcean, along with its integration with TheHive, allows for detailed threat detection and incident response capabilities.

The addition of custom configurations, such as setting up firewalls, ingesting logs with filebeat, and tailoring Wazuh to monitor Sysmon and Mimikatz logs, has enhanced the environment’s ability to track and analyze potential security breaches. By implementing custom rules to detect intrusions, such as Mimikatz attacks, and verifying their accuracy through the Wazuh dashboard, I have created a security system that can actively monitor and respond to advanced threats.

This project not only strengthens my detection and response mechanisms but also positions my infrastructure for future improvements. As the next step, focusing on automating alerts and refining your incident response processes will help ensure swift and accurate handling of security events in real time.

Tools Used:
----------
wazuh - A free and open-source integrated Security Information and Event Management (SIEM) and Extended Detection and Response (XDR) soloution.  
VirtualBox - A free and open-source software that allows you to create and run virtual machines.  
SysMon - A windows system service that logs detailed information about system activity.  
TheHive - An open-source Security Incident Response Platform (SIRP) designed to help manage and respond to security incidents.  
Digital Ocean - A cloud infrastructure provider offering scalable virtual servers (Droplets) and managed services.
Mimikatz - An open-source tool used to extract plaintext passwords, hashes, and Kerberos tickets from memory on Windows systems. It is commonly used in penetration testing and red teaming exercises to demonstrate security vulnerabilities related to credential management and authentication protocols.
Shuffler.io - A cloud-based security automation platform that streamlines incident response and threat management. Shuffler.io integrates various security tools, enhancing security operations and improving response times.

Skills Learnt in this project:
------------------------------
Virtual Machine Setup and Configuration – Downloading and setting up VirtualBox, creating a virtual machine with a Windows ISO.<br>
Operating System Installation and Configuration – Installing Windows on a virtual machine and configuring basic settings.<br>
System Monitoring Tool Deployment – Downloading, configuring, and installing Sysmon for system activity monitoring.<br>
Firewall Configuration and Network Security – Creating and applying firewalls in DigitalOcean to secure network environments.<br>
Cloud Infrastructure Management – Creating and managing DigitalOcean droplets for various services, including Wazuh and TheHive.<br>
Linux System Administration – Performing system upgrades and updates via SSH, managing services, and configuring critical security tools like Wazuh and TheHive.<br>
Log Analysis and Threat Detection – Configuring Wazuh to detect Sysmon events, managing SIEM dashboards, and analyzing logs for potential threats.<br>
ElasticSearch and Cassandra Configuration – Setting up and configuring both ElasticSearch and Cassandra databases to work with TheHive.<br>
Threat Hunting and Intelligence – Using Wazuh’s Threat Hunting features for monitoring suspicious activities such as Mimikatz intrusions.<br>
Incident Detection with Custom Rules – Writing and implementing custom security rules for intrusion detection and automating alerts for critical incidents.<br>
Automation of Security Alerts – Implementing tools and processes to automate responses to specific security events like Mimikatz intrusions.<br>
