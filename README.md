# 🔐 PENETRATION TESTING REPORT
## Footprinting, Reconnaissance & Network Scanning

**W2-PM-FINAL  |  CYBERSECURITY  |  NETWORKWALKS**

| Field | Detail |
|---|---|
| **Pentester Name (Cybersecurity Professional)** | **Aashish Kumar Chaudhary** |
| **Program/Batch** | Cybersecurity Internship – B084 |
| **Date** | 14 November 2026 |
| **Modules Completed** | W2-PM1 (Multiple Kali Tools)<br>W2-PM5 (Zenmap Scanning) |
| **Client/Target** | 1. Networkwalks (`networkwalks.com`)<br>2. Local LAN Network (`10.15.130.0/24`) |
| **Permission Secured from Client?** | [Confirm Before Submission] |
| **Phases Covered** | **Phase 1:** Reconnaissance & Footprinting – ✅ Completed<br>**Phase 2:** Scanning & Network Discovery – ✅ Completed<br>**Phase 3–5:** ✅ Completed |
---

## ⚠️ Disclaimer

This report is prepared for educational and authorized cybersecurity assessment purposes only. All activities described must be performed only against systems and networks for which explicit permission has been granted.

The findings in this report are based on the command outputs collected during the assessment. They represent observations and potential areas for review, not confirmed vulnerabilities. No exploitation or intrusive vulnerability validation is claimed.

**Authorization status:** Confirm written authorization before submission or publication.

---

**Liability Disclaimer**

I have performed these activities only on the systems & devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.

## 📌 1. Introduction

The objective of this assessment was to perform basic footprinting, reconnaissance, DNS enumeration, web technology fingerprinting, HTTP header analysis, Web Application Firewall (WAF) detection, and internal network discovery.

The assessment used common Kali Linux tools to collect publicly observable information about `networkwalks.com` and identify live hosts on the supplied internal network range `10.15.130.0/24`.

The collected information can help understand the target's externally visible technology stack, DNS configuration, security controls, and network visibility.

### 🎯 Assessment Objectives

- Collect domain registration information using WHOIS.
- Identify web technologies and server details using WhatWeb.
- Resolve the domain to its IP address using Nslookup.
- Inspect HTTP response headers using cURL.
- Identify the presence of a Web Application Firewall using Wafw00f.
- Enumerate DNS records using DNSRecon.
- Discover live hosts on the internal network using Zenmap/Nmap.
- Document observations, potential risks, and security recommendations.

---

## 🧰 2. Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Security testing environment |
| WHOIS | Domain registration lookup |
| WhatWeb | Website technology fingerprinting |
| Nslookup | Domain-to-IP resolution |
| cURL | HTTP response and header inspection |
| Wafw00f | WAF detection |
| DNSRecon | DNS record enumeration |
| Zenmap / Nmap | Network host discovery |
| Windows CMD | Supporting network information |

---

## 🔎 3. Footprinting & Reconnaissance

### Task 1: WHOIS – Domain Registration Information

**Command:**

```bash
whois networkwalks.com
```

**Purpose:**  
WHOIS is used to retrieve publicly available domain registration information, including registrar details, creation and expiry dates, name servers, and registration status.

**Observed Findings:**

| Field | Result |
|---|---|
| Domain | networkwalks.com |
| Registrar | GoDaddy.com, LLC |
| Creation Date | 06 November 2019 |
| Expiry Date | 06 November 2027 |
| Updated Date | 12 November 2025 |
| WHOIS Server | whois.godaddy.com |
| Name Server 1 | NS6135.HOSTGATOR.COM |
| Name Server 2 | NS6136.HOSTGATOR.COM |
| DNSSEC | Unsigned |
| Registrant Information | Privacy protected |

**Observation:**  
The domain is registered through GoDaddy and uses HostGator name servers. Registrant details are privacy protected. The WHOIS result reports DNSSEC as unsigned; this is a configuration observation and should not, by itself, be treated as proof of a vulnerability.

---

### Task 2: WhatWeb – Website Technology Fingerprinting

**Command:**

```bash
whatweb https://networkwalks.com
```

**Purpose:**  
WhatWeb identifies technologies used by a website, such as the web server, content management system, JavaScript libraries, and other detectable components.

**Observed Findings:**

| Field | Result |
|---|---|
| Website | Networkwalks Academy |
| IP Address | 192.232.216.135 |
| Web Server | Apache |
| CMS | WordPress 7.1.4 |
| Plugin | WordPress Download Manager 3.3.76 |
| Detected Framework / Library | Bootstrap 7.1.4 |
| JavaScript Library | jQuery 3.7.1 |
| Other Technologies | Google Tag Manager, HTML5, Open Graph |
| HTTP Response | 301 redirect to HTTPS |
| HTTPS Response | 200 OK |

**Observation:**  
The website is hosted on an Apache server and uses WordPress. WhatWeb also identified several web technologies and components. Technology fingerprinting can help administrators maintain an accurate software inventory and review whether exposed versions are current and securely configured.

---

### Task 3: Nslookup – Domain-to-IP Resolution

**Command:**

```bash
nslookup networkwalks.com
```

**Purpose:**  
Nslookup queries DNS to obtain the IP address associated with a domain name.

**Observed Findings:**

| Field | Result |
|---|---|
| Domain | networkwalks.com |
| Resolved IP | 192.232.216.135 |
| DNS Server Used | 192.168.92.2 |

**Observation:**  
The domain resolved to `192.232.216.135` using the DNS server shown in the command output.

---

### Task 4: cURL – HTTP Header Analysis

**Command:**

```bash
curl -I https://networkwalks.com
```

**Purpose:**  
The cURL command sends an HTTP HEAD request and displays response headers. These headers provide information about the server response and some browser-facing security settings.

**Observed Findings:**

| Header / Detail | Result |
|---|---|
| HTTP Status | 200 OK |
| Server | Apache |
| Content Type | text/html |
| Referrer Policy | Present in response |
| X-Endurance-Cache-Level | 0 |
| X-nginx-cache | WordPress |
| WordPress REST API | `/wp-json/` |
| Cookie Attributes | `__wpdm_client` reported Secure and HttpOnly |

**Observation:**  
The server returned `200 OK`. The response exposed some server and WordPress-related information. The observed cookie attributes include `Secure` and `HttpOnly`; the actual cookie value should never be published in a public repository.

---

### Task 5: Wafw00f – Web Application Firewall Detection

**Command:**

```bash
wafw00f https://networkwalks.com
```

**Purpose:**  
Wafw00f sends requests to a website and analyzes responses to identify whether a Web Application Firewall (WAF) is present.

**Observed Findings:**

| Field | Result |
|---|---|
| Tool Version | Wafw00f 2.4.2 |
| WAF Detection | ModSecurity |
| Identified Vendor / Signature | SpiderLabs |
| Requests Made | 2 |

**Observation:**  
Wafw00f identified a ModSecurity WAF signature associated with SpiderLabs. This indicates that a WAF may be protecting the website. Detection results do not establish the effectiveness or completeness of the WAF configuration.

---

### Task 6: DNSRecon – DNS Enumeration

**Command:**

```bash
dnsrecon -d networkwalks.com
```

**Purpose:**  
DNSRecon enumerates DNS records to identify domain infrastructure, including A, NS, MX, SOA, TXT, and service records.

**Observed Findings:**

| Record Type | Observed Result |
|---|---|
| SOA | ns6135.hostgator.com – 50.87.144.87 |
| NS | ns6136.hostgator.com – 192.232.216.131 |
| NS | ns6135.hostgator.com – 50.87.144.87 |
| MX | mail.networkwalks.com – 192.232.216.135 |
| A | networkwalks.com – 192.232.216.135 |
| TXT (SPF) | `v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all` |
| TXT | Google site verification record detected |
| BIND Version | 9.16.23-RH |
| SRV Records | 8 records detected |
| Autodiscover SRV | `cpanelemaildiscovery.cpanel.net`, port 443 |
| DNSSEC Query | No answer returned |

**Observation:**  
DNSRecon identified the domain's authoritative name servers, mail server, IP address, TXT records, and service records. The DNSSEC query returned no answer in the collected output. This should be verified with the DNS administrator and authoritative DNS tools before drawing conclusions.

---

## 🌐 4. Internal Network Scanning – Zenmap

**Tool:** Zenmap (Nmap graphical interface)

**Target Network:**

```text
10.15.130.0/24
```

**Purpose:**  
Zenmap was used to discover active hosts on the specified local network range. Host discovery helps identify devices that are responding on the network.

**Observed Findings:**

| Metric | Result |
|---|---:|
| Network Range | 10.15.130.0/24 |
| Total IP Addresses Scanned | 256 |
| Live Hosts Found | 149 |
| Scan Duration | 16.33 seconds |
| MAC Addresses Available | 148 |
| Host with MAC Not Reported | 10.15.130.138 |

**Observation:**  
The scan reported 149 live hosts out of 256 scanned IP addresses. MAC addresses were available for 148 hosts. The host `10.15.130.138` was reported as up, but its MAC address was not available in the collected output.

The discovered hosts should be reviewed by the network administrator to confirm that each device is authorized and expected to be connected.

---

## 📊 5. Risk Analysis

The following table classifies observations by their potential security significance. These are preliminary review priorities, not confirmed vulnerabilities.

| Finding | Potential Risk | Priority |
|---|---|---|
| Web technology and version disclosure | May help an attacker identify software for further research | Medium |
| DNS infrastructure details exposed | Reveals domain and hosting architecture | Medium |
| DNSSEC reported as unsigned / query returned no answer | DNS integrity configuration should be reviewed | Medium |
| HTTP response reveals server and CMS-related details | Provides additional technical information | Low |
| WAF detected | Positive security control; effectiveness not tested | Informational |
| 149 live hosts discovered | Unknown or unmanaged devices may increase network exposure | Medium |
| One live host had no MAC address in output | Device identity requires verification | Low |

**Risk Assessment Note:**  
No exploitation was performed, and no confirmed vulnerability is claimed in this report. The risk levels are preliminary and should be validated through authorized security testing and configuration review.

---

## 🛡️ 6. Security Recommendations

### Website & Technology Security

- Keep WordPress, plugins, themes, Apache, and supporting libraries updated.
- Remove unused plugins, themes, and components.
- Review whether detailed technology information needs to be exposed publicly.
- Perform regular vulnerability and configuration assessments under written authorization.

### HTTP Security

- Review HTTP security headers and configure appropriate protections.
- Ensure HTTPS is enforced across the website.
- Verify that cookies containing sensitive information use appropriate attributes, including `Secure`, `HttpOnly`, and suitable `SameSite` settings.
- Avoid exposing session identifiers, cookie values, or tokens in reports and logs shared publicly.

### DNS Security

- Review DNS records and remove outdated or unnecessary entries.
- Verify SPF, DKIM, and DMARC configuration for email security.
- Review DNSSEC configuration with the authoritative DNS provider.
- Avoid exposing unnecessary service records or software version details.

### WAF Security

- Review ModSecurity rules and ensure the WAF is maintained.
- Monitor WAF logs for suspicious activity.
- Test WAF rules only within an approved scope and with explicit authorization.
- Do not assume WAF detection alone proves that the application is secure.

### Internal Network Security

- Verify that all 149 discovered hosts are known and authorized.
- Investigate devices that cannot be identified from the scan output.
- Maintain an updated network asset inventory.
- Segment sensitive systems and restrict unnecessary network access.
- Perform periodic network discovery and monitoring with approval from the network owner.

---

## 📝 7. Conclusion

The assessment successfully performed basic footprinting and reconnaissance against `networkwalks.com`, including WHOIS lookup, web technology fingerprinting, DNS resolution, HTTP header inspection, WAF detection, and DNS enumeration.

The internal network scan of `10.15.130.0/24` identified 149 live hosts from 256 scanned addresses. The collected results provide an overview of the domain's publicly observable infrastructure and the responding devices on the supplied network.

The assessment identified several areas that may benefit from configuration review, particularly technology disclosure, DNS configuration, HTTP security settings, and internal asset management. These observations are not confirmed vulnerabilities. Further validation should be performed only with explicit written authorization.

---


## 📸 8. Screenshots

<img width="1122" height="637" alt="01-whois png" src="https://github.com/user-attachments/assets/cca08a09-5f2f-4a80-babf-02259fe808f1" />

<img width="1302" height="462" alt="02-whatweb png" src="https://github.com/user-attachments/assets/03dfa10a-d5ce-4dfa-b6f5-fdb12a91da41" />

<img width="787" height="377" alt="03-nslookup png" src="https://github.com/user-attachments/assets/1e908943-078a-44df-9507-de96864158c9" />

<img width="1257" height="457" alt="04-curl png" src="https://github.com/user-attachments/assets/0a2827e2-2bc1-4040-abf6-10672d094ad9" />

<img width="1307" height="572" alt="05-wafw00f png" src="https://github.com/user-attachments/assets/5975288d-3aec-4664-8cf3-4a75c566d44b" />

<img width="1262" height="627" alt="06-dnsrecon png" src="https://github.com/user-attachments/assets/52cc4500-2057-4c36-ad83-585dec164562" />

<img width="1917" height="1020" alt="07-zenmap-scan png" src="https://github.com/user-attachments/assets/9db2c837-a40a-4856-99aa-4615b25d16e2" />

<img width="962" height="472" alt="08-zenmap-topology" src="https://github.com/user-attachments/assets/88ae99df-a79c-44d9-a985-f334aa878edc" />


## 📸 9. Evidence Checklist

Attach screenshots or sanitized command outputs for the following activities:

- [✅] WHOIS – Domain registration details
- [✅ ] WhatWeb – Website technology fingerprinting
- [✅ ] Nslookup – Domain-to-IP resolution
- [✅ ] cURL – HTTP response headers
- [✅ ] Wafw00f – WAF detection output
- [✅ ] DNSRecon – DNS enumeration results
- [✅ ] Zenmap – Network scan summary and host discovery results

**Evidence handling:** Before uploading evidence to GitHub, remove or mask private IP/MAC mappings where disclosure is not authorized, cookie values, session identifiers, credentials, tokens, and other sensitive information. Do not publish raw authentication or session data.

---

## 📁 10. Repository Structure

```text
Networkwalks-Penetration-Testing-Report/
│
├── README.md
│
├── screenshots/
│   ├── whois.png
│   ├── whatweb.png
│   ├── nslookup.png
│   ├── curl-headers.png
│   ├── wafw00f.png
│   ├── dnsrecon.png
│   └── zenmap.png
```


---

<p align="center">
  <strong>🔐 End of Report</strong>
  <br>
  <sub>Prepared for educational and authorized cybersecurity assessment purposes.</sub>
</p>
