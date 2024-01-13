# CN
So first of all there is ISP which gives you a modem/router. Your router has a global IP address. All devices connected to this router will have the same IP address all around the world. Inside the modem let's say there are three devices D1 D2 and D3 connected. The modem will give unique IP addresses to all the devices connected to the modem. These IP addresses are known as local IP addresses. DHCP (Dynamic Host Configuration Protocol) is responsible for providing these local devices with their own IP. Now suppose D2 sent a request to google.com,  and google returns a response to the global IP address of the router. How does the router know that the response should be sent to D2? NAT (Network address translation) is responsible for it.
Now device D2 has the response from google, but how does it determine which application needs the response? How does it know that firefox asked for the response? Well, that is done using a port number. The Port numbers have 16 bits. The total port numbers possible are 2^16. Port 0-1023 are reserved.
Suppose you're chatting with your friend. Both of you have an IP address and a port number. IP address determines to which location I should send the data and the port number determines the application that requested the data. 

Network Topology : Network topology specifies the layout of a computer network. It shows how devices and cables are connected to each other.
Also if you ever decide to make a youtube video on this then give credit to love babbar and say that in the end I included the questions in his roadmap. 
Types of Network Topology:
- Star:
-  Star topology is a network topology in which all the nodes are connected to a single device known as a central device.
-  Star topology requires more cable compared to other topologies.
-  Therefore, it is more robust as a failure in one cable will only disconnect a specific computer connected to this cable.
-  If the central device is damaged, then the whole network fails.
-  Star topology is very easy to install, manage and troubleshoot. It is commonly used in office and home networks.
  
- Ring: 
	- Ring topology is a network topology in which nodes are exactly connected to two or more nodes and thus, forming a single continuous path for the transmission.
	- It does not need any central server to control the connectivity among the nodes.
	- If the single node is damaged, then the whole network fails.
	- Ring topology is very rarely used as it is expensive, difficult to install and manage.
	- Examples of Ring topology are SONET network, SDH network, etc.
	  
- Bus:
	- Bus topology is a network topology in which all the nodes are connected to a single cable known as a central cable or bus.
	- It acts as a shared communication medium, i.e., if any device wants to send the data to other devices, then it will send the data over the bus which in turn sends the data to all the attached devices.
	- Bus topology is useful for a small number of devices.
	- As if the bus is damaged then the whole network fails.
	  
- Mesh: 
	- Mesh topology is a network topology in which all the nodes are individually connected to other nodes.
	- It does not need any central switch or hub to control the connectivity among the nodes.
	- Mesh topology is categorized into two parts: Fully connected mesh topology: In this topology, all the nodes are connected to each other. Partially connected mesh topology: In this topology, all the nodes are not connected to each other.
	- It is robust as a failure in one cable will only disconnect the specified computer connected to this cable.
	- Mesh topology is rarely used as installation and configuration is difficult when connectivity gets more.
	- Cabling cost is high as it requires bulk wiring
	  
	  
- Tree: 
	- Tree topology is a combination of star and bus topology. It is also known as the expanded star topology.
	- In tree topology, all the star networks are connected to a single bus.
	- Ethernet protocol is used in this topology.
	- In this, the whole network is divided into segments known as star networks which can be easily maintained. If one segment is damaged, there is no effect on other segments.
	- Tree topology depends on the "main bus," and if it breaks, then the whole network gets damaged
	  
- Hybrid: 
	- A hybrid topology is a combination of different topologies to form a resulting topology.
	- If star topology is connected with another star topology, then it remains a star topology. If star topology is connected with different topology, then it becomes a Hybrid topology.


MAC address -- identifies a device to other devices on the same local network as every device has a physical mac address.
Please note that A port number is a way to identify a specific process to which an internet or other network message is to be forwarded when it arrives at a server and the mac address is physically attached to hardware devices. A computer does not have just one mac address, every hardware device attached to it has a unique mac address.

Just a quick definition - A modem is a device that converts data from a digital format to a format suitable for analog transmission mediums such as telephone or radio and vice versa. A router routes the data packet based on their IP addresses.

Now let's talk about the structure of the network i.e. The OSI model (the most important).
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