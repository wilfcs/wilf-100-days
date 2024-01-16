# OSI Model
There are 7 layers in the OSI model.
1. Application Layer
2. Presentation Layer
3. Session Layer
4. Transport Layer
5. Network Layer
6. Data Link Layer
7. Physical Layer
   

Let us see all the layer's jobs as a story.
Once upon a time in the vast world of computer networks, a user decided to send a message to their friend.
Application Layer: The journey started at the Application layer. This is where our user's message was born. Think of it as a post office where you hand over your letter.
Presentation Layer: The letter then moved to the Presentation layer, the scribe of our story. This scribe translated the user's words into a language the machines understood, encrypted it to keep secrets hidden, and compressed it so it could travel faster.
Session Layer: With the message prepped and ready, the Session layer took over, the organizer of the story. This layer ensured a dedicated connection was established, so the message had a clear path to its destination. Think of it as setting up a private meeting room for the message's journey.
Transport Layer: Then came the Transport layer. Like a dependable courier, it took responsibility for making sure the message would reach its destination safely. It split the message into smaller packets, numbered them, and made arrangements for their safe travel. If any packet was lost or arrived damaged, the Transport layer promised to resend it.
Network Layer: Our message, now in packets, entered the Network layer's realm, the grand navigator. This layer decided the best route the packets should take. Like a postman deciding the best route to deliver all his letters, this layer assigned addresses, guiding our packets across the vast interconnected networks using routers.
Data Link Layer: As the packets traveled, they reached the Data Link layer, the gatekeeper of the local network. It gave each packet a physical MAC address, much like a stamp, ensuring the packet's safe local delivery. Think of it as a local guide making sure the letter reaches the correct house within a town.
Physical Layer: Finally, the packets reached the Physical layer, where they were converted into electrical signals, or light pulses, depending on the medium. These signals zoomed across cables, like cars on a highway, heading towards their destination.
Upon reaching the friend's computer, the journey reversed. The electrical signals were transformed back into data packets. The packets traveled up the layers, getting reassembled, decrypted, and finally presented as the original message at the Application layer of the friend's computer. 
It traveled something like this - Physical Layer of your friend -> Data Link -> Network -> Transport -> Session -> Presentation -> Application layer of your friend.
And thus, in the grand world of networks, the user's message completed its epic journey, all in the blink of an eye. 

OSI Model (Open System Interconnections) (more formal way)
It is a network architecture model based on the ISO standards (International Organization for Standardization). It is called the OSI model as it deals with connecting the systems that are open for communication with other systems. The OSI model has seven layers->

1. Application Layer:
- Application layer enables the user to access the network.
- It is used by network applications i.e. computer applications that use internet. eg. Google chrome, Zoom , Call of Duty, etc. These all are dependent on application layer protocols to function.
- There are multiple application layer protocols that enable various functions at this layer. eg. HTTP, HTTPS, FTP, FMTP, DHCP, TELNET, etc. All these protocols collectively form application layer.
- For example file transfer is done using FTP protocol. Web surfing using HTTP/S. Email using SMTP. Virtual terminals using TELNET.

2. Presentation Layer:
- Presentation layer is responsible for three tasks: translation, data compression, and data encryption/decryption.
	- Application layer sends data to presentation layer. The data is in human-readable format. The presentation layer converts this into machine-readable format. eg. It converts ASCII to EBCDIC. This is known as translation.
	- Presentation layer also reduces the number of bits that are used to represent original data. This is called data compression. Data compression can be lossy or lossless.
		- Lossy: To reduce the size of a file as much as possible.
		- Lossless: To reduce the size of a file without any loss of data or quality.
	- Presentation layer also encrypts and decrypts the data that is sent or received. It uses SSL (Secure Socket Layer) to do so.
- At the sender side, this layer translates the data format used by the application layer to the common format and at the receiver side, this layer translates the common format into a format used by the application layer.

3. Session Layer:
- The session layer helps in setting up and managing connections, enabling the sending and receiving of data followed by the termination of sessions. 
- The session layer has helpers called APIs (Application Programing Interfaces). APIs help in the communication process. 
- Before a connection or session is established with a server, the server performs two functions called authentication and authorization.
	- Authentication: Process of verifying who you are? Uses username and password.
	- Authorization: Process used by server to identify if you have permission to access a file. 
- Both the functions authentication and authorization are performed by session layer. 
- Session layer also keeps a track of the file sent and received.
- So basically session layer helps in session management, authentication and authorization. 
- Your web browser performs all the functions of application layer, presentation layer and session layer. 

4. Transport Layer:
- It delivers the message through the network and provides us with reliability of transportation using segmentation, flow control and error control.
	-  Segmentation: Data received from the session layer is divided into small units called segments. Each segment contains a source and destination port number, a sequence number, and a checksum. Port number helps to direct each segment to the correct application. Sequence number helps to assemble the segments in the correct order to deliver the correct message in the correct order later. 
	- Flow control: In flow control, transport layer controls the flow of the data being transmitted. Suppose a server can transmit data at 100mbps and a mobile can receive data at 10mbps, so mobile phone with the help of transport layer can tell the server to slow down data transmission rate to 10mbps so that no data is lost.
	- Error control: If some data does not arrive the destination, the transport layer uses an automatic repeat request scheme to retransmit the lost or corrupted data. A group of bits called checksum is added to each data segment by the transport layer to find out the received corrupted segment. 
- Protocols of transport layer are TCP(Transmission Control Protocol) and UDP(User Datagram Protocol).
- Transport layer provides with two additional types of services - connection-oriented transmission, and connectionless transmission.
	- Connection-oriented transmission: In this transmission, the receiver sends the acknowledgment to the sender after the packet has been received. This is done by TCP. TCP provides feedback. Therefore lost data can be retransmitted in TCP. It is full duplex. Used in WWW, Email, FTP, etc. where full data delivery is important. 
	- Connectionless transmission: In this transmission, the receiver does not send the acknowledgment to the sender.This is done by UDP. UDP is faster than TCP because it does not provide us with feedback on whether the data was really delivered. Used in online streaming, games, TFTP, DNS, etc. where speed and real-time data sync are more important.
- TCP (Transmission Control Protocol):
	- TCP is a connection-oriented protocol that guarantees the delivery of data packets in the order they were sent. It achieves this through sequence numbers, acknowledgments, retransmission of lost packets, and flow control. It's widely used for applications where data integrity and order are crucial, like file transfers and web browsing.
- UDP (User Datagram Protocol):
	- UDP is a connectionless protocol. It sends datagrams without establishing a formal connection and without guarantees regarding order or delivery. It’s faster and has less overhead than TCP because it lacks many of TCP’s reliability features. UDP is suited for applications where speed or low latency is a priority, like streaming audio/video or online gaming.
- Congestion Control:
	- Congestion control mechanisms manage the sending rate of data packets in networks to prevent network congestion and collapse. When too much data is sent too quickly, network resources (like bandwidth and buffer space in routers) can be overwhelmed, leading to dropped packets and reduced overall network throughput. TCP, for example, uses congestion control algorithms like Slow Start, Congestion Avoidance, and Fast Recovery to modulate its sending rate based on perceived network conditions.
- Checksum:
	- The checksum is a value calculated from a data set and is used for error detection. It's often a simple mathematical calculation where you sum up byte or word values in the data and sometimes apply modulo arithmetic. When data is transmitted, the checksum value is sent along with it. The receiving end recalculates the checksum from the received data and compares it to the received checksum. If they match, it's likely (but not guaranteed) that the data was received correctly.
- Retransmission Timers:
	- Retransmission timers are used in reliable transport protocols like TCP. When a segment is sent, a timer is started. If the acknowledgment (ACK) for that segment isn’t received before the timer expires, the segment is assumed to be lost in transit and is retransmitted. The duration of this timer might be adaptive, adjusting based on network conditions.
- 3-way Handshake:
	- This is a process used by TCP to establish a connection between a client and a server. The steps are:
		1. SYN: The client sends a segment with the SYN (synchronize) flag set, indicating an initial sequence number.
		2. SYN-ACK: The server responds with a segment with both the SYN and ACK flags set, acknowledging the client's SYN and providing its own initial sequence number.
		3. ACK: The client sends an acknowledgment segment back to the server to tell the server that it received the SYN and ACK from the server. After these steps, the TCP connection is established, and data transfer can begin.
	- The three-way handshake in TCP occurs only at the beginning of a TCP session to establish the connection between the client and the server. It doesn't happen every time a data packet is sent.
	- After the connection is established through the three-way handshake, data segments can be sent bidirectionally between the client and server. Each segment of data that's sent will typically be acknowledged by the receiving party, but this acknowledgment is not another three-way handshake; it's simply an ACK segment.
	- A separate set of steps (often referred to as the "four-way handshake") is used to gracefully close a TCP connection. It involves FIN (finish) and ACK flags.
- Data Transfer and Acknowledgments:
	- After the connection is established, the client and server can send data segments to each other.
	- When a data segment is received, the recipient (either client or server) sends an acknowledgment (ACK) for that specific segment. This ACK is just a simple acknowledgment and is not part of the three-way handshake.
	- The acknowledgment is used to inform the sender that the segment was received successfully. If the sender doesn't get this ACK within a certain time frame (due to the ACK being lost, the data segment being lost, or other network issues), it will retransmit the segment.
- Two army problem / Acknowledgment Problems: 
	- It happens at the time of data transfer. If the data is received by the receiver, but the acknowledgment (ACK) sent by the receiver doesn't reach the sender, the sender assumes the data was lost and retransmits it.
	- If the receiver gets the same data again, it can detect that this data is a duplicate, thanks to the sequence numbers used by reliable protocols like TCP which helps in avoiding this problem. If the receiver gets this retransmitted segment but has already received the original segment (i.e., it's a duplicate), the receiver can recognize this because the sequence number of the retransmitted segment won't match the expected next sequence number.

5. Network Layer: 
- Transport layer passes the data to the network layer. This is the layer where the routers recite. This is the layer where segments are converted into packets. 
- Logical addressing is done here.
- These are the functions of network layer:
	- Logical Addressing: IP Addressing done in network layer is called logical addressing. Network layer assigns sender's and receiver's IP address and mask to the data segment it has received to form data packet. 
	- Packetizing: The network layer receives the data from the upper layer and converts the data into packets. This process is known as packetizing.
	- Routing: The network layer determines the best route from source to destination. This function is known as routing. Routing is done by looking at the IP and subnet mask.
	- Path Determination: Choosing the best path from source to destination is called path determination. There are several protocols used to perform this task like OSPF, BGP, IS-IS, etc.
	- Internetworking: The network layer provides the logical connection between the different types of networks for forming a bigger network.

6. Data Link Layer: 
- Data Link layer receive data packets from the network layer. 
- Physical addressing is done here.
- As we saw in Network layer that logical addressing was done there, it means that the sender's and receiver's IP Address was added to each segment to form data packets. 
- In Data Link layer physical addressing is done, i.e. MAC Address of sender and receiver is added to the data packet to form a frame.
- 
- MAC Address is a 12 digit alphanumeric number embedded in network interface card of your computer by the computer manufacturer. Data link layer is embedded as software in this network interface card of your computer. It helps in uniquely identifying the devices on a local network. 
- Functions of data link layer:
	- Frame synchronization: Data-link layer converts the data into frames, and it ensures that the destination must recognize the starting and ending of each frame.
	- Flow control: Data-link layer controls the data flow within the network
	- Error control: It detects and corrects the error occurred during the transmission from source to destination.
	- Addressing: Data-link layers attach the physical address with the data frames so that the individual machines can be easily identified.
	- Link management: Data-link layer manages the initiation, maintenance and termination of the link between the source and destination for the effective exchange of data.
	- Media access control: Media here refers to the medium that connects two devices. A media can either be a wire or it can be wireless. The data link layer prevents the collision of the data traveling in the same media by keeping an eye on them. There might be two or more devices connected to the same media. If they send the data at the same time collision can happen but the data link layer prevents this collision. 

7. Physical Layer:
- The physical layer is responsible for the actual transmission and reception of raw data bits over a physical medium, such as copper cables, fiber optics, or wireless communication channels.
- The physical layer converts the binary data sent by the data link layer into electrical signals and transmits it over local media. 
- At the receiver end the physical layer receives signals, converts them to bits, and passes them to the data link layer. Finally data is further moved to the application layer. 


There is another model called TCP/IP model where there are 5 layers. The presentation and session layer is merged into the application layer. So the 5 layers of TCP/IP is Application, Transport, Network, Data Link, Physical.


This is where the OSI layers reside->
1. Layer 7: Application Layer
	- Resides in: End-user applications and some network services.
	- Examples: Web browsers (like Chrome, Firefox), email clients (like Outlook), FTP clients, SSH clients, and network services like DNS and DHCP.
2. Layer 6: Presentation Layer
	- Resides in: Libraries or components within the operating system, some specialized software.
	- Examples: SSL/TLS (for encryption), data format transformations (like JPEG, MPEG for multimedia), character encoding conversions (like ASCII, EBCDIC).
3. Layer 5: Session Layer
	- Resides in: OS libraries or components, some application-level services.
	- Examples: NetBIOS, RPC (Remote Procedure Call), PPTP (Point-to-Point Tunneling Protocol).
4. Layer 4: Transport Layer
	- Resides in: Operating system's networking stack.
	- Examples: Transmission Control Protocol (TCP), User Datagram Protocol (UDP), Stream Control Transmission Protocol (SCTP).
5. Layer 3: Network Layer
	- Resides in: Routers, and within the OS's networking subsystem for end-hosts.
	- Examples: Internet Protocol (IP), Internet Control Message Protocol (ICMP), and routing protocols like OSPF and BGP.
6. Layer 2: Data Link Layer
	- Resides in: Network interface cards (NICs), switches, and bridge devices. Some parts of this layer, especially the upper sub-layer (LLC), might be handled by device drivers or the OS.
	- Examples: Ethernet, Wi-Fi, PPP, MAC addresses, VLANs.
7. Layer 1: Physical Layer
	- Resides in: Physical networking hardware and transmission media.
	- Examples: Ethernet cables (like Cat 5e, Cat 6), optical fiber, hubs, and the actual electrical/optical signaling mechanisms.