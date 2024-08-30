<h1>Digital Forensics and Incident Response with Volatility Lab</h1>



<h2>Description</h2> 
In this lab, I was tasked with investigating a suspected malware intrusion using the tool Volatility. I began my investigation by opening the memory.dmp file with Volatility and using the command windows.netstat to reveal the machine's connections. The process ChromeSetup.exe immediately stood out as warranting further investigation. I took the IP address it was communicating with, 58[.]64[.]204[.]181, and searched it on VirusTotal, where I discovered that a majority of antivirus vendors flagged the communicating files as malicious, further raising my suspicion about this program. Next, I used Volatility's windows.cmdline command to determine the program's location and the PID (Process Identifier). The program was found to be running out of the user's Downloads folder, and the PID was 4628. The next step was to export the binary from the memory.dmp to my virtual machine for further analysis, which I accomplished using the -o flag and windows.dumpfiles command with --pid 4628. Once the suspicious binary was on my virtual machine, I took caution not to execute it and used the command sha256sum to obtain the program's hash. With the hash, I uploaded it to VirusTotal, where my suspicion was confirmed as the vast majority of vendors labeled the executable as malware. It was commonly seen as ChromeSetup.exe, further confirming the presence of this malware on the machine. I conducted additional open-source intelligence on the binary and the IP address it was communicating with. I discovered the IP to be based out of Hong Kong, the program was first created on 2019-12-01, and the primary domain the malware contacts: ddos[.]dnsb8[.]net.

<br />


<h2>Languages and Utilities Used</h2>

- <b>Volatility3</b> 
- <b>Linux Terminal</b>
- <b>VirusTotal</b>
<h2>Environments Used </h2>

- <b>Oracle Virtual Box for Virtual Machine</b>
- <b>Kali Linux</b>
<h2>Lab Overview</h2>

The scenario for the lab/walkthorugh:<br/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/47365d7c-b1da-4fe6-92ea-56bbd79d2a67" height="100%" width="100%" alt="Volatility Lab"/>
<br />
<br />
To begin the investigation with Volatility, I used to the -r flag with the memory.dmp file with the windows.netstat command that shows all the connections the programs are making. The program that jumped out me for further investigation was ChromeSetupexe.<br/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/9af82b0e-1ebe-49ae-9acb-79195f3301bb" height="100%" width="100%" alt="Volatility Lab"/>
<br />
<br />
A simple yet oftentimes effective part of the intial investiation is to use OSINT tools to see if the IP communcated with the appliaction is flagged as malicious. While the IP itself was not not flagged as malicious, under details, the communicating files were clearly malicious confirming suspicion that the program ChromeSetup and the IP address it is communicating with are malciious.<br/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/bb1869b1-9c40-4eae-93d6-07f4814fed1c" height="100%" width="100%" alt="Volatility Lab"/>

<br />
<br />
Now it is clear that the process warrants further investigation, so I pivoted back to Volatility and used the command windows.cmdline to determine where the program is on the machine and the PID or Process Identifier. This revealed the program to be run out of .../alex/Downloads\ChromeSetup.exe and the PID to be 4628.<br/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/166ab10b-625f-403d-ad34-b15563f8a545" height="100%" width="100%" alt="Volatility Lab"/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/9eba8250-53a8-4dac-a9ea-7e2e537afe68" height="100%" width="100%" alt="Volatility Lab"/>
<br />
<br />
In order obtain more information about the program for the investigation, I went to help page of volatility to see if I can export the binary for further analysis and discoered with the -o flag and windows.dumpfiles I am able to extract it.<br/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/0dfa2a8c-780d-4f07-83dd-a73187b14492" height="100%" width="100%" alt="Volatility Lab"/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/9ca22b20-475e-4d81-b6b2-9a73f436f273" height="100%" width="100%" alt="Volatility Lab"/>
<br />
<br />
The binary if safely downloaded in a virtual machine. I used sha256sum on the suspicious binary to obtian the hash. With the hash I uploaded it to VirusTotal and confirmed the suspicion that it is a malware, particualrly an info stealer.<br/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/be22033c-6b71-41c5-a3b4-de5baa2c29c0" height="100%" width="100%" alt="Volatility Lab"/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/1d9fbbd5-a5bb-4043-8063-34ce2f017374" height="100%" width="100%" alt="Volatility Lab"/>
<br />
<br />
A couple other important bits of evidence that can be helpful in the investigation, report or future defense from the same or similar threats is dicovering the contacted URLs in virus total which shows one particular domain ddos[.]dnsb8[.]net, that should likely be entirely blocked, and seeing that the traffic origianted in Hong Kong. There may potentially be no legitmate traffic for that geographic region, resulting in the abiluty to block IP traffic from that region as a defensive measure.<br/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/493d5e84-a937-4be1-9c11-e1714f338a0d" height="100%" width="100%" alt="Volatility Lab"/>
<img src="https://github.com/KirkDJohnson/Volatility-Incident-Response-Lab/assets/164972007/a4268229-4a2a-413a-8988-c48dfd7ff9dc" height="100%" width="100%" alt="Volatility Lab"/>
<br />
<br />


<h2>Thoughts</h2>
This lab/walkthrough provided me with my first hands-on experience with the tool Volatility. I know that it is mainly a digital forensics and incident response tool; however, there may be times as a SOC analyst where using or having familiarity with a tool like Volatility can be beneficial. It was pretty straightforward, and I used the help page of Volatility to learn the flags and commands necessary to complete the lab. Incorporating OSINT in so many different labs, I can say I am proficient in this area now, as I know where to look and what to look for when determining if something may be malicious at the surface level. Overall, this was a good lab as I gained hands-on experience with a popular DFIR tool that I may come across or use in the future.
