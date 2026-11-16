
# T-Pot Honeypot Lab – Monitoring & Attack Analysis

## 🔍 MONITORING OVERVIEW

After deploying T-Pot, I explored its monitoring capabilities to understand how honeypots collect, process, and visualize network activity.

T-Pot provides a centralized environment for monitoring interactions with deployed honeypot services. Its dashboards and visualization tools make it possible to explore network activity, review collected events, and investigate potential security threats.

This section documents my exploration of the T-Pot monitoring interface, Attack Map, Kibana Discover, and selected security events.

## 🌎 T-POT ATTACK MAP

The T-Pot Attack Map provides a visual representation of network activity targeting the honeypot environment.

Using the Attack Map, I explored how incoming activity is displayed geographically and how network interactions can be visualized.

**Areas explored:**
- Geographic visualization of observed network activity.
- Source and destination information displayed by the platform.
- Patterns in observed connection activity.
- How honeypot interactions are represented visually.

### 📸 Attack Map Screenshot

*Insert Attack Map screenshot here.*

## 📊 SECURITY EVENT MONITORING

T-Pot uses multiple honeypot services and supporting components to collect and process network activity.

I explored the available monitoring dashboards and used Kibana Discover to examine individual log records and their associated metadata.

**Areas explored:**
- Available monitoring dashboards.
- Collected security events and log sources.
- Event timestamps and network connection details.
- Protocol information and alert metadata.
- How different monitoring components contribute to security visibility.

### 📸 Monitoring Dashboard Screenshot
<img src="../screenshots/10.8.26 Alert filter dashboard.png" width="1050" height="950" alt="Alert Filter Dash">


## ⚔️ ATTACK ANALYSIS

To gain practical experience with security event triage, I examined selected records collected by the T-Pot environment.

The investigation focused on understanding what the available evidence showed, interpreting network metadata, and distinguishing informational activity from events requiring further investigation.

### **🔎Investigation 1: Informational External IP Lookup**

**Detection:** `ET INFO External IP Address Lookup Domain (ipify .org) in TLS SNI`

One Suricata alert identified a TLS connection to `api.ipify.org`, a service commonly used to retrieve a device's public IP address.

**Event Details:**

| Field | Observed Value |
|---|---|
| Date | October 9, 2026 |
| Source IP | `[T-Pot Internal IP]` |
| Source Port | `37572` |
| Destination IP | `[Destination IP Redacted]` |
| Destination Port | `443` |
| Protocol | TCP |
| Application Protocol | TLS |
| TLS Version | TLS 1.3 |
| SNI Hostname | `api.ipify.org` |
| Alert Severity | 3 |
| Signature Severity | Informational |
| Action | Allowed |

**Analysis:**

The event showed an outbound connection from the T-Pot host's recorded internal address to an external destination over port 443.

The TLS Server Name Indication (SNI) field identified the requested hostname as `api.ipify.org`. Suricata generated an informational alert because the hostname matched a rule associated with external IP address lookup activity.

The recorded flow statistics included:

- 1,825 bytes sent toward the destination.
- 3,242 bytes received from the destination.
- 5 packets sent toward the destination.
- 6 packets received from the destination.

I also identified a related Fatt TLS metadata record from October 8 containing the same hostname and destination IP. This demonstrated how different log sources can provide additional context about network activity.

The two records occurred at different times and had different TLS fingerprints, so they were treated as separate connections.

**Assessment:**

The available evidence was consistent with a connection to a public IP lookup service. The informational alert did not establish malicious activity, and the records did not identify which application initiated the connection.


### **🔎Investigation 2: TCP Stream Anomaly**

**Detection:** `SURICATA STREAM FIN recv but no session`

A second Suricata event identified a TCP stream-tracking anomaly.

**Event Details:**

| Field | Observed Value |
|---|---|
| Date | October 9, 2026 |
| Source IP | `[Source IP Redacted]` |
| Source Port | `42426` |
| Destination IP | `[Remote IP Redacted]` |
| Destination Port | `443` |
| Protocol | TCP |
| Alert Severity | 3 |
| Action | Allowed |
| Flow ID | `2116478010308516` |

**Analysis:**

TCP connections use session tracking to associate packets with an established connection. A FIN flag is normally used when a device is closing its side of a TCP connection.

In this event, Suricata reported receiving a FIN-related packet without finding the session state it expected.

The flow summary recorded:

- 54 bytes sent toward the destination.
- 1 packet sent toward the destination.
- 0 bytes recorded in the reverse direction.
- 0 packets recorded in the reverse direction.

Possible explanations for this type of alert include incomplete packet visibility, asymmetric traffic capture, out-of-order packets, or unavailable connection-tracking state.
The source address, `[Source IP Redacted]`, was different from the T-Pot host's recorded internal address, `[T-Pot Internal IP]`. The available event did not establish which device or network the source address represented.

**Assessment:**

This event represented a TCP stream-tracking anomaly rather than a confirmed attack. The available flow information was insufficient to determine the exact cause of the alert.


## 📌 ADDITIONAL OBSERVATIONS

I also identified another Suricata alert:

`SURICATA STREAM RST recv but no session`

The October 5 event involved traffic between `[Remote IP Redacted]:443` and `[T-Pot Internal IP]:36644`.

This alert provided another example of TCP stream-tracking behavior. Although it shared a destination IP with the October 9 FIN alert, the connection details differed. I did not assume that the events represented one continuous connection or a common incident.


## 🧠 INVESTIGATION APPROACH

For each selected event, I followed a basic investigation process:

1. Identified the alert signature and severity.
2. Reviewed the timestamp and source/destination information.
3. Examined available protocol and application metadata.
4. Reviewed packet counts, byte counts, and flow information.
5. Compared related records across different log sources.
6. Distinguished confirmed observations from possible explanations.
7. Identified what additional evidence would be needed.

This process helped develop a more structured approach to reviewing security alerts and interpreting network telemetry.

## ⚠️ INVESTIGATION LIMITATIONS

The available records provided useful network-level information, but they did not identify the originating application or process behind every connection.

The TCP stream alerts also lacked sufficient packet-level context to determine their exact cause.

GeoIP enrichment fields in the reviewed records referred to the lab's recorded external IP address rather than necessarily identifying the remote destination. Location information from GeoIP databases is an estimate and should not be treated as proof of a device's physical location.

These limitations were considered when evaluating and documenting the observed activity.

## 🎯 KEY TAKEAWAYS

This exercise provided hands-on exposure to:

- Network activity monitoring through a honeypot platform.
- Reviewing security events using Kibana Discover.
- Interpreting Suricata alert signatures and metadata.
- Examining TCP and TLS connection information.
- Understanding TLS SNI, JA3, and JA4 fingerprints.
- Comparing records across multiple log sources.
- Reviewing timestamps, flow IDs, packet counts, and byte counts.
- Distinguishing informational alerts from potentially suspicious activity.
- Documenting findings while recognizing evidence limitations.

The investigation reinforced the importance of reviewing the underlying event data rather than relying solely on an alert name or severity value.

The observations documented here represent an initial investigation of selected events, not evidence of a confirmed security compromise.
