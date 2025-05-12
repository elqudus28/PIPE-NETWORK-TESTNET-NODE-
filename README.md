# PIPE-NETWORK-TESTNET-NODE-SETUP
# Step-by-Step Guide to Running a Pipe Network Testnet Node

## 1. 📩 Obtain an Invite Code

To participate in the Testnet, you'll need an invite code. Request one by filling out the form here: https://airtable.com/apph9N7T0WlrPqnyc/pagSLmmUFNFbnKVZh/form

## 2. 🖥️ System Requirements

Ensure your system meets the following specifications:

CPU: 4 or more cores

RAM: 16GB or more

Storage: SSD with at least 100GB free space

Network: 1Gbps or higher connection

## Install Dependencies 

```console 

sudo apt update
sudo apt install -y libssl-dev ca-certificates

```

## 3. Optimize Network with sysctl
## -  Create the Configuration File

```console
sudo nano /etc/sysctl.d/99-popcache.conf
```
## - Paste the Following command 
 These values improve connection handling, port availability, and TCP performance:
```console
net.ipv4.ip_local_port_range = 1024 65535
net.core.somaxconn = 65535
net.ipv4.tcp_low_latency = 1
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
```
save and exit by CTRL X +Y then ENTER

## - Apply the settings

```console
sudo sysctl -p /etc/sysctl.d/99-popcache.conf
```
## - Increase file limits for performance
```console
sudo nano /etc/security/limits.d/popcache.conf
```
add the 2 lines, saveand exit with CTRL X+Y then ENTER
```console
*    hard nofile 65535
*    soft nofile 65535
```
### - Reminder: To Apply Immediately
These limits require a new login session to apply (or a reboot), unless you run it in your current terminal session:
```console
ulimit -n 65535
```




