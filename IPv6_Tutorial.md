# IPv6 Tutorial:

* Module 1: Introduction to IPv6
* Module 2: IPv6 Protocol
* Module 3: IPv6 Packet
* Module 4: IPv6 Security
* Module 5: Transition Mechanisms
* Module 6: Interoperability


## Module 1: Introduction to IPv6

|       		| Plugin 	| README 		|
| ----------------------|---------------|-----------------------|
| Address space      	|32 bits	|128 bits		|
| Possible addresses    |$$2^{32}$$	|$$2^{128}$$		|
| Address format      	|192.0.2.1	|2001:db8:3:4:5:6:7:8 	|
| Header length      	|20bytes 	|40bytes 		|
| Header fields      	| 14 		| 8			|
| IPsec      		|optional	|SHOULD*		|


### IPsec on IPv6
* IPv6 Node Requirements (RFC6434) states
that all IPv6 nodes SHOULD support IPsec
_SHOULD - means that there may exist valid reasons in particular circumstances to ignore a particular item, but the full implications must be understood and carefully weighed before choosing a different course_

### Terminology
* node - a device that implements Internet
protocol (IP)
* router - a node that forwards IP packets
not explicitly addressed to itself
* host - any node that is not a router
* RFC4861 - [Terminology](https://datatracker.ietf.org/doc/html/rfc4861#section-2)
* Wikipedia - [IPv6 address](https://en.wikipedia.org/wiki/IPv6_address#:~:text=IPv6%20addresses%20are%20assigned%20to,times%20larger%20than%20the%20%2F8)


### Address Notation
* IPv6 consists of 8 fields each 16 bits long
* Written in hexadecimal numerals (base 16)
* Separated by a colon ":"
```
		2001:0db8:1234:5678:9abc:def0:1234:567
```

|       Field Hexadecimal(16 bits)		| Hexadecimal 		| Binary 		|
| ----------------------------------------------|-----------------------|-----------------------|
| 1      					|2001			|0010 0000 0000 0001	|
| 2    						|0db8			|0000 1101 1011 1000	|
| 3      					|0be0			|0000 1011 1110 0000 	|
| 4      					|75a1 			|0111 0101 1010 0001 	|
| 5      					|0000 			|0000 0000 0000 0000	|
| 6      					|0000			|0000 0000 0000 0000	|
| 7      					|0000			|0000 0000 0000 0000	|
| 8      					|0001			|0000 0000 0000 0001	|

**2001:0db8:0be0:75a1:0000:0000:0000:0001**

|       Global Routing Prefix			| Subnet ID 		| Interface ID 		|
| ----------------------------------------------|-----------------------|-----------------------|
|	       48 Bits				|	16 Bits		|	64 Bits		|
|	       2001:0db8:0be0			|	75a2		|0000:0000:0000:0001	|


### Global Unicast Address (GUA):
* 2000::/3 (First hextet: 2000::/3 to 3FFF::/3).
* Globally unique and routable.
* Similar to public IPv4 addresses.

> 2001:db8::/32 – RFC 2839 and RFC 6890 reserve this range of addresses for documentation.

64-bit Interface ID = 18 quintillion (18,446,744,073,709,551,616) devices/subnet.
16 bit subnet ID (initially recommended) = 65,536 subnets
IPv6 Global Unicast Address Format Fields:

1. Global routing prefix: global routing prefix is the portion of the address that is assigned by the provider such as an ISP to a customer or site. The most significant 48-bits are assigned as a Global routing prefix which is assigned to a specific autonomous system.
2. Subnet ID: The subnet ID is the portion between the global routing prefix and the interface ID.
3. Interface ID: The Interface ID is equal to the host part of an IPv4 address, It is must recommend that in most cases /64 subnets must be used which creates a 64-bit ID.


### Address Notation
```
2001:0db8:0be0:75a2:0000:0000:0000:0001
```
* Leading zeros can be left out
```
2001:db8:be0:75a2:0:0:0:1
```
* Consecutive fields of zeros can be replaced with "::"
```
2001:db8:be0:75a2::1
```
> If there are several consecutive fields of zeros only one can be replaced with ::

**Example 1:** 

_Compress the following IPv6 addresses to shortest form possible_
```
2001:0db8:0ab0:0d00:0000:0000:0000:0c01
2001:0db8:0000:4c05:0000:0000:05ad:0bb1
2001:0db8:0000:0000:1234:0000:0000:da61
```
**Solve** 
```
2001:0db8:0ab0:0d00:0000:0000:0000:0c01
2001:0db8:0ab0:0d00::0c01
2001:0db8:0:4c05::05ad:0bb1
2001:0db8::1234:0000:0000:da61
or
2001:0db8:0:0:1234::da61
```

**Example 2:** 

_Expand the following IPv6 addresses to full notation_

```
2001:db8:ab::bc0:c1ab
2001:db8:a000:c05:b0::1
2001:db8:0:1234::61
```

**Solve** 
```
2001:db8:ab:0000:0000:0000:bc0:c1ab
2001:db8:a000:c05:b0:0000:0000:0001
2001:db8:0000:1234:0000:0000:0061
```

| Address block (CIDR) | First address      | Last address                             | Number of addresses | Usage       | Purpose                                                                        |
|----------------------|--------------------|------------------------------------------|---------------------|-------------|--------------------------------------------------------------------------------|
| ::/128               | ::                 | ::                                       | 1                   | Software    | Unspecified address                                                            |
| ::1/128              | ::1                | ::1                                      | 1                   | Host        | Loopback address—a virtual interface, the local host |
| ::ffff:0:0/96        | ::ffff:0:0:0:0 ::ffff:0:0:0     | ::ffff:0:255.255.255.255 ::ffff:0:ffff:ffff                 | $$2^{32}$$                | Software    | IPv4-translated addresses                                                      |
| ::ffff:0:0:0/96      |  	::ffff:0.0.0.0 ::ffff:0:0      | ::ffff:255.255.255.255 ::ffff:ffff:ffff                    | $$2^{32}$$                | Software    | IPv4-mapped addresses                                                          |
| 5f00::/16            | 5f00::             | 5f00:ffff:ffff:ffff:ffff:ffff:ffff:ffff  | $$2^{112}$$               | Routing     | IPv6 Segment Routing (SRv6)                                                    |
| 64:ff9b::/96         | 64:ff9b::0.0.0.0 64:ff9b::0:0 | 64:ff9b::255.255.255.255 64:ff9b::ffff:ffff             | $$2^{32}$$                | The global Internet | IPv4/IPv6 translation                                                      |
| 64:ff9b:1::/48       | 64:ff9b:1::        | 64:ff9b:1:ffff:ffff:ffff:ffff:ffff:ffff  | $$2^{80}$$, with $$2^{48}$$ for each IPv4 | Private internets | IPv4/IPv6 translation                                 |
| 100::/64             | 100::              | 100::ffff:ffff:ffff:ffff                 | $$2^{64}$$                | Routing     | Discard prefix                                                                 |
| 2001::/32            | 2001::             | 2001:0:ffff:ffff:ffff:ffff:ffff:ffff     | $$2^{96}$$                | The global Internet | Teredo tunneling                                                         |
| 2001:20::/28         | 2001:20::          | 2001:2f:ffff:ffff:ffff:ffff:ffff:ffff    | $$2^{100}$$               | Software    | ORCHIDv2                                                                        |
| 2001:db8::/32        | 2001:db8::         | 2001:db8:ffff:ffff:ffff:ffff:ffff:ffff   | $$2^{96}$$                | Documentation | Addresses used in documentation and example source code                  |
| 2002::/16            | 2002::             | 2002:ffff:ffff:ffff:ffff:ffff:ffff:ffff  | $$2^{112}$$               | The global Internet | The 6to4 addressing scheme (deprecated)                                 |
| fc00::/7             | fc00::             | fdff:ffff:ffff:ffff:ffff:ffff:ffff:ffff  | $$2^{121}$$               | Private internets | Unique local address                                                      |
| fe80::/64 from fe80::/10 | fe80::       | fe80::ffff:ffff:ffff:ffff                 | $$2^{64}$$                | Link        | Link-local address                                                              |
| ff00::/8             | ff00::             | ff00:ffff:ffff:ffff:ffff:ffff:ffff:ffff  | $$2^{120}$$              | The global Internet | Multicast address                                                          |




### Introduction

Extended Unique Identifier (EUI), as per RFC2373, allows a host to assign iteslf a unique 64-Bit IP Version 6 interface identifier (EUI-64). This feature is a key benefit over IPv4 as it eliminates the need of manual configuration or DHCP as in the world of IPv4. The IPv6 EUI-64 format address is obtained through the 48-bit MAC address. The MAC address is first separated into two 24-bits, with one being OUI (Organizationally Unique Identifier) and the other being NIC specific. The 16-bit 0xFFFE is then inserted between these two 24-bits for the 64-bit EUI address. IEEE has chosen FFFE as a reserved value which can only appear in EUI-64 generated from the an EUI-48 MAC address.
Here is an example showing how a the MAC Address is used to generate EUI.
```
00:0c:29:0c:47:d5
```
Next, the seventh bit from the left, or the universal/local (U/L) bit, needs to be inverted. This bit identifies whether this interface identifier is universally or locally administered. If 0, the address is locally administered and if 1, the address is globally unique. It is worth noticing that in the OUI portion, the globally unique addresses assigned by the IEEE has always been set to 0 whereas the locally created addresses has 1 configured. Therefore, when the bit is inverted, it maintains its original scope (global unique address is still global unique and vice versa). The reason for inverting can be found in RFC4291 section 2.5.1.
```
00:0c:29:ff:fe:0c:47:d5
```
Once the above is done, we have a fully functional EUI-64 format address.
Another doubt or frequently asked question is, are IPv6 devices (routers etc) today doing anything to that universal/local bit? Currently, nothing is being done be the U/L bit 1 or 0. However, per RFC4291 2.5.1 (The use of the universal/local bit in the Modified EUI-64 format identifier is to allow development of future technology that can take advantage of interface identifiers with universal scope), this may change in the future as the technology evolves.


### Modified EUI-64
* Used in stateless address autoconfiguration (SLAAC)
* 7th bit from the left, the universal/local (U/L) bit, needs to be inverted
```
00 (L) → 02 (U)
```
```
02:0c:29:ff:fe:0c:47:d5
```

### IPv6 EUI-64 (Extended Unique Identifier)

The IPv6 addressing scheme is the successor of the IPv4 addressing scheme. Along with a larger pool of routable addresses, it has a lot of additional features. One such update is in the Global Unicast Address configuration of a host on the network.

The IPv6 GUA configuration can be done in the following ways:

-   [Stateless Address Autoconfiguration – (SLAAC)](https://www.geeksforgeeks.org/what-is-ipv6-stateless-address-autoconfiguration/)
-   SLAAC with stateless DHCPv6 Server
-   Stateful DHCPv6 Server

In the first two kinds, the host must generate its own unique Interface ID. There are two ways in which a unique Interface ID can be generated:

-   EUI-64 (Extended Unique Identifier)
-   Randomly Generated ID

### **The EUI-64:**

The EUI-64 or modified Extended Unique Identifier uses the [Media Access Control (MAC)](https://www.geeksforgeeks.org/introduction-of-mac-address-in-computer-network/)  Address to generate a unique 64-bit EUI-64 Interface ID. However, an  [IPv6 address](https://www.geeksforgeeks.org/internet-protocol-version-6-ipv6/) is a 128-bit address, therefore, the first 64 bits are the Global Routing Prefix(48 bits) and the Subnet ID(16 bits) as shown below:


|       Global Routing Prefix			| Subnet ID 		| Interface ID 		|
| ----------------------------------------------|-----------------------|-----------------------|
|	       48 Bits				|	16 Bits		|	64 Bits		|


The Media Access Control is the permanent address provided to the Network Interface Card (NIC) of the host by the manufacturer. It is used by the MAC sublayer of the Datalink Layer.

The MAC Address is a 48-bit address. To make it a 64-bit address, two operations are performed:

The hexadecimal value of FFFE(16-bits) is added in the middle of the 48-bit mac address.
The 7th bit from the start is toggled from 0 to 1.

**For example:**

For the MAC address  **FC:99:47:75:CE:E0** the steps are performed as shown in the figure:

![MAC Address of Host](https://media.geeksforgeeks.org/wp-content/uploads/20220704120230/fc994775cee01.png)

After the Interface ID is configured, a Duplicate Address Detection (DAD) packet is sent by the host which is similar to an  [Address Resolution Protocol (ARP)](https://www.geeksforgeeks.org/arp-reverse-arprarp-inverse-arp-inarp-proxy-arp-and-gratuitous-arp/)  Request to the generated IPv6 address. If no host answers the request, the generated address is unique. Cisco routers are configured to use the EUI-64 ID generation by default.

### **Conclusion:**

The EUI-64 or the modified Extended Unique Identifier is the procedure performed by an IPv6 SLAAC configured host to generate its own unique Interface ID using the MAC address of the host. The unique Interface ID is generated by performing the following steps:

-   Add the hexadecimal value FFFE in the middle of the 48-bit MAC Address.
-   Toggle the 7th bit from 0 to 1.


### EUI-64
* 64-bit extended unique identifier (EUI)
* Derived from 48-bit MAC address
```
00:0c:29:0c:47:d5
+
ff:fe
00:0c:29:ff:fe:0c:47:d5
```
* Used in stateless address autoconfiguration
(SLAAC)
* 7th bit from the left, the universal/local
(U/L) bit, needs to be inverted
00 (L) → 02 (U)
```
02:0c:29:ff:fe:0c:47:d5
```

### Modified EUI-64

IPv6 prefix
```
2001:db8:be0:75a2::/64
```
> and modified EUI-64 from MAC address
```
02:0c:29:ff:fe:0c:47:d5
```
> Results in the following IPv6 address
```
2001:db8:be0:75a2:020c:29ff:fe0c:47d5
```

***Example3***

Given a MAC address: ``00:1A:2B:3C:4D:5E``

### 1-Split into two halves:

```
00:1A:2B and 3C:4D:5E
```

* Insert 0xFFFE in the middle:

```
00:1A:2B:FF:FE:3C:4D:5E
```

### 2-Convert to EUI-64 format:

*	Convert 00 to binary: 00000000
*	Flip the seventh bit (from the left), resulting in 00000010 (binary) which is 02 (hex).

_Updated identifier:_

```
02:1A:2B:FF:FE:3C:4D:5E
```

### 3-Embed in IPv6 address:

    Using the link-local prefix fe80::/64, the final IPv6 address is:

```
    fe80::21a:2bff:fe3c:4d5e
```

### Steps in Detail

1.    Original MAC Address: 00:1A:2B:3C:4D:5E

2.    Split into two halves:
*       00:1A:2B
*       3C:4D:5E

3.    Insert 0xFFFE in the middle:
*        Result: 00:1A:2B:FF:FE:3C:4D:5E

4.    Convert to EUI-64 format:
*        Convert the first byte 00 to binary: 00000000
*        Flip the seventh bit: 00000000 (binary) to 00000010 (binary)
*        Convert back to hex: 02
*        Result: 02:1A:2B:FF:FE:3C:4D:5E

5.    Embed in IPv6 address:
*        Using the prefix fe80::/64, we get: fe80::21a:2bff:fe3c:4d5e

This process will convert a MAC address into an EUI-64 format and then embed it into an IPv6 address. For different prefixes, replace fe80::/64 with the desired prefix.



### SLAAC Address Construction

|       Routing prefix		| Subnet identifier 	| Interface identifier 	|
| ------------------------------|-----------------------|-----------------------|
|	  0-64 bits		|	0-64 bits	|	64 bits		|



### Subnetting

``2001:db8:be0:75a2:020c:29ff:fe0c:47d5``

|                               		  |                      	|                    			       |
| ------------------------------------------------|-----------------------------|----------------------------------------------|
|2001:0db8:0be0 (Routing prefix: 48 bits)	  |	5a2 (Subnet: 16) 	|0000:0000:0000:0001 (65536 x /64) 	       |
|2001:0db8:0be0:7 (Routing prefix: 52 bits)	  |     5a2 (Subnet: 12)        |0000:0000:0000:0001 (4096 x /64)              |		
|2001:0db8:0be0:75 (Routing prefix: 56 bits)      |      a2 (Subnet: 8)         |0000:0000:0000:0001 (256 x /64)	       |
|2001:0db8:0be0:75a (Routing prefix: 60 bits)     |      2  (Subnet: 4)         |0000:0000:0000:0001 (16 x /64) 	       |


### Address Types

|Type	   	|   Range 	|
| --------------|---------------|
|Link local	|fe80::/10 	|
|Global unicast |2000::/3 	|
|Multicast	|ff00::/8 	|
|Unique local   |fc00::/7 	|



### Special Addresses

|Type	   	|   Range 	|
|---------------|---------------|
|Loobpack	|fe80::/10 	|
|Documentation  |2000::/3 	|
|6to4		|ff00::/8 	|
|Unspecified address   |fc00::/7|
|Teredo   	|	fc00::/7|
|Anycast   	|fc00::/7 	|

### Unique Local Address

* Meant to never be used on the Internet
* fc00::/7 prefix is reserved for ULA
* Divided into fc00::/8 and fd00::/8
* fd00::/8 currently is the only valid ULA prefix
* fc00::/8 prefix has not been defined

### Anycast Address
* Multiple hosts can have the same anycast address
* Send to any one member of this group (usually the nearest)
* Indistinguishable from a unicast address
* Use cases: load balancing, content delivery networks (CDN)
* When using anycast address, Duplicate Address Detection has to be disabled for that IP

### IPv4-mapped IPv6 address

* IPv6 address that holds an embedded IPv4 address.
* Is used to represent the addresses of IPv4 nodes as IPv6 addresses.


|IPv4 address	|   IPv4-mapped IPv6 address 	|
|---------------|-------------------------------|
|192.0.2.123	|::ffff:192.0.2.123	 	|
|		|::ffff:c000:027b	 	|


# Module 2

## Address Configuration

* Auto configuration of link local address
* Stateless
  * Stateless address autoconfiguration (SLAAC)
      * Additional options with DHCPv6
* Stateful
      * DHCPv6
* Static

			## IPv6
	### SLAAC					### DHCPv6
	- IP address			- DHCPv6			DHCPv6 PD
	- Gateway			 (for Userts)   		(for Networks)
	- DNS				- IP addresses			- Perfix
					- Gateway			- Route to Network
	- Additional Options		- DNS				- Binding (lease)
	  with DHCPv6			- Additional Options
	  				  with DHCPv6
	  				  
### What is IPv6 Stateless and Stateful ?	  				  

* 2.1. Stateful Addressing

Stateful addressing, also known as DHCPv6, is like the way IPv4 addresses are assigned. In stateful addressing, the DHCPv6 server assigns an IPv6 address to the requesting device.

This process involves a series of messages between the DHCPv6 client and server. Moreover, the client sends a message requesting an IPv6 address, and the server replies with an IPv6 address that is leased to the client for a specified period. It then uses this address until the lease time expires or until the client releases the address.

Stateful addressing is useful for assigning static IPv6 addresses to devices that require consistent IP addresses, such as servers or printers. It is also useful in cases where the administrator wants to control the network configuration of the clients.

* 2.2. Stateless Addressing

Stateless address, also known as autoconfiguration, is a simpler way of assigning IPv6 addresses.

In stateless addressing, each device generates its own IPv6 address based on its MAC address and network prefix. The process that generates EUI-64 addresses is called EUI-64 address generation. In EUI-64 address generation, the device’s 48-bit MAC address is split in half, and an FF: FE value is inserted in between them. The resulting 64-bit value is then combined with the network prefix to create a unique IPv6 address.

Stateless addressing is useful for assigning temporary IPv6 addresses to devices that do not require consistent IP addresses, such as mobile devices or laptops. It is also useful in cases where the administrator wants to simplify the network configuration and avoid the complexity of managing DHCPv6 servers.

### Neighbor Discovery
* Neighbor discovery (ND) protocol
* Replaces ARP on IPv4
* Tracks and discovers other IPv6 hosts
* Auto-configures address
* Uses ICMPv6 protocol
* Has 5 message types:
	* Router solicitation (type 133)
	* Router advertisement (type 134)
	* Neighbor solicitation (type 135)
	* Neighbor advertisement (type 136)
	* Redirect (type 137)

### Link Local

* 1st step is to generate link local (LL) address
fe80:: + Interface ID (Modified EUI-64)

* 2nd: perform ‘neighbor solicitation’
``A: This is my IPv6 address, is this in use? What’s your MAC address?``

* 3rd: ‘neighbor advertisement’
``B: Yes, I’m using this address. My MAC is 12:34:56:78:90:12``

* If nobody answers, host uses generated LL address


### SLAAC
* Stateless address autoconfiguration
* Uses router solicitation and router advertisement messages
* Asks for a router
* Receives the address of the router and IP configuration

### DHCPv6 (Stateless)
* If necessary additional configuration can be obtained (for example static routes)
* It is done by DHCPv6
* To configure open IPv6 → ND

### IPv6 Routing

* Works similar like IPv4 classless routing
* Subnet size can be arbitrary
* SLAAC works only with /64 prefixes

|       			| 	   IPv6	 	|          IPv4		|
| ------------------------------|-----------------------|-----------------------|
|	 			|   0:0:0:0:0:0:0:0/0   |			|
|	Default Gateway		|        ::/0		|	0.0.0.0/0	|
|				|       2000::/3	|			|


* Several ways how to write default gateway


### IPv6 Subnetting

* You have been assigned /48 block
* You’re planning to assign /60 to your customers
* Each of your customers will have 16x /64
* $$2^12$$ = 4096 /60 subnets
* Each of your customers will have 16x /64 prefixes for their devices.

```
2001:0db8:0be0:|000|0::
	       | 12|
Routing prefix: 48 bits
		…

2001:0db8:0be0:|FFF|0::
	       | 12|
Routing prefix: 48 bits	You can assign 4096x 60 bit prefixes
		…
2001:0db8:0be0:000|0|::
	       	  |4|
Routing prefix: 60 bits

2001:0db8:0be0:000|F|::
	       	  |4|
Routing prefix: 60 bits	Customer can assign 16x 64 bit prefixes

```

| Prefix | Subnet Example       			| Total IP Addresses                         | # of /64 nets                      |
|--------|----------------------------------------------|--------------------------------------------|------------------------------------|
| /4     | x::                     			| 2^124                                      | 2^60                               |
| /8     | xx::                    			| 2^120                                      | 2^56                               |
| /12    | xxx::                   			| 2^116                                      | 2^52                               |
| /16    | xxxx::                  			| 2^112                                      | 2^48                               |
| /20    | xxxx:x::                			| 2^108                                      | 2^44                               |
| /24    | xxxx:xx::               			| 2^104                                      | 2^40                               |
| /28    | xxxx:xxx::              			| 2^100                                      | 2^36                               |
| /32    | xxxx:xxxx::             			| 2^96                                       | 4,294,967,296                      |
| /36    | xxxx:xxxx:x::           			| 2^92                                       | 268,435,456                        |
| /40    | xxxx:xxxx:xx::          			| 2^88                                       | 16,777,216                         |
| /44    | xxxx:xxxx:xxx::         			| 2^84                                       | 1,048,576                          |
| /48    | xxxx:xxxx:xxxx::     			| 2^80                                       | 65,536                             |
| /52    | xxxx:xxxx:xxxx:x::     			| 2^76                                       | 4,096                              |
| /56    | xxxx:xxxx:xxxx:xx::     			| 2^72                                       | 256                                |
| /60    | xxxx:xxxx:xxxx:xxx::    			| 2^68                                       | 16                                 |
| /64    | xxxx:xxxx:xxxx:xxxx::   			| 2^64 (18,446,744,073,709,551,616)          | 1                                  |
| /68    | xxxx:xxxx:xxxx:xxxx:x:: 			| 2^60 (1,152,921,504,606,846,976)           | 0                                  |
| /72    | xxxx:xxxx:xxxx:xxxx:xx::			| 2^56 (72,057,594,037,927,936)              | 0                                  |
| /76    | xxxx:xxxx:xxxx:xxxx:xxx::			| 2^52 (4,503,599,627,370,496)               | 0                                  |
| /80    | xxxx:xxxx:xxxx:xxxx:xxxx::			| 2^48 (281,474,976,710,656)                 | 0                                  |
| /84    | xxxx:xxxx:xxxx:xxxx:xxxx:x::			| 2^44 (17,592,186,044,416)                  | 0                                  |
| /88    | xxxx:xxxx:xxxx:xxxx:xxxx:xx::		| 2^40 (1,099,511,627,776)                   | 0                                  |
| /92    | xxxx:xxxx:xxxx:xxxx:xxxx:xxx::		| 2^36 (68,719,476,736)                      | 0                                  |
| /96    | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx::		| 2^32 (4,294,967,296)             	     | 0                                  |
| /100   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:x::		| 2^28 (268,435,456)             	     | 0                                  |
| /104   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xx::		| 2^24 (16,777,216)             	     | 0                                  |
| /108   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxx::		| 2^20 (1,048,576)             		     | 0                                  |
| /112   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx::		| 2^16 (65,536)          		     | 0                                  |
| /116   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:x::	| 2^12 (4,096)             		     | 0                                  |
| /120   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xx::	| 2^8 (256)               		     | 0                                  |
| /124   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxx::	| 2^4 (16)               		     | 0                                  |
| /128   | xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx	| 2^0 (1)                 		     | 0                                  |


### IPv6

* It is possible to split /64 prefix even further
* SLAAC requires /64 prefix length
* If the prefix is split beyond /64 will have to use DHCPv6 or static configuration
* Simpler devices might not support DHCPv6 (only SLAAC)		

## Module 3

### IPv6 Header

* Version - always contains ‘6’ (0110 in binary)
* Traffic class - holds 2 values.
	* 6 most significant bits - to classify packets for QoS
	* 2 remaining bits - for Explicit Congestion Notification (ECN) where supported
* Flow label - used to maintain packet sequence
* Payload length - Length of the IPv6 payload, i.e., the rest of the packet following this IPv6 header, in octets
* Next header - Identifies the type of header immediately following the IPv6 header
* Hop limit - Decremented by 1 by each router that forwards the packet. The packet is discarded if hop limit is 0
* Source address - address of the originator of the packet
* Destination address - address of the intended recipient of the packet
* Length: fixed size 40 bytes (320 bits)
* Field count: 8
* Simplified in comparison to IPv4
* IPv6 header has fixed size
* Optional information is encoded in separate extension headers
* Situated between the IPv6 and the upper-layer headers
* Each Next Header is identified by a distinct value

### Next Header Field

* IPv6 packet may carry zero, one, or more extension headers.

|      Extension Header 	          |Value|
| ----------------------------------------|-----|
|	Hop-by-Hop Options 	   	  |  0  |
|	Fragment		  	  |  44 |
|	Routing (Type 0)		  |  43 |
|	Destination Options		  |  60 |
|	Authentication			  |  51 |
|	Encapsulating Security Payload	  |  50 |


### Fragmentation
• Performed only by source nodes
• Fragment header is identified by a Next Header value of 44
• For every packet the source node generates an identification value
• ID must be different than any other fragmented packet sent recently with the same Src and Dst Address
• The packet consists of “unfragmentable” and “fragmentable” parts
• Unfragmentable = IPv6 header + extenstion headers that must be processed by routers en route to the destination
• Fragmentable = the rest of the packet

### Path MTU

• Path MTU (PMTU) is the largest packet size that can traverse between a source and destination without fragmentation
• IPv6 requires MTU 1280 bytes or greater
	• IPv4 requires MTU 68 bytes

### Path MTU Discovery

* PMTU discovery is a technique for determining the path MTU between two IP hosts
• To discover and take advantage of PMTUs greater than 1280, it is strongly recommended to implement PMTU discovery
• For packets that are larger than PMTU fragmentation is used


## Module 4

### ICMPv6

• ICMPv6 is an integral part of IPv6
• It is used to report errors encountered in processing packets, and to perform other functions, such as diagnostics
• There are 2 ICMPv6 message classes - error (types 0-127) and information (types 128-255)

| type  | 	   Meaning	     |     class	|
| -----	|----------------------------|------------------|
|   1	|   Destination Unreachable  |	   Error	|
|   3	|	Time Exceeded	     |	   Error	|
|   	|		       	     |       		|
|  128	|	Echo Request	     |     Information  |	
|  129	|	Echo Reply  	     |	   Information	|


### Neighbor Discovery

• NDP uses 5 different ICMPv6 packet types:
	• Router solicitation (type 133)
	• Router advertisement (type 134)
	• Neighbor solicitation (type 135)
	• Neighbor advertisement (type 136)
	• Redirect (type 137)

• Neighbor Discovery makes use of a number of different special addresses including:
	• Link-local scope address to reach all nodes (multicast address) - FF02::1
• Link-local scope address to reach all routers (multicast address) - FF02::2
• And others, for more info see - IPv6 Multicast Address Space Registry

### Router Solicitation
• Hosts send Router Solicitations in order to prompt routers to generate Router Advertisements quickly rather than at their next scheduled time
• It is sent to all-routers multicast address
• Source - IP address assigned to the sending interface
• Or the unspecified address (::/128) if no address is assigned
• Destination - typically the all-routers multicast address

### Router Advertisement
• Routers advertise their presence periodically, or in response to a Router Solicitation message
• A host receives Router Advertisements from all routers, building a list of default routers
• Various internet and link parameters are advertised such as prefixes, address configuration, MTU, etc.
• Facilitates centralized administration of critical parameters, that can be set on routers and automatically propagated to all attached hosts
• Allow routers to inform hosts how to perform address autoconfiguration
• Routers can specify whether hosts should use DHCPv6 and/or autonomous (stateless) address configuration
• Contains - source, link-local address assigned to the interface from which this message is sent
• Destination, typically the Source Address of an invoking Router Solicitation or the all- nodes multicast address
• M: 1-bit "Managed address configuration" flag
• O: 1-bit "Other configuration" flag

### Neighbor Solicitation
• Nodes accomplish address resolution by multicasting a Neighbor Solicitation, that asks the target node to return its link-layer address
• To verify that a neighbor is still reachable
• The target returns its link-layer address in a unicast Neighbor Advertisement message
• A single request-response pair of packets is sufficient for both to resolve each other's link-layer addresses
• Neighbor Solicitation is also used for Duplicate Address Detection
• Contains - source, either an address assigned to the interface from which this message is sent or (if Duplicate Address Detection is in progress) the unspecified address
• Destination, either the solicited-node multicast address corresponding to the target address, or the target address

### Neighbor Advertisement
• A response to a Neighbor Solicitation message
• A node may also send unsolicited Neighbor Advertisements in order to (unreliably) propagate new information quickly
• E.g. to announce a link-layer address change
• Source: an address assigned to the interface from which the advertisement is sent
• Destination: the Source Address of an invoking Neighbor Solicitation or the all- nodes multicast address

### Redirect
• Used by routers to inform hosts of a better first hop for a destination
• Hosts can also be informed by a redirect that the destination is in fact a neighbor
• Separate address resolution is not needed upon receiving a redirect

### Managed Address
- Configuration
• Router Advertisement 1-bit M flag
• When set, it indicates that addresses are available via DHCPv6
• If the M flag is set, the O flag is redundant
and can be ignored because DHCPv6 will return all available configuration information
• SLAAC will not be used

### Other Configuration
• Router Advertisement 1-bit O flag
• When set, it indicates that other configuration information is available via DHCPv6
• E.g. DNS-related information (necessary for Windows clients)
• If neither M nor O flags are set, this indicates that no information is available via DHCPv6
• M Flags: Managed Address Configuration
• O Other Configuration
### Duplicate Address
- Detection (DAD)
• Using Neighbor Solicitation a node can determine whether or not an address it wishes to use is already in use
• DAD sends a message with an unspecified source address targeting its own "tentative" address
• Such messages trigger nodes already using the address to respond with a multicast Neighbor Advertisement indicating that the address is in use
• If no response is received, the node uses the chosen address

### Neighbor Unreachability Discovery
- Detection (NUD)
• Communication to or through a neighbor may fail for numerous reasons at any time, including hardware failure, hot-swap of an interface card, etc.
• NUD detects the failure of a neighbor or the failure of the forward path to the neighbor
• NUD uses confirmation from two sources
• When possible, upper-layer protocols provide a positive confirmation that a connection is making "forward progress"
• When positive confirmation is not forthcoming, a node sends unicast Neighbor Solicitation messages that solicit Neighbor Advertisements as reachability confirmation from the next hop
• If node address changes NUD ensures that all nodes will reliably discover the new address

### Multicast Listener
- Discovery (MLD)
• MLDv2 is a translation of the IGMPv3 protocol for IPv6 semantics
• It is used by an IPv6 router to discover multicast listeners (nodes that wish to receive multicast packets) on directly attached links
• To discover which multicast addresses are of interest to those neighboring nodes

### MLD
• The purpose of MLD is to enable each multicast router to learn, which multicast addresses and which sources have interested listeners
• Specifies multicast address listeners and multicast routers
• A node can subscribe to certain multicast messages
• One router becomes elected as the Querier
• It will gather and maintain information about listeners and their subscriptions
• If the router fails another router on the same subnet takes over the role

### SEND
• If not secured, NDP is vulnerable to various attacks 
• SEcure Neighbor Discovery (SEND) is a proposed standard which helps to mitigate possible threats
• For more info see RFC3971

### Temporary Addresses
• Addresses generated using SLAAC contain an embedded interface identifier, which remains constant over time
• When a fixed identifier is used in multiple contexts, it becomes possible to correlate seemingly unrelated activity using this
identifier
• For a "road warrior" who has Internet connectivity both at home and at the office, the interface identifier contained within the address remains the same
• Privacy Extensions for SLAAC in IPv6 (RFC4941) suggests improvements to this behavior
• There are various implementations
• macOS and Windows10 generate new temporary IPv6 address every 24 hours
• Linux may create new temporary address for each new SSL/TLS connection

### LA
• Find out the temporary address(es) of your computer
• If you’re using Linux/macOS, open terminal and use command ``ifconfig``
• For Windows - ``ipconfig``

### Firewall
• RouterOS IPv6 → Firewall is similar with IP → Firewall
• RouterOS IPv6 Firewall implements same Filter and Mangle rules as with IPv4
• As well as Address Lists
• By default RouterOS IPv6 firewall does not have any filter rules

### NAT
• There’s no IPv6 → Firewall → NAT menu For mikrotik Devices.
• No need for NAT
	• There are plenty IPv6 addresses available
• One should not confuse NAT box with firewall - it does not provide security in itself
• See RFC5902: IAB Thoughts on IPv6 NAT

### IPsec
• Internet Protocol Security (IPsec) - a set of protocols to support secure communication at the IP layer
• Originally developed for IPv6, later backported also to IPv4
• Provides encryption to the IP protocol
• Can be used both with IPv4 and IPv6
• Multiple approaches can be used to implement IPsec:
• Header only encryption (AH)
• Data only encryption (ESP)
• Header and data encryption (AH+ESP)
• ESP (packet data encryption) is the most widely used, the other two are used rarely
• Can be configured to operate in two different modes:
	• Transport
	• Tunnel
• Both can be used to encrypt IPv6 traffic.
• IPv6 Node Requirements (RFC6434) states that all IPv6 nodes SHOULD support IPsec

***SHOULD - means that there may exist valid reasons in particular circumstances to ignore a particular item, but the full implications must be understood and carefully weighed before choosing a different course***

### Tunnel Mode
The original packet is wrapped, encrypted, a new IP header is added and the packet is sent to the other side of the tunne

### Transport Mode
The data of the packet is encrypted, but the header is sent in open clear text, IP header is copied to the front


## Module 5

### Dual Stack
• Fully functional IPv4 and IPv6 work side by side
• The most recommended way of implementing IPv6
• Also endorsed by RIPE

## 6to4
• Allows IPv6 packets to be transmitted over an IPv4 network
• A 6to4 relay server with native IPv6 connectivity needs to be configured on the other end
• Intended only as a transition mechanism, not as a permanent solution
• IPv6 packets are encapsulated in IPv4 packets
• Delivered to a 6to4 relay via IPv4 network
• Decapsulated and sent forward as IPv6 packets


• Ready to use services offer 6to4 tunnels free of charge
• E.g. Hurricane Electric, SixXS
• Can setup your own
• Hurricane Electric (tunnelbroker.net) provides a 6to4 service with ready to use configuration for RouterOS
• Additional information how to get IPv6 connectivity can be found on wiki.mikrotik.com
• RouterOS 6to4 interface is used to set up the tunnel
• Local and remote public IPv4 addresses have to be entered
• 6to4 uses encapsulation, the MTU has to be changed to a smaller one

### 6RD
• IPv6 Rapid Deployment is 6to4 derivative
• IPv6 relay is controlled by your ISP
• From client to ISP is IPv4 network only
• On the client side additional software is needed to encapsulate IPv6 into IPv4 packets
• Described in RFC5569

### Teredo
• Teredo encapsulates IPv6 traffic into IPv4 DP packets
• The traffic is sent through IPv4 Internet
• Unlike 6to4, Teredo works behind an IPv4 NAT
• Uses Teredo prefix ``2001::/32``
• Can only provide a single IPv6 address per tunnel endpoint
• Cannot be used to distribute addresses to multiple hosts like 6to4
• Developed by Microsoft
• Described in RFC4380

### DS-lite
• Dual stack lite
• IPv6 only links are used between the ISP and the client
• Client has native IPv6 connectivity
• When and IPv4 packet needs to be sent, it is encapsulated into an IPv6 packet

• Sent to the ISP’s NAT box which decapsulates and forwards it as IPv4 traffic
• NAT is centralized at the ISP level
• Clients use private IPv4 addresses (e.g.10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)
• ISP → Client network is IPv6 only

## Module 6

### DHCPv6 PD Client
• For acquiring IPv6 prefix from a DHCPv6 PD server
• PD client sets route to the DHCPv6 PD server
• Afterwards the router can subdivide the acquired prefix and hand out to it’s clients
• DHCPv6 PD (prefix delegation)
• It is used to assign prefixes to network hosts (e.g. routers)
• To configure - enable “Other Configuration” in IPv6 → ND **for Mikrotik Os**
• For acquiring IPv6 address from a DHCPv6 server
• Client can set default route to the DHCPv6 server
• Acquires DNS, NTP and other information

### DHCP unique identifier
• DHCP unique identifier (DUID). Each DHCP client and server has exactly one DUID
• DHCP servers use DUIDs to identify clients for the selection of configuration parameters
• DHCP clients use DUIDs to identify a server in messages where a server needs to be identified.

### IPv6 Tunnels
• Currently RouterOS supports following
- IPv6 tunnels
	• IPIPv6
	• EoIPv6
	• GRE6
• Work in a similar way as IPv4 counterparts

### IP Version Agnostic
• IP → DNS supports both IPv4 and IPv6 addresses
• Both for DNS servers and static entries

### IPv6 Reverse DNS
• Entry consists or 32 values separated by dots
• Zeros are not omitted
• ip6.arpa. is added at the end

| AAAA  | 	  2001:db8:3:4:5:6:7:8						     |
| -----	|----------------------------------------------------------------------------|
|  PTR	|  8.0.0.0.7.0.0.0.6.0.0.0.5.0.0.0.4.0.0.0.3.0.0.0.8.b.d.0.1.0.0.2.ip6.arpa. |


### NTP 
• NTP client supports both IPv4 and IPv6 addresses

### PPP IPv6 Support
• PPP supports prefix delegation (PD) to PPP clients
• Use PPP Profile DHCPv6 PD Pool option to specify pools that will be assigned to clients
• If a RouterOS device is a client, a DHCPv6 PD client must be configured on PPP client interface

### Routing
• IPv6 global routing works similar as in IPv4
• Concepts are the same
• Static and/or dynamic routing can be used
• Dynamic routing protocols such as OSPF (v3), RIP (ng), BGP support IPv6
• IPv6 link-local addresses can be used to communicate between hosts
• There’s no need for global IPv6 addresses
• Fully functional internal IPv6 network can be created with LL addresses

### IPv6 NAT
• NAT was originally used for ease of rerouting traffic in IP networks without renumbering every host
• It has become a popular tool in conserving global IPv4 addresses
• There are 2 IPv6 addresses vs 2 IPv4 
• Each IPv6 enabled host can have a global IPv6 address
• In most common cases there’s usually no need for IPv6 NAT
• NAT is not a security feature, firewall is needed also for IPv4
• Companies can apply for Provider Independent (PI) address space
• In case a provider has to be changed, IP’s can remain the same


