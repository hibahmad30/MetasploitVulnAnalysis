<h1>Metasploitable Vulnerability Assessment and Prioritization</h1>

<h2>Description</h2>
This project focuses on utilizing Nessus Essentials to conduct both credentialed and non-credentialed vulnerability scans on Metasploitable 2, an intentionally insecure virtual machine, from a Windows 10 environment. By analyzing the scan results, vulnerabilities are evaluated and prioritized based on severity, aiding in the development of a strategic remediation plan. This approach highlights the importance of thorough vulnerability assessment and effective risk management in maintaining secure systems.
<br />

<h2>Environments Used </h2>

- <b>Oracle VM VirtualBox</b>
- <b>Nessus Essentials Vulnerability Scanner</b>
- <b>Windows 10</b>
- <b>Rapid7 Metasploitable 2</b>
- <b>Microsoft Excel</b>
- <b>Pivot Tables</b>

<h2>Prerequisites:</h2> 

<p align="center">
The Nessus scan will be run on a Metasploitable 2 virtual machine from a Windows 10 virtual machine. Metasploitable 2 is an intentionally vulnerable Linux-based virtual machine used for testing and training in penetration testing and vulnerability assessments. In this analysis, the virtual machine will be created using Oracle VirtualBox, however any hypervisor can be used. The download links and instructions are provided as follows:<br /><br />Download Oracle VirtualBox: https://www.virtualbox.org/wiki/Downloads<br />Download Windows 10 Disc Image (ISO File): https://www.microsoft.com/en-us/software-download/windows10<br />Download Metasploitable 2: https://docs.rapid7.com/metasploit/metasploitable-2/
 <br/> 
 <br/>
Here are the configuration settings I used for the Metaspolitable 2 and Windows 10 virtual machines: 
 <br/> 
 <br/>
<img src="https://i.imgur.com/jYE5z1Z.png" alt="Configure Metasploitable VM"/>
<img src="https://i.imgur.com/32XCSgZ.png" alt="Configure Windows 10 VM"/>

<h2>Network Configuration and Testing:</h2> 

<p align="center">
Next, we need to configure both VMs to communicate with each other to ensure the scan functions properly. In VirtualBox, navigate to 'File > Tools > Network Manager > NAT Networks', then right-click to create a new NAT network and ensure that DHCP is enabled.
 <br/> 
 <br/>
Next, in the Metasploitable VM, run the 'ifconfig' command and note the IP address located after 'inet addr':
 <br/> 
 <br/>
<img src="https://i.imgur.com/QBaGUZ7.png" alt="Metasploitable VM IP address"/>
 <br/> 
 <br/>
From the Windows 10 virtual machine, open PowerShell and run the 'ping' command followed by the IP address of the Metasploitable virtual machine to test connectivity.
 <br/> 
 <br/>
<img src="https://i.imgur.com/Qo22DYP.png" alt="Test Network Connection"/>
 <br/> 
 <br/>
Additionally, on the Windows 10 virtual machine, navigate to the following link to download Nessus Essentials: https://www.tenable.com/products/nessus/nessus-essentials. Optionally, you can run the following command in PowerShell to verify the SHA-256 hash of the downloaded file against the provided hash on Tenable's download page for Nessus Essentials.
 <br/> 
 <br/>
<img src="https://i.imgur.com/GmA24Bw.png" alt="Verify Hash"/>

<h2>Uncredentialed scan:</h2> 
<p align="center">
In Nessus, click 'New Scan', then select 'Basic Network Scan'. Enter a name for the scan and provide the virtual machine's IPv4 address in the 'Targets' field. Nessus Essentials also allows the administrator to schedule scans at desired intervals and specify contacts to receive scan notifications. Once the scan is configured, click the 'Launch' icon to begin the scan.
<br />
<br />
<img src="https://i.imgur.com/27U9Nqw.png" alt="Nessus Scanner Page"/> 
<br />
<br />
The image below shows the results of the initial scan. Most of the vulnerabilities detected fall under the 'Info' severity, with several categorized as 'Medium' severity. Since the scan was performed without privileged credentials, it could not perform a deep analysis and missed many vulnerabilities. The next step is to conduct a credentialed scan for more comprehensive results.  
<br />
<br />
<img src="https://i.imgur.com/3ghN34P.png" alt="Uncredentialed Scan"/> 

<h2>Credentialed scan:</h2> 
 <p align="center">
To perform a credentialed scan, the virtual machine must be configured to accept authenticated scans. In Nessus, select the previously created scan, click 'More', then 'Configure'. Navigate to the 'Credentials' tab located next to 'Settings'. Set the 'Authentication method' to 'Password', and enter the username and password for the target Metasploitable virtual machine. After saving the credentials, launch the scan. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/W7IuPhL.png" alt="Credentialed Scan Configuration"/>
  <br/>
 <br/>
The image below shows the results of the credentialed scan, where the total number of vulnerabilities increased from 68 to 90: 
  <br/>
 <br/>
 <img src="https://i.imgur.com/7bVcbEM.png" alt="Credentialed Scan Vulnerabilities"/>
 <br/>
 <br/>
Here is a side-by-side comparison of the two scans, with the credentialed scan displayed on the right: 
  <br/>
 <br/>
 <img src="https://i.imgur.com/pZ7HRqC.png" height="35%" width="35%" alt="Uncredentialed Scan Results"/>     <img src="https://i.imgur.com/OALjpWr.png" height="35%" width="35%" alt="Credentialed Scan Results"/> 
  <br/>
 <br/>
For detailed analysis, click 'Generate Report', choose 'CSV', and select the desired columns. While Nessus also offers the option to download an HTML report, I chose CSV for the additional customization options.
 <br/>
 <br/>
 <img src="https://i.imgur.com/QjDmAlT.png" alt="Generate Report"/>
  <br/>
 <br/>

<h2>Remediation:</h2> 
 <p align="center">
Prior to running a vulnerability scan, it is essential to ensure that both the operating system and all third-party software are fully up-to-date. This will significantly reduce the number of vulnerabilities present in the scan results and will allow the analyst to have more efficient vulnerability management. To further maximize efficiency, be sure to set up automatic updates on your operating system and third-party software.
 <br/>
 <br/>
<img src="https://i.imgur.com/aMYixCT.png" height="60%" width="60%" alt="Uninstall Firefox"/>
<br />
<br />
 <img src="https://i.imgur.com/2aNsH9T.png" height="60%" width="60%" alt="Windows Updates"/>
 <br />
<br />
After uninstalling the deprecated Firefox and running Windows updates, I recieved the following scan result:
<br />
<br />
<img src="https://i.imgur.com/a1LVKQZ.png" height="60%" width="60%" alt="Windows Update Scan"/>
<br />
<br />
 <img src="https://i.imgur.com/XqWuev6.png" height="60%" width="60%" alt="Windows Update Scan"/>
<br />
<br />
I then investigated more specific vulnerabilities for remediation. I found a few more vulnerabilities related to out-of-date software and installed the necessary software updates.
<br />
<br />
<img src="https://i.imgur.com/lj5Xcv0.png" height="70%" width="70%" alt="3D Viewer Vulnerability"/>
<br />
<br />
 <img src="https://i.imgur.com/vY8a8LB.png" height="70%" width="70%" alt="3D Viewer Update"/>
<br />
<br />
<img src="https://i.imgur.com/Nv8j19h.png" height="70%" width="70%" alt="Onedrive Update"/>
<br />
<br />
Missing or misconfigured registry keys can also cause vulnerabilities on a system. When configuring registry keys, it is crucial to ensure that the changes made align with the intended system or application settings and do not introduce unintended consequences or conflicts with existing configurations. To address the WinVerifyTrust Signature Validation Vulnerability (CVE-2013-3900), refer to the following link: https://msrc.microsoft.com/update-guide/vulnerability/CVE-2013-3900.
<br />
<br />
 <img src="https://i.imgur.com/Zrx2GOj.png" height="70%" width="70%" alt="WinVerifyTrust Vulnerability"/>
<br />
<br />
<img src="https://i.imgur.com/0eXXMzV.png" height="70%" width="70%" alt="WinVerifyTrust Remediation"/>
<br />
<br />
I also addressed the 'SMB Signing not required' vulnerability, which has a 'Medium' severity level. This involved navigating to the 'Local Security Policy' and making the necessary configuration changes. The following link provides more information on how to resolve this vulnerability: https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/microsoft-network-server-digitally-sign-communications-always.
<br />
<br />
<img src="https://i.imgur.com/AtNAkOd.png" height="70%" width="70%" alt="SMB Signing Vulnerability"/>
<br />
<br />
 <img src="https://i.imgur.com/lOaujty.png" height="70%" width="70%" alt="SMB Signing Remediation"/>
<br />
<br />
<img src="https://i.imgur.com/3zNJDy1.png" height="70%" width="70%" alt="SMB Signing Remediation"/>
<br />
<br />
After addressing these more specific vulnerabilities on the target system, I launched the final Nessus scan:
<br />
<br />
<img src="https://i.imgur.com/X4tXd1e.png" height="60%" width="60%" alt="Final Scan Results"/>
<br />
<br />
 <img src="https://i.imgur.com/0rygP8U.png" height="60%" width="60%" alt="Final Scan Results"/>
<br />
<br />
Rather than having 5 'Medium' level vulnerabilities as in the previous scan, there are now 0. The 'High' severity vulnerabilities decreased from 16 to 3, and the 'Critical' level vulnerabilities decreased from 2 to 1. Depicted below is a comparison of the previous scan after Firefox was uninstalled and Windows updates were run (left) to the most recent scan where more specific remediations were also made (right):
<br />
<br />
<img src="https://i.imgur.com/cVK2O89.png" height="35%" width="35%" alt="Update Scan Results"/>     <img src="https://i.imgur.com/229wO2C.png" height="35%" width="35%" alt="Remediation Scan Results"/>
<h2>Key takeaways:</h2>
 Credentialed scans are essential for identifying vulnerabilities in a system, as they allow Nessus to access and scan all parts of the system, including system files, registry entries, and application settings. This results in a more comprehensive and accurate assessment of the system's vulnerabilities.
<br/>
<br/>
Deprecated software is a major source of vulnerabilities, making it crucial to keep all software up to date, including your operating system, browsers, and applications. Deprecated software often contains known security vulnerabilities that have been addressed in newer versions. While vulnerability remediation does often involve applying patches and updating software, it may also require changing system configurations. Nessus scans can be used to identify these vulnerabilities and remediate them. 
<br/>
<br/>
Regularly scanning your systems on an ongoing basis is also crucial to address any new vulnerabilities that may appear. The vulnerability management lifecycle is a powerful tool used to ensure that vulnerabilities are addressed continuously: 
<br/>
<br/>
In the planning stage of vulnerability management (Stage 0), any stakeholders and/or resources that will be involved should be identified. Guidelines and metrics outlining any goals or expectations should also be provided. In this project, the target system was a Windows 10 virtual machine, and the goal was to gain an overview of how vulnerability management works. 
<br/>
<br/>
In this project, the target system was a Windows 10 virtual machine, and the goal was to understand how various configuration changes to both the Windows 10 machine and Nessus can result in varying scan results. Another goal was to explore different vulnerability remediation techniques.
<br/>
<br/>
In the asset discovery phase (Stage 1), an inventory of all hardware and software assets should be created and/or updated regularly. It is common to use specialized solutions for asset management. This stage also involves scanning the assets for vulnerabilities. In this project, the primary asset was the Windows 10 virtual machine, and Nessus Essentials was used as the vulnerability scanner. 
<br/>
<br/>
Stage 2 of the vulnerability management lifecycle is vulnerability prioritization, where the vulnerabilities discovered in Stage 1 are prioritized based on their severity level. This stage focuses on maximizing efficiency. The prioritization is often based on threat intelligence sources such as CVE or CVSS, and may also be based upon how critical the particular asset is, and how likely it is to be exploited. In this project, the prioritization was primarily based on the severity levels provided by Nessus Essentials. 
<br/>
<br/>
Stage 3 involves vulnerability resolution, which involves remediation, mitigation, or acceptance. Remediation may involve patching software or making changes to system configurations, and essentially removing the vulnerability from the system entirely. Mitigation either lessens the severity of the vulnerability or makes it more challenging to exploit, but does not remove it from the system. For vulnerabilities that are less severe or not as high of a priority, they may just be accepted rather than being remediated or mitigated. This project focused on remediation primarily, however it is still crucial to understand mitigation and acceptance when working on larger scale systems in an organization. 
<br/>
<br/>
Stage 4, or the verification and monitoring phase, involves scanning the systems again to ensure that their remediation and mitigation efforts were effective. This is also to ensure that there were no new vulnerabilities created while making any changes to the system. 
<br/>
<br/>
The final stage, reporting and improvement (Stage 5), involves documenting the vulnerabilities that were found, as well as their outcomes. This stage focuses on the most recent scans and actions taken in the life cycle, as vulnerability management is an ongoing process. It is very important to have clear, up-to-date, and comprehensive documentation, particularly when working with stakeholders.

<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
