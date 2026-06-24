<p align="center">
<img width="600" height="300"  alt="download (1)" src="https://github.com/user-attachments/assets/17a37c1d-3eca-4976-a339-0e2fd80fe10e" />
</p>

<h1>Datacenter network</h1>
Zabbix is an open-source enterprise monitoring platform used by organizations to track the health, performance, and availability of their IT infrastructure. It provides real-time visibility across servers, virtual machines, networks, applications, and cloud environments — all from a single centralized dashboard. This demonstration will showcase a small monitoring environment using Zabbix to installing and configuring monitoring agents, setting up triggers, creating dashboards, receiving alerts, and responding to simulated incidents. <br />


<h2>Environments and Technologies Used</h2>

- Cisco Packet Tracer

<h2>Operating Systems Used </h2>

- Windows 11 x64


<h2>Installing and Configuring Zabbix Server and Agents Steps</h2>
<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/373f1079-746e-4801-a3cd-8726dfa52ef2" />
</p>
<p>
This ISP router is essentially going to act as our internet service Provider for this whole lab. Normally we would hae hundreds or thousands of routers representing the internet but we're just going to have one. This ISP router is going to connect to our edge router. The problem with 1 edge router is if that router potentionally goes down, then what will happen is that entire datacenter becomes unreachable so that's why I put 2 edge routers. This significantly reduces potential downtime.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/f6fd814b-0f69-4005-91f3-a2eba57b84f6" />
</p>
<p>
For the ISP router, I created 3 loopbacks. If we have connectivity to either 3 these addresses, then we can make the assumption that our lab has connectivity to the internet for this simulation.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/d4e48cbc-ab77-4c65-9168-7c968d789574" />
</p>
<p>
I added some layer 3 switches. Now for the end devices or servers. I need to be able connect them to a virtual IP address which is why we will be using HSRP to add some sort of redundancy for the end devices. So now the virtual gateway for the network is 10.1.1.254. We still need to give the edge router interfaces an IP which I'll give Edge 1, 10.1.1.1 and Edge 2 10.1.1.2.
</p>
<br />

<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/98481834-0630-4794-b577-c4a572e29843" /> <img width="400" alt="image" src="https://github.com/user-attachments/assets/8ac9e76d-7dc5-4acd-825d-ed0ed2c8a661" />

</p>
<p>
HSRP allows multiple routers to function as a single virtual router, meaning if the primary router fails, another router can immediately take over without disrupting network services. 
  <p> So now i have done the HSRP configurations with the virtual IP address of 10.1.1.254. 
  This means that if a end device's connection to an Edge router fails, the standby router will switch to an active router and the servers will now continue sending traffic to the new active router.
    <p> Its important that I will configure the end devices' default gateway as the Virtual IP address instead of the physical interface address.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/323de5c5-2118-4a52-8ff3-86c3969a9425" />
</p>
<p>
Lets now look at the link between Core 1 and Core 2. Because they are connected to each other over 2 access links, spanning-tree protocol is going to take affect and we're essentially losing bandwidth of one of the links highlighted in orange. If one of the links were fail, spanning tree will take over and allow the other link to come up but only after 50 seconds of going in between the forwarding protocols.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/70eb2a90-cb0e-487d-a791-b0305c24673e" />
</p>
<p>
Now, I'll set up everything on the external side onto VLAN 90. The link between the Core Switches will not be be for the Access or Server layers. It is for the redundancy for the Edge side of the Core Switches.
  <p> I configured interface g1/0/24 to be on Vlan 90 in access mode and configured interface g01/0/20 - 21 into a portchannel mode desireable. It now says PAGP currently not enabled on the remote port. This means that I have not set Core 2's side as a port channel.
</p>
<br />

<p>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/05484a9d-d24f-43f3-a147-608c7c204bd7" /> <img width="400" alt="image" src="https://github.com/user-attachments/assets/6e00e4f5-95c5-4865-b448-4a39bcded562" />
</p>
<p>
So now that I have configured the same thing on Core 2, the area in red from the Core side should be in VLAN 90. We can then confirm this by going into Edge 1 and ping and verify if edge 2 is reachable from edge 1. If I receive a reply, it confirms that the devices can communicate over the network.
  <p> Now that the ping is successful, not only do we have connectivity between the two Edge routers but we also confirmed that all of the connectivity in the area is now working over VLAN 90 which means the configuration is working as intended. Since the external link on VLAN 90 is built for redundancy, if one of the core switches, Core 1, were to fail, all traffic would through to Core 2.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
So now that we have set up the Core (Spine) and Edge side, I am now going to look at the Access Layer or the Leaf of the topology or essentially, "Spine and Leaf" side of the topology. In this topology, the access layer, also known as leaf switches, plays a critical role in connecting devices within a network, especially in data center environments. The leaf switches also facilitate east-to-west traffic, which is the communication between servers in a data center. <p> 
  When talking about East-to-West traffic, what it essentially means is that it is the data comunication between different systems or devices within the same data center or network, like in our topology currently being server to server communication, as opposed to North-to-South traffic, which involves data moving between the data center and the external world. An example of North-to-West traffic is if a user requests data from the external internet. <p> 
  Each of these leaf switches needs to connect to each of our Core network, and the best practice to do so is with redundant links.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
