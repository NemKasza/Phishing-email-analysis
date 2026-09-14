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

 The email presents itself as a legitimate communication from a Cloud storage provider. However several source file artifacts and technical details indacate that this is a malicious phishing email.

---

## Header Analysis

### Header & Authentication Analysis

Indicators of potential malicious activity:\
**Sender domain:** google-apple-amazon[.]chelsea[.]org[.]ferdaus[.]my

\
Possible brand-stuffing indicators. Attackers frequently register subdomains or domains containing multiple high-profile brand names to decieve automated filters or users glancing quickly at the headers.



### Authentication Results

| Mechanism | Result |
| --------- | ------ |
| SPF       | Pass   |

**Description:**

Email authentication results were reviewed to determine whether the sending infrastructure was authorized to send on behalf of the claimed domain.

**Analyst Note:**

> SPF passed for the envelope sender domain google-apple-amazon.chelsea.org.ferdaus.my; however, this does not establish that the sender is legitimate because the authenticated domain does not correspond to the apparent Cloud storage brand.
---

## URL Analysis

**Extracted URL:**\
hxxps\://storage[.]googleapis[.]com/whilewait/brightway[.]html#ZX=zUxMCdQKLWkFpNYXqOXABCnFCaEo&5GpCEpBGTKy&256552/996/nbcwmooats[.]home[.]php=?sq=3D32-195982&lk=3D38563-5&page=3D368

**Used tools: \
urlscan.io \
gchq.github.io/CyberChef/**



**Finding:**\
The phishing links point to storage.googleapis.com, a legitimate Google-hosted domain, while embedding a long fragment containing obfuscated/randomized parameters and a reference to a second domain. The use of a trusted hosting domain may assist in bypassing simplistic URL reputation controls. Because the suspected destination appears after the # fragment, further dynamic analysis would be required to confirm whether client-side JavaScript processes the fragment and redirects the victim.

**IOC:**

- Domain: trusted domain (storage.googleapis.com) containing malicious scripts
- URL: \
  hxxps\://storage[.]googleapis[.]com/whilewait/brightway[.]html#ZX=zUxMCdQKLWkFpNYXqOXABCnFCaEo&5GpCEpBGTKy&256552/996/nbcwmooats[.]home[.]php=?sq=3D32-195982&lk=3D38563-5&page=3D368



---

## Social Engineering Analysis

The following indicators were identified:
| Indicator             | Evidence                                | Risk                              |
| --------------------- | --------------------------------------- | --------------------------------- |
| Urgency               | "Final Notice", "immediate update"      | Encourages impulsive action       |
| Threat                | Account suspension                      | Creates fear of losing access     |
| Credential harvesting | "Update Account Now" CTA                | Attempts to induce authentication |
| Brand impersonation   | Cloud branding                          | Establishes false legitimacy      |
| URL deception         | Google-hosted URL + suspicious fragment | Obscures actual destination       |
| Content obfuscation   | Hidden unrelated HTML                   | Potential filtering evasion       |


**Description:**

> The message uses urgency and impersonation to encourage the recipient to interact with the provided link. These characteristics are commonly associated with phishing campaigns. You can also discover spelling mistakes.

---

## Email body analysis / Hidden/Irrelevant Content: 

>The HTML body contains hidden elements (display:none) containing unrelated newsletter, news, travel, and gaming content. The presence of unrelated text and high-entropy/random strings within hidden HTML is inconsistent with the visible Cloud storage notification and may represent content-obfuscation or filtering-evasion behavior.

---

## Indicators of Compromise (IOCs)

| Type   | Indicator                                            | Classification                    |
| ------ | ---------------------------------------------------- | --------------------------------- |
| Email  | `nooreply[@]mqbixnjnvoq[.]us`                        | Suspicious sender                 |
| Domain | `google-apple-amazon[.]chelsea[.]org[.]ferdaus[.]my` | Suspicious envelope-sender domain |
| IP     | `89.43.67.40`                                        | Sending infrastructure            |
| URL    | `hxxps://storage[.]googleapis[.]com/...`             | Phishing URL                      |
| Domain | `[destination domain if dynamically confirmed]`      | Malicious destination             |


---

## MITRE ATT&CK Mapping

| Technique                                   | ID        | Evidence                                  | Confidence |
| ------------------------------------------- | --------- | ----------------------------------------- | ---------- |
| Phishing: Spearphishing Link                | T1566.002 | Malicious-looking URL delivered via email | High       |
| Obfuscated/Compressed Files and Information | TBD       | Hidden/randomized HTML content            | Medium     |


**Description:**

> Only T1566.002 is confidently mapped from static evidence. Additional technique mapping would require dynamic analysis.

---

## Analyst Assessment

**Verdict:** MALICIOUS

**Severity:** MEDIUM


> The message presents a credential-phishing risk but no evidence of successful credential submission, endpoint compromise, malware execution, or account takeover was identified during static analysis. Severity would increase if telemetry confirms user interaction, credential submission, or subsequent authentication anomalies.

**Confidence:** HIGH

**Reasoning:**

> The combination of brand stuffing, suspicious URL infrastructure, credential-harvesting behavior, and social-engineering techniques provides sufficient evidence to classify the email as malicious phishing activity.

---

## Recommended Response

### 1. Immediate containment
- Quarantine the message.
- Search for identical/similar messages.
- Identify all recipients.
- Block confirmed malicious URLs/domains where appropriate.
### 2. User-impact investigation
- Determine whether recipients clicked the URL.
- Review proxy/DNS/browser telemetry.
- Identify credential submissions.
- Review authentication logs for anomalous sign-ins.
### 3. Post-compromise investigation
- Reset credentials if credential exposure is confirmed/suspected.
- Revoke active sessions/tokens where appropriate.
- Investigate affected endpoints.
### 4. Detection improvement
- Create SIEM/Email Security detections for:
 - suspicious sender-domain patterns
 - hidden HTML
 - display:none content
 - excessive random strings
 - suspicious external links
 - brand impersonation
---

## Analysis Limitations

This assessment was primarily based on static analysis of the supplied email. No evidence of successful credential submission, user interaction, endpoint compromise, or account takeover was available during analysis. Dynamic execution of the embedded URL was not used to establish the final destination or browser behavior. Therefore, conclusions regarding JavaScript execution, redirection, and credential harvesting should be treated as unconfirmed unless supported by sandbox, proxy, browser, or endpoint telemetry.

## Conclusion

> The analyzed email was determined to be a phishing attempt designed to steal credentials, multiple technical and behavioral indicators support this assessment. The identified IOCs should be added to appropriate security controls and used to search for additional instances of the campaign.
