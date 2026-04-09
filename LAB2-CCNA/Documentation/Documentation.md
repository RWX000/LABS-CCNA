# LAB 2 CCNA

## Objective
### Divide a network provided by an ISP into 8 smaller subnets
-Basic configuration on devices  
-Subnetting (FLSM)  
-Static routing  
-Remote access (SSH)  
-VLANs (Support, Administration, Sales)  
-Trunk port configuration  
-Router-on-a-Stick

## Materials / Topology
- The topology for each network will be star type  
- Routers 8  
- SW 8  
- PCs 1  
- Serial cables 4  
- HTTP servers 2  

## IP Addressing  
| Device | IP | Mask | Gateway | VLAN|
|:--------|:------:|------:|------:|-----:|
|R1          ||       |       |
|R2          ||       |       |
|R3          ||       |       |
|R4          ||       |       |
|R5          ||       |       |
|R6          ||       |       |
|R7          ||       |       |
|R8          ||       |       |
|SW1         |        |       |       |
|SW2         |        |       |       |
|SW3         |        |       |       |
|SW4         |        |       |       |
|SW5         |        |       |       |
|SW6         |        |       |       |
|SW7         |        |       |       |
|SW8         |        |       |       |
| PC1    | 192.168.50.2   | 255.255.255.0     | 192.168.50.1     | 10   |
| PC2    | 192.168.50.3   | 255.255.255.0     | 192.168.50.1     | 10   |
| PC3    | 192.168.50.35  | 255.255.255.0     | 192.168.50.1     | 20   |
| PC4    | 192.168.50.130 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC5    | 192.168.50.131 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC6    | 192.168.50.132 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC7    | 192.168.50.133 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC8    | 192.168.50.134 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC9    | 192.168.50.135 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC10   | 192.168.50.136 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC11   | 192.168.50.137 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC12   | 192.168.50.138 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC13   | 192.168.50.139 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC14   | 192.168.50.140 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC15   | 192.168.50.141 | 255.255.255.0     | 192.168.50.1     | 30   |
| PC16   | 192.168.50.142 | 255.255.255.0     | 192.168.50.1     | 30   |


## Procedure and Topology
- [ ] Place all devices  
- [ ] Perform subnetting using the method (TLSM)  
- [ ] Create VLANs  
- [ ] Configure all devices  
- [ ] Verify connectivity  

## Configuration and commands  
