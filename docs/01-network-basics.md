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
