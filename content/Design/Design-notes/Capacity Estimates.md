---
Creation Time: Thursday, July 4th 2024
Modified Time: Saturday, February 15th 2025
---
2^2 = 4
2^3 = 8
2^4 = 16
2^5= 32
2^6= 64
2^10= 1024
2^11=2048
2^8= 256
2^9=512
2^30=1,073,741,824 =**1 billion, 73 million, 741 thousand, 824**.   **1 gigabyte (GB)** in computing terms, where 1 GB=2^30
2^64 = 18,446,744,073,709,551,616 : **18 quintillion, 446 quadrillion, 744 trillion, 73 billion, 709 million, 551 thousand, 616**.
2^32=4,294,967,296 = **4 billion, 294 million, 967 thousand, 296**.




1 Million = 10^6
1 Billion = 10^9
1 Trillion = 10^12


1B = 8 bits
1KB = 1024 B
1mB = 1024 KB
1 GB = 1024 mB

1 Char=8 bits=1 byte {-128 to 127} {-2^7+1 to 2^7}

Type Name | 32–bit Size | 64–bit Size
:-- | :--: | :--
char | 1 byte | 1 byte
short | 2 byte | 2 byte
int | 4 byte | 4 byte
long | 4 byte | 8 byte









1 Million/ Day = 12 Sec
10 Million/ Day = 120 Sec
1 Billion / Day = 720 Sec
1 Million / Day = 720 Min

**Always trying Rounding Off in power of 10 or 2**


### Availability Estimates:
#### 99.9% availability:
**Total time in a year** = 365 days = 365 × 24 hours = **8,760 hours**
**99.9% uptime** = 0.999 × 8,760 hours = **8,751.24 hours of uptime**
**Allowable downtime** = 8,760 - 8,751.24 = **8.76 hours**
Thus, **99.9% availability** allows for approximately **8 hours and 45 minutes of downtime per year**.



1000 MB * 1M = 1TB



Hexadecimal is a [number system](https://www.geeksforgeeks.org/number-system-in-maths/) combining "hexa" for 6 and "deci" for 10. It uses 16 digits: 0 to 9 and A to F,where A stands for 10, B for 11, and so on.
example, in the number 7B3, 7 is multiplied by 16 squared, B by 16 to the power of 1, and 3 by 16 to the power of 0.
(A7B)16 = A × 162 + 7 × 161 + B × 160(convert symbols A and B to their decimal equivalents; A = 10, B = 11)
herefore, the decimal equivalent of (A7B)16 is (2683)10.




64 Bit === 2^64 = 18,446,744,073,709,551,616  === to Million == 1.8447×1013 million
32 bit == 2 Billion singed, 4 Billion unsigned



Web socket average concurrent connection: 50K
Application server concurrent connection: 10K
DB server: 
Kafka message standard size: 1MB
1 Kafka broker can store around 1TB of data and handle around 10K messages per second.

`Contrary to a common misconception, the capacity isn’t limited to 65,535 connections. That number refers to the range of port numbers, not the number of connections a single server port can handle. Each TCP connection is identified by a unique combination of source IP, source port, destination IP, and destination port. With proper OS tuning and resource allocation, a single listening port can handle hundreds of thousands or even millions of concurrent SSE connections.

In practice, however, the server’s hardware and OS limits—rather than the theoretical port limit—determine the maximum number of simultaneous connections.


`

Spring boot tomcat container : 200 threads