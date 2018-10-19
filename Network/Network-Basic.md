# Network Basics

## parking lot

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
