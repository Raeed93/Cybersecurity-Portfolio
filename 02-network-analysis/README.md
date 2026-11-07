# Network Analysis: Palmgate Family Clinic

> **Practice project.** Palmgate Family Clinic is a fictional company, and I wrote these scenarios for practice. The domain names, IP addresses, and network log are simulated. This project follows my [security audit](../01-security-audit/) of the same clinic.

## Overview

This project looks at Palmgate's network security from two angles: reducing weaknesses before an attack, and investigating an attack after it happens.

| Report | What it covers |
|---|---|
| [Security risk assessment](./security-risk-assessment-report.md) | Network weaknesses and three recommended hardening methods |
| [Security incident report](./security-incident-report.md) | Investigation of a brute force attack on the clinic's booking website |
| [Incident response using the NIST CSF](./incident-response-nist-csf.md) | The clinic's actions, organized under Identify, Protect, Detect, Respond, and Recover |

## Skills and tools

- Security hardening
- Network protocol analysis
- DNS, HTTP, HTTPS, and TCP
- tcpdump log analysis
- Incident documentation
- NIST Cybersecurity Framework

## Security takeaway

The two reports are connected. Weak authentication appeared as a risk in the assessment, then became the way into the booking website in the incident. Both also show that outside systems need the same attention as internal ones, because patients trust a vendor-run website as much as the clinic itself.
