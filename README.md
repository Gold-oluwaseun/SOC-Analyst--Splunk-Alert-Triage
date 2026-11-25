# SOC Alert Triage & Phishing Investigation Lab

## Overview

This project documents hands-on SOC alert triage and phishing investigations conducted in a SOC Simulator environment.

The investigations involved analysing email alerts, reviewing firewall and network activity in Splunk, checking suspicious URLs using threat intelligence, and determining whether alerts were True Positives or False Positives.

Three security alerts were investigated, documented, and closed based on the available evidence.

## Objectives

- Investigate and triage security alerts.
- Identify phishing indicators in suspicious emails.
- Analyse firewall and network events using Splunk.
- Verify URLs using threat intelligence.
- Determine whether suspicious activity was blocked or allowed.
- Classify alerts accurately as True Positives or False Positives.
- Document findings and recommend appropriate response actions.

## Tools & Technologies

- Splunk — SIEM and log investigation
- Threat Intelligence — URL reputation checks
- SOC Simulator — Alert investigation and triage
- Firewall Logs — Network activity analysis

---

# Alert 1: High — Access to Blacklisted External URL

**Severity:** High  
**Classification:** True Positive  
**Firewall Action:** BLOCKED

## Alert Details

The alert involved an attempt to access an external URL associated with a phishing-related investigation.

The activity was reviewed using firewall logs in Splunk to establish whether the connection was permitted or blocked.

**Investigation findings:**

- Source IP: `10.20.2.17`
- Source Port: `34257`
- Destination IP: `67.199.248.11`
- Destination Port: `80`
- URL: `http://bit.ly/3sHkX3da12340`
- Application: `web-browsing`
- Protocol: TCP
- Firewall Rule: `Blocked websites`
- Action: BLOCKED
- Threat Intelligence Result: CLEAN

## Investigation & Analysis

I reviewed the alert details and searched the available firewall events in Splunk to determine the outcome of the connection attempt.

The firewall logs confirmed that the connection was blocked.

Although the URL returned a clean result during the threat intelligence check, this did not conclusively establish that the URL was safe. The wider phishing context remained relevant to the investigation.

## Outcome

**True Positive.**

The alert was treated as a True Positive based on the phishing-related activity and supporting investigation context. The firewall successfully blocked the connection attempt.

## Evidence

### Alert Details

![Alert 1 - Alert Details](screenshots/alert1-alert-details.png)

### Splunk Investigation - Blocked Connection

![Alert 1 - Splunk Blocked Connection](screenshots/alert1-splunk-blocked.png)

### Threat Intelligence - Clean Result

![Alert 1 - Threat Intelligence Clean Result](screenshots/alert1-threat-intelligence-clean.png)

### Incident Report

![Alert 1 - Incident Report](screenshots/alert1-incident-report.png)

---

# Alert 2: Inbound Email — Microsoft Impersonation Phishing

**Severity:** As provided by the simulator  
**Classification:** True Positive  
**Firewall Action:** ALLOWED

## Alert Details

This alert involved an inbound email impersonating Microsoft and attempting to direct the recipient to a suspicious login website.

**Email details:**

- Subject: `Unusual Sign-In Activity on Your Microsoft Account`
- Sender: `no-reply@m1crosoftsupport.co`
- Recipient: `c.allen@thetrydaily.thm`
- Claimed sign-in location: Lagos, Nigeria
- URL: `https://m1crosoftsupport.co/login`
- Attachment: None
- Threat Intelligence Result: MALICIOUS

The sender used the number `1` in place of the letter `i` in the Microsoft-related domain, a common impersonation technique.

## Investigation & Analysis

I reviewed the email details and examined the associated URL using threat intelligence.

The URL was identified as malicious.

I then investigated the related firewall and network activity in Splunk. The logs showed that the connection was **ALLOWED**, meaning the firewall permitted the request to reach the destination.

This does not independently confirm that the recipient submitted credentials or that the Microsoft account was compromised.

## Outcome

**True Positive.**

The email was identified as a phishing attempt, and the malicious URL had an allowed connection.

Further investigation of endpoint activity and account sign-in logs would be necessary to determine whether credentials were submitted or the account was compromised.

## Evidence

### Alert Details

![Alert 2 - Alert Details](screenshots/alert2-alert-details.png)

### Splunk Investigation - Allowed Connection

![Alert 2 - Splunk Allowed Connection](screenshots/alert2-splunk-allowed.png)

### Threat Intelligence - Malicious Result

![Alert 2 - Threat Intelligence Malicious Result](screenshots/alert2-threat-intelligence-malicious.png)

---

# Alert 3: Inbound Email — Employee Onboarding Link

**Severity:** As provided by the simulator  
**Classification:** False Positive

## Alert Details

This alert involved an inbound email containing an employee onboarding link.

**Email details:**

- Subject: `Action Required: Finalize Your Onboarding Profile`
- Sender: `onboarding@hrconnex.thm`
- Recipient: `j.garcia@thetrydaily.thm`
- URL: `https://hrconnex.thm/onboarding/15400654060/i`
- Attachment: None
- Direction: Inbound
- Threat Intelligence Result: CLEAN

## Investigation & Analysis

I reviewed the alert details, checked the URL using threat intelligence, and examined the full event information in Splunk.

The URL returned a clean result, and the available event details supported the legitimacy of the onboarding email.

No evidence of malicious activity was identified in the information available during the investigation.

## Outcome

**False Positive.**

The alert was closed as a False Positive because the investigation did not identify indicators supporting malicious activity.

## Evidence

### Alert Details

![Alert 3 - Alert Details](screenshots/alert3-alert-details.png)

### Threat Intelligence - Clean Result

![Alert 3 - Threat Intelligence Clean Result](screenshots/alert3-threat-intelligence-clean.png)

### Splunk - Full Event Details

![Alert 3 - Splunk Full Event Details](screenshots/alert3-splunk-full-details.png)

---

# Additional Evidence — Closed Alerts Overview

The screenshot below shows the overview of alerts that were investigated and closed during the exercise.

![Closed Alerts Overview](screenshots/closed-alerts-overview.png)

---

# Lessons Learned

- A clean threat intelligence result does not automatically prove that a URL is safe.
- Firewall logs help establish whether suspicious network connections were allowed or blocked.
- A malicious URL that was allowed requires further investigation and appropriate escalation.
- Email sender domains, message content, and embedded links are important phishing indicators.
- Alert classification should be based on investigation findings rather than alert severity alone.
- Splunk provides useful visibility into network activity and supports SOC investigations.

# Skills Demonstrated

- Security alert triage
- Phishing email investigation
- SIEM log analysis using Splunk
- Firewall event investigation
- URL threat intelligence analysis
- True Positive and False Positive classification
- Incident documentation
- Security investigation and reporting

---

# Author

**Oluwaseun Feyisayo Olatunji**

[LinkedIn Profile](https://www.linkedin.com/in/oluwaseun-olatunji-b7790927b)
