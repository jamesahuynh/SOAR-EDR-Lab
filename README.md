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

### 5. Setting Up Slack and Tines
On Slack, a workspace was created and named 'SOAREDR-Lab'. A new public channel was created named 'alerts'. Once a detection was received from LimaCharlie on Tines, Tines will then send a message over to Slack in the 'alerts' channel. On Tines, an account was created. A new playbook draft (called a story in Tines) was created to establish a link between LimaCharlie and Tines. A webhook was added to the playbook and named 'Retrieve Detections'. The webhook URL was copied. In LimaCharlie, a detection output stream named 'SOAREDR-Lab' was created using the copied webhook URL from Tines. Tines was selected as the output destination. LaZagne was executed in PowerShell on the Windows Server 2022 virtual machine to generate a test event and confirm the connection between LimaCharlie and Tines. Refer to Figure 6 for the retrieved detection event in Tines.

<img width="468" height="224" alt="image" src="https://github.com/user-attachments/assets/139efcd2-57a0-4027-ba31-f093630bcf3b" />

Figure 6. Retrieved Detection Event in Tines

### 6. Creating a Playbook
On Slack, the Tines app was added. On Tines, a new credential was added using the Tines app for Slack. A Slack action was added to the playbook and 'Send a message' was selected as the action. The channel ID of the 'alerts' channel in Slack was copied into the Slack action. The Slack action was connected to the 'Retrieve Detections' webhook. Slack was specified to send a message with the contents defined in Figure 1. Refer to Figure 7 for the contents of the Slack message.

<img width="470" height="148" alt="image" src="https://github.com/user-attachments/assets/565fe709-f09f-41ac-b78a-f76d44afbd4d" />

Figure 7. Slack Message Contents

A 'Send email' action was added to the playbook. A disposable email was set as the recipient of the action. The 'Send email' action was connected to the webhook. An email was specified to be sent with a message containing the contents defined in Figure 1. Refer to Figure 8 for the contents of the email sent.

<img width="437" height="234" alt="image" src="https://github.com/user-attachments/assets/5aad893a-f47a-4c75-8e9b-54d9268f2dc4" />

Figure 8. Email Contents

A page was added to the playbook and named 'User Prompt'. The page was connected to the webhook. The page was edited to prompt the user if they wanted to isolate their machine. A boolean button was added to prompt the user 'Yes' or 'No'. A trigger action if the user selected 'No' was added to the playbook and connected to the user prompt. A Slack action was connected to the trigger action to tell the user that their machine was not isolated. Another trigger action if the user selected 'Yes' was added to the playbook and connected to the user prompt. A LimaCharlie action was added to the playbook and connected to the trigger action. 'Isolate sensor' was set as the LimaCharlie action. A new credential for LimaCharlie was added to Tines using the organization API key from LimaCharlie. A Slack action was added to the playbook and connected to the LimaCharlie action to tell the user that their machine was isolated. A test run was executed to evaluate the output if the user selected 'No' on whether to isolate their machine. In LimaCharlie, the network access was specified as 'Isolated' (Refer to Figure 9).

<img width="220" height="68" alt="image" src="https://github.com/user-attachments/assets/57f75de7-1bcc-4682-ad3f-87bbecd9b86f" />

Figure 9. LimaCharlie Network Access Status

On the Windows Server 2022 virtual machine, pings to YouTube resulted in general failure (Refer to Figure 10). 

<img width="402" height="99" alt="image" src="https://github.com/user-attachments/assets/6835f141-3987-433b-9495-f480e03e02b3" />

Figure 10. Ping Output
