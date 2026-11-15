# T-Pot Honeypot Lab – Findings & Lessons Learned

## 🔍 PROJECT FINDINGS

This project provided hands-on experience deploying and exploring **T-Pot Community Edition**, an open-source multi-honeypot platform designed to observe and collect network activity.

During monitoring and analysis, I reviewed events from multiple data sources, including Suricata and Fatt. Two observations provided useful opportunities to practice interpreting security telemetry.

### Key Observation 1: Informational External IP Lookup

On October 9, 2026, Suricata generated an informational alert:

- **Alert:** `ET INFO External IP Address Lookup Domain (ipify .org) in TLS SNI`
- **Category:** Misc Activity
- **Severity:** 3
- **Protocol:** TCP/TLS
- **Destination Port:** 443
- **TLS SNI:** `api.ipify.org`
- **Action:** Allowed

The event indicated that a connection to an external IP lookup service was observed during a TLS handshake.

This was an informational detection, not confirmation of malicious activity. The alert showed that the traffic was allowed and did not establish which application initiated the connection or whether the lookup completed successfully.

A related Fatt event from October 8 contained TLS metadata for the same hostname. This provided another example of how different monitoring components can capture different details about network activity.

### Key Observation 2: TCP Stream Anomaly

On October 9, Suricata generated the following alert:

- **Alert:** `SURICATA STREAM FIN recv but no session`
- **Category:** Generic Protocol Command Decode
- **Severity:** 3
- **Protocol:** TCP
- **Destination Port:** 443
- **Action:** Allowed

This alert indicated that Suricata observed a TCP FIN packet without finding the expected session state.

Possible explanations include incomplete packet capture, asymmetric routing, out-of-order packets, or lost session state. The available event data was not sufficient to determine the exact cause.

I also reviewed a separate `SURICATA STREAM RST recv but no session` alert from October 5. This provided an additional example of how Suricata identifies unusual TCP stream conditions.

These observations reinforced the importance of investigating alert context rather than assuming every security event represents a confirmed attack.

## 🧠 TECHNICAL SKILLS DEVELOPED

Throughout this project, I gained practical experience with:

- **Linux Administration:** Working within an Ubuntu Server environment and navigating the Linux command line.
- **Virtualization:** Configuring and managing a virtual machine using Oracle VirtualBox.
- **Docker:** Exploring the containerized architecture used by the T-Pot platform.
- **Network Configuration:** Configuring NAT networking and port forwarding to access the T-Pot web interface.
- **Security Monitoring:** Exploring T-Pot dashboards, Kibana Discover, and network activity visualizations.
- **Log Analysis:** Reviewing and interpreting event records from multiple monitoring sources.
- **Alert Triage:** Examining alert signatures, severity, protocols, connection details, and available flow metadata.
- **Technical Documentation:** Recording observations, distinguishing confirmed facts from possible explanations, and documenting investigation limitations.

## ⚠️ CHALLENGES & TROUBLESHOOTING

During deployment, configuration, and monitoring, I worked through several environment and access-related challenges.

These included:

- Configuring VirtualBox networking and port forwarding.
- Establishing access to the T-Pot web interface.
- Navigating the Linux command-line environment.
- Verifying the installed T-Pot version.
- Exploring the platform's Docker-based architecture.
- Navigating Kibana Discover and filtering events from different data sources.
- Interpreting alerts that contained incomplete or limited network-session information.

Working through these challenges reinforced the importance of troubleshooting methodically, understanding the environment, and validating findings before drawing conclusions.

## 🎯 KEY TAKEAWAYS

This lab helped strengthen my understanding of:

- How honeypots support cybersecurity research and threat observation.
- How security monitoring tools collect and present network telemetry.
- How Suricata identifies network events and unusual traffic conditions.
- How multiple data sources can provide different perspectives on network activity.
- Why alert severity and classification should be considered alongside the underlying event details.
- Why an alert should not automatically be treated as proof of malicious activity.
- The importance of distinguishing observed evidence from possible explanations.
- How virtualization and containerization support security experimentation.

One of the most important lessons was that effective alert triage requires context. Reviewing the signature alone is not enough; understanding the protocol, flow information, available metadata, and limitations of the event is essential before reaching a conclusion.

## 🚀 NEXT STEPS

Future work on this lab may include:

- Reviewing additional honeypot-generated events.
- Identifying recurring patterns in observed network activity.
- Examining individual honeypot interactions in greater detail.
- Exploring additional event fields and network metadata.
- Expanding the investigation with supporting screenshots and documented findings.

## 📚 PROJECT CONCLUSION

The T-Pot Honeypot Lab provided an opportunity to deploy and explore an open-source security monitoring platform in a virtual environment.

Through this project, I gained practical experience with Linux administration, virtualization, Docker, network configuration, security dashboards, and event analysis.

Most importantly, I practiced reviewing security alerts, interpreting available telemetry, and documenting findings without overstating what the evidence showed. This project provided a foundation for continued development in cybersecurity monitoring, alert triage, and incident investigation.
