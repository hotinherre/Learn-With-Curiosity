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

## DCHP

1. dynamically allocate ip to device. Map ip to mac address.
2. bootstrap/install system in data center.

LAN: Local Area network.

## HUB vs Switch

- HUB(physical layer): Simple and cheap. broadcast packet to all connected host.
- Switch(data link layer):  At first behave like HUB. But it will remember the location and MAC ADDR of each host over the time. Then it can direct packet to exact target host.

Use STP(spanning tree protocal) algorithm to resolve Broadcast storm problem.

### Network partition

1. physical partition with different switch
2. Use VLAN. set vlan id tag in packet.

## Ping and Traceroute

**Ping** command send ICMP request to target network host.
Return success response(responce time, sequence ID) or error message if packet id not delivered to host.

**Traceroute** command prints route packets take to get to network host.

## Router

- connect LAN to other network like internet. Work as gateway.
- Assign IP address to host with DHCP
- Provide WIFI signal
- network address translation(NAT)

### Router Protocals

1. static: mannualy set up routing strategy  
2. dynamic protocal: find shortest path between two router
    1. distance vector routing(bellman-ford)
    2. link state routing(dijkstra):OSPF, BGP

### local network

- Build packet: source MAC/IP, target MAC(Router/Switch)/IP(Target host)
- Send to Router
- Router Use ARP find target mac address. Router rewrite target MAC ADDR.
- Send to target host

### access public network with NAT

total number of ipv4 is 4.2 billion. A workaround to let private device connect to public without having public ip address. All local host connect to router which has public IP assigned by ISP.

192.168.x.x and 10.x.x.x are two ranges reserved for private network.  

Steps host send request to web server:

- Build packet: source MAC/(IP:port), target MAC(Router)/(IP:port)(Target host). Port is 80 if use http protocal.
- Send to Router
- Router change target MAC with ARP.
- Router change source IP:port.
- Router add record in NAT forwarding table. Private ip:port -> public ip:port
- Router forward packet to target host
- When response comes back to router, router will look up NAT forwarding table. Create another packet, make the packet seems sent from server to local host directly.
  
## UDP vs TCP

use port number to identify target/recource.

### UDP

stateless protocal. No order and delivery gurantee. Used in streaming, online video game and other scenario doesn't care about the complete of packet.

### TCP

stateful protocal. 3 handshake to make connection. Use sliding window and buffer to gurantee packet order and completeness. Have capbility to do trafic control and congest control. Algorithm and mechanism is pretty complex.. Not paying attention to how the internal works..

## Socket

low level OS API to send and receive data from different host. Unless you need to implement your own protocal, usually developer won't touch this complex api.  Each socket will be kept in memory.  The limitation for supporting multiple request is memory and fd.

## HTTP

Most http api is built on top of XMLHttpRequest. Which makes AJAX programming happens. Send request to server and process reponse data asyncronously.  For other web communication API, 1) Server push: server-sent event  2) two-day tunnel: websocket.  

HTTP is stateless, but it is sessionful by using cookies and authentication headers.

### HTTP Request

* First line of HTTP: method path http-version
* HTTP Header
* HTTP Body

### HTTP Response

* First line of HTTP: HTTP-version Status-Code Status message
* HTTP Header
* HTTP Body
  
### HTTP Workflow

Application layer: initialize http request.
Transport layer: Divide HTTP Body data into small packet. Append port number. Make TCP connetion.
Internet layer: Append IP.
Data Link Layer: Append MAC with ARP.


### HTTP 2.0

Encapsulating http message into frame(binary data).  
Then HTTP/2 provide optimizaiton:  
1. Compression on header with index table.
2. Mutipleplexing. Send mutiple request in parallel to reduce TCP connection.(HTTP 1.1 send syncronously)

### QUIC

Quick UDP internet connection. Use UDP to improve performance of TCP used in web apps. Still experimental protocal, designed by Google.

### HTTP Caching

- private caching, public caching.
- change caching strategy by set up http header.
- max-age=<...seconds>

### HTTP Cookies

cookies is small piece of data that a server sends to the user's browser by `Set-Cookie` header. Browser store it and send it back to the same server with subsequent request.

```text
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

```text
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

#### Purpose

- session management
- Personalization
- tracking for ads

Without specifying `Max-Age` or `Expires`, cookie will be deleted when browser is closed.  
Set-Cookie with `HTTPOnly` will blocks JS `Document.cookie` API.

#### Scope of cookies

if `Domain` is specified, then subdomains are always included. Otherwise, exluding subdomains.  
For example, if domain=mozilla.org, then cookies are included on subdomains like developer.mozilla.org  

`Path` indicates a URL path must exist in the requested URL in order to send the Coockie Header.

### CORS

Use additional HTTP headers to enable cross origin resource sharing.
Client side send option request as preflight request before real resource request.

### Evolution of HTTP

- HTTP/0.9, request is one line, response is html only.
- HTTP/1.0, 1)add headers, 2) add status code/message in response 3) able to transmit different types of data
- HTTP/1.1, 1)reuse connection(persistent connection) 2) add cache control
- HTTP/2.0, 1)binary protocal,divide hears/data into frames 2)enable multiplexing 3) compress headers by index table

## HTTPS

HTTPS is an encrypted version of the HTTP protocol. It use SSL/TLS to encrypt all data.

### Asymmetric encription

Encode with public key, decode with private key.
Encode with private key, decode with public key.

IN HTTPS, public key is a digital certificate issued by a trusted CA.

## File Transfer Protocal 

### FTP 

- Transfer files between client and server
- One TCP connection to send command from client to server. List,reter, store.
- One TCP connection to every file transmitting.

### P2P

- Download and share block files with other peers in the network.
- Have a tracker server to maintain the info of which peer has which file
- DHT: No more tracker server. Save part of node info in each node in the p2p network.

## DNS

- DNS server keep track of Domain name to IP address mapping.  
- DNS look up may go to mutiple layer DNS server, root -> first -> second.  
- Usually local network has set up local DNS server.
- DNS are cached in OS. 
- Set local DNS record in /etc/hosts.
- Enable load balancing by mapping one domain name to mutiple IP.

### DNS record

#### A Record and AAAA Record
pointing domain name to IPv4/IPv6.  
Google.com =   8.8.8.8

#### CNAME(alias)
Point one domain name to another domain name. Which makes it as an alias.  
www.Google.com(alias) = Google.com. ALlows the domain to resolve same server with or without www. subdomain.  
If using A record, it requires to update mutiple record, when ip changes. With alias, you only need to change A record. 

#### MX(Mail Exchange)
point mail address to mail server. Always try to send the mail server with lowest priority order first.

#### SRV(Service Record)
- Location of service on the network
- Contains several setting data
- Created automatically by applications










