# Security Audit: Palmgate Family Clinic

> Based on the Google Cybersecurity Certificate's security audit activity. I adapted it to **Palmgate Family Clinic**, a healthcare scenario I designed in place of the course's example company.

| | |
|---|---|
| **Audit type** | Internal controls and compliance audit |
| **Framework** | NIST Cybersecurity Framework (CSF) 2.0 |
| **Standards reviewed** | PCI DSS v4.0.1, Saudi Personal Data Protection Law (PDPL), SOC 2 Trust Services Criteria (as a benchmark) |
| **Result** | 10 of 45 items met, overall risk rated **high** |
| **Full results** | [Controls and compliance checklist](./controls-and-compliance-checklist.md) |

## Scenario

Palmgate Family Clinic is a private outpatient clinic with one location in Riyadh, Saudi Arabia. Its 34 staff provide family medicine, pediatrics, on-site laboratory testing, and occupational health screening for corporate clients. The clinic has about 18,000 registered patients.

Patients can now book and pay deposits online, and more corporate clients are asking how their employees' results are protected. The clinic's security practices have not kept up with this growth. One IT administrator supports the whole clinic, and there is no security team.

The clinic's Operations Manager requested an internal audit after a corporate client sent a security questionnaire that the clinic could not answer confidently. She wants to know which risks matter most, where the clinic falls short of its obligations, and what to fix first.

## Scope and goals

**In scope:** IT systems, staff access, data handling, physical security of the clinic, and vendors that handle patient or payment data.

**Out of scope:** Clinical quality, medical device safety, penetration testing, and legal advice.

**Goals:**

1. Assess current controls against the NIST CSF 2.0 functions: Govern, Identify, Protect, Detect, Respond, and Recover.
2. Identify gaps against PCI DSS and the PDPL, and benchmark the clinic against the SOC 2 criteria its corporate client asked about.
3. Give management a prioritized plan to reduce risk.

## Assets

| Category | Assets |
|---|---|
| **Data** | Patient records, lab results, insurance details, corporate screening results, staff records, payment card data |
| **On-site systems** | Electronic medical record (EMR) server, file server, backup storage device, 28 workstations, 4 card terminals |
| **Network** | Firewall, switches, staff Wi-Fi, guest Wi-Fi, VPN |
| **Cloud and vendors** | Cloud email, booking website (run by a web agency), hosted payment page (run by a payment gateway), EMR vendor support |
| **Legacy** | Windows 7 PC that connects to the laboratory analyzer |
| **Physical** | Reception, consultation rooms, laboratory, records storage, server room |

## Audit observations

These are the facts the checklist is based on. Each answer in the checklist points back to one or more of these IDs.

### Governance and people

- **O-01:** All 34 staff share one EMR role that can open every patient record, lab result, and billing entry. Receptionists can read clinical notes.
- **O-02:** Six active accounts belong to former staff. There is no joiner and leaver process, and access has never been reviewed.
- **O-03:** Passwords need only 6 characters, and nurses share one login on treatment-room workstations.
- **O-04:** The billing supervisor creates invoices, issues refunds, and reconciles the bank account without a second approver.
- **O-05:** There is no written incident response plan, no named incident owner, and no breach notification procedure.
- **O-06:** There is no disaster recovery plan and no agreed recovery time for clinical systems.
- **O-07:** All staff sign a confidentiality agreement when hired.
- **O-08:** Staff receive a privacy briefing at induction and no security training afterwards.
- **O-09:** The EMR vendor's contract includes data protection terms. The web agency has no written security terms, and the clinic has never requested the payment gateway's PCI DSS compliance evidence.

### Technical

- **O-10:** A managed firewall denies inbound traffic by default, and its rules are reviewed quarterly.
- **O-11:** The network is flat. Workstations, servers, the lab PC, CCTV, card terminals, and guest Wi-Fi can all reach each other.
- **O-12:** No intrusion detection or prevention is active. The firewall includes this feature, but it has never been enabled.
- **O-13:** Multi-factor authentication (MFA) is not used on email, the VPN, or EMR administrator accounts.
- **O-14:** The EMR logs logins but not which records a user opens. Logs are kept for 30 days and nobody reviews them.
- **O-15:** Nightly backups go to a storage device in the server room on the same network. There is no offsite or offline copy, and no restore has ever been tested.
- **O-16:** Centrally managed endpoint protection runs on every workstation and server except the legacy lab PC.
- **O-17:** Workstations and servers receive security updates each month through a managed process. The legacy lab PC is excluded.
- **O-18:** The lab PC runs unsupported Windows 7. The analyzer vendor offers no upgrade path, and the PC has no maintenance schedule or isolation.
- **O-19:** The booking website uses HTTPS. The EMR database, file server, backup device, and laptops are not encrypted at rest.
- **O-20:** Shared system passwords are kept in a spreadsheet on the file server. The clinic has no password manager.

### Physical

- **O-21:** Exterior doors, offices, and records storage have working locks, and alarms protect the building after hours.
- **O-22:** The server room is locked, but its key sits in an unlocked reception drawer and access is not logged.
- **O-23:** CCTV covers entrances, reception, and corridors, and the footage is retained for 30 days.
- **O-24:** Fire alarms, sprinklers, and extinguishers are installed and maintained on schedule.
- **O-25:** Printed lab results and referral letters go into ordinary waste. There are no shredders or disposal procedures.
- **O-26:** Reception screens face the waiting area, and workstations do not lock automatically.

### Data handling

- **O-27:** For phone bookings, reception records the card number, expiry date, and security code in a shared spreadsheet so that billing can charge the card later. All staff can open the spreadsheet.
- **O-28:** Online deposits use the gateway's hosted payment page over HTTPS. In-person payments use the gateway's card terminals.
- **O-29:** A privacy notice is published online, and the patient registration form explains how data is used.
- **O-30:** IT keeps an asset list, but the data on those assets is not classified by sensitivity.
- **O-31:** There is no record of processing activities, data protection officer, or data protection impact assessment.
- **O-32:** The web agency hosts the booking database outside Saudi Arabia. It holds names, phone numbers, and reasons for visits, and the clinic has not assessed the transfer risk.
- **O-33:** The EMR validates required fields, and a physician must review and sign each lab result before it is released.

## Method

1. Listed assets and identified the data each one holds.
2. Recorded current practices as numbered observations.
3. Scored the main risks by likelihood and impact.
4. Assessed each control across administrative, technical, and physical categories.
5. Checked each compliance item against the same observations.
6. Grouped recommendations by urgency and effort.

I marked a control **No** when it exists but is not effective. Palmgate has backups, for example, but they would probably not survive ransomware and have never been restored, so they do not meet the control's purpose.

## Results

| Area | Met | Not met |
|---|---:|---:|
| Administrative controls | 1 | 8 |
| Technical controls | 3 | 8 |
| Physical controls | 3 | 3 |
| PCI DSS | 1 | 5 |
| PDPL | 1 | 7 |
| SOC 2 benchmark | 1 | 4 |
| **Total** | **10** | **35** |

## Top risks

Likelihood and impact use a 1 to 5 scale, and the score is one multiplied by the other. These scores are my judgment based on the observations, not measured values.

| Risk | Evidence | Likelihood | Impact | Score |
|---|---|---:|---:|---:|
| Ransomware encrypts the EMR and its backups, stopping clinical work | O-06, O-11, O-13, O-15 | 4 | 5 | **20** |
| Staff or a stolen account exposes patient records without detection | O-01, O-02, O-03, O-14 | 4 | 4 | **16** |
| Phishing leads to takeover of email or VPN access | O-08, O-13 | 4 | 4 | **16** |
| Card data, including security codes, is stolen from the shared spreadsheet | O-11, O-27 | 3 | 5 | **15** |
| The unsupported lab PC is compromised and used to reach other systems | O-11, O-18 | 3 | 5 | **15** |
| A breach is reported late or handled poorly | O-05, O-14 | 3 | 4 | **12** |

## What to fix first

1. **Stop recording card details.** Securely delete the spreadsheet and send payment links for phone bookings.
2. **Enable MFA** on email, the VPN, and administrator accounts.
3. **Create an offline backup** and prove that it can be restored.
4. **Remove former staff accounts** and end shared logins.
5. **Write an incident response plan** that covers the PDPL 72-hour notification deadline.

These steps cost little and reduce the highest risks quickly. The [full plan](./controls-and-compliance-checklist.md#recommendations) sets out 30, 90, and 180-day actions.

## Security takeaway

Palmgate's biggest problems are not missing technology. It already owns a firewall with an unused detection feature, it already has a backup process, and its payment gateway could keep card data out of the clinic entirely. The strongest early gains come from changing how the clinic uses what it has.

This audit also showed me why having a control is different from having an effective one. A backup that shares a network with the system it protects and has never been restored gives false confidence, which is why I assessed each control by whether it would work when needed.

## References

- [NIST Cybersecurity Framework 2.0](https://www.nist.gov/cyberframework)
- [PCI Security Standards Council](https://www.pcisecuritystandards.org/)
- [Saudi Data and AI Authority (SDAIA)](https://sdaia.gov.sa/)
- [SOC 1 and SOC 2 overview (Secureframe)](https://secureframe.com/blog/soc-1-vs-soc-2)

*Regulatory details reflect my understanding as of November 2026. SDAIA proposed amendments to the PDPL Implementing Regulations in October 2026, so the breach notification rules may change. This is a learning project and not legal advice, a PCI DSS assessment, or a SOC 2 report.*
