# SOAR EDR Lab

## Objective
The SOAR EDR Lab aimed to integrate Security Orchestration, Automation, and Response (SOAR) with Endpoint Detection and Response (EDR) by using tools such as Tines (SOAR) and LimaCharlie (EDR). Its purpose was to handle alerts from endpoint devices, orchestrate actions (such as isolating a machine and sending Slack or email notifications), and reduce manual SOC workload. The lab focused on creating automated playbooks for incidents like credential harvesting, with the goal of achieving faster, more consistent security operations and a stronger cybersecurity posture.

## Skills Learned
- Advanced understanding of Security Orchestration, Automation, and Response (SOAR) and Endpoint Detection and Response (EDR), along with their practical applications
- Configuration of detection rules, monitoring of endpoint behavior, and collection of telemetry data using LimaCharlie
- Creation of automated workflows (playbooks) for incident response
- Use of Tines to trigger actions based on EDR alerts
- Development of critical thinking and problem-solving skills in cybersecurity

## Tools Used
- Tines as the Security Orchestration, Automation, and Response (SOAR) platform
- LimaCharlie as the Endpoint Detection and Response (EDR) solution
- Windows Server 2022 virtual machine with internet connectivity

## Steps
### 1. Creating a Playbook Workflow
The objective of the playbook used in the lab was to send a Slack message and an email containing information about the detection that LimaCharlie had generated. Tines then prompted the user to decide whether to isolate the machine. If the user selected yes, LimaCharlie automatically isolated the machine. Refer to Figure 1 for the playbook workflow used in the lab. The diagram was created using Draw.io.

<img width="725" height="812" alt="image" src="https://github.com/user-attachments/assets/8f86dbf9-2003-4c02-862d-95592b828269" />

Figure 1. Lab Playbook Workflow

### 2. Installing and Setting Up LimaCharlie
After creating an account on [limacharlie.io](https://limacharlie.io/), an organization named 'SOAREDR-Lab' was created. In LimaCharlie, an installation key with the same name (SOAREDR-Lab) was generated. On the Windows Server 2022 virtual machine, the Windows 64-bit sensor was downloaded from LimaCharlie. The sensor key was then copied from LimaCharlie. Once the sensor installer was downloaded to the Windows Server 2022 virtual machine, the sensor was installed using the copied key through PowerShell. 
