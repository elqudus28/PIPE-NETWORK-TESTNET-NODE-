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
## Binary node installation
create and change directory
```console
sudo mkdir -p /opt/popcache
cd /opt/popcache
```
### Download the binary
You need the invite code sent to you in your email
https://download.pipe.network
download the binary and upload it, use MOBAXTERM to upload the binary to the directory (popcache)
## Make the binary executable
```console
chmod +x pop
```

## 4. Configuration
configure your file
```console
nano config.json
```
Insert the following configuration, adjusting values as needed: your pop-name,node name, email, your solana pubkey, discord name, TG name
Set `memory_cache_size_mb` based on your available RAM ("memory_cache_size_mb": 4096,) this stands for 4GB, to set yours 1024=1GB 
Set `disk_cache_size_gb` based on your available disk space
set `worker_CPU`  set `workers` to 0 which auto-detects the number of CPU cores
```console
{
  "pop_name": "your-pop-name",
  "pop_location": "Your Location, Country",
  "server": {
    "host": "0.0.0.0",
    "port": 443,
    "http_port": 80,
    "workers": 40
  },
  "cache_config": {
    "memory_cache_size_mb": 4096,
    "disk_cache_path": "./cache",
    "disk_cache_size_gb": 100,
    "default_ttl_seconds": 86400,
    "respect_origin_headers": true,
    "max_cacheable_size_mb": 1024
  },
  "api_endpoints": {
    "base_url": "https://dataplane.pipenetwork.com"
  },
  "identity_config": {
    "node_name": "your-node-name",
    "name": "Your Name",
    "email": "your.email@example.com",
    "website": "https://your-website.com",
    "discord": "your_discord_username",
    "telegram": "your_telegram_handle",
    "solana_pubkey": "YOUR_SOLANA_WALLET_ADDRESS_FOR_REWARDS"
  }
}
```
## 5 Create a systemd service file for automatic startup and management:
 edit the popcache service configuration
```console
sudo nano /etc/systemd/system/popcache
```
Add the following content:
```console
[Unit]

Description=POP Cache Node

After=network.target

[Service]

Type=simple

User=popcache

Group=popcache

WorkingDirectory=/opt/popcache

ExecStart=/opt/popcache/pop

Restart=always

RestartSec=5

LimitNOFILE=65535

StandardOutput=append:/opt/popcache/logs/stdout.log

StandardError=append:/opt/popcache/logs/stderr.log

Environment=POP_CONFIG_PATH=/opt/popcache/config.json

[Install]

WantedBy=multi-user.target
```
## Enable and start the services
```console
sudo systemctl daemon
sudo systemctl enable popcache
sudo systemctl start popcache
```
Check the service status
```console
sudo systemctl status
```



