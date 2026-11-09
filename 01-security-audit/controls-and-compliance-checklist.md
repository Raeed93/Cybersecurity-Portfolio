# Controls and Compliance Checklist: Palmgate Family Clinic

> Part of my [security audit](./README.md), based on the Google Cybersecurity Certificate's security audit activity. The evidence IDs refer to the [audit observations](./README.md#audit-observations).

An **X** under **Yes** means the control exists and works as intended. An **X** under **No** means it is missing or not effective. The **CSF** column shows the main NIST CSF 2.0 function each control supports: Govern (GV), Identify (ID), Protect (PR), Detect (DE), Respond (RS), or Recover (RC).

## Controls assessment

**Question:** Does Palmgate Family Clinic currently have this control in place?

### Administrative controls

| ID | Yes | No | Control | Explanation | Evidence | CSF |
|---|:-:|:-:|---|---|---|:-:|
| A-01 | | X | Least privilege | Every employee can open every patient record. Access should match each job role. | O-01 | PR |
| A-02 | | X | Separation of duties | One person can create invoices, issue refunds, and reconcile the bank account, which raises the risk of undetected fraud or error. | O-04 | GV |
| A-03 | | X | Account lifecycle and access reviews | Former staff still have active accounts, and access has never been reviewed. | O-02 | PR |
| A-04 | | X | Password policy | A 6-character minimum is weak, and shared logins remove individual accountability. | O-03 | PR |
| A-05 | | X | Incident response plan | Staff have no plan, owner, or procedure to follow during a security incident. | O-05 | RS |
| A-06 | | X | Disaster recovery plan | The clinic has no plan or recovery target for restoring clinical systems. | O-06 | RC |
| A-07 | | X | Security awareness training | Staff are not trained to recognize phishing or handle data securely after induction. | O-08 | PR |
| A-08 | | X | Vendor security management | Only the EMR vendor has data protection terms, and the gateway's compliance evidence has not been requested. | O-09 | GV |
| A-09 | X | | Confidentiality agreements | All staff sign a confidentiality agreement when hired. | O-07 | GV |

### Technical controls

| ID | Yes | No | Control | Explanation | Evidence | CSF |
|---|:-:|:-:|---|---|---|:-:|
| T-01 | X | | Firewall | The firewall denies inbound traffic by default, and its rules are reviewed quarterly. | O-10 | PR |
| T-02 | | X | Network segmentation | One compromised device, including one on guest Wi-Fi, could reach critical systems. | O-11 | PR |
| T-03 | | X | Intrusion detection or prevention system | The firewall's detection feature is not enabled, so attacks may go unnoticed. | O-12 | DE |
| T-04 | | X | Multi-factor authentication | A stolen password alone could unlock email, the VPN, or administrator accounts. | O-13 | PR |
| T-05 | | X | Logging and monitoring | Logs do not show who opened a record, are short-lived, and are not reviewed. | O-14 | DE |
| T-06 | | X | Backups | Backups exist but are not isolated from the network and have never been restore-tested. | O-15 | RC |
| T-07 | X | | Antivirus or endpoint protection | Endpoint protection is centrally managed. The legacy lab PC is an exception covered under T-09. | O-16 | PR |
| T-08 | X | | Patch management | Supported systems receive monthly updates. The legacy lab PC is an exception covered under T-09. | O-17 | PR |
| T-09 | | X | Legacy system monitoring and maintenance | The unsupported lab PC has no schedule, isolation, or endpoint protection. | O-16, O-17, O-18 | PR |
| T-10 | | X | Encryption | Data is encrypted in transit on the website, but sensitive data at rest is not. | O-19 | PR |
| T-11 | | X | Password management system | Shared passwords are stored in a spreadsheet on the file server. | O-20 | PR |

### Physical controls

| ID | Yes | No | Control | Explanation | Evidence | CSF |
|---|:-:|:-:|---|---|---|:-:|
| P-01 | X | | Locks on building, offices, and records storage | Working locks and after-hours alarms are in place. | O-21 | PR |
| P-02 | | X | Server room access control | The key is easy to reach, and access is not recorded. | O-22 | PR |
| P-03 | X | | CCTV surveillance | CCTV covers key entry and public areas, and footage is retained for 30 days. | O-23 | DE |
| P-04 | X | | Fire detection and prevention | Fire alarms, sprinklers, and extinguishers are installed and maintained. | O-24 | PR |
| P-05 | | X | Secure disposal of paper records | Documents containing patient data go into ordinary waste. | O-25 | PR |
| P-06 | | X | Screen privacy and automatic lock | Patients may see screens, and unattended workstations stay unlocked. | O-26 | PR |

## Compliance checklist

**Question:** Does Palmgate Family Clinic currently adhere to this best practice?

### Payment Card Industry Data Security Standard (PCI DSS)

| ID | Yes | No | Best practice | Explanation | Evidence |
|---|:-:|:-:|---|---|---|
| PCI-01 | | X | Access to cardholder data is limited by business need. | All staff can open the card spreadsheet. | O-27 |
| PCI-02 | | X | Security codes are not stored after payment authorization. | Security codes are kept in the spreadsheet. This is the clinic's most urgent compliance gap. | O-27 |
| PCI-03 | | X | Stored card numbers are protected and unreadable. | Card numbers are stored in plain text. | O-19, O-27 |
| PCI-04 | X | | Card data is protected during transmission over open networks. | Online card entry happens on the gateway's hosted HTTPS page, and in-person payments use its terminals. | O-28 |
| PCI-05 | | X | Users are individually identified and strongly authenticated. | Shared logins, weak passwords, and missing MFA fall short of this requirement. | O-03, O-13, O-27 |
| PCI-06 | | X | Service providers are managed and their compliance status is monitored. | The clinic has never requested the gateway's compliance evidence. | O-09 |

### Saudi Personal Data Protection Law (PDPL)

The PDPL treats health data as sensitive data, so a clinic's main activity carries stronger obligations. This section replaces the GDPR section used in the original course activity because Palmgate operates in Saudi Arabia.

| ID | Yes | No | Best practice | Explanation | Evidence |
|---|:-:|:-:|---|---|---|
| PDPL-01 | | X | Health data access is limited by role and to the minimum necessary. | Reception and other staff can read clinical data they do not need. | O-01 |
| PDPL-02 | | X | Appropriate organizational, administrative, and technical safeguards protect personal data. | Missing encryption, MFA, and access controls leave patient data exposed. | O-01, O-13, O-19 |
| PDPL-03 | | X | A process exists to notify SDAIA within 72 hours of a reportable breach and to notify affected people without undue delay. | There is no breach notification procedure. | O-05 |
| PDPL-04 | X | | People are told how their personal data is used. | The privacy notice and registration form explain data use. | O-29 |
| PDPL-05 | | X | Personal data is classified, and processing activities are recorded. | An asset list exists, but there is no classification or record of processing activities. | O-30, O-31 |
| PDPL-06 | | X | Sensitive processing is supported by an impact assessment and a data protection officer. | Neither is in place, although sensitive data is central to the clinic's work. | O-31 |
| PDPL-07 | | X | Processors are bound by contract to protect personal data. | The web agency handles patient booking data without written security terms. | O-09 |
| PDPL-08 | | X | Transfers outside Saudi Arabia have a lawful basis and, where required, a risk assessment. | The booking database is hosted abroad, and the transfer has not been assessed. | O-32 |

### SOC 2 Trust Services Criteria (benchmark)

Palmgate is not seeking a SOC 2 report. A clinic is usually not the kind of service organization that gets one. I used the five criteria as a benchmark because a corporate client asked about them.

| ID | Yes | No | Criterion | Explanation | Evidence |
|---|:-:|:-:|---|---|---|
| SOC-01 | | X | **Security:** Systems are protected against unauthorized access. | Access controls, MFA, and monitoring are not adequate. | O-01, O-02, O-13, O-14 |
| SOC-02 | | X | **Availability:** Systems and data can be restored and remain available when needed. | Backups are untested, and there is no recovery plan. | O-06, O-15 |
| SOC-03 | X | | **Processing integrity:** Data is complete, valid, accurate, and authorized. | The EMR validates required fields, and physicians sign lab results before release. | O-33 |
| SOC-04 | | X | **Confidentiality:** Confidential data is identified and protected. | Data is not classified, encrypted at rest, or limited by role. | O-01, O-19, O-30 |
| SOC-05 | | X | **Privacy:** Personal data is handled as described and disposed of properly. | A notice exists, but broad access and unsafe paper disposal undermine it. | O-01, O-25, O-29 |

## Recommendations

### Within 30 days: stop the highest risks

| Action | Addresses |
|---|---|
| Stop recording card details, securely delete the spreadsheet, and use payment links for phone bookings | PCI-01, PCI-02, PCI-03 |
| Enable MFA on email, the VPN, and administrator accounts | T-04, PCI-05 |
| Remove former staff accounts, end shared logins, and raise the password standard | A-03, A-04 |
| Create an offline backup and complete a test restore | T-06, SOC-02 |
| Write an incident response plan, name an owner, and add PDPL breach notification steps | A-05, PDPL-03 |
| Move the server room key to controlled storage and log access | P-02 |

### Within 90 days: limit damage

| Action | Addresses |
|---|---|
| Create EMR roles by job function and restrict clinical notes | A-01, PDPL-01, SOC-01 |
| Separate clinical systems, guest Wi-Fi, CCTV, and card terminals, and isolate the lab PC | T-02, T-09 |
| Enable the firewall's intrusion prevention feature | T-03 |
| Log record access in the EMR, extend retention, and review logs regularly | T-05 |
| Encrypt laptops, the EMR database, file storage, and backups | T-10, PDPL-02, SOC-04 |
| Adopt a password manager | T-11 |
| Train staff on phishing and data handling, and repeat the training at least yearly | A-07 |
| Require a second approver for refunds and bank reconciliation | A-02 |
| Add automatic screen locks, privacy filters, shredders, and a disposal procedure | P-05, P-06, SOC-05 |

### Within 180 days: build the program

| Action | Addresses |
|---|---|
| Appoint a data protection officer and complete an impact assessment | PDPL-06 |
| Classify data and create a record of processing activities | PDPL-05 |
| Add security terms to vendor contracts and collect the gateway's compliance evidence annually | A-08, PCI-06, PDPL-07 |
| Assess the booking database transfer, or move the data to Saudi hosting | PDPL-08 |
| Write and test a disaster recovery plan | A-06 |
