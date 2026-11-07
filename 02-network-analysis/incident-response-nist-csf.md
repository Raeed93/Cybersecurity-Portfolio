# Incident Response Using the NIST CSF

> **Practice project.** Palmgate Family Clinic and this incident are fictional. The technical evidence is in the [security incident report](./security-incident-report.md).

## Summary

An attacker used a brute force attack to break into the admin panel of Palmgate's booking website. They added a script that prompted patients to download malware, which then sent them to a look-alike website. Patients reported the problem to reception by phone. The clinic took the site offline, restored a clean copy, and escalated the incident to management for a PDPL assessment.

## Identify

These findings show what was affected and which weaknesses made the attack possible.

- **Attack type:** Brute force attack followed by website tampering and malware delivery.
- **Affected systems:** The booking website, its admin panel, and some patients' computers.
- **Data at risk:** Booking details, including names, phone numbers, and reasons for visits. The logs showed no data export, but the investigation could not rule out that bookings were viewed.
- **Weaknesses:**
  - The admin account was shared.
  - Its password had not changed since the site launched.
  - It had no MFA.
  - It had no login attempt limit.
  - The web agency had no written security terms with the clinic.

## Protect

These actions are recommended to prevent the same attack from happening again.

- Require MFA on every booking website admin account.
- Give each administrator an individual account.
- Set a strong password standard and limit failed login attempts.
- Add security terms to the web agency's contract.
- Tell patients that the clinic will never ask them to install software to view a booking.

## Detect

The clinic learned about the attack from patients, not from a security tool. These steps cover how it was investigated and how to catch it sooner next time.

**During the incident:**

- Visited the booking site from an isolated sandbox computer.
- Captured traffic with `tcpdump` and found the fake domain and malicious file.
- Reviewed the admin logs and found about 1,400 failed logins followed by a successful login from the same IP address.

**Recommended:**

- Send an alert after repeated failed admin logins.
- Send an alert when website files or templates change.
- Have the web agency review admin logs regularly and report anything unusual to the clinic.

## Respond

These actions contained the incident and managed its effects.

1. The web agency took the booking site offline.
2. The agency replaced the admin credentials.
3. The IT administrator blocked the look-alike domain and IP address on the clinic's firewall.
4. Reception told affected patients not to run the file and to scan their computers.
5. The incident was escalated to management to assess whether it must be reported to SDAIA under the PDPL.

**Recommended:** Add website compromises to the clinic's incident response plan, including who contacts the web agency and who makes the PDPL decision.

## Recover

These actions restore normal service and turn the incident into improvements.

1. The web agency removed the script and restored a clean copy of the site.
2. Reception used phone bookings while the site was offline.
3. Bring the site back online after confirming that the script is gone and the Protect actions for admin access are in place.
4. Watch admin logins and website changes closely after the site returns.
5. Update affected patients on what happened and what they should do.
6. Hold a review with the IT administrator, web agency, and management to record lessons learned.
