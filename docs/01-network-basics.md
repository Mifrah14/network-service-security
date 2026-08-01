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

