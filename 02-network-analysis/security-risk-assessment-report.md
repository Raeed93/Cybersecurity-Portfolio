# Security Risk Assessment Report

> **Practice project.** Palmgate Family Clinic is fictional.

## Scenario

After the [security audit](../01-security-audit/), Palmgate's Operations Manager asked for a closer look at how someone could get into the clinic's network and move around it. The review found four main weaknesses:

- Nurses share one login on treatment-room workstations.
- The backup storage device still uses its default administrator password.
- Multi-factor authentication (MFA) is not used on email, the VPN, or administrator accounts.
- The network is flat, so workstations, servers, guest Wi-Fi, and the lab PC can all reach each other.

The firewall itself is in good shape. It denies inbound traffic by default, and its rules are reviewed quarterly.

## Part 1: Hardening tools and methods

Three hardening methods the clinic can use to address these weaknesses are:

1. Implementing multi-factor authentication (MFA)
2. Setting and enforcing a strong password policy
3. Separating the network into segments

MFA requires users to verify their identity in more than one way before accessing a system. Methods include an authenticator app, a staff badge, or a fingerprint scan, used together with a password.

A password policy sets rules for password length, reuse, and sharing. It can also lock an account after repeated failed login attempts.

Network segmentation divides one network into smaller zones, such as VLANs. Firewall rules then control which zones can communicate.

## Part 2: Recommendations

**MFA** adds a layer of security beyond the password. A stolen or guessed password would no longer be enough to access email, the VPN, or an administrator account. It should be enforced at all times, starting with those three, since they would give an attacker the most access. For busy treatment rooms, a staff badge tap could keep logins quick without going back to shared accounts.

**A strong password policy** would make passwords harder to guess and remove the clinic's riskiest habits. I recommend at least 12 characters, a check against commonly used passwords, no sharing or reuse, and a temporary lockout after five failed attempts. Default passwords, including the one on the backup device, should be changed immediately and whenever a new device is installed. Staff should change a password when it may have been exposed. Forcing regular changes isn't necessary, as NIST guidance has moved away from that practice.

**Network segmentation** limits how far an attacker can move after getting in. Clinical systems, guest Wi-Fi, CCTV, card terminals, and the lab PC should each sit in their own zone. This matters most for the lab PC, which runs an unsupported operating system and can't be patched. The rules between zones should be reviewed during the existing quarterly firewall review and whenever a new device is added.

Together, these methods make it harder to break in and reduce the damage if someone does.
