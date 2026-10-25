# Technical Report: Common Network Security Threats

## Executive Summary
Modern enterprise networks encounter persistent cyber risks that undermine data integrity, confidentiality, and operational uptime. This technical report details four fundamental network-level threat vectors: Denial of Service (DoS/DDoS), Man-in-the-Middle (MITM), IP Spoofing, and DNS Poisoning. Each section examines operational mechanics, real-world case studies, mitigation controls, and comparative security postures.

---

## 1. Denial of Service (DoS) & Distributed Denial of Service (DDoS)

### 1.1 Attack Mechanics
DoS and DDoS attacks aim to degrade or disrupt service availability for legitimate users by overwhelming network resources, link capacity, or system memory. 
* **Volumetric Attacks:** Saturate bandwidth using reflection/amplification techniques (e.g., NTP/DNS amplification).
* **Protocol Attacks:** Consume state tables of network infrastructure devices like firewalls and load balancers (e.g., SYN Floods).
* **Application Layer Attacks:** Target application resources (e.g., HTTP POST/GET floods) to exhaust CPU and database threads.

### 1.2 Real-World Case Study
In February 2018, GitHub suffered a record-setting DDoS attack reaching 1.35 Tbps of incoming traffic. The attack utilized Memcached reflection amplification, where attackers spoofed GitHub's IP address to exposed Memcached instances on UDP port 11211, generating an amplification factor of approximately 50,000x.

### 1.3 Technical Mitigation Strategies
1. **Anycast Routing & BGP Anycasting:** Distribute incoming attack volumes across geographically dispersed data centers.
2. **Rate Limiting & Web Application Firewalls (WAF):** Configure ingress filters to drop anomalous traffic spikes exceeding defined HTTP/TCP request thresholds.
3. **Upstream Scrubbing Services:** Route incoming network traffic through dedicated DDoS mitigation providers (e.g., Cloudflare, Akamai) during active attacks.

---

## 2. Man-in-the-Middle (MITM) Attacks

### 1.1 Attack Mechanics
An attacker secretly intercepts, alters, or relays communications between two parties who believe they are directly communicating with each other. Common LAN-level mechanisms include:
* **ARP Spoofing:** Broadcasting fraudulent ARP replies to bind the attacker's MAC address to a legitimate gateway IP address.
* **Rogue Access Points:** Deploying unauthorized wireless networks configured with identical SSIDs to intercept traffic.

### 1.2 Real-World Case Study
The Superfish visual search software pre-installed on Lenovo consumer laptops in 2014 installed a self-signed root CA certificate. This vulnerability allowed attackers on public Wi-Fi networks to generate fake SSL/TLS certificates on the fly, performing MITM inspection on encrypted HTTPS traffic without triggering browser warnings.
---

## 3. IP Address Spoofing

### 3.1 Attack Mechanics
IP spoofing involves generating IP packets with a forged source IP address to conceal the identity of the sender or to impersonate another computing system. It serves as a primary building block for session hijacking, blind spoofing, and DDoS amplification vectors.

### 3.2 Real-World Case Study
In the 2016 Dyn Cyberattack, IP spoofing combined with Mirai botnet traffic allowed attackers to disguise request originations while executing DNS flood attacks, rendering major web services inaccessible globally.

### 3.3 Technical Mitigation Strategies
1. **Ingress and Egress Filtering (BCP 38):** Network operators must drop outgoing packets with source IPs outside their assigned subnet allocation.
2. **Reverse Path Forwarding (uRPF):** Configure routers to verify if incoming packets on an interface match the routing table return path for the source address.
3. **Cryptographic Authentication:** Use IPsec protocol suites to validate source authenticity at the IP layer.

---

## 4. DNS Poisoning / Cache Spoofing

### 4.1 Attack Mechanics
DNS cache poisoning compromises the integrity of a DNS resolver's cache memory. By injecting forged DNS resource records (RR) into resolver caches, attackers redirect users from legitimate domain names to malicious IP addresses hosted on attacker infrastructure.

### 4.2 Real-World Case Study
In April 2018, attackers hijacked the DNS resolution of MyEtherWallet.com via BGP route hijacking targeting Amazon's Route 53 DNS servers. Users requesting the legitimate site were redirected to a phishing page in Russia, resulting in substantial cryptocurrency theft.

### 4.3 Technical Mitigation Strategies
1. **DNSSEC Deployment:** Implement Domain Name System Security Extensions to cryptographically sign DNS records using public key infrastructure.
2. **DNS Resolver Source Port Randomization:** Randomize transaction IDs and source UDP ports for outgoing DNS queries to increase entropy against race-condition spoofing.
3. **DNS-over-HTTPS (DoH) / DNS-over-TLS (DoT):** Encrypt DNS queries between client systems and resolvers to prevent on-path tampering.

---

## 5. Comparative Threat Matrix

| Attack Vector | Primary Target | Difficulty | Real-World Impact | Primary Defense |
| :--- | :--- | :--- | :--- | :--- |
| **DDoS** | Availability | Low - Medium | System Downtime, Financial Loss | Scrubbing Services, Rate Limiting |
| **MITM** | Confidentiality / Integrity | Medium | Session Hijacking, Credential Theft | HSTS, End-to-End Encryption |
| **IP Spoofing** | Authentication / Origin | Medium | Anonymization, Reflection Amplification | BCP 38, uRPF Filtering |
| **DNS Poisoning** | Integrity / Traffic Flow | High | Mass Phishing Redirection | DNSSEC Implementation |

---

## 6. References
1. NIST Special Publication 800-47 Rev. 1: *Managing the Security of Information Exchanges*.
2. IETF BCP 38: *Network Ingress Filtering: Defeating Denial of Service Attacks which employ IP Source Address Spoofing*.
3. CISA Technical Guide: *Understanding and Mitigating Layer 7 DDoS Attacks*.
4. OWASP Top 10: *Transport Layer Protection & Cryptographic Failures*.
### 1.3 Technical Mitigation Strategies
1. **Enforce TLS with HSTS:** Implement HTTP Strict Transport Security (HSTS) to prevent SSL stripping attacks.
2. **Dynamic ARP Inspection (DAI):** Enable DAI on managed network switches to validate ARP requests and responses against a trusted DHCP snooping binding database.
3. **Virtual Private Networks (VPNs):** Encrypt end-to-end traffic between end-user endpoints and corporate infrastructure over untrusted networks.
