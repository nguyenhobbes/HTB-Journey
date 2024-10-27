# 1. Firewall and IDS/IPS Evasion - Medium Lab: 
After the configurations are transferred to the system, our client wants to know if it is possible to find out our target's DNS server version. Submit the DNS server version of the target as the answer. 
Nmap scan result in port 53 being filtered. We will use the command `--source-port 53` to request the version since it usually doesn't filter request from DNS server.

# 2. Firewall and IDS/IPS Evasion - Hard Lab
Regular nmap scan doesn't show port 50000, which could possibly blocked by a firewall. hen we scan using the port 53, we saw it.
Trying to connect to it by `ncat` / `nc`
```
sudo ncat -nv --source-port 53 10.129.50.108 50000
sudo nc -p 53 10.129.50.108 50000
```
