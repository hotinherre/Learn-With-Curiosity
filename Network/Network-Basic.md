# Network Basic

## OSI Model

### Layer 1 - Physical Layer

transmit and receive raw data. Converts digital bits into electrical, radio or optical signal.
Example: network cable. 

### Layer 2 - Data Link Layer

provides link between two directly connected nodes. (LAN)
Detects and corrects error passing from physical layer.  

**Packet**  
target mac addr(6byte) / source mac addr(6byte) / type(2byte) / data(46-1500 byte) / CRC(4byte)

**APR**
if target mac addr is known, send packet.  
If target mac addr is unknown. Broadcast in LAN. 

## IP

IP 被分为 5 类. abcde. 划分不合理，使用度不高。

**CIDR**

(subnetting)将 32 位的 IP 地址一分为二，前面是网络号（network number），后面是主机号(host/node number)。10.100.122.2/24.

广播地址(broadcast address)：10.100.122.255。所有机器 10.100.122 都收到信号. boradcast address = invert subnet mask | private network address.

子网掩码(subnet mask)：255.255.255.0. 将子网掩码和 IP 地址按位计算 AND，就可得到网络号（private network address）

## MAC Address

ID for NIC(network interface card). Works in local network. For external network, look for IP address first.

## gateway

Gateway can be router, switch, isp router... Sending request to external network(has different network number), need to pass through gateway.

## DCHP

1. dynamically allocate ip to device. Map ip to mac address.
2. bootstrap/install system in data center.

LAN: Local Area network.

## HUB vs Switch vs Router

- HUB(physical layer): Simple and cheap. broadcast packet to all connected host.
- Switch(data link layer):  At first behave like HUB. But it will remember the location and MAC ADDR of each host over the time. Then it can direct packet to exact target host.

Use STP(spanning tree protocal) algorithm to resolve Broadcast storm problem.

### Network partition

1. physical partition with different switch
2. Use VLAN. set vlan id tag in packet.

- Router(network layer):
    - connect LAN to other network like internet
    - Assign IP address to host with DHCP
    - Provide WIFI signal
    - network address translation

## Ping and Traceroute

**Ping** command send ICMP request to target network host.
Return success response(responce time, sequence ID) or error message if packet id not delivered to host.

**Traceroute** command prints route packets take to get to network host.

