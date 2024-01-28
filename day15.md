# CN ->
Different Types of Networks : 
- Networks can be divided on the basis of area of distribution. For example:
	● PAN (Personal Area Network): Its range limit is up to 10 meters. It is created forpersonal use. Generally, personal devices are connected to this network. For examplecomputers, telephones, fax, printers, etc.● LAN (Local Area Network): It is used for a small geographical location like office,hospital, school, etc.● HAN (House Area Network): It is actually a LAN that is used within a house and used toconnect homely devices like personal computers, phones, printers, etc.● CAN (Campus Area Network): It is a connection of devices within a campus area whichlinks to other departments of the organization within the same campus.● MAN (Metropolitan Area Network): It is used to connect the devices which span tolarge cities like metropolitan cities over a wide geographical area.● WAN (Wide Area Network): It is used over a wide geographical location that may rangeto connect cities and countries.● GAN (Global Area Network): It uses satellites to connect devices over the global area.
Resolve your confusion regarding IP addressing in the network layer:
When you want to send a packet from one LAN to another LAN over the internet, several processes come into play, especially involving private and public IP addresses. Let's break down the steps for clarity:
1. Private IP Addresses:
	- Devices within a LAN have private IP addresses assigned, typically by a DHCP server (often a feature of the local router). These addresses are unique within the LAN but are not globally unique. That means another LAN can have devices with the same private IP addresses.
2. Public IP Addresses:
	- For devices in a LAN to communicate over the internet, the LAN must have at least one globally unique IP address, known as a public IP address. This public IP is usually assigned to the WAN interface of the router by the ISP.
3. Sending a Packet from One LAN to Another:
   a. Packet Creation: The device in the source LAN creates a packet with its own private IP as the source IP and the private IP of the destination device (in the other LAN) as the destination IP.
   b. Routing Decision: The device realizes that the destination IP is not in its own network. It sends the packet to its default gateway (typically the local router) for further routing.
   c. Network Address Translation (NAT): Before the packet leaves the source LAN, the source router modifies it. Using NAT, the router changes the source IP from the private IP of the sending device to the public IP of the router. The original source IP (the private one) is stored by the router so it can manage the response. The destination IP (still the private IP of the destination device) remains unchanged for now.
   d. Routing Across the Internet: The packet is now sent over the internet using standard IP routing mechanisms. Internet routers don't care about the private IP addresses; they make routing decisions based on the public IP.
   e. Destination Router and NAT: The destination router, recognizing that the destination IP is a private IP of a device in its LAN and also recognizing the public IP (as it is its own), performs a NAT operation. It determines the actual private IP of the destination device and modifies the packet's destination IP accordingly. It then forwards the packet to the specific device in its LAN.
   f. Packet Reaches the Destination: The destination device in the second LAN receives the packet.
4. Returning Traffic: If the destination device responds, the process works in reverse. The destination LAN's router modifies the source IP of the returning packet from the private IP of the responding device to its own public IP, and the original LAN's router translates the destination IP back to the private IP of the original sender.
In essence, the NAT process allows multiple devices in a LAN to share a single public IP address for communicating over the internet. It also provides a level of security since unsolicited incoming traffic from the internet can't directly address devices with private IPs in a LAN unless port forwarding or another mechanism is set up.
How the MAC Address is assigned in the data link layer confusion:
1. The data link layer deals with MAC addresses.
2. Router's MAC Address: When a device within a local network wants to communicate with a device outside the local network (e.g., on the public internet), it sends the data to its default gateway, which is typically a router. The destination MAC address on the Ethernet frame will be the router's MAC address, because the router is the next hop in the path to the destination.
3. Device's MAC Address: When a device within the same local network wants to communicate with another device in the same network, it will use the destination device's MAC address. This is because they're on the same local network segment, so the data can be sent directly to the destination device without passing through a router.
So basically the MAC Address is assigned in a hop to hop manner.
Some important Protocols:
Network protocols are a set of rules outlining how connected devices communicate across a network to exchange information easily and safely. Protocols serve as a common language for devices to enable communication irrespective of differences in software, hardware, or internal processes.
Some protocols:
- DHCP (Dynamic Host Configuration Protocol):
	- Purpose: Automatically assigns IP addresses and other network configuration information to devices on a network.
	- How it works: A device (client) requests an IP address, and the DHCP server responds with an available address, along with other info like subnet mask, default gateway, and DNS servers.
- FTP (File Transfer Protocol):
	- Purpose: Used for transferring files between a client and a server over a network.
	- How it works: FTP operates on two channels - a command channel (for control information) and a data channel (for the actual file transfer). It can work in either active or passive mode, which determines how the data connection is established.
- ICMP (Internet Control Message Protocol):
	- Purpose: Primarily used for error reporting and diagnostics in the IP layer.
	- How it works: Examples include the "ping" tool (which uses ICMP Echo Request and Echo Reply messages) and error messages such as "destination unreachable" or "time exceeded."
- ARP (Address Resolution Protocol):
	- Purpose: Maps 32-bit IP addresses to MAC addresses within a local network.
	- How it works: When a device knows an IP address but needs the corresponding MAC address to send data on the local network, it broadcasts an ARP request. The device with the matching IP address responds with its MAC address.
- RIP (Routing Information Protocol):
	- Purpose: One of the oldest distance-vector routing protocols used to route traffic within a network.
	- How it works: Routers using RIP send their entire routing table to their direct neighbors every 30 seconds. RIP uses hop count as its metric, with a maximum hop count of 15 (which limits the size of RIP networks)
Subnet:
- A subnet is a network inside a network achieved by the process called subnetting which helps divide a network into subnets. It is used for getting a higher routing efficiency and enhances the security of the network. It reduces the time to extract the host address from the routing table.
Node and Link:
- A network is a connection setup of two or more computers directly connected by some physical mediums like optical fiber or coaxial cable. This physical medium of connection is known as a link, and the computers that it is connected to are known as nodes.
POP3 vs IMAP:
- POP3: Designed to download emails from the server to the client. Once downloaded, the emails are often deleted from the server. This means they reside on the client device.
- IMAP: Designed to let the client view emails as if they were stored locally, but they remain on the server. This allows multiple devices to access the same mailbox, seeing the same emails.
Network Security Questions
- What is Firewall:
	- A firewall is a network security device or software that monitors and filters incoming and outgoing network traffic based on an organization's previously established security policies. At its most basic level, a firewall is essentially a barrier to keep destructive forces away from your property. Its purpose is to establish a barrier between your internal network and incoming traffic from external sources (such as the internet) in order to block malicious traffic like viruses and hackers.
	- Types:
		- Packet-Filtering Firewalls: The most basic type. They inspect packets of data and allow or block them based on source and destination addresses, ports, and protocols. They do not check the packet content.
		- Stateful Inspection (or Dynamic Packet Filtering) Firewalls: More advanced than packet filtering. They monitor the state of active connections and make decisions based on context. For instance, if you initiate a connection to a web server, the responses from that server are allowed because they are part of an established connection.
		- Proxy Firewalls (Application-Level or Gateway Firewalls): Instead of allowing traffic to connect directly, traffic is directed through the firewall. It effectively hides the true network addresses and decides whether to allow the traffic to pass, based on content inspection.
		- Circuit-Level Gateways: Operate at the session layer of the OSI model. They monitor TCP handshakes and other protocol sessions to determine if the session is legitimate.
		- Next-Generation Firewalls (NGFWs): Advanced firewalls that incorporate the features of the previous types but also include deep packet inspection, intrusion prevention systems, and application filtering, often coupled with threat intelligence feeds.
		- Software Firewalls: Installed on individual devices (like PCs) and filter incoming and outgoing traffic for that specific device only.
		- Hardware Firewalls: Standalone devices placed between a network and its connection to the internet. They often come with additional features like VPN support, antivirus, and more.
- Some common types of attacks:
	- Phishing: Fraudulent attempts to obtain sensitive information through deceptive emails or messages.
	- Man-in-the-Middle (MitM) Attack: Intercepting and potentially altering communication between two parties.
	- Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS): malicious attempt to disrupt the normal functioning of a computer network, service, or website by overwhelming it with a flood of traffic, requests, or other malicious actions.
	- SQL Injection: Injecting malicious SQL code to manipulate or access a database.
	- Cross-Site Scripting (XSS): Injecting malicious scripts into web pages to run on another user's browser.
	- Cross-Site Request Forgery (CSRF): Making a user perform unwanted actions on a web application where they're authenticated.
	- Ransomware: Malware that encrypts user data and demands payment for decryption.
	- Zero-Day Attack: Exploiting software vulnerabilities before vendors can patch them.
	- Brute Force Attack: Trying many combinations to guess passwords or decryption keys.
	- Spoofing: Faking the origin of communication packets or messages.
	- Smurf Attack: Flooding a network using ICMP echo requests.
	- SYN Flood: Exploiting the TCP handshake process to overload systems.
	- DNS Spoofing/Cache Poisoning: Redirecting domain names to other IP addresses.
	- ARP Spoofing: Associating an attacker's MAC address with the IP of another node on a network.
	- Session Hijacking: Taking over a user's session to gain unauthorized access.
	- Malware: Broad category encompassing malicious software, including viruses, worms, trojans, etc.
	- Botnet: A group of compromised computers controlled without the owner's knowledge, often used for malicious purposes like sending spam or launching attacks.
	- IP Spoofing: Sending packets with a fake source IP address to disguise the sender's identity.
	- Compromised-Key Attack: Using a stolen cryptographic key to gain unauthorized access to secured communication.
	- Rootkit: Stealthy software that gains unauthorized access and control over a computer system.