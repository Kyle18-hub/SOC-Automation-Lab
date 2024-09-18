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

15. Next you will create another droplet for theHive


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







Dependences
apt install wget gnupg apt-transport-https git ca-certificates ca-certificates-java curl  software-properties-common python3-pip lsb-release

wget -qO- https://apt.corretto.aws/corretto.key | sudo gpg --dearmor -o /usr/share/keyrings/corretto.gpg
echo "deb [signed-by=/usr/share/keyrings/corretto.gpg] https://apt.corretto.aws stable main" | sudo tee /etc/apt/sources.list.d/corretto.list
sudo apt update
sudo apt install -y java-11-amazon-corretto-jdk
echo JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto" | sudo tee -a /etc/environment
export JAVA_HOME="/usr/lib/jvm/java-11-amazon-corretto"

Install Cassandra
wget -qO -  https://downloads.apache.org/cassandra/KEYS | sudo gpg --dearmor  -o /usr/share/keyrings/cassandra-archive.gpg
echo "deb [signed-by=/usr/share/keyrings/cassandra-archive.gpg] https://debian.cassandra.apache.org 40x main" |  sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
sudo apt update
sudo apt install cassandra

Install ElasticSearch
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch |  sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" |  sudo tee /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install elasticsearch

