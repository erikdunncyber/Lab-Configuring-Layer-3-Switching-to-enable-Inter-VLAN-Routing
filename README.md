<h1>Packet Tracer - Configuring Layer 3 Switching to enable Inter-VLAN Routing</h1>

<p align="center">
This is the network diagram of the network I will enable inter-VLAN routing via Layer 3 Switching. To provide more context to the current configurations of the network, it is important to note that SW2 is a multi-layer switch, SW1-SW2 are connected via trunking, and R1-SW2 are connected via 'Router On A Stick'. I'll be changing this 'Router On A Stick" configuration to do inter-VLAN routing through SVIs (Switch Virtual Interfaces) on SW2: <br/>
<img src="https://i.imgur.com/YSSzPRA.png" height="80%" width="80%"/>
<br />
<br />
I'll first go on R1's CLI and check the current interface configurations with the command "show ip interface brief". The brief shows that interface g0/0 and its sub-interfaces, the interface currently supporting the ROAS configuration, is up: <br/>
<img src="https://i.imgur.com/27dx5fX.png" height="80%" width="80%"/>
<br />
<br /> 
Now I'll remove the sub-interfaces from R1's interface configurations since they won't be needed in the layer 3 connection I'll be implementing. To do this I enter Global Configuration mode and remove the first sub-interface with the command, "no interface g0/0.10". I use the same command as it applies to remove the other sub-interfaces as well: <br/>
<img src="https://i.imgur.com/VD7RyuQ.png" height="80%" width="80%"/>
<br />
<br /> 
Next I'll configure interface g0/0 with an IP address. So I'll declare to configure the interface with the "int g0/0" command and assign the ip address with the command "ip address 10.0.0.194 255.255.255.252". This is all that needs to be configured on R1 so now I'll move on to configuring SW2's g1/0/2 interface: <br/>
<img src="https://i.imgur.com/VTyEXU8.png" height="80%" width="80%"/>
<br />
<br /> 
So I'll go into Global Configuration mode on SW2's CLI and select to configure the g1/0/2 interface and put it into layer 3 mode with the command "no switchport". Now I'll assign an IP address to the interface with the command "ip address 10.0.0.193 255.255.255.252": <br/>
<img src="https://i.imgur.com/7Tl2y9d.png" height="80%" width="80%"/>
<br />
<br /> 
I'll also enable IP routing on the SW2 with the "ip routing" command so it can enable local and connected routes. Then I'll configure the default route with the command "ip route 0.0.0.0 0.0.0.0 10.0.0.194": <br/>
<img src="https://i.imgur.com/V2X7yUF.png" height="80%" width="80%"/>
<br />
<br /> 
Moving forward, I'll configure the Switch Virtual Interfaces on SW2. To do this I'll use the command "interface vlan 10" and I'll assign an IP address, the last usable address within the subnet in this case, using the command "ip address 10.0.0.62 255.255.255.192". I'll repeat this step as it applies to VLAN 20 and 30: <br/>
<img src="https://i.imgur.com/zcrQCQh.png" height="80%" width="80%"/>
<br />
<br /> 
To verify if the VLAN SVIs I configured are up and running I'll use the "do show ip interface brief" command. The brief shows that they are all up and running as intended: <br/>
<img src="https://i.imgur.com/IWIVraQ.png" height="80%" width="80%"/>
<br />
<br />
The inter-VLAN routing should be successfully configured now so I'll try testing the connection by pinging from PC7 in VLAN 10 to another end-host on the network. I'll try pinging PC3 in VLAN 30 using the command "ping 10.0.0.129". The ping was sent and recieved successfully. I'll also try pinging the internet to see if the default route to the R1 working with the command "ping 1.1.1.1". The ping was sent and recieved successfully.: <br/>
<img src="https://i.imgur.com/hi9sySl.png" height="80%" width="80%"/>
<br />
<br />
</p>

