## DHCP and DNS: functionality, attacks, and mitigations

### DHCP (Dynamic Host Configuration Protocol)

DHCP automatically assigns IP addresses and other network configuration (such as the default gateway and DNS server) to devices when they join a 
network. Without it, each device would need to be manually configured with an IP address. When a device connects, it broadcasts a request for an IP, 
and a DHCP server responds with an available address along with a lease time for how long it can be used.

**DHCP Spoofing (Rogue DHCP Server):** Since DHCP requests are broadcast and answered on a first-come-first-served basis, an attacker can set up their 
own fake DHCP server on the network. If it responds faster than the legitimate server, victim devices accept the attacker's configuration 
instead — including a malicious gateway or DNS server — allowing the attacker to intercept or redirect the victim's traffic.

**DHCP Starvation:** An attacker floods the network with DHCP requests using randomly generated, spoofed MAC addresses. Since the DHCP server 
distinguishes devices only by MAC address, it cannot tell these apart from real devices and assigns an IP to each one. Once the server's limited pool 
of addresses is exhausted, legitimate devices are unable to obtain an IP and are denied network access. This is often used as a setup step before 
deploying a rogue DHCP server.

**Mitigations:**
- **DHCP Snooping** — a switch-level feature that only allows DHCP responses from trusted, pre-configured ports, blocking rogue servers from 
responding at all.
- **Port security** — limiting the number of MAC addresses allowed on a single switch port, preventing starvation attacks that rely on spoofed MACs.

### DNS (Domain Name System)

DNS translates human-readable domain names (e.g., `mycourses.rit.edu`) into the IP addresses computers use to route traffic. A device queries a DNS 
resolver, which looks up the domain (via cache or authoritative servers) and returns the corresponding IP address.

**DNS Spoofing / Cache Poisoning:** An attacker sends a forged DNS response, or corrupts a DNS server's cache, mapping a legitimate domain to a malicious 
IP address. The victim believes they are visiting the real site, but are actually redirected to an attacker-controlled server — often a cloned login page 
used to steal credentials.

**DNS Tunneling:** Attackers encode stolen data or malware command-and-control instructions inside DNS queries, since DNS traffic is typically allowed 
through firewalls without inspection. Real data is broken into small chunks, encoded into a domain-safe format, and sent out disguised as normal 
DNS lookups, allowing attackers to exfiltrate data or communicate with malware while evading detection.

**Mitigations:**
- **DNSSEC** — cryptographically signs DNS responses so devices can verify they came from a legitimate authoritative server, preventing spoofed 
responses from being trusted.
- **Encrypted DNS (DNS over HTTPS/TLS)** — prevents attackers on the same network from reading or tampering with DNS queries, directly relevant to 
the ARP-poisoning scenario used in this project's lab.
- **Monitoring DNS traffic patterns** — abnormal query sizes or unusually high volumes can indicate DNS tunneling activity.


