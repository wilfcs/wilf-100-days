# CN->

- Caching and how is a website cached:
	- Caching is a technique used in computing to store and reuse previously fetched or computed data so that future requests for the same data can be served more quickly. By storing frequently-used data in a "cache" (a storage mechanism, either in memory or on disk), systems can reduce redundant operations, minimize data fetching times, and ultimately deliver content faster.
	- Browser Cache (Client-side cache):
		- When a user visits a website, the browser can store certain files locally, such as stylesheets, images, and JavaScript files.
		- The next time the user visits the same site, the browser can load the files from the local cache instead of fetching them from the server, leading to faster page loads.
	- Content Delivery Network (CDN) Cache:
		- CDNs are third-party networks of powerful servers that store cached copies of static website content.
		- When a user requests content that's available on a CDN, the nearest server (in terms of network latency) delivers the cached content, reducing the need to fetch it from the website's origin server.
		- CDNs help in reducing latency and speeding up content delivery for users spread across different geographical locations
- Difference Between REST and HTTP:
  
	- HTTP is a protocol used for transferring data over the web, while REST is an architectural style for designing networked applications, often using HTTP. While all RESTful APIs use HTTP, not all HTTP APIs are RESTful. In essence, REST provides guidelines on how to use HTTP effectively for APIs.
- What is Container in CN :
		1. Definition: Container Networking is an emerging application sandboxing mechanism used in home desktops and web-scale enterprise networking solutions similar in concept to a virtual machine.  A container is a lightweight, stand-alone, and executable software package that encompasses an application, its dependencies, system libraries, and settings required to run it, all bundled into one package.
		2. Isolation: Containers run in isolation from each other, even if they are on the same host. This ensures that the application's environment remains consistent across development, testing, and production.
		3. Efficiency: Containers are more lightweight than traditional virtual machines because they share the host system's OS kernel, rather than needing their own operating system. This means faster start-up times and better utilization of underlying resources.
		4. Popular Technology: Docker is one of the most popular containerization technologies. Kubernetes, on the other hand, is a popular system for orchestrating and managing containers.
- Difference between Containerization and Virtualization:
	- Containerization:
	  
		1. Level: Operates at the application layer.
		2. Architecture: Containers share the same OS kernel as the host. They package the application and its dependencies but use the underlying host OS.
		3. Efficiency: Lightweight and fast because they don't carry the overhead of entire OS instances.
		4. Examples: Docker, rkt, LXC.
	- Virtualization:
	  
		1. Level: Operates at the hardware level.
		2. Architecture: Uses hypervisors (either Type 1, bare-metal, or Type 2, hosted) to create and manage virtual machines (VMs). Each VM has its own full OS instance.
		3. Efficiency: Generally heavier than containers because each VM runs its own OS kernel and associated overhead.
		4. Examples: VMware, Microsoft Hyper-V, Oracle VirtualBox, KVM.
- DIfference between Latency and Throughput:
	- Latency: The amount of time it takes for a specific piece of data or a request to travel from one point to another in a system or network
	- Throughput: The rate at which data can be transmitted or processed over a certain period. It's about how much data can flow through a system or network over time.
- VPN:
	- A VPN (Virtual Private Network) is a service that allows you to create a secure and encrypted connection over the internet. This ensures that your online activities remain private and that your data remains safe, especially when using public Wi-Fi networks.
	- How It Works:
	  
		1. Encryption: When you connect to a VPN, your internet traffic is encrypted, meaning that outsiders cannot decipher the data being transmitted.
		2. Tunneling: VPNs create a virtual “tunnel” through which your encrypted data travels, shielding it from other users on the same network and potential eavesdroppers.
		3. IP Masking: VPNs mask your actual IP address. When you connect to a VPN server, your device takes on the IP address of that server. This not only hides your personal IP but also makes it appear as if you're browsing from the location of the VPN server.
	- Advantages: Enhanced Security, Online Privacy, Bypass Geo-restrictions, Avoid Censorship, Safe Online Transactions, Reduced Online Tracking, Protection on Public Wi-Fi.
- Router vs Gateway:
	- Router:
		- Directs data packets between networks.
		- Determines the best path for data based on its routing table.
		- Primarily operates at the Network layer (Layer 3) of the OSI model.
	- Gateway:
		- Serves as an access point from one network to another.
		- Can convert data between different network architectures or protocols.
		- Acts as a "gate" between two networks, especially between a local network and the internet.
- NIC and MAC Address:
	- When you connect a computer to your Ethernet LAN, do you know what you're plugging the Ethernet cable into? From the outside, it looks like you're plugging it into a metal case, but you're not. Inside the case is a Network Interface Card (NIC). A NIC is a special hardware card within any networked device (computer, printer, router, etc.) that handles all the technical aspects of sending and receiving data packets over a computer network. 
	- Like your mailing address at home, your computer's NIC has a unique address. This address must be unique, otherwise, network traffic cannot find its way to the right computer. The distinctive address that identifies a NIC is called the Media Access Control (MAC) address. A MAC address is formatted as a six-byte, hexadecimal number.
- Why do we need IP when we have MAC?:
	- Actually MAC address are fixed hence they are not as scalable compared to IP address. IP address have several other features like subnetting and supernetting which gives a logical understanding of the presence of a machine in a network. These facilities are not with the MAC address.
	- Also MAC address are not routable. The Internet Protocols will not treat them as an address of a source or destination. Hence IP address in many ways simplifies our task. 
	- The malleable IP address gives your network some flexible manageability. The never-changing MAC provides a specific, reliable address for a physical device. IP addresses route a packet across the whole global Internet, while MAC addresses help the packet make the small, local hop between hardware devices. Sophisticated networking is possible because each of your networked devices has both a MAC and an IP address.
- Multiplexing, Demultiplexing:
	- Multiplexing is a technique used to combine and send the multiple data streams over a single medium.
	- Multiplexing is achieved by using a device called Multiplexer (MUX) that combines n input lines to generate a single output line. Multiplexing follows many-to-one.
	- Demultiplexing is achieved by using a device called Demultiplexer (DEMUX) available at the receiving end. DEMUX separates a signal into its component signals (one input and n outputs).

    - Modem vs Router:
	- Modem:
		- Function: Converts analog signals from a telephone or cable line to digital data that a computer can understand, and vice versa.
		- Purpose: To allow digital devices to communicate over analog transmission lines, such as the ones used by ISPs.
		- Connection: Directly connects to the service provider's line (like a DSL or cable line).
	- Router:
		- Function: Directs data packets between devices on a local network and external networks, including the internet.
		- Purpose: To let multiple devices in a local network communicate with each other and with external networks.
		- Connection: Connects to a modem to get access to the internet and provides local IP addresses to devices in the local network.
- How does Bluetooth work:
	- Bluetooth devices communicate wirelessly through short-range devices called Piconets.
	- Bluetooth devices exist in small ad-hoc configurations with the ability to act either as master or slave the specification allows a mechanism for master and slave to switch their roles
	- Piconets can contain up to seven slaves centered around one master. The device initiating the Piconet becomes the master, responsible for transmission control.
	- Piconets can dynamically establish connections, with each device being able to communicate with several Piconets at once.
	- Devices can jump between Piconets, adjusting to the master device's timing and frequency in the new Piconet.
	- Adjacent Piconets can interconnect through certain devices, creating a scatternet.
- How does a Wi-Fi hotspot work:
	- A Wi-Fi hotspot works by taking an internet connection (often wired) and converting it into a wireless signal that other Wi-Fi-enabled devices can connect to. Essentially, it acts as a bridge between a wired network and wireless devices, allowing them to access the internet or other networked resources without a physical connection. Hotspots can be public (like in cafes or airports) or private (like using your smartphone to give internet access to your laptop).
- How does file transfer work:
	- FTP was created to facilitate the transfer of files between computers. While other protocols like HTTP can also transfer files, FTP provides clarity and specificity. FTP is especially useful in transferring data between different systems that may have different operating systems, directory structures, character sets, etc. FTP handles these differences efficiently.
	- When you initiate an FTP session, the client (typically your computer) starts a control TCP connection to the server. Through this connection, the client sends control information. In response, the server creates a data connection to the client for actual file transfer. While each data connection can transfer only one file, the control connection remains active for the entire FTP session. Unlike HTTP, which is stateless, FTP maintains a state for each user throughout its session.
	- FTP was specifically designed for file transfer, so its functionalities are optimized for this purpose. HTTP, on the other hand, is a general-purpose web protocol optimized for fetching web resources, not necessarily for large file transfers.
- How does ATM work:
	- Asynchronous Transfer Mode (ATM) is a network technology that transfers data in fixed-size packets called cells. It's designed to handle both traditional high-throughput data traffic and real-time, low-latency content.
	- Unlike other packet-switching technologies that use variable-sized packets, ATM uses small, fixed-size cells (53 bytes; 48 bytes of data and 5 bytes of header).
	- Before data exchange begins, a virtual circuit between source and destination is established.
	- Designed for high-speed data transfer, suitable for both voice and data.
- What happens when you enter google.com in the web browser? (Most Imp)
	- Browser Cache Check: When you enter "google.com" in the web browser, the browser first checks its cache to see if it has a recent version of the requested page. If a fresh version exists, it can be displayed immediately without fetching it from the server.
	- DNS Lookup:
		- If the IP address for "google.com" isn't present in the browser's cache, the browser asks the OS to resolve the domain name.
		- The OS checks its own cache to see if it has the IP for the requested domain.
		- If not found, the OS performs a DNS lookup using UDP to retrieve the IP address for "google.com". The DNS resolution process can involve multiple queries, starting from root servers, TLD servers, to the authoritative DNS servers.
	- TCP Connection:
		- Once the IP address is known, the browser establishes a TCP connection to the server. This is done using the three-way handshake you mentioned: SYN, SYN-ACK, ACK.
	- HTTP/HTTPS Request:
		- If the website is using HTTPS (which Google does), there's an additional step of TLS (Transport Layer Security) handshake to ensure a secure, encrypted connection.
		- The browser then sends an HTTP or HTTPS GET request to the server using the established connection.
	- Server Response:
		- Web servers handle the incoming HTTP/HTTPS request. The server processes the request and sends back an appropriate response. This response contains not only the webpage's content but also metadata in the form of HTTP headers.
	- Response Processing:
		- Upon receiving the response, the browser may choose to cache the content if the HTTP headers allow it. It takes cues from headers like Cache-Control to determine if and how long to cache the content.
		- The browser then processes the received data. If it's an HTML page, it parses the HTML, CSS, and JavaScript, potentially making additional requests for resources like images, scripts, or stylesheets.
	- Rendering: After processing, the browser renders the webpage on the screen, displaying content and executing any dynamic scripts.
	- Connection Termination/Reuse:
		- Depending on the Connection header's value in the HTTP response (like keep-alive), the browser might keep the TCP connection open for a short duration to reuse it for subsequent requests, or it might close it.
	- In modern web browsing, other elements can be taken into consideration, such as:
		- Prefetching: Some browsers try to optimize for user behavior by pre-fetching domain details they anticipate the user might visit next.
		- Content Delivery Networks (CDNs): Large sites, including Google, might serve content through CDNs, adding a layer of complexity to where and how content gets fetched.
		- Service Workers: Modern websites can register service workers in the browser that can intercept requests and serve content, providing capabilities for offline browsing and more efficient network usage

