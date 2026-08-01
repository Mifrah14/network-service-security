# Network Fundamentals & Port Security

## What is network traffic, and why does it matter?

Network traffic refers to the flow of data between devices over a network. Each unit of traffic (a packet) is made up of two parts: a **header** and a **payload**.

The **header** contains metadata about the packet, including the source and destination IP addresses, the port number (which identifies which application 
or service the data is meant for, e.g., port 80 for HTTP, port 443 for HTTPS), and the protocol being used (TCP, UDP, etc.).

The **payload** is the actual content being transmitted — this could be a webpage, an email, a file, a message, or login credentials entered into a form.

Network traffic can contain sensitive information such as:
- Login credentials and session data
- Personal messages or files
- Browsing activity and behavioral patterns
- Device and network identifiers (IP/MAC addresses)

Protecting this traffic matters because exposure of either part can cause harm. If the payload is unencrypted (e.g., plain HTTP), an attacker intercepting the 
traffic can read sensitive data directly, leading to credential theft or privacy loss. Even when the payload is encrypted, the header alone can still leak 
valuable information — such as who is communicating with whom, what services are being used, and when — which can be used for surveillance or to plan 
further attacks.

## Ports: the gateway hackers exploit

A port is a logical, numbered channel on a device that allows multiple network conversations to happen at the same time. Every device has 65,536 
possible ports (0–65535), grouped into ranges: well-known ports (0–1023, e.g., HTTP=80, HTTPS=443, SSH=22), registered ports (1024–49151), and 
dynamic/private ports (49152–65535).

We need ports because a single device can run many applications or services at once. Ports ensure that incoming traffic is routed to the correct 
application — for example, web traffic goes to the browser, while email traffic goes to the mail client, even though both arrive at the same IP address.

Ports become a security risk because they can reveal what services are running on a device, and whether those services are vulnerable. Attackers 
commonly use **port scanning** tools (like Nmap) to identify which ports are open on a target device. An open port tells them a service is actively 
listening there — and if that service is outdated or misconfigured, it becomes a direct point of entry. For example, an open FTP port (21) running 
an old, unpatched FTP server could be exploited using a known vulnerability for that version.

In short, every open port is a potential entry point, and unnecessary open ports increase a device's attack surface — giving attackers more opportunities 
to find something exploitable.

## Hardening strategy: should unused ports stay open?

By default, ports that don't have an intended use should **not** be left open. This follows the security principle of "default deny" (or least 
privilege): every open port represents a potential entry point for an attacker, so a port that serves no functional purpose is pure risk with no 
benefit. The standard practice is to close everything by default and only open what is explicitly needed.

Several strategies are used to enforce this in practice:

**Firewalls** act as the first line of defense, blocking all traffic by default and only allowing specific, defined ports or services through. This 
is the practical enforcement of "default deny" — rather than manually locking down every port, a firewall rule can simply state which ports are 
allowed and block everything else.

**Disabling unused services** goes a step further than firewalls. A firewall blocks *access* to a port, but the service behind it may still be 
running. If the firewall is ever misconfigured or bypassed, that live service becomes reachable again. The more thorough fix is to disable or 
uninstall services that are not needed, so there is genuinely nothing listening on that port at all.

**Changing default ports** (e.g., running SSH on port 2222 instead of 22) reduces exposure to automated bots that scan for common ports. However, 
this is "security through obscurity" — a targeted attacker can still scan all 65,536 ports, so this should only be used alongside real access control, 
never as a replacement for it.

**Port knocking** hides a service completely by keeping its port closed until a specific, pre-arranged sequence of connection attempts to other 
ports is received. Only after this "secret knock" does the firewall temporarily open the real port for that client. This defeats basic port 
scanning entirely, though it adds complexity and can be vulnerable if the knock sequence is intercepted on an unencrypted network.

**Network segmentation** limits how far an attacker can move even if they do get in. By splitting a network into separate zones (e.g., servers, 
employee devices, guest WiFi) with firewalls between them, an open port on a server is only reachable from the specific subnet allowed to access it — 
not from the entire network. This is also why isolated/segmented lab environments (like the one used in this project's DNS spoofing exercise) 
are used to safely contain attacks during testing.

## Deprecated protocols still found in the wild


Many older protocols were designed decades ago, before security was a major concern — back then, the priority was just "make it work," not "make it safe." The biggest common flaw across most of them: they send data in plain text (unencrypted), meaning anyone intercepting the traffic can read everything — including passwords.

**Telnet (port 23)** is a protocol used for remote command-line access to devices. It sends all data, including usernames and passwords, in plain 
text. Anyone intercepting the traffic (e.g., via a man-in-the-middle attack) can read credentials directly. It has been replaced by **SSH (port 22)**, 
which encrypts the entire session.

**FTP (port 21)** is used for transferring files between a client and a server. Like Telnet, it transmits login credentials and file contents in 
plain text, making it vulnerable to interception. It has largely been replaced by **SFTP** or **FTPS**, which add encryption on top of file 
transfer.

**SMBv1 (port 445)** is an older version of the Server Message Block protocol, used for file and printer sharing on Windows networks. It contains 
several serious vulnerabilities, most notably the one exploited by the **WannaCry ransomware** in 2017, which used the EternalBlue exploit to 
spread rapidly across networks without any user interaction. SMBv1 has since been disabled by default on modern systems in favor of SMBv2/SMBv3.
