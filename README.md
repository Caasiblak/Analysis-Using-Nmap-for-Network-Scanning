# Analysis-Using-Nmap-for-Network-Scanning
The analysis employed various scanning techniques, including service enumeration, version detection, script scanning, and host discovery to identify potential security vulnerabilities within the network infrastructure.
Technical Cyber Security Report  


1. Executive Summary
Overview:


This technical report documents a comprehensive network scanning assessment conducted on 192.168.0.168 using Nmap. The analysis employed various scanning techniques, including service enumeration, version detection, script scanning, and host discovery to identify potential security vulnerabilities within the network infrastructure. The assessment aimed to provide a detailed inventory of active hosts, open ports, running services, and their versions to support security posture improvement.

Key Findings:


Identified two live hosts (192.168.0.1 and 192.168.0.168 [Caasi]) in the target network
Discovered multiple open ports and services on both hosts, including HTTP (80), HTTPS (443), and domain (53) services on the gateway device
Located several potentially vulnerable Windows services on the Caasi host (192.168.0.168), including: Microsoft Windows RPC (135/tcp), NetBIOS-SSN (139/tcp), Microsoft-DS (445/tcp), Microsoft HTTPAPI (5357/tcp)
Detected hardware manufacturer information: Guangzhou Tozed Kangwei Intelligent Technology
Network infrastructure appears to be a standard Windows-based environment with minimal security hardening


2. Background and Objectives
Project Context:


This network scanning project was initiated to assess the security posture of the internal network infrastructure at 192.168.0.168/24. By conducting a thorough network reconnaissance, we aimed to establish a baseline of network assets, identify potential security weaknesses, and provide actionable recommendations for enhancing the overall security posture. Nmap was selected as the primary tool due to its versatility, reliability, and comprehensive scanning capabilities. detection system.


Objective of the Tool Use:
The specific objectives of using Nmap for this assessment were:
To identify all active hosts within the target subnet
To discover open ports and services running on these hosts
To determine service versions and operating system information
To perform basic vulnerability assessment using built-in scripts
To document the network topology for security analysis and hardening

3. Methodology
3.1 Tool Configuration
Nmap Configuration
Multiple Nmap scanning configurations were utilized to achieve a comprehensive assessment of the target network. The tool was run with various command-line options to gather different types of information:
Basic Network Scanning: nmap -Pn 192.168.0.168/24 : Used to identify active hosts in the subnet while treating all hosts as online
Service Version Detection: nmap -sV 192.168.0.168: Employed to determine specific versions of services running on target hosts
Script Scanning: nmap -sC 192.168.0.168: Leveraged Nmap's built-in scripts to gather additional information and perform basic vulnerability assessment
Verbose Output Scanning: nmap -vv 192.168.0.168: Generated detailed information about the scanning process and results
Host List Scanning: nmap -sL 192.168.0.168/24: Listed all potential hosts in the subnet without sending packets
Ping Scanning: nmap -sn 192.168.0.168/24
Performed host discovery without port scanning
All scans were executed through Zenmap, Nmap's graphical user interface, which facilitated easier visualization and analysis of scan results.







3.2 Execution Process
The network scanning process was executed systematically through the following steps:
Initial Host Discovery: First, a basic ping scan (-sn) was performed to identify live hosts within the subnet without conducting port scanning.
Comprehensive Port Scanning: For each identified host, a full port scan was conducted to enumerate open ports and services.
Service Version Detection: The -sV flag was used to determine the specific versions of services running on open ports.
Script-Based Assessment: The -sC flag was employed to run Nmap's default script set against the targets, gathering additional information about services and potential vulnerabilities.
Detailed Output Generation: Verbose output options (-v and -vv) were used to capture comprehensive details of the scanning process and results.
Throughout the execution, care was taken to minimize network disruption while maximizing information gathering.
3.3 Monitoring and Analysis:
System Monitoring:
During the scanning process, Nmap monitored and logged various system attributes of target hosts, including:
Operating system detection (identified Windows on the Caasi host)
System uptime and latency measurements
Service configurations and security settings
Hardware identifiers (MAC addresses)

Network Monitoring:
Network-level monitoring captured:
Active hosts and their responsiveness
Open ports and accessible services
Network topology and relationships between hosts
Communication protocols and potential security weaknesses
All monitoring was performed passively, with data analysis conducted through Zenmap's built-in visualization tools.

4. Findings and Analysis
4.1 Discovered Hosts and Network Infrastructure:
Gateway Device (192.168.0.1)
Appears to be a router or gateway device
MAC Address: 98:A9:42:6E:F8 (Guangzhou Tozed Kangwei Intelligent Technology)

Open ports:
53/tcp (domain)
80/tcp (http)
443/tcp (https)

Low latency (0.0078s) suggesting local network device
Host "Caasi" (192.168.0.168)
Windows-based system
Open ports:
135/tcp (msrpc) - Microsoft Windows RPC
139/tcp (netbios-ssn) - NetBIOS Session Service
445/tcp (microsoft-ds) - Microsoft-DS (SMB file sharing)
5357/tcp (wsdapi) - Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
Latency varied between 0.0015s and 0.0062s depending on the scan type

4.2 Behavioral Analysis:
Service Analysis
Gateway Device Services
The gateway device (192.168.0.1) presented several standard network services:
DNS services (53/tcp) for name resolution
Web interface (80/tcp) likely for device administration
Secure web interface (443/tcp) also likely for administration
These services align with typical router/gateway functionality, providing internet connectivity and basic network services.
Windows Host Services
The Caasi host (192.168.0.168) showed several standard Windows networking services:
Microsoft RPC (135/tcp): Used for inter-process communication
NetBIOS-SSN (139/tcp): Legacy Windows networking service
Microsoft-DS (445/tcp): SMB file sharing service
WSDAPI (5357/tcp): Web Services on Devices API

The script scan (-sC) revealed additional information:
SMB security mode: Message signing enabled but not required (3:1:1)
SMB time information: Date 2025-04-09T13:42:34
Clock skew: -1s
4.3 Risk and Impact Assessment:
Based on the services discovered, several potential security concerns are identified:
SMB Services Exposure:
The presence of open SMB ports (139/tcp and 445/tcp) could expose the system to various SMB-related vulnerabilities if not properly patched. SMB message signing is enabled but not required, which could potentially allow man-in-the-middle attacks.
RPC Service Exposure: Open RPC service (135/tcp) could potentially be exploited for remote code execution if vulnerable.
Limited Network Segmentation: Both the gateway and Windows host are on the same subnet with no apparent segmentation.
Default Configurations: Services appear to be running with standard configurations without security hardening.
The overall risk level is assessed as MODERATE, with potential for unauthorized access and lateral movement if external threat actors gain initial access to the network.

5. Recommendations
1 Service Hardening:
Configure SMB message signing to be required rather than just enabled.
Review and restrict RPC services to only necessary functions.
Disable legacy NetBIOS services if not required for operations.
2. Firewall Configuration:
Implement host-based firewall rules to restrict access to Windows services (ports 135, 139, 445, 5357) to only trusted hosts.
Configure the gateway device to block these ports from external access.
3. Patch Management:
Ensure all Windows systems are fully patched, particularly for SMB-related vulnerabilities.
Update router/gateway firmware to the latest stable version.
5.2 Long-Term Mitigation:
Service Minimization:
Evaluate all running services and disable those not absolutely necessary for operations.
Consider moving file sharing to a dedicated, hardened file server rather than using workstation-based sharing

6. Conclusion
The network scanning assessment of the 192.168.0.168/24 subnet successfully identified two active hosts with multiple running services. The network appears to be a standard Windows-based environment with default configurations and minimal security hardening. Several potentially vulnerable services were identified, particularly Windows SMB and RPC services, which could pose security risks if not properly secured.
Immediate action should be taken to harden the identified services, particularly by implementing firewall rules to restrict access to sensitive Windows networking services. Long-term security improvements should focus on network segmentation, regular patch management, and enhanced monitoring capabilities. This assessment provides a foundation for understanding the current network posture, but ongoing security maintenance and periodic reassessment will be essential to maintain an effective security posture as the environment evolves.








Appendix: Additional Data
A.1 Raw Nmap Command Outputs
Basic Network Scan:
nmap -Pn 192.168.0.168/24

Results showed comprehensive host discovery across the subnet.
Version Detection Scan:
nmap -sV 192.168.0.168

Identified:
135/tcp open msrpc Microsoft Windows RPC
139/tcp open netbios-ssn Microsoft Windows netbios-ssn
445/tcp open microsoft-ds?
5357/tcp open http Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
Service Info: OS: Windows; CPE: cpe:/o:microsoft
Script Scan:
nmap -sC 192.168.0.168

Revealed SMB security settings including:
smb2-security-mode: 3:1:1 (Message signing enabled but not required)
smb2-time: date: 2025-04-09T13:42:34
Clock-skew: -1s
A.2 Network Topology Visualization
The network appears to follow a standard home/small office topology:
Gateway Device (192.168.0.1) providing internet connectivity and basic network services
Windows Host (Caasi - 192.168.0.168) connected to the gateway
Both devices on the same subnet (192.168.0.0/24)
A.3 Scan Performance Metrics
Scan Completion Times:
Host Discovery (-sn): 7.31 seconds for 256 IP addresses
Service Version Scan (-sV): 20.24 seconds for single host
Script Scan (-sC): 46.67 seconds for single host
Verbose Scan (-vv): 5.95 seconds, sending 1000 packets (44.000KB) and receiving 2004 packets (84.176KB)
These metrics provide baseline performance data for future scanning operations and network growth assessment.
