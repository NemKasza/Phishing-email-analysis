# Phishing Infrastructure OSINT Investigation

**Case ID:** OSINT-001 \
**Date:** 14-09-2026 \
**Analyst:** Nagy-Kasza Bence \
**Investigation Type:** Passive OSINT / Threat Intelligence \
**Related Case:** PHISH-001 

---

## Objective

The objective of this investigation was to identify and assess publicly available information related to infrastructure observed during a phishing email investigation.

The investigation focused primarily on the IP address `89.43.67.40` and several domains extracted from the original email and its headers.

No suspicious URLs were directly accessed during the investigation.

---

## Tools Used

The following tools were used through the OSINT Framework and independently:

* AbuseIPDB — IP reputation and abuse reports
* VirusTotal — IP, domain and URL reputation
* Shodan — exposed services and infrastructure
* SynapsInt — OSINT and infrastructure correlation
* IPVoid — IP reputation and blacklist checks
* IPinfo — IP ownership, ASN, hostname and geolocation
* OSINT Framework — discovery of relevant OSINT resources

---

## Investigated Indicators

| Type        | Indicator                                            | Status                                      |
| ----------- | ---------------------------------------------------- | ------------------------------------------- |
| IP          | `89.43.67.40`                                        | Active infrastructure information available |
| Domain      | `mqbixnvoq[.]us`                                     | No current DNS resolution                   |
| Domain      | `google-apple-amazon[.]chelsea[.]org[.]ferdaus[.]my` | No current DNS resolution                   |
| Domain/Path | `nbcwmooats[.]home`                                  | No current DNS resolution                   |
| Hostname    | `fitness.bigpond.com`                                | Reported by IPinfo                          |

---

# IP Address Analysis

<img width="527" height="176" alt="VirusTotal findings" src="https://github.com/user-attachments/assets/064f2edb-1706-4d52-9946-f7ad7b3050eb" />


## IP Information

**IP:** `89.43.67.40`

According to IPinfo, the address is associated with:

| Field          | Finding                              |
| -------------- | ------------------------------------ |
| ASN            | AS51559                              |
| Organization   | Netinternet Bilisim Teknolojileri AS |
| Country        | Turkey                               |
| City           | Denizli                              |
| Network        | `89.43.67.0/24`                      |
| AS Type        | ISP                                  |
| Hostname       | `fitness.bigpond.com`                |
| Hosted domains | 0                                    |

IPinfo currently reports `89.43.67.40` as part of AS51559 and the `89.43.67.0/24` network.

The network is also independently listed as being announced by AS51559 / Netinternet Bilisim Teknolojileri AS.

### Analyst Note

The IP geolocation and ISP information identify the network associated with the address. They do **not** identify the threat actor or prove that the network provider was involved in the phishing activity.

No attribution to Netinternet Bilisim Teknolojileri AS was established.

---

# IP Reputation Analysis

The IP address was checked using:

* AbuseIPDB
* VirusTotal
* Shodan
* SynapsInt
* IPVoid
* IPinfo

The investigation did not produce additional evidence that was strong enough to independently attribute the IP to the phishing campaign beyond its presence in the original email evidence.

The IP should therefore be treated as a **suspicious/associated infrastructure IOC**, rather than definitive proof of malicious ownership.

### Important limitation

Reputation databases can change over time. A lack of current detections does not prove that an IP was never involved in malicious activity, particularly when investigating infrastructure associated with an older phishing email.

---

# Domain Analysis

##  `mqbixnvoq[.]us`

The domain was extracted from the sender address:

`nooreply@mqbixnvoq.us`

Current DNS investigation returned no resolution for the domain.

### Assessment

The domain cannot currently be reached through normal DNS resolution. This may indicate that the domain has expired, been suspended, had its DNS records removed, or is otherwise inactive.

The lack of current DNS resolution does not remove its value as a historical IOC because it was present in the original phishing email.

---

##  `google-apple-amazon[.]chelsea[.]org[.]ferdaus[.]my`

This hostname was identified during header analysis.

The hostname contains several recognizable brand names:

* Google
* Apple
* Amazon

This naming structure may be intended to create an appearance of legitimacy or confuse users and automated analysis.

Current DNS analysis returned no result for the hostname.

### Assessment

The hostname is suspicious in the context of the phishing email, but the current absence of DNS resolution prevents further live infrastructure analysis.

The hostname should therefore be retained as a historical IOC.

---

## 6.3 `nbcwmooats[.]home`

This domain/path was identified within the suspicious URL contained in the email.

Current DNS analysis returned no result.

Because the URL is no longer resolvable, it was not directly accessed.

### Assessment

The domain is retained as an IOC associated with the phishing URL. Its current inactivity prevents confirmation of the original server behavior.

---

#  Infrastructure Correlation

The investigation produced the following relationship:

```text
PHISHING EMAIL
      |
      +-- Sender
      |     nooreply@mqbixnvoq[.]us
      |
      +-- Suspicious hostname
      |     google-apple-amazon[.]chelsea[.]org[.]ferdaus[.]my
      |
      +-- Sending infrastructure
      |     89.43.67.40
      |     AS51559
      |     Netinternet Bilisim Teknolojileri AS
      |
      +-- Phishing URL
            storage.googleapis.com
                    |
                    +-- suspicious path/fragment
                    |
                    +-- nbcwmooats[.]home
```

The infrastructure relationship is based primarily on the original email evidence and passive OSINT findings.

No evidence was found that would justify attributing the infrastructure to the hosting provider itself.

---

#  Key Findings

The investigation identified the following:

1. `89.43.67.40` belongs to AS51559, operated by Netinternet Bilisim Teknolojileri AS.
2. IPinfo currently associates the IP with the hostname `fitness.bigpond.com`.
3. Current IPinfo data reports no hosted domains directly associated with the IP.
4. The sender domain `mqbixnvoq[.]us` currently has no DNS resolution.
5. The suspicious multi-brand hostname currently has no DNS resolution.
6. `nbcwmooats[.]home` currently has no DNS resolution.
7. The infrastructure appears to have limited current visibility, consistent with the possibility that some of the infrastructure has been removed or changed.
8. No evidence was found that would support attributing the phishing activity to Netinternet Bilisim Teknolojileri AS.

---

# IOC List

| Type     | IOC                                                  | Status                  |
| -------- | ---------------------------------------------------- | ----------------------- |
| Email    | `nooreply[@]mqbixnvoq[.]us`                          | Historical IOC          |
| Domain   | `mqbixnvoq[.]us`                                     | Currently unresolved    |
| Hostname | `google-apple-amazon[.]chelsea[.]org[.]ferdaus[.]my` | Currently unresolved    |
| IP       | `89.43.67.40`                                        | Infrastructure IOC      |
| Domain   | `nbcwmooats[.]home`                                  | Currently unresolved    |
| URL      | `hxxps://storage[.]googleapis[.]com/...`             | Historical phishing URL |

---

# Assessment

**Assessment:** Suspicious / Malicious-associated infrastructure

**Confidence:** Medium

The OSINT findings support the original phishing assessment but do not independently prove malicious ownership of the identified IP address.

The strongest evidence comes from correlation with the original phishing email, including the suspicious sender, brand impersonation, malicious-looking URL structure, social-engineering content, and the associated infrastructure indicators.

The current lack of DNS resolution for several domains limits further infrastructure investigation.

---

# 11. Limitations

This investigation was performed using passive OSINT and publicly available information.

The following limitations apply:

* Several investigated domains are currently unresolved.
* No live phishing infrastructure was accessed.
* Historical ownership of the infrastructure could not be conclusively established.
* IP geolocation does not identify the person responsible for the activity.
* A hosting provider or ASN should not be considered malicious solely because an investigated IP belongs to its network.
* Reputation databases may change after the original phishing campaign.

---

#  Conclusion

The OSINT investigation identified `89.43.67.40` as infrastructure associated with AS51559 / Netinternet Bilisim Teknolojileri AS in Turkey. Several domains and hostnames extracted from the phishing email are currently not resolvable, suggesting that the infrastructure is no longer fully active.

The available OSINT findings support the original phishing investigation but do not provide sufficient evidence to attribute the activity to the network provider or identify the threat actor.

The identified IP addresses, domains, hostnames and URLs should be retained as historical IOCs and can be used for detection and retrospective searches if additional telemetry becomes available.
