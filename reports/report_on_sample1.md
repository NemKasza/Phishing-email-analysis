# Phishing Email Analysis Report

**Case ID:** PHISH-001\
**Date:** 09-08-2026\
**Analyst:** Nagy-Kasza Bence\
**Severity:** Medium \
**Verdict:** Malicious \
**Analysis Type:** Static Email Analysis

---

## Executive Summary

**Description:**\
*A suspicious email claiming to be a cloud storage provider was analysed for indicators of phishing. Header analysis identified suspicous ARC records and domain names. 
The embedded URL redirected to an external domain, likely running malicious scripts. 
Massage source analysis identified possible keyword stuffing techniques aimed to avoid detection by email scanners.
Based on the combined email-header, URL, and social-engineering indicators, the message was assesed as malicious phishing activity*



---

## Email Overview

| Field      | Finding                                                                                                                                                        |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Subject    | RE: Critical\_Update: *redacted*, Your payment Failed On Tue, 08 Sep 2026 19:37:27 +0000, And Data Protectioin Is Off! Your photos and videos will be removed! |
| From       | *redacted* <[nooreply@mqbixnjvoq.us](mailto\:nooreply@mqbixnjvoq.us)>                                                                                                                                                                                                                                                            
| Date       | 09-08-2026                                                                                                                                                     |
| Message-ID | [41476432.10178941.ko4z9.bad1smtpin\_added\_broken@mx.google.com](mailto:41476432.10178941.ko4z9.bad1smtpin_added_broken@mx.google.com)                        |
| Attachment | No                                                                                                                                                             |


**Description:**

> The email presents itself as a legitimate communication from a Cloud storage provider. However several source file artifacts and technical details indacate that this is a malicious phishing email.

---

## Header Analysis

### ARC analysis

Indicators of potential malicious activity:\
**Sender domain:** google-apple-amazon[.]chelsea[.]org[.]ferdaus[.]my

\
Possible brand-stuffing indicators. Attackers frequently register subdomains or domains containing multiple high-profile brand names to decieve automated filters or users glancing quickly at the headers.



### Authentication Results

| Mechanism | Result |
| --------- | ------ |
| SPF       | Pass   |
| DKIM      | None   |
| DMARC     | None   |

**Description:**

> Email authentication results were reviewed to determine whether the sending infrastructure was authorized to send on behalf of the claimed domain.

**Analyst Note:**

> Authentication success does not by itself indicate that an email is legitimate. A threat actor may send phishing messages from infrastructure they control, including a legitimate or compromised domain.

---

## URL Analysis

**Extracted URL:**\
hxxps\://storage[.]googleapis[.]com/whilewait/brightway[.]html#ZX=zUxMCdQKLWkFpNYXqOXABCnFCaEo&5GpCEpBGTKy&256552/996/nbcwmooats[.]home[.]php=?sq=3D32-195982&lk=3D38563-5&page=3D368

**Used tools: \
urlscan.io \
gchq.github.io/CyberChef/**



**Finding:**

> The attacker is using a trusted cloud storage service (storage.googleapis.com) to host a malicious site or files.\
> By hosting an innocent-looking HTML container on a trusted domain, threat actors bypass automated security filters that check links before a user clicks them. Once clicked. the hidden JavaScript executes to push the user toward a malicious destination.

**IOC:**

- Domain: trusted domain (storage.googleapis.com) containing malicious scripts
- URL: \
  hxxps\://storage[.]googleapis[.]com/whilewait/brightway[.]html#ZX=zUxMCdQKLWkFpNYXqOXABCnFCaEo&5GpCEpBGTKy&256552/996/nbcwmooats[.]home[.]php=?sq=3D32-195982&lk=3D38563-5&page=3D368



---

## Social Engineering Analysis

The following indicators were identified:

- Urgency or time pressure
- Threat of account suspension
- Request for credentials
- Suspicious branding
- Bad grammar
- Suspicious hyperlink

**Description:**

> The message uses urgency and impersonation to encourage the recipient to interact with the provided link. These characteristics are commonly associated with phishing campaigns. You can also discover spelling mistakes.

---

## Email body analysis (keyword stuffing)

> The email body contains hidden keywords and sentences (keyword stuffing). These techniques can help evade detection by bypassing email content checks by company scanners. Using partitions of a newsletter or particular keywords, imitating credible messages. is a way to evade scans.

---

## Indicators of Compromise (IOCs)

| Type   | Indicator                                                                                                                                                                             | Notes                      |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| Email  | nooreply\@mqbixnjnvoq.us                                                                                                                                                              | Suspicious sender          |
| Domain | storage.googleapis.com...                                                                                                                                                             | Suspicious sender domain   |
| URL    | hxxps\://storage[.]googleapis[.]com/whilewait/brightway[.]html#ZX=zUxMCdQKLWkFpNYXqOXABCnFCaEo&5GpCEpBGTKy&256552/996/nbcwmooats[.]home[.]php=?sq=3D32-195982&lk=3D38563-5&page=3D368 | Credential-harvesting link |
|        |                                                                                                                                                                                       |                            |

---

## MITRE ATT&CK Mapping

| Technique                    | ID        | Evidence                           |
| ---------------------------- | --------- | ---------------------------------- |
| Phishing: Spearphishing Link | T1566.002 | Malicious link delivered via email |
|                              |           |                                    |

**Description:**

> The observed behavior is consistent with MITRE ATT&CK technique T1566.002, Spearphishing Link, where a malicious link is delivered to the victim through email.

---

## Analyst Assessment

**Verdict:** MALICIOUS

**Severity:** MEDIUM

**Confidence:** HIGH

**Reasoning:**

> The combination of brand stuffing, suspicious URL infrastructure, credential-harvesting behavior, and social-engineering techniques provides sufficient evidence to classify the email as malicious phishing activity.

---

## Recommended Response

1. Quarantine/remove the email from affected mailboxes.
2. Block identified malicious domains and URLs.
3. Search mail logs for additional recipients of the same campaign.
4. Review authentication logs for users who interacted with the message.
5. Reset credentials if compromise is suspected.
6. Investigate affected endpoints for additional indicators.
7. Develop a scanner for keyword stuffing attempts.

---

## Conclusion

> The analyzed email was determined to be a phishing attempt designed to steal credentials, multiple technical and behavioral indicators support this assessment. The identified IOCs should be added to appropriate security controls and used to search for additional instances of the campaign.
