# SOC Automation Lab

Description:
------------
This lab is designed to simulate a Security Operations Center (SOC) environment with a focus on automating key tasks and processes. It includes scripts and tools for automating threat detection, incident response, and log analysis, leveraging technologies such as PowerShell, VirtualBox, Sysmon, and Bash. The lab also integrates with various security platforms and APIs to streamline workflows, reduce manual effort, and enhance the efficiency of SOC operations. This project is aimed at both learning and demonstrating the implementation of automation in a SOC setting, showcasing how automation can significantly improve security posture and operational effectiveness.

<img src="https://github.com/user-attachments/assets/ebe31b15-6f0f-472f-b616-a7dcd0e7926d" width="400" height="300"/>

Steps:

-------
1. Download & install VirtualBox  <br> <img src="https://github.com/user-attachments/assets/3b083dca-300c-4673-8530-cb0ebadc350a" width="400" height="300"/>

2. Download the Windows Media Creation Tool, which will allow you to create an ISO file <br> <img src="https://github.com/user-attachments/assets/95dd5f95-d6fd-483b-936c-6deaecc97dbe" width="400" height="300"/>


3. Return to VirtualBox and create a new virtual machine, make sure to use the previously downloaded Windows ISO file as the ISO image in VirtualBox
4. Power on the virtual machine and wait for Windows to install  <br><img src="https://github.com/user-attachments/assets/a8d42c1b-a895-41ee-bf6f-041a713960a5" width="400" height="300"/>

5. Download Sysmon from the Sysinternals website. Save the SysmonConfig.xml file (use a pre-configured file from sources like SwiftOnSecurity's GitHub).
6. In powershell, navigate to the Sysmon directory and install Sysmon with the xml file. Command: .\sysmon64.exe -i sysmonconfig.xml
7. Create a Digital Ocean Droplet by logging into your account and selecting Ubuntu as the operating system.
8. SSH into your Droplet using the following command: ssh root@your-droplet-ip
9. Install Wazuh on your Droplet. Follow the Wazuh documentation to set up Wazuh as a SIEM system for monitoring logs and security events.
10. Set up TheHive on your server to handle security incidents. Follow the installation guide from TheHive GitHub to integrate it into your SOC workflow.
11. Integrate Sysmon with Wazuh by setting up Filebeat on your Windows machine to forward Sysmon logs to Wazuh:
   - Install Filebeat on Windows using PowerShell.
   - Configure sysmon.yml in Filebeat to forward logs to Wazuh.
   - Example command: .\filebeat.exe setup
12. Automate alert generation by creating a Bash script to query ElasticSearch APIs for alerts, triggering incident response actions in TheHive. Example: curl -X GET "your-elastic-server:9200/_search?q=sysmon"
13. Create automated incident response workflows using Python and TheHive4py to send alerts to TheHive from Wazuh
14. Test your setup by generating security events (e.g., Nmap scans or PowerShell commands) on your Windows VM to trigger Sysmon logs and Wazuh alerts.
15. Create dashboards in Elastic to visualize logs and alerts, ensuring that your SOC automation workflow is working correctly.

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
