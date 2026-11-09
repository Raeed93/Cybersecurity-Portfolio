# Security Incident Report

> Part of my [network analysis](./README.md), based on the Google Cybersecurity Certificate's network analysis activity. The network log was created for this exercise, using reserved `.example` domains and documentation IP ranges.

## Scenario

Several patients called reception to say the clinic's booking website had asked them to install an "appointment viewer." After running the file, their computers slowed down and their browsers opened a website that looked like Palmgate's but wasn't.

I visited the booking site from an isolated sandbox computer, used `tcpdump` to capture the traffic, and ran the file in the sandbox. The clinic's web agency then shared the site's admin logs.

## Log excerpt

```text
14:18:32.192571 IP 10.99.0.15.52444 > 10.99.0.2.53: 35084+ A? book.palmgateclinic.example. (45)
14:18:32.204388 IP 10.99.0.2.53 > 10.99.0.15.52444: 35084 1/0/0 A 203.0.113.22 (61)
14:18:32.215902 IP 10.99.0.15.36086 > 203.0.113.22.443: Flags [S], seq 2873951608, win 64240, length 0
14:18:32.231744 IP 203.0.113.22.443 > 10.99.0.15.36086: Flags [S.], seq 918273645, ack 2873951609, win 65160, length 0
14:18:32.231851 IP 10.99.0.15.36086 > 203.0.113.22.443: Flags [.], ack 1, win 64240, length 0
...<TLS session with the booking website; contents encrypted>...
14:19:47.402716 IP 10.99.0.15.41822 > 10.99.0.2.53: 21899+ A? palmgate-booking.example. (42)
14:19:47.414053 IP 10.99.0.2.53 > 10.99.0.15.41822: 21899 1/0/0 A 198.51.100.47 (58)
14:19:47.425310 IP 10.99.0.15.38114 > 198.51.100.47.80: Flags [S], seq 1204837765, win 64240, length 0
14:19:47.439872 IP 198.51.100.47.80 > 10.99.0.15.38114: Flags [S.], seq 3320918472, ack 1204837766, win 65160, length 0
14:19:47.439961 IP 10.99.0.15.38114 > 198.51.100.47.80: Flags [.], ack 1, win 64240, length 0
14:19:47.440218 IP 10.99.0.15.38114 > 198.51.100.47.80: Flags [P.], seq 1:88, ack 1, win 64240, length 87: HTTP: GET /appointment-viewer.exe HTTP/1.1
14:19:47.468530 IP 198.51.100.47.80 > 10.99.0.15.38114: Flags [P.], seq 1:1449, ack 88, win 65160, length 1448: HTTP: HTTP/1.1 200 OK
...<file download continues>...
14:21:03.118264 IP 10.99.0.15.38120 > 198.51.100.47.80: Flags [P.], seq 1:72, ack 1, win 64240, length 71: HTTP: GET / HTTP/1.1
14:21:03.146009 IP 198.51.100.47.80 > 10.99.0.15.38120: Flags [P.], seq 1:1449, ack 72, win 65160, length 1448: HTTP: HTTP/1.1 200 OK
```

## Section 1: Network protocol involved

The main protocol involved is **Hypertext Transfer Protocol (HTTP)**, which works at the application layer. The log shows the malicious file being requested and delivered over HTTP on port 80. It also shows the browser loading the fake website over HTTP.

## Section 2: Incident documentation

**Summary:** An attacker gained access to the admin panel of Palmgate's booking website through a brute force attack. They added a script that prompted visitors to download malware, which then sent them to a look-alike website.

**What the log shows:**

1. The browser used DNS to look up `book.palmgateclinic.example`, which returned `203.0.113.22`.
2. It connected to the real site on port 443. This traffic was encrypted.
3. The injected script then caused a new DNS lookup for `palmgate-booking.example`, which returned `198.51.100.47`.
4. The browser connected to that address on port 80 and downloaded `appointment-viewer.exe` over HTTP.
5. After the file ran in the sandbox, the browser requested the look-alike site's main page over HTTP.

**Root cause:** The web agency's logs showed about 1,400 failed logins to one admin account overnight, followed by a successful login from the same IP address. The account was shared, its password hadn't changed since the site launched, and it had no MFA or login attempt limit. The attacker then edited the site's homepage template.

**Impact:**

- Some patients' computers may have been infected with malware.
- Patients were sent to a fake website, which damaged trust in the clinic.
- The admin panel can display bookings, including names, phone numbers, and reasons for visits. The logs showed no data export, but the investigation could not rule out that bookings were viewed.
- The logs showed no changes to the gateway's hosted payment page, and the look-alike website did not ask for card details in the sandbox.

**Response:**

1. The web agency took the booking site offline.
2. The agency removed the script, restored a clean copy of the site, and replaced the admin credentials.
3. The IT administrator blocked the look-alike domain and IP address on the clinic's firewall.
4. Reception told affected patients not to run the file and to scan their computers.
5. The incident was escalated to management to assess whether it must be reported to SDAIA under the PDPL. The 72-hour period starts when the clinic becomes aware of a reportable breach.

## Section 3: Recommended remediation for brute force attacks

I recommend requiring **multi-factor authentication (MFA)** for every admin account on the booking website. MFA means a guessed password alone can't unlock the account, which would have stopped this attack even after the attacker found the right password. Each administrator should also have their own account so that activity can be traced to one person.
