# Common Network Security Threats

**Author:** Dhrumit Asari  
**Track:** Security Analyst  
**Date:** October 2026  
**Repository:** [security-analyst-task4-network-threats](https://github.com/Paperlan1729/security-analyst-task4-network-threats)

---

## Introduction

In today's hyper-connected digital landscape, network security threats pose significant risks to individuals, organizations, and critical infrastructure. As reliance on networked systems grows—spanning cloud services, IoT devices, remote work environments, and global supply chains—the attack surface expands dramatically. Threat actors ranging from opportunistic cybercriminals to sophisticated nation-state groups continuously exploit vulnerabilities in network protocols, configurations, and human factors. Understanding common network security threats is essential for security analysts, network administrators, and decision-makers to implement effective defenses, reduce risk exposure, and maintain operational resilience. This report examines key threats, their mechanisms, real-world impacts, and mitigation strategies.

## DoS/DDoS Attacks

### How They Work
Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS) attacks aim to overwhelm a target system, service, or network with excessive traffic or resource requests, rendering it unavailable to legitimate users. In a classic DoS, a single source floods the target. In DDoS, multiple compromised devices (a botnet) coordinate the attack, amplifying volume and making attribution harder. Common techniques include volumetric floods (UDP/ICMP floods), protocol attacks (SYN floods exhausting connection tables), and application-layer attacks (HTTP floods targeting web servers).

### Real-World Example
In February 2020, Amazon Web Services (AWS) mitigated a record-breaking DDoS attack peaking at 2.3 Tbps. The attack leveraged CLDAP reflection techniques. Earlier, the 2016 Dyn DNS DDoS (powered by the Mirai botnet) disrupted major services including Twitter, Netflix, and Reddit across the eastern United States.

### Impact
- Service unavailability leading to lost revenue and productivity
- Reputational damage
- Potential for secondary attacks during downtime
- High mitigation costs for cloud and enterprise providers

### Mitigation Strategies
1. Deploy DDoS protection services (e.g., AWS Shield, Cloudflare, Akamai) that absorb and filter volumetric traffic.
2. Implement rate limiting, traffic shaping, and anomaly detection at network edges and application layers.
3. Use anycast routing and geographically distributed infrastructure to dilute attack impact, combined with upstream ISP blackholing or scrubbing centers.

## Man-in-the-Middle (MITM) Attacks

### How They Work
A Man-in-the-Middle attack occurs when an adversary secretly intercepts and potentially alters communications between two parties who believe they are communicating directly. Common vectors include ARP spoofing on local networks, rogue Wi-Fi access points (evil twin), SSL/TLS stripping, and compromised routers or proxies. Once positioned, the attacker can eavesdrop, inject malware, steal credentials, or modify data in transit.

### Real-World Example
In 2015, the Superfish adware controversy revealed that Lenovo pre-installed software that installed a root certificate enabling MITM interception of HTTPS traffic on affected laptops. Separately, nation-state actors have used MITM techniques against diplomats and journalists via compromised network infrastructure.

### Impact
- Credential and session token theft
- Exposure of sensitive data (financial, personal, corporate)
- Potential for further lateral movement or ransomware deployment
- Erosion of trust in encrypted communications if certificates are compromised

### Mitigation Strategies
1. Enforce HTTPS Everywhere with HSTS (HTTP Strict Transport Security) and certificate pinning where appropriate.
2. Use VPNs or zero-trust network access for sensitive traffic; avoid untrusted public Wi-Fi or verify networks carefully.
3. Deploy network monitoring for ARP anomalies, implement 802.1X authentication, and educate users on certificate warnings.

## IP Spoofing

### How They Work
IP spoofing involves forging the source IP address in packet headers to impersonate a trusted host or conceal the attacker's identity. It is frequently used in reflection/amplification DDoS attacks, to bypass weak IP-based access controls, or to poison routing tables. Because the IP protocol lacks built-in authentication of source addresses, spoofed packets can be injected into the network.

### Real-World Example
The 2013 Spamhaus DDoS attack involved massive DNS amplification using spoofed source addresses, generating traffic volumes exceeding 300 Gbps and disrupting internet services globally. Many historical SYN flood attacks also relied on spoofed source IPs.

### Impact
- Enables amplification attacks that magnify traffic volume
- Bypasses simple IP allow-listing
- Complicates forensic attribution and incident response
- Can facilitate session hijacking in certain protocol scenarios

### Mitigation Strategies
1. Implement ingress and egress filtering (BCP 38 / RFC 2827) at network boundaries to drop packets with invalid source addresses.
2. Use cryptographic authentication (IPsec, TLS, SSH) rather than relying solely on IP addresses for trust.
3. Deploy anti-spoofing controls on routers and firewalls, and monitor for asymmetric traffic patterns indicative of spoofing.

## DNS Poisoning / Spoofing

### How They Work
DNS poisoning (or cache poisoning) involves injecting false DNS records into a resolver's cache so that legitimate domain names resolve to attacker-controlled IP addresses. Classic techniques exploit weaknesses in DNS transaction IDs and source ports (as demonstrated by Dan Kaminsky in 2008). Modern variants include compromising authoritative servers, exploiting recursive resolvers, or using BGP hijacks in combination with DNS.

### Real-World Example
In 2008, the Kaminsky vulnerability allowed widespread cache poisoning until patches were applied. More recently, the 2019 Sea Turtle campaign by advanced actors systematically compromised DNS registries and registrars to redirect traffic for government and telecommunications domains in the Middle East and beyond.

### Impact
- Users redirected to phishing or malware sites
- Interception of email and web traffic
- Potential for widespread credential harvesting or watering-hole attacks
- Undermines the trust model of the Domain Name System

### Mitigation Strategies
1. Deploy DNSSEC (DNS Security Extensions) to cryptographically sign DNS records and validate responses.
2. Use secure recursive resolvers (e.g., those supporting DNS-over-HTTPS or DNS-over-TLS) and keep resolver software patched.
3. Monitor DNS query patterns for anomalies, implement response rate limiting, and enforce strict registry/registrar security controls.

## Comparison Table

| Threat              | Attack Vector                  | Who is at Risk                          | Difficulty to Execute | Ease of Mitigation      |
|---------------------|--------------------------------|-----------------------------------------|-----------------------|-------------------------|
| DoS/DDoS            | Network/Application flooding   | Online services, enterprises, ISPs      | Medium–High (botnets) | Medium (with services)  |
| MITM                | Local network, rogue AP, proxy | Users on untrusted networks, orgs       | Medium                | Medium–High             |
| IP Spoofing         | Packet injection               | Systems relying on IP trust             | Low–Medium            | High (with filtering)   |
| DNS Poisoning       | Resolver cache / registry      | All internet users, specific domains    | Medium–High           | Medium (DNSSEC adoption)|

## Conclusion: Key Takeaways for Network Administrators

1. **Defense in Depth is Non-Negotiable** — No single control stops all threats. Combine network filtering, encryption, monitoring, and application hardening.
2. **Authentication and Integrity Over Implicit Trust** — Never rely solely on IP addresses or unauthenticated protocols. Prefer cryptographic verification (TLS, DNSSEC, IPsec).
3. **Visibility and Preparedness Matter** — Continuous monitoring, anomaly detection, and tested incident response plans significantly reduce the impact of successful attacks.

Network security is an ongoing process of risk assessment, control implementation, and adaptation to evolving threats. Security analysts play a critical role in identifying gaps and recommending prioritized mitigations.

## References

1. National Institute of Standards and Technology (NIST). *Guide to Intrusion Detection and Prevention Systems (IDPS)*. Special Publication 800-94.
2. Cybersecurity and Infrastructure Security Agency (CISA). *Understanding Denial-of-Service Attacks*. https://www.cisa.gov/
3. MITRE ATT&CK. Techniques related to Network Denial of Service, Man-in-the-Middle, and Spoofing. https://attack.mitre.org/
4. Krebs on Security / Wired reporting on major DDoS and DNS incidents (Dyn 2016, Spamhaus 2013, Sea Turtle).
5. RFC 2827 (BCP 38) – Network Ingress Filtering.
6. ICANN and DNSSEC deployment resources.

---

*This report was prepared as part of the Security Analyst track requirements. All analysis is based on publicly available information and established cybersecurity frameworks.*
