# MikroTik-DMZ-Security
This project will be about having a DMZ hosted on a Mikrotik with a home connection provided by an ISP.
The advantages of doing it this way are presented: 

on the MikroTik router. Here I explain why and in which scenarios it is especially useful:

## Better Traffic Control:
- Having the DMZ IP directly on the MikroTik router allows you to apply firewall, NAT and mangle rules more efficiently.
- You can segment and control traffic between the LAN, WAN and DMZ directly on a single device.

## Latency Reduction:
- By handling routing directly from the MikroTik, you avoid unnecessary network hops, which reduces latency and improves service performance.

## Better Security Implementation:
- Traffic filtering rules (IP firewall filter) are more effective on the main router.
- It allows strategies such as port knocking, limiting connections per second and DDoS protection to be implemented on the same device.

## Simplicity in management:- 
- You centralize network configurations, preventing other devices (such as additional switches or firewalls) from having to manage additional routes.
- Facilitates remote access and VPN configurations directly on the MikroTik.
