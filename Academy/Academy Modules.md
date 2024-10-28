#05/2024 - This is the start of Matthew's Hack The Box Journey

# HTB Certified Penetration Testing Specialist (HTB CPTS)
	think outside the box, not just search for CVEs

## Syllabus
**Introduction**
1. [[#Penetration Testing Process]]
2. Getting Started
**Reconnaissance, Enumeration & Attack Planning**
3. Network Enumeration with Nmap
4. Footprinting
5. Information Gathering - Web Edition
6. Vulnerability Assessment
7. File Transfers
8. Shells & Payloads
9. Using the Metasploit Framework
**Exploitation & Lateral Movement**
10. Password Attacks
11. Attacking Common Services
12. Pivoting, Tunneling, and Port Forwarding
13. Active Directory Enumeration & Attacks
**Web Exploitation**
14. Using Web Proxies
15. Attacking Web Applications with ffuf
16. Login Brute Forcing
17. SQL Injection Fundamentals
18. SQLMap Essentials
19. Cross-Site Scripting (XSS)
20. File Inclusion
21. File Upload Attacks
22. Command Injections
23. Web Attacks
24. Attacking Common Applications
**Post-Exploitation**
25. Linux Privilege Escalation
26. Windows Privilege Escalation
**Reporting & Capstone**
27. Documentation & Reporting
28. Attacking Enterprise Networks

--------------------------------------------------------------------------

## Penetration Testing Process
	Module 1
### Core concepts
+ External Pentest
+ Internal Pentest (both network and Active Directory)
+ Web Application Security Assessments.
### Real-world targets
+ [HackerOne](https://hackerone.com/directory/programs)
+ [Bugcrowd](https://bugcrowd.com/programs)

> While rare, some criminal organizations may pose as legitimate companies to recruit talent to assist with **illegal actions**. If you participate, even if your intentions are good, you can still be liable and get into legal and even criminal trouble

### After landing first Pentesting job
+ Stay within the scope of testing
+ If in doubt, reach out
+ In writing, not just given on a phone call
+ Always work ethically and within the bounds of the law
+ Document, document, document

### Pentesting Process
![[Pasted image 20240804134519.png]]
#### Pre Engagement 
This is the stage where these things are being documented: 
+ Main commitments
+ Tasks 
+ Scope 
+ Limitations 
+ Related agreements
Contractual documents are drawn up, and essential information is exchanged that is relevant for penetration testers and the client, depending on the type of assessment.
[[#List of Documents in the pre-engagement process]]


#### Information Gathering
Before any target system is examined/attacked, we must identify them. Customer may not give us any info about their network or components, so that is our job to get an overview of them.
Information gathering influence the results of the `Exploitation` stage.

Most web applications nowadays are dynamic and include `JavaScript`, which we must also be familiar with to handle the dynamics of the web page correctly. JavaScript is a very popular programming language and is often obfuscated to make it difficult for attackers (and defenders) to understand the exact functionality of the code.

Nowadays, most companies use a structured way of managing hundreds or thousands of computers and users. `Active Directory` is used to simplify and speed up management for administrators.
> Time, patience, and personal commitment all play a significant role in information gathering

We can obtain the necessary information by using these categories:
+ OSINT (Open Source Intelligence)
+ Infrastructure Enumeration
+ Service Enumeration
+ Host Enumeration

##### Open-Source Intelligence (OSINT)
Every freely available sources
We can find:
+ password
+ hashes
+ keys
+ tokens

##### **Private and Public SSH Keys**
![[Pasted image 20240819161320.png]]

##### Infrastructure Enumeration
Purpose: To understand how their infrastructure is structured.
Include: 
+ name servers
+ mail servers
+ we servers
+ cloud instances
Compare what we got to the scope

##### Service Enumeration
Like the name, we find version, information and reason it can be used.
Admin often prefer to accept the risk of leaving one or more vulnerabilities open and maintaining the functionality instead of closing the security gap

##### Host Enumeration
Examine every host in the scope
Identify: 
+ OS
+ Service
+ Version
We try to determine what role this host or server plays and what network components it communicates with, along with the services and port related.
When examine host/server from the inside, look for: 
+ files
+ local services
+ scripts
+ applications
+ information

##### Pillaging
After hitting the `Post-Exploitation` stage, pillaging is performed to collect sensitive information locally on the already exploited host, such as employee names, customer data, and much more. However, this information gathering only occurs after exploiting the target host and gaining access to it.


#### Vulnerability Assessment
Divided into two areas:
+ Approach to scan for **known vulnerabilities** using automated tools
	1. Nessus: A comprehensive vulnerability scanner that can identify vulnerabilities, configuration issues, and malware across various devices and applications.
	2. OpenVAS: An open-source tool that offers a range of vulnerability scanning capabilities and is often used for network vulnerability assessments.
	3. QualysGuard: A cloud-based suite of tools that provides vulnerability scanning, compliance monitoring, and web application scanning.
	4. Nmap (with NSE scripts): While primarily a network scanning tool, Nmap can use its scripting engine (NSE) to detect vulnerabilities.
	5. Nexpose: Developed by Rapid7, Nexpose offers vulnerability management and scanning solutions to identify and prioritize risks.
	6. Acunetix: A web vulnerability scanner that focuses on detecting vulnerabilities in web applications, including SQL injection, XSS, and more.
	7. OpenSCAP: An open-source framework for conducting vulnerability assessment, policy compliance, and security automation.
	8. Retina Network Security Scanner: A tool that provides network vulnerability scanning and assessment, detecting security weaknesses in various systems.
	9. Burp Suite: A popular web vulnerability scanner used by penetration testers and security professionals to identify security flaws in web applications.
	10. Nikto: An open-source web server scanner that performs comprehensive tests to detect various web vulnerabilities.
+ Analyze for **potential vulnerabilities** through the information found

We try to discover **gaps and opportunities** to trick the systems and applications to our advantage and gain **unintended** access or privileges.

From this stage, there are four path: 
+ Exploitation
	When we don't yet have access to the system, assume that we are already found 1 gap and ready to exploit it.
+ Post-Exploitation
	When we already on the target and escalating privileges.
+ Lateral Movement
	Under certain circumstances, privileges escalation is not strictly necessary because we can **move from the already exploited system through the network and attack other systems**
+ Information gathering
	When we don't have enough information on hand


An analysis is:
+ Detailed examination of an event or process
+ It origin + impact
+ Precaution + actions, can be triggered to support or prevent future occurrences

##### Analysis type
| `Descriptive`  | Descriptive analysis is essential in any data analysis. On the one hand, it describes a data set based on individual characteristics. It helps to detect possible errors in data collection or outliers in the data set.                                                                                                                                                |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Diagnostic`   | Diagnostic analysis clarifies conditions' causes, effects, and interactions. Doing so provides insights that are obtained through correlations and interpretation. We must take a backward-looking view, similar to descriptive analysis, with the subtle difference that we try to find reasons for events and developments.                                           |
| `Predictive`   | By evaluating historical and current data, predictive analysis creates a predictive model for future probabilities. Based on the results of descriptive and diagnostic analyses, this method of data analysis makes it possible to identify trends, detect deviations from expected values at an early stage, and predict future occurrences as accurately as possible. |
| `Prescriptive` | Prescriptive analytics aims to narrow down what actions to take to eliminate or prevent a future problem or trigger a specific activity or process.                                                                                                                                                                                                                     |


#### Exploitation
Often many companies and systems use the same applications but make different decisions about their configuration. This is because the same application can often be used for various purposes, and each organization will have different objectives.

From this stage, there are four paths we can take, depending on how far we have come:
+ Information Gathering
	==Regardless of how high of the privilege== we got, we need to gather information again using that privilege we got from exploit 
+ Post-exploitation
	This is kind of like the whole 4 stages but from the internal perspective. There is a direct jump to post-exploitation if we are already the ==highest privilege==.
+ Lateral Movement
	Well, this depends. If we have achieved the high privilege on a dual-homed system used to connect two networks, we can likely use this host to start enumerating hosts that were not previously available to us.
+ Proof-of-Concept (PoC)
	The last part after gaining the highest privilege of the target. We don't have to take over all systems. If we gain Domain Admin in AD, we can perform anything in the network.
	We can create the `Proof-of-Concept` from our notes to detail and potentially automate the paths and activities and make them available to the technical department.

#### Post-exploitation
> *a.k.a Privileges Escalation*

Services are typically configured in a certain way "isolated" to stop potential attackers, bypassing these restrictions is the next step we take in this stage.

Before we can begin escalating privileges, we must first get an overview of the inner workings of the exploited system. After all, we do not know which users are on the system and what options are available to us up to this point. This step is also known as `Pillaging`. This path is not optional, as with the others, but essential. Again, entering the `Information Gathering` stage puts us in this perspective. This inevitably takes us to the vulnerability assessment stage, where we analyze and evaluate the information we find.

Along with that, we can move on with the 3 step:
+ Exploit
	Exploit local applications or services with higher privileges to execute commands with those privileges. 
+ Lateral Movement
+ PoC

#### Lateral Movement
Lateral movement is one of the essential components for moving through a corporate network. 
+ Overlap other internal hosts
+ Privesc within current subnet or other part of network 
Lateral Movement requires access to at least one of the systems in the corporate network.

Path	Description
+  Vulnerability Assessment	
	If the penetration test is not finished yet, we can jump from here to the Vulnerability Assessment stage. Here, the information already obtained from pillaging is used and analyzed to assess where the network services or applications using an authentication mechanism that we may be able to exploit are running.
+ Information Gathering / Pillaging	
	After a successful lateral movement, we can jump into Pillaging once again. This is local information gathering on the target system that we accessed.
+ Proof-of-Concept
	Once we have made the last possible lateral movement and completed our attack on the corporate network, we can summarize the information and steps we have collected and perhaps even automate certain sections that demonstrate vulnerability to the vulnerabilities we have found.

#### Proof-of-Concept

The `Proof-Of-Concept` (`POC`) is merely proof that a vulnerability found exists. As soon as the administrators receive our report, they will try to confirm the vulnerabilities found by reproducing them. After all, no administrator will change business-critical processes without confirming the existence of a given vulnerability. A large network may have many interoperating systems and dependencies that must be checked after making a change, which can take a considerable amount of time and money. Just because a pentester found a given flaw, it doesn't mean that the organization can easily remediate it by just changing one system, as this could negatively affect the business. Administrators must carefully test fixes to ensure no other system is negatively impacted when a change is introduced. PoCs are sent along with the documentation as part of a high-quality penetration test, allowing administrators to use them to confirm the issues themselves.

At this point, we can only go to the `Post-Engagement` stage, where we optimize and improve the documentation and send it to the customer after an intensive review.

#### Post-Engagement
The `Post-Engagement` stage also includes cleaning up the systems we exploit so that none of these systems can be exploited using our tools.
it is essential to remove all content that we have transferred to the systems during our penetration test so that the corporate network is left in the same state as before our penetration test. We also should note down any system changes, successful exploitation attempts, captured credentials, and uploaded files in the appendices of our report so our clients can cross-check this against any alerts they receive to prove that they were a result of our testing actions and not an actual attacker in the network.

In addition, we have to reconcile all our notes with the documentation we have written in the meantime to make sure we have not skipped any steps and can provide a comprehensive, well-formatted and neat report to our clients.

-------------------------------------------------
### Testing Method
External: As an anonymous user from the internet
Internal: Within the network, could require physical presence.

### Type of Penetration Testing
| **Type**         | **Information Provided**                                                                                                                                                                                                                                              |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Blackbox`       | `Minimal`. Only the essential information, such as IP addresses and domains, is provided.                                                                                                                                                                             |
| `Greybox`        | `Extended`. In this case, we are provided with additional information, such as specific URLs, hostnames, subnets, and similar.                                                                                                                                        |
| `Whitebox`       | `Maximum`. Here everything is disclosed to us. This gives us an internal view of the entire structure, which allows us to prepare an attack using internal information. We may be given detailed configurations, admin credentials, web application source code, etc. |
| `Red-Teaming`    | May include physical testing and social engineering, among other things. Can be combined with any of the above types.                                                                                                                                                 |
| `Purple-Teaming` | It can be combined with any of the above types. However, it focuses on working closely with the defenders.                                                                                                                                                            |

### Type of Testing Environments
+ Network, Firewall, IDS/IPS
+ Web App, API
+ Mobile
+ IoT
+ Source Code
+ Security Policies, Employee, Physical Security
+ Hosts/Server
+ Cloud
+ Thick Clients

### Law and Regulation
|**Categories**|**USA**|**Europe**|**UK**|**India**|**China**|
|---|---|---|---|---|---|
|Protecting critical information infrastructure and personal data|[Cybersecurity Information Sharing Act](https://www.cisa.gov/resources-tools/resources/cybersecurity-information-sharing-act-2015-procedures-and-guidance) (`CISA`)|[General Data Protection Regulation](https://gdpr-info.eu/) (`GDPR`)|[Data Protection Act 2018](https://www.legislation.gov.uk/ukpga/2018/12/contents/enacted)|[Information Technology Act 2000](https://www.indiacode.nic.in/bitstream/123456789/13116/1/it_act_2000_updated.pdf)|[Cyber Security Law](https://digichina.stanford.edu/work/translation-cybersecurity-law-of-the-peoples-republic-of-china-effective-june-1-2017/)|
|Criminalizing malicious computer usage and unauthorized access to computer systems|[Computer Fraud and Abuse Act](https://www.justice.gov/jm/jm-9-48000-computer-fraud) (`CFAA`)|[Network and Information Systems Directive](https://www.enisa.europa.eu/topics/cybersecurity-policy/nis-directive-new) (`NISD`)|[Computer Misuse Act 1990](https://www.legislation.gov.uk/ukpga/1990/18/contents)|[Information Technology Act 2000](https://www.indiacode.nic.in/bitstream/123456789/13116/1/it_act_2000_updated.pdf)|[National Security Law](https://www.chinalawtranslate.com/en/2015nsl/)|
|Prohibiting circumventing technological measures to protect copyrighted works|[Digital Millennium Copyright Act](https://www.congress.gov/bill/105th-congress/house-bill/2281) (`DMCA`)|[Cybercrime Convention of the Council of Europe](https://www.europarl.europa.eu/cmsdata/179163/20090225ATT50418EN.pdf)|||[Anti-Terrorism Law](https://web.archive.org/web/20240201044856/http://ni.china-embassy.gov.cn/esp/sgxw/202402/t20240201_11237595.htm)|
|Regulating the interception of electronic communications|[Electronic Communications Privacy Act](https://www.congress.gov/bill/99th-congress/house-bill/4952) (`ECPA`)|[E-Privacy Directive 2002/58/EC](https://eur-lex.europa.eu/legal-content/EN/ALL/?uri=CELEX%3A32002L0058)|[Human Rights Act 1998](https://www.legislation.gov.uk/ukpga/1998/42/contents) (`HRA`)|[Indian Evidence Act of 1872](https://legislative.gov.in/sites/default/files/A1872-01.pdf)||
|Governing the use and disclosure of protected health information|[Health Insurance Portability and Accountability Act](https://aspe.hhs.gov/reports/health-insurance-portability-accountability-act-1996) (`HIPAA`)||[Police and Justice Act 2006](https://www.legislation.gov.uk/ukpga/2006/48/contents)|[Indian Penal Code of 1860](https://legislative.gov.in/sites/default/files/A1860-45.pdf)||
|Regulating the collection of personal information from children|[Children's Online Privacy Protection Act](https://www.ftc.gov/legal-library/browse/rules/childrens-online-privacy-protection-rule-coppa) (`COPPA`)||[Investigatory Powers Act 2016](https://www.legislation.gov.uk/ukpga/2016/25/contents/enacted) (`IPA`)|||
|A framework for cooperation between countries in investigating and prosecuting cybercrime|||[Regulation of Investigatory Powers Act 2000](https://www.legislation.gov.uk/ukpga/2000/23/contents) (`RIPA`)|||
|Outlining individuals' legal rights and protections regarding their personal data||||[Personal Data Protection Bill 2019](https://www.congress.gov/bill/116th-congress/senate-bill/2889)|[Measures for the Security Assessment of Cross-border Transfer of Personal Information and Important Data](https://www.mayerbrown.com/en/perspectives-events/publications/2022/07/china-s-security-assessments-for-cross-border-data-transfers-effective-september-2022)|
|Outlining individuals' fundamental rights and freedoms|||||[State Council Regulation on the Protection of Critical Information Infrastructure Security](http://english.www.gov.cn/policies/latestreleases/202108/17/content_WS611b8062c6d0df57f98de907.html)|

---

### Precautionary Measures during Penetration Tests

| `☐` | Obtain written consent from the owner or authorized representative of the computer or network being tested                                                     |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `☐` | Conduct the testing within the scope of the consent obtained only and respect any limitations specified                                                        |
| `☐` | Take measures to prevent causing damage to the systems or networks being tested                                                                                |
| `☐` | Do not access, use or disclose personal data or any other information obtained during the testing without permission                                           |
| `☐` | Do not intercept electronic communications without the consent of one of the parties to the communication                                                      |
| `☐` | Do not conduct testing on systems or networks that are covered by the Health Insurance Portability and Accountability Act (HIPAA) without proper authorization |

---------------------------------------------------------------------
### List of Documents in the pre-engagement process


The entire pre-engagement process consists of three essential components:

1. Scoping questionnaire
    
2. Pre-engagement meeting
    
3. Kick-off meeting
    

Before any of these can be discussed in detail, a `Non-Disclosure Agreement` (`NDA`) must be signed by all parties. There are several types of NDAs:

|**Type**|**Description**|
|---|---|
|`Unilateral NDA`|This type of NDA obligates only one party to maintain confidentiality and allows the other party to share the information received with third parties.|
|`Bilateral NDA`|In this type, both parties are obligated to keep the resulting and acquired information confidential. This is the most common type of NDA that protects the work of penetration testers.|
|`Multilateral NDA`|Multilateral NDA is a commitment to confidentiality by more than two parties. If we conduct a penetration test for a cooperative network, all parties responsible and involved must sign this document.|

Exceptions can also be made in urgent cases, where we jump into the kick-off meeting, which can also occur via an online conference. It is essential to know `who in the company is permitted` to contract us for a penetration test. Because we cannot accept such an order from everyone. Imagine, for example, that a company employee hires us with the pretext of checking the corporate network's security. However, after we finished the assessment, it turned out that this employee wanted to harm their own company and had no authorization to have the company tested. This would put us in a critical situation from a legal point of view.

Below is a sample (not exhaustive) list of company members who may be authorized to hire us for penetration testing. This can vary from company to company, with larger organizations not involving the C-level staff directly and the responsibility falling on IT, Audit, or IT Security senior management or the like.

|                               |                               |                                           |
| ----------------------------- | ----------------------------- | ----------------------------------------- |
| Chief Executive Officer (CEO) | Chief Technical Officer (CTO) | Chief Information Security Officer (CISO) |
| Chief Security Officer (CSO)  | Chief Risk Officer (CRO)      | Chief Information Officer (CIO)           |
| VP of Internal Audit          | Audit Manager                 | VP or Director of IT/Information Security |

| **Document**                                                         | **Timing for Creation**                             |
| -------------------------------------------------------------------- | --------------------------------------------------- |
| `1. Non-Disclosure Agreement` (`NDA`)                                | `After` Initial Contact                             |
| `2. Scoping Questionnaire`                                           | `Before` the Pre-Engagement Meeting                 |
| `3. Scoping Document`                                                | `During` the Pre-Engagement Meeting                 |
| `4. Penetration Testing Proposal` (`Contract/Scope of Work` (`SoW`)) | `During` the Pre-engagement Meeting                 |
| `5. Rules of Engagement` (`RoE`)                                     | `Before` the Kick-Off Meeting                       |
| `6. Contractors Agreement` (Physical Assessments)                    | `Before` the Kick-Off Meeting                       |
| `7. Reports`                                                         | `During` and `after` the conducted Penetration Test |

#### Contract Checklist

| **Checkpoint**                       | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `☐ NDA`                              | Non-Disclosure Agreement (NDA) refers to a secrecy contract between the client and the contractor regarding all written or verbal information concerning an order/project. The contractor agrees to treat all confidential information brought to its attention as strictly confidential, even after the order/project is completed. Furthermore, any exceptions to confidentiality, the transferability of rights and obligations, and contractual penalties shall be stipulated in the agreement. The NDA should be signed before the kick-off meeting or at the latest during the meeting before any information is discussed in detail. |
| `☐ Goals`                            | Goals are milestones that must be achieved during the order/project. In this process, goal setting is started with the significant goals and continued with fine-grained and small ones.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `☐ Scope`                            | The individual components to be tested are discussed and defined. These may include domains, IP ranges, individual hosts, specific accounts, security systems, etc. Our customers may expect us to find out one or the other point by ourselves. However, the legal basis for testing the individual components has the highest priority here.                                                                                                                                                                                                                                                                                              |
| `☐ Penetration Testing Type`         | When choosing the type of penetration test, we present the individual options and explain the advantages and disadvantages. Since we already know the goals and scope of our customers, we can and should also make a recommendation on what we advise and justify our recommendation accordingly. Which type is used in the end is the client's decision.                                                                                                                                                                                                                                                                                  |
| `☐ Methodologies`                    | Examples: OSSTMM, OWASP, automated and manual unauthenticated analysis of the internal and external network components, vulnerability assessments of network components and web applications, vulnerability threat vectorization, verification and exploitation, and exploit development to facilitate evasion techniques.                                                                                                                                                                                                                                                                                                                  |
| `☐ Penetration Testing Locations`    | External: Remote (via secure VPN) and/or Internal: Internal or Remote (via secure VPN)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `☐ Time Estimation`                  | For the time estimation, we need the start and the end date for the penetration test. This gives us a precise time window to perform the test and helps us plan our procedure. It is also vital to explicitly ask how time windows the individual attacks (Exploitation / Post-Exploitation / Lateral Movement) are to be carried out. These can be carried out during or outside regular working hours. When testing outside regular working hours, the focus is more on the security solutions and systems that should withstand our attacks.                                                                                             |
| `☐ Third Parties`                    | For the third parties, it must be determined via which third-party providers our customer obtains services. These can be cloud providers, ISPs, and other hosting providers. Our client must obtain written consent from these providers describing that they agree and are aware that certain parts of their service will be subject to a simulated hacking attack. It is also highly advisable to require the contractor to forward the third-party permission sent to us so that we have actual confirmation that this permission has indeed been obtained.                                                                              |
| `☐ Evasive Testing`                  | Evasive testing is the test of evading and passing security traffic and security systems in the customer's infrastructure. We look for techniques that allow us to find out information about the internal components and attack them. It depends on whether our contractor wants us to use such techniques or not.                                                                                                                                                                                                                                                                                                                         |
| `☐ Risks`                            | We must also inform our client about the risks involved in the tests and the possible consequences. Based on the risks and their potential severity, we can then set the limitations together and take certain precautions.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `☐ Scope Limitations & Restrictions` | It is also essential to determine which servers, workstations, or other network components are essential for the client's proper functioning and its customers. We will have to avoid these and must not influence them any further, as this could lead to critical technical errors that could also affect our client's customers in production.                                                                                                                                                                                                                                                                                           |
| `☐ Information Handling`             | HIPAA, PCI, HITRUST, FISMA/NIST, etc.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `☐ Contact Information`              | For the contact information, we need to create a list of each person's name, title, job title, e-mail address, phone number, office phone number, and an escalation priority order.                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `☐ Lines of Communication`           | It should also be documented which communication channels are used to exchange information between the customer and us. This may involve e-mail correspondence, telephone calls, or personal meetings.                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `☐ Reporting`                        | Apart from the report's structure, any customer-specific requirements the report should contain are also discussed. In addition, we clarify how the reporting is to take place and whether a presentation of the results is desired.                                                                                                                                                                                                                                                                                                                                                                                                        |
| `☐ Payment Terms`                    | Finally, prices and the terms of payment are explained.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

#### Rules of Engagement - Checklist

| **Checkpoint**                              | **Contents**                                                                                          |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `☐ Introduction`                            | Description of this document.                                                                         |
| `☐ Contractor`                              | Company name, contractor full name, job title.                                                        |
| `☐ Penetration Testers`                     | Company name, pentesters full name.                                                                   |
| `☐ Contact Information`                     | Mailing addresses, e-mail addresses, and phone numbers of all client parties and penetration testers. |
| `☐ Purpose`                                 | Description of the purpose for the conducted penetration test.                                        |
| `☐ Goals`                                   | Description of the goals that should be achieved with the penetration test.                           |
| `☐ Scope`                                   | All IPs, domain names, URLs, or CIDR ranges.                                                          |
| `☐ Lines of Communication`                  | Online conferences or phone calls or face-to-face meetings, or via e-mail.                            |
| `☐ Time Estimation`                         | Start and end dates.                                                                                  |
| `☐ Time of the Day to Test`                 | Times of the day to test.                                                                             |
| `☐ Penetration Testing Type`                | External/Internal Penetration Test/Vulnerability Assessments/Social Engineering.                      |
| `☐ Penetration Testing Locations`           | Description of how the connection to the client network is established.                               |
| `☐ Methodologies`                           | OSSTMM, PTES, OWASP, and others.                                                                      |
| `☐ Objectives / Flags`                      | Users, specific files, specific information, and others.                                              |
| `☐ Evidence Handling`                       | Encryption, secure protocols                                                                          |
| `☐ System Backups`                          | Configuration files, databases, and others.                                                           |
| `☐ Information Handling`                    | Strong data encryption                                                                                |
| `☐ Incident Handling and Reporting`         | Cases for contact, pentest interruptions, type of reports                                             |
| `☐ Status Meetings`                         | Frequency of meetings, dates, times, included parties                                                 |
| `☐ Reporting`                               | Type, target readers, focus                                                                           |
| `☐ Retesting`                               | Start and end dates                                                                                   |
| `☐ Disclaimers and Limitation of Liability` | System damage, data loss                                                                              |
| `☐ Permission to Test`                      | Signed contract, contractors agreement                                                                |

#### Contractors Agreement - Checklist for Physical Assessments

| **Checkpoint**                    |
| --------------------------------- |
| `☐ Introduction`                  |
| `☐ Contractor`                    |
| `☐ Purpose`                       |
| `☐ Goal`                          |
| `☐ Penetration Testers`           |
| `☐ Contact Information`           |
| `☐ Physical Addresses`            |
| `☐ Building Name`                 |
| `☐ Floors`                        |
| `☐ Physical Room Identifications` |
| `☐ Physical Components`           |
| `☐ Timeline`                      |
| `☐ Notarization`                  |
| `☐ Permission to Test`            |
#### Kick-off meeting
+ Usually there is no (D)DOS Testing.
+ A pentest will leave many log entries and alarms in their security applications, accidentally lock some users.

---------------------------------------------------------------------
## Vocabulary Changes
- `VPN, SSH` - a protocol used for secure remote administration
- `SSL/TLS` - technology used to facilitate secure web browsing
- `Hash` - the output from an algorithm commonly used to validate file integrity
- `Password Spraying` - an attack in which a single, easily-guessable password is attempted for a large list of harvested user accounts
- `Password Cracking` - an offline password attack in which the cryptographic form of a user’s password is converted back to its human-readable form
- `Buffer overflow/deserialization/etc.` - an attack that resulted in remote command execution on the target host
- `OSINT` - Open Source Intelligence Gathering, or hunting/using data about a company and its employees that can be found using search engines and other public sources without interacting with a company's external network
- `SQL injection/XSS` - a vulnerability in which input is accepted from the user without sanitizing characters meant to manipulate the application's logic in an unintended manner


## Enumeration Methodology
### 1. Infrastructure Based
#### a. Internet Presence: 
In other word: "Everything you can find about that subject on the internet", which include:
- IP Address
- Domains
- Subdomains
- vHosts
- Cloud Instances
- Netblocks
- Security Measures
#### b. Gateway
The security guard of the infrastructure
- Firewalls 
- DMZ (Demilitarized Zone): Services that exposed to the internet is place in DMZ to isolate Company internal network from the Internet.
- IPS/IDS
- EDR, Proxies
- NAC
- Network Segmentation
- VPN
- Cloudflare: DDoS Protection, Content Delivery Network (CDN), SSL/TLS Encryption.


#### DMZ (Demilitarized Zone)
