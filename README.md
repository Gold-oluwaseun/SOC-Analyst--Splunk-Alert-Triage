# SOC Alert Triage & Phishing Investigation Lab

**Hands-on SOC Analyst | Alert Investigation | Splunk | Threat Intelligence**

## Project Overview

This project documents my practical experience investigating and triaging security alerts in a SOC simulation environment.

Throughout this lab, I investigated firewall and inbound email alerts, analysed security events using Splunk, checked URLs against threat intelligence, reviewed network connection outcomes, and classified alerts as True Positives or False Positives.

The investigations covered phishing activity, brand impersonation, suspicious external URLs, and legitimate business communications.

## Objectives

- Investigate and triage security alerts.
- Analyse firewall and email logs using Splunk.
- Identify suspicious URLs, domains, and email senders.
- Use threat intelligence to assess URL reputation.
- Determine whether network connections were allowed or blocked.
- Distinguish True Positive alerts from False Positives.
- Document investigation findings and recommend remediation actions.

## Tools and Technologies

- **Splunk:** SIEM searches and log analysis.
- **Threat Intelligence:** URL and domain reputation checks.
- **SOC Simulator:** Alert triage and investigation.
- **Firewall Logs:** Network connection and traffic analysis.

---

## Alert 1: High — Access to Blacklisted External URL

**Alert Description:** A user attempted to access an external URL listed in the organisation's blacklist or threat intelligence feeds. The firewall successfully blocked the outbound connection.

### Alert Details

| Field | Value |
|---|---|
| Severity | High |
| Data Source | Firewall |
| Timestamp | November 24, 2026 — 19:58:09.411 |
| Source IP | `10.20.2.17` |
| Source Port | `34257` |
| Destination IP | `67.199.248.11` |
| Destination Port | `80` |
| URL | `http://bit.ly/3sHkX3da12340` |
| Protocol | TCP |
| Application | web-browsing |
| Firewall Action | **BLOCKED** |
| Rule | Blocked websites |
| Classification | **True Positive** |

### Investigation

I reviewed the alert details and searched Splunk using the URL to locate the corresponding firewall event.

The firewall logs confirmed that the outbound connection attempt was blocked.

I also checked the URL using a threat intelligence platform. Interestingly, the result showed a **CLEAN** reputation, despite the phishing activity identified during the investigation.

### Investigation Outcome

The alert was classified as a **True Positive** because the URL was associated with a phishing attempt. The phishing message impersonated the Amazon brand using the suspicious sender domain `urgentamazon.biz`. It created urgency by claiming that a package could not be delivered due to an incomplete address, encouraging the recipient to click an embedded Bitly shortened link.

The firewall successfully blocked the attempted connection, preventing access to the external URL.

### Evidence

**1. Alert Details**

![Alert 1 - Alert Details](screenshots/alert1-alert-details.png)

**2. Splunk Investigation — Blocked Connection**

![Alert 1 - Splunk Blocked Connection](screenshots/alert1-splunk-blocked.png)

**3. Threat Intelligence — Clean Result**

![Alert 1 - Threat Intelligence Clean Result](screenshots/alert1-threat-intelligence-clean.png)

**4. Incident Investigation Report**

![Alert 1 - Incident Investigation Report](screenshots/alert1-incident-report.png)

---

## Alert 2: Inbound Email — Microsoft Impersonation Phishing

**Alert Description:** An inbound email contained an external link with potentially suspicious characteristics. The investigation required checking firewall or proxy logs to determine whether an endpoint attempted to access the URL and whether the connection was allowed or blocked.

### Alert Details

| Field | Value |
|---|---|
| Data Source | Email |
| Timestamp | November 24, 2026 — 19:59:13.411 |
| Direction | Inbound |
| Subject | Unusual Sign-In Activity on Your Microsoft Account |
| Sender | `no-reply@m1crosoftsupport.co` |
| Recipient | `c.allen@thetrydaily.thm` |
| Attachment | None |
| URL | `https://m1crosoftsupport.co/login` |
| Network Action | **ALLOWED** |
| Classification | **True Positive** |

### Investigation

I reviewed the email and identified indicators of Microsoft brand impersonation.

The sender used the deceptive domain `m1crosoftsupport.co`, replacing the letter **i** in Microsoft with the number **1** to make the domain appear legitimate.

The email claimed that an unusual sign-in attempt had been detected from Lagos, Nigeria, and encouraged the recipient to review their account activity through the embedded link.

I checked the URL using a threat intelligence platform, which identified it as **MALICIOUS**.

I then searched the relevant activity in Splunk and reviewed the firewall/proxy logs. The connection action was recorded as **ALLOWED**, meaning the firewall permitted the request to reach the external website.

### Investigation Outcome

The alert was classified as a **True Positive** because the email contained clear indicators of a phishing attempt involving Microsoft brand impersonation, and the embedded URL was confirmed as malicious.

The connection was allowed, potentially exposing the recipient to a phishing page designed to steal account credentials.

**Important:** An ALLOWED network connection does not confirm that the recipient successfully logged in to their Microsoft account or submitted credentials. Further investigation would be required to establish whether credentials were entered or the account was compromised.

### Reason for Escalation

The alert required escalation because the malicious URL was accessed and the firewall/proxy permitted the connection. This created a potential risk of credential theft and unauthorised account access.

### Recommended Remediation

- Block the malicious domain `m1crosoftsupport.co` across the organisation's security infrastructure.
- Quarantine or remove the phishing email.
- Investigate the affected endpoint and review browser activity.
- Check Microsoft account sign-in logs for suspicious or unauthorised activity.
- If credentials were exposed, reset the affected password and revoke active sessions.
- Continue monitoring the affected account and endpoint.

### Evidence

**1. Alert Details**

![Alert 2 - Alert Details](screenshots/alert2-alert-details.png)

**2. Splunk Investigation — Allowed Connection**

![Alert 2 - Splunk Allowed Connection](screenshots/alert2-splunk-allowed.png)

**3. Threat Intelligence — Malicious URL**

![Alert 2 - Malicious Threat Intelligence Result](screenshots/alert2-threat-intelligence-malicious.png)

---

## Alert 3: Inbound Email — Employee Onboarding Link

**Alert Description:** An inbound email contained an external link with potentially suspicious characteristics. The investigation required checking the URL and reviewing relevant network activity.

### Alert Details

| Field | Value |
|---|---|
| Data Source | Email |
| Timestamp | November 24, 2026 — 19:53:42.411 |
| Direction | Inbound |
| Subject | Action Required: Finalize Your Onboarding Profile |
| Sender | `onboarding@hrconnex.thm` |
| Recipient | `j.garcia@thetrydaily.thm` |
| Attachment | None |
| URL | `https://hrconnex.thm/onboarding/15400654060/i` |
| Classification | **False Positive** |

### Investigation

I reviewed the email content and identified that it related to an employee onboarding process.

The message requested that the recipient complete their onboarding profile through the provided link.

I checked the URL using a threat intelligence platform, which returned a **CLEAN** result.

I also reviewed the corresponding events in Splunk and examined the full event details. The investigation supported the URL being associated with the legitimate onboarding process rather than a phishing attempt.

### Investigation Outcome

The alert was classified as a **False Positive** because the investigation supported that the email was a legitimate onboarding communication rather than a phishing attempt.

Although the alert flagged the external link for investigation, the available evidence did not indicate malicious activity.

### Evidence

**1. Alert Details**

![Alert 3 - Alert Details](screenshots/alert3-alert-details.png)

**2. Threat Intelligence — Clean URL**

![Alert 3 - Threat Intelligence Clean Result](screenshots/alert3-threat-intelligence-clean.png)

**3. Full Splunk Investigation — Event Details**

![Alert 3 - Full Splunk Investigation](screenshots/alert3-splunk-full-details.png)

---

## Additional Evidence — Closed Alerts Overview

The screenshot below shows the list of alerts I successfully closed during the SOC simulation, providing an overview of my alert management activity beyond the three investigations documented above.

![Closed Alerts Overview](screenshots/closed-alerts-overview.png)

---

## Lessons Learned

1. **A clean threat intelligence result does not guarantee safety.** Threat intelligence platforms rely on known indicators. New, unreported, or unclassified malicious URLs may still return clean results. Analysts must correlate threat intelligence with alert context and other available evidence.

2. **Allowed does not mean successfully authenticated.** An ALLOWED firewall action means a network request was permitted. It does not prove that a user entered credentials or successfully logged in.

3. **Blocked connections can prevent potential compromise.** A blocked request shows that the firewall prevented the connection, but the attempted activity still needs to be investigated.

4. **Lookalike domains are common phishing indicators.** Attackers may replace letters with numbers or use similar-looking domains to impersonate trusted organisations.

5. **Not every suspicious email is malicious.** Legitimate business communications can trigger security alerts. Analysts must investigate the sender, URL, message context, and supporting logs before making a classification.

6. **Security decisions should be evidence-based.** A SOC analyst must distinguish confirmed findings from assumptions and document the reasoning behind each classification.

## Skills Demonstrated

- SOC alert triage and investigation
- Splunk SIEM log analysis
- Firewall and proxy event investigation
- Email phishing analysis
- URL and domain threat intelligence checks
- Identification of brand impersonation and lookalike domains
- True Positive and False Positive classification
- Incident reporting and documentation
- Security recommendations and escalation

## Conclusion

This lab provided practical experience investigating different types of security alerts and making evidence-based decisions.

It strengthened my understanding of how SOC analysts correlate email alerts, SIEM logs, firewall activity, and threat intelligence to identify potential threats, distinguish legitimate activity from malicious behaviour, and determine appropriate next steps.

**Author:** Oluwaseun Feyisayo Olatunji

**LinkedIn:** [linkedin.com/in/oluwaseun-olatunji-b7790927b](https://www.linkedin.com/in/oluwaseun-olatunji-b7790927b)

**Areas of Interest:** Cybersecurity | SOC Analysis | Blue Team | Security Operations
