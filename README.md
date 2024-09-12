# SOC Automation Lab

Description:
------------
This lab is designed to simulate a Security Operations Center (SOC) environment with a focus on automating key tasks and processes. It includes scripts and tools for automating threat detection, incident response, and log analysis, leveraging technologies such as PowerShell, VirtualBox, Sysmon, and Bash. The lab also integrates with various security platforms and APIs to streamline workflows, reduce manual effort, and enhance the efficiency of SOC operations. This project is aimed at both learning and demonstrating the implementation of automation in a SOC setting, showcasing how automation can significantly improve security posture and operational effectiveness.

![Screenshot 2024-08-29 140646](https://github.com/user-attachments/assets/ebe31b15-6f0f-472f-b616-a7dcd0e7926d)
Steps:

-------
1. Download & install VirtualBox  ![VirtualBox](https://github.com/user-attachments/assets/3b083dca-300c-4673-8530-cb0ebadc350a)

2. Download the Windows Media Creation Tool, which will allow you to create an ISO file  ![Windows](https://github.com/user-attachments/assets/95dd5f95-d6fd-483b-936c-6deaecc97dbe)

3. Return to VirtualBox and create a new virtual machine, make sure to use the previously downloaded Windows ISO file as the ISO image in VirtualBox
4. Power on the virtual machine and wait for Windows to install   ![InVirtualBox](https://github.com/user-attachments/assets/a8d42c1b-a895-41ee-bf6f-041a713960a5)

5. Next download Sysmon & save the Sysmonconfig.xml file to your device
6. In powershell, navigate to the Sysmon directory and install Sysmon with the xml file. Command: .\sysmon64.exe -i sysmonconfig.xml
7. Navigate to digital ocean and begin creating your cloud server (droplet)
8. 

Tools Used:
----------
wazuh - A free and open-source integrated Security Information and Event Management (SIEM) and Extended Detection and Response (XDR) soloution.  
VirtualBox - A free and open-source software that allows you to create and run virtual machines.  
SysMon - A windows system service that logs detailed information about system activity.  
TheHive - An open-source Security Incident Response Platform (SIRP) designed to help manage and respond to security incidents.  
Digital Ocean - A cloud infrastructure provider offering scalable virtual servers (Droplets) and managed services.
