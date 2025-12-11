# SOAR EDR Lab

## Objective
The SOAR EDR Lab aimed to integrate Security Orchestration, Automation, and Response (SOAR) with Endpoint Detection and Response (EDR) by using tools such as Tines (SOAR) and LimaCharlie (EDR). Its purpose was to handle alerts from endpoint devices, orchestrate actions (such as isolating a machine and sending Slack or email notifications), and reduce manual SOC workload. The lab focused on creating automated playbooks for incidents like credential harvesting, with the goal of achieving faster, more consistent security operations and a stronger cybersecurity posture.

## Skills Learned
- Advanced understanding of Security Orchestration, Automation, and Response (SOAR) and Endpoint Detection and Response (EDR), along with their practical applications
- Configuration of detection rules, monitoring of endpoint behavior, and collection of telemetry data using LimaCharlie
- Creation of automated workflows (playbooks) for incident response
- Triggering actions with Tines based on EDR alerts
- Development of critical thinking and problem-solving skills in cybersecurity

## Tools Used
- Tines as the Security Orchestration, Automation, and Response (SOAR) platform
- LimaCharlie as the Endpoint Detection and Response (EDR) solution
- Windows Server 2022 virtual machine with internet connectivity
- LaZagne, an open-source application used to retrieve passwords stored on a local computer, to generate telemetry

## Steps
### 1. Creating a Playbook Workflow
The objective of the playbook used in the lab was to send a Slack message and an email containing information about the detection that LimaCharlie had generated. Tines then prompted the user to decide whether to isolate the machine. If the user selected yes, LimaCharlie automatically isolated the machine. Refer to Figure 1 for the playbook workflow used in the lab. The diagram was created using Draw.io.

<img width="725" height="812" alt="image" src="https://github.com/user-attachments/assets/8f86dbf9-2003-4c02-862d-95592b828269" />

Figure 1. Lab Playbook Workflow

### 2. Installing and Setting Up LimaCharlie
After creating an account on [limacharlie.io](https://limacharlie.io/), an organization named 'SOAREDR-Lab' was created. In LimaCharlie, an installation key with the same name (SOAREDR-Lab) was generated. On the Windows Server 2022 virtual machine, the Windows 64-bit sensor was downloaded from LimaCharlie. The sensor key was then copied from LimaCharlie. After downloading the sensor installer to the Windows Server 2022 virtual machine, the sensor was installed using the copied key through PowerShell. 

### 3. Generating Telemetry using LaZagne
The LaZagne project is an open-source application used to retrieve passwords stored on a local computer. It has been developed with the purpose of finding passwords for the most commonly used software. On the [LaZagne GitHub page](https://github.com/AlessandroZ/LaZagne), LaZagne was installed on the Windows Server 2022 virtual machine. LaZagne was executed through PowerShell to generate a process that was detected by LimaCharlie. Refer to Figure 2 for LaZagne executed in PowerShell. Refer to Figure 3 for the new process event detected by LimaCharlie.

<img width="800" height="515" alt="image" src="https://github.com/user-attachments/assets/d1ec6590-4005-493e-a4d5-05205f8893c3" />

Figure 2. LaZagne

<img width="980" height="498" alt="Screenshot from 2025-12-11 17-08-33" src="https://github.com/user-attachments/assets/cb2b7c1c-912b-468e-a8ae-05c43ba4b4ba" />

Figure 3. LaZagne Process in LimaCharlie

### 4. Creating a Detection and Response Rule
In LimaCharlie, a new detection and response rule was created using a pre-existing Windows process creation rule. Refer to Figure 4 for the Windows process creation rule that served as the template for the new detection and response rule.

<img width="500" height="478" alt="image" src="https://github.com/user-attachments/assets/1ccee986-8879-4818-b6bc-91cde41daf0b" />

Figure 4. Windows Process Creation Rule

Events for the new rule were specified in LimaCharlie. The event type must be either 'New_Process' or 'Existing_Process', and it must be on Windows. The file path must end with 'lazagne.exe', the command line must end with 'all', the command line must contain 'lazagne', or the hash must match LaZagne's known hash. Refer to Figure 5 for the specified events for detection. 

<img width="1029" height="390" alt="image" src="https://github.com/user-attachments/assets/0dab94e6-a4c9-4579-a99b-81cffa580a8f" />

Figure 5. Specified Events for Detection

The rule's response action was set to 'report'. If a certain event met the detection criteria for the rule, a detection would be generated under the 'Detections' tab in LimaCharlie. The rule was saved and named 'SOAREDR-Lab-LaZagne'.
