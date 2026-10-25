# Lab Report 01: Nmap Network Reconnaissance & Port Scanning

Student Name: Saeed Ahmed  
Domain: Security Analyst  
Environment: Kali Linux 2024.x (VirtualBox VM) / Target: 127.0.0.1  

---

## What is Nmap & Why Network Scanning Matters
Nmap (Network Mapper) is an open-source command-line tool used by penetration testers and network admins to discover active hosts on a network, detect open ports, identify running service versions, and determine the operating system of a host. 

Network scanning is the foundational phase of security audits (Reconnaissance). Before defending or testing a system, we need to map the exposed attack surface. Knowing which ports are open helps us identify misconfigurations, outdated services, or unauthorized open doors.

---

## Tool Installation
I performed this lab on Kali Linux where Nmap comes pre-installed. For reference, installation on Debian/Ubuntu systems is done using:

```bash
sudo apt update && sudo apt install nmap -y
