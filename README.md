<p align="center">
<img src="images/WiresharkPic.jpg" width="450">
</p>

<h2>Analyzing DHCP Traffic in Wireshark</h2>

<p>
In this section of the lab, we will analyze <b>DHCP (Dynamic Host Configuration Protocol)</b> traffic using Wireshark.
</p>

<p>
DHCP is a network protocol responsible for automatically assigning IP addresses to devices when they connect to a network. Without DHCP, network administrators would need to manually configure IP addresses on every device, which would be extremely inefficient in most environments.
</p>

<p>
When a device joins a network, DHCP performs a process known as the <b>DORA process</b>:
</p>

<ul>
<li><b>Discover</b> – The device broadcasts a request asking for an available IP address.</li>
<li><b>Offer</b> – The DHCP server responds with an available IP address.</li>
<li><b>Request</b> – The device requests the offered IP address.</li>
<li><b>Acknowledge</b> – The DHCP server confirms the assignment.</li>
</ul>

<p>
This process happens very quickly and usually occurs automatically when a device connects to a network.
</p>

<p>
In this lab, we will manually trigger DHCP activity so we can capture and analyze the packets in Wireshark.
</p>

---

<h3>Applying a DHCP Filter in Wireshark</h3>

<p>
To focus only on DHCP-related traffic, we apply a display filter in Wireshark.
</p>

<p>
<img src="images/01-DHCP.png" alt="Wireshark DHCP filter applied">
</p>

<p>
The filter used is:
</p>

<p>
<b>dhcp</b>
</p>

<p>
This filter instructs Wireshark to display only DHCP packets, hiding other types of network traffic so the analysis is easier to follow.
</p>

---

<h3>Generating DHCP Traffic</h3>

<p>
To generate DHCP activity, we will release and renew the IP address of the Windows virtual machine.
</p>

<p>
This forces the system to request a new IP address from the DHCP server.
</p>

<p>
The following commands are used:
</p>

<p>
<img src="images/02-DHCP.png" alt="DHCP script commands">
</p>

<p>
The commands inside the script are:
</p>

<ul>
<li><b>ipconfig /release</b> – Releases the current IP address.</li>
<li><b>ipconfig /renew</b> – Requests a new IP address from the DHCP server.</li>
</ul>

---

<h3>Executing the DHCP Script</h3>

<p>
After creating the script, it can be executed from PowerShell.
</p>

<p>
<img src="images/03-DHCP.png" alt="Running DHCP script in PowerShell">
</p>

<p>
When the script runs, the system first releases its current IP address and then immediately requests a new one from the DHCP server.
</p>

<p>
This action generates DHCP traffic that Wireshark can capture.
</p>

---

<h3>DHCP Lease Process</h3>

<p>
After running the commands, the system receives a new network configuration.
</p>

<p>
<img src="images/04-DHCP.png" alt="New IP address obtained">
</p>

<p>
The system is assigned a new IPv4 address along with additional network configuration information such as:
</p>

<ul>
<li>Subnet Mask</li>
<li>Default Gateway</li>
<li>DNS Server</li>
</ul>

<p>
These values allow the system to properly communicate with other devices and access the internet.
</p>

---

<h3>Captured DHCP Traffic</h3>

<p>
Now we can return to Wireshark to examine the packets that were generated during the DHCP process.
</p>

<p>
<img src="images/05-DHCP.png" alt="Captured DHCP packets">
</p>

<p>
Several DHCP packets appear in the capture results. These packets represent the communication between the client machine and the DHCP server.
</p>

---

<h3>Understanding the DHCP Packet Sequence</h3>

<p>
Looking closely at the captured packets, we can see the steps of the DHCP process.
</p>

<p>
<img src="images/06-DHCP.png" alt="DHCP packet breakdown">
</p>

<p>
The packets captured include:
</p>

<ul>
<li><b>DHCP Release</b> – The client releases its previous IP address.</li>
<li><b>DHCP Discover</b> – The client broadcasts a request for a new IP address.</li>
<li><b>DHCP Offer</b> – The DHCP server offers an available IP address.</li>
<li><b>DHCP Request</b> – The client requests the offered IP address.</li>
<li><b>DHCP ACK</b> – The server confirms the assignment.</li>
</ul>

<p>
These packets represent the complete DHCP handshake that allows devices to automatically join a network and obtain the configuration necessary for communication.
</p>

<p>
By analyzing these packets in Wireshark, network administrators and security analysts can observe how devices obtain network configurations and troubleshoot issues related to IP addressing and connectivity.
</p>

<h3>Analyzing DNS Traffic in Wireshark</h3>

<p>
After examining DHCP traffic, we can now observe how <b>DNS (Domain Name System)</b> traffic appears in Wireshark.
</p>

<p>
DNS is responsible for translating human-readable domain names into IP addresses. When users type a website such as <b>google.com</b> into a browser, the computer does not actually understand the domain name. Instead, it sends a DNS request asking a DNS server for the IP address associated with that domain.
</p>

<p>
Once the DNS server responds with the IP address, the system can then establish a connection to the correct server hosting that website.
</p>

<p>
This process happens constantly in the background whenever users browse the internet.
</p>

---

<h3>Filtering DNS Traffic</h3>

<p>
To analyze only DNS traffic in Wireshark, we apply the following display filter:
</p>

<p>
<b>dns</b>
</p>

<p>
This filter allows Wireshark to display only DNS packets while hiding other network traffic captured on the interface.
</p>

<p>
<img src="images/01-DNS.png" alt="DNS traffic filter applied in Wireshark">
</p>

<p>
After applying the filter, we can observe DNS queries and responses occurring on the network.
</p>

---

<h3>Generating DNS Traffic with nslookup</h3>

<p>
To generate DNS traffic manually, we can use the <b>nslookup</b> command in PowerShell. This command queries a DNS server and asks it to resolve a domain name into an IP address.
</p>

<p>
The following command was executed:
</p>

<p>
<b>nslookup google.com</b>
</p>

<p>
<img src="images/01-DNS.png" alt="nslookup google.com command">
</p>

<p>
When this command is run, the system sends a DNS query to the configured DNS server. The DNS server then responds with the IP addresses associated with the requested domain.
</p>

<p>
In Wireshark, this activity appears as a DNS query packet followed by a DNS response packet.
</p>

---

<h3>Querying Another Domain</h3>

<p>
To generate additional DNS traffic, another domain was queried using the following command:
</p>

<p>
<b>nslookup www.nba2k.com</b>
</p>

<p>
<img src="images/02-DNS.png" alt="nslookup nba2k.com DNS query">
</p>

<p>
This command sends another DNS query to the DNS server requesting the IP address associated with <b>www.nba2k.com</b>.
</p>

<p>
The DNS server responds with the IP address of the web server hosting the website.
</p>

---

<h3>Confirming the DNS Resolution</h3>

<p>
After the DNS server returns the IP address, we can verify that the resolution worked by navigating to the IP address in a web browser.
</p>

<p>
<img src="images/03-DNS.png" alt="website loaded using resolved IP address">
</p>

<p>
The browser successfully loads the website using the resolved IP address, confirming that the DNS query and response process completed successfully.
</p>

<p>
In real-world environments, DNS traffic occurs constantly as users access websites and applications communicate with remote services.
</p>

<p>
Network administrators and security analysts often analyze DNS traffic to troubleshoot connectivity issues, investigate suspicious domains, and identify potential malware communications.
</p>
