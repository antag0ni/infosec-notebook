# Introduction to the Penetration Tester Path

- **Core Philosophy and Ethics:** The curriculum is built on a hands-on, "learn by doing" approach that teaches students how to discover, exploit, remediate, detect, and prevent vulnerabilities. Because penetration testers are highly skilled and trusted, students must always operate ethically and within legal boundaries. Furthermore, rigorous documentation and proactive communication regarding compliance are strongly emphasized.

- **Path Structure and Scenario:** The coursework simulates a realistic penetration test against a company named Inlanefreight. Students should complete the modules in the exact order provided, as the concepts progressively build upon one another. This sequence is highly recommended for both beginners and advanced users who find themselves stuck.

- **Prerequisites and Future Development:** Students who lack confidence for this path should first complete the Information Security Foundations Skill Path to build prerequisite knowledge. After finishing the penetration tester path, students are encouraged to specialize in areas like Active Directory, Web, or Reverse Engineering while maintaining a well-rounded skill set.

- **Required Mindset and Practice:** Cybersecurity requires a deep understanding of standard IT disciplines, such as networking, databases, scripting, and system administration. Students are encouraged to develop their own thorough and repeatable methodology. The text notes that analytical skills cannot simply be taught in a module; much like learning to play the guitar, mastering penetration testing requires considerable hands-on practice.

---
# Penetration Testing Overview

**Core Concepts and Responsibilities**

- Unlike scenario-based red team assessments that focus on a specific end goal, a pentest aims to identify _all_ vulnerabilities within the investigated systems.
- Testers act as trusted advisors by documenting findings, providing reproduction steps, and offering remediation recommendations, but it is the client's responsibility to actually apply patches or fixes.
- A pentest represents a momentary snapshot of an organization's security status.
- While generic vulnerability assessments rely purely on automated scanners (like Nessus or OpenVAS), penetration tests require complex planning and combine automated tools with tailored, manual testing and extensive information gathering.

**Authorization and Privacy**

- Explicit written authorization is legally required before testing to avoid criminal charges.
- Testers must verify asset ownership and obtain written approval from third-party hosting entities, though some providers like AWS have updated their policies to no longer require prior authorization for certain services.
- Testers must strictly protect any discovered personal or sensitive data (like credit card numbers or salaries) to uphold regulations such as the Data Protection Act.

**Testing Perspectives**

- **External Penetration Test:** Conducted from the internet as an anonymous user to test the external network perimeter. This can be a stealthy approach to avoid alarms or a "hybrid" approach to test detection capabilities, with the ultimate goal of accessing external hosts, sensitive data, or the internal network.

- **Internal Penetration Test:** Conducted from within the corporate network, often simulating an assumed breach after a successful external pentest. This may require the tester's physical presence at the facility to access isolated systems without internet connectivity.

**Types of Penetration Testing**

|**Type**|**Information Provided**|**Details**|
|---|---|---|
|**Blackbox**|Minimal|Only essential info like IPs and domains is provided; requires extensive time for reconnaissance to map the infrastructure.|
|**Greybox**|Extended|Additional information is provided, such as specific URLs, hostnames, and subnets.|
|**Whitebox**|Maximum|Full disclosure is provided, including admin credentials, web application source code, and detailed configurations, allowing for internal-view attack preparation.|
|**Red-Teaming**|Varies|May include physical testing and social engineering, and can be combined with other testing types.|
|**Purple-Teaming**|Varies|Focuses on working closely with the defenders, and can be combined with other testing types.|

Penetration testing can be applied across a vast array of environments, including networks, web and mobile apps, APIs, cloud infrastructure, IoT, thick clients, source code, firewalls, physical security, and employees.

---

# Laws and Regulations

**Global Legal Frameworks** 

- **USA:** Key regulations include CISA for critical infrastructure, CFAA for criminalizing malicious access, DMCA for copyright protection, ECPA for communication interception, HIPAA for health information, and COPPA for children's data.
- **Europe:** Operations are governed by GDPR, NISD 2, the Cybercrime Convention of the Council of Europe, and the E-Privacy Directive.
- **UK:** Relevant laws include the Data Protection Act 2018, Computer Misuse Act 1990, Human Rights Act 1998, Police and Justice Act 2006, and Investigatory Powers Acts (IPA and RIPA).
- **India:** Cyber activities are regulated by the Information Technology Act 2000, Indian Evidence Act 1872, Indian Penal Code of 1860, and the Personal Data Protection Bill 2019.
- **China:** Frameworks include the Cyber Security Law, National Security Law, Anti-Terrorism Law, and specific state regulations regarding cross-border data transfers and critical infrastructure protection.

**European Regulations in Detail**

- **General Data Protection Regulation (GDPR):** Strengthens individual data rights and applies globally to any company processing EU citizens' data. Non-compliance can result in penalties up to 20 million euros or 4% of global annual revenue.
- **Network and Information Systems Directive (NISD):** Mandates that digital service providers and operators of essential services implement appropriate security measures and report specific incidents.
- **Cybercrime Convention of the Council of Europe:** Functions as the first international treaty facilitating cooperation between countries for investigating and prosecuting internet-based crimes.
- **E-Privacy Directive 2002/58/EC:** Regulates personal data processing specifically within the EU's publicly available electronic communications sector.

**Precautionary Measures for Penetration Testers** To avoid violating legal boundaries, penetration testers must follow a strict set of precautions during engagements:

- Obtain written consent from the authorized representative or owner of the target network.
- Test strictly within the scope of the obtained consent and respect all specified limitations.
- Implement measures that prevent any damage to the networks or systems being tested.
- Do not access, use, or disclose any personal data or information discovered during the test without explicit permission.
- Do not intercept electronic communications unless consent is granted by at least one party involved in the communication.
- Secure proper authorization before conducting tests on any systems covered by HIPAA.

---

# Pentetration Testing Process

**Preparation and Discovery**

- **Pre-Engagement:** The process begins by drafting contractual documents that outline the main commitments, scope, limitations, and necessary information exchanges between the tester and the client.
- **Information Gathering:** Testers identify the target systems and obtain a comprehensive overview of the network or applications. This stage demands significant time, patience, and organization to ensure critical details are not missed before attempting an exploit.
- **Vulnerability Assessment:** This involves scanning for known vulnerabilities using automated tools and analyzing gathered information to creatively find logical gaps. Depending on the assessment's progress, testers may move on to exploitation, post-exploitation, lateral movement, or return to gathering more information.

**Attack Execution**

- **Exploitation:** Using the data gathered and analyzed in previous steps, testers execute targeted attacks against the discovered system or application vulnerabilities.
- **Post-Exploitation:** Because initial exploits rarely grant maximum privileges, testers must bypass isolated restrictions to escalate their access. This requires deep knowledge of Windows and Linux environments and involves a process called "Pillaging," where testers gather internal system information to exploit local services.
- **Lateral Movement:** Testers navigate through the corporate network from an initially compromised host to access other internal systems. This step can often be initiated without administrator rights.

**Validation and Reporting**

- **Proof-of-Concept (PoC):** Testers generate a PoC to prove that a vulnerability genuinely exists. This allows network administrators to safely reproduce and confirm the flaw before implementing fixes that could potentially disrupt interoperating business systems.
- **Post-Engagement:** Testers finalize and format comprehensive documentation for the client. Crucially, testers must clean up all exploited systems by removing transferred tools or shells to ensure the network is left in its original state. Testers must also log all system changes, file uploads, and captured credentials so the client can differentiate testing actions from actual malicious attacks.

