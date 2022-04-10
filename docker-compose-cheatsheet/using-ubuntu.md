---
description: All about using
---

# 🟠 Using Ubuntu

## Initial hardening

References for some of these steps are at [this site](https://gofoss.net/server-hardening-advanced/#sysctl-network-security). It's pasted here for my convenience.

### Encrypt your hard-drive

:warning::warning::warning: **THIS IS ONLY DOABLE WHEN YOU INSTALL UBUNTU** :warning::warning::warning:

On the installation page where you have to select your partition, opt to erase the disk and then select "**Use LVM...**" option and encrypt it using Linux Unified Key System (LUKS)

![Source: https://jumpcloud.com/blog/how-to-enable-full-disk-encryption-on-an-ubuntu-20-04-desktop](<../.gitbook/assets/image (2).png>)

### Setup Antivirus

#### Install and Bootstrap ClamAV

```
# install clamav
sudo apt install clamav clamav-freshclam;

# bootstrap clamav
sudo systemctl stop clamav-freshclam;
sudo freshclam;
sudo systemctl start clamav-freshclam;

# enable clamav to run at startup
sudo systemctl enable clamav-freshclam;
```

#### Setup ClamAV to run daily

Create the log file:

```
sudo touch /var/log/clamav/clamscan.log;
```

Use `cron` to setup a daily job:

```
sudo vim /etc/cron.daily/clamav;
```

Paste in the following contents:

```
#!/bin/sh

MYLOG=/var/log/clamav/clamscan.log
echo "Scanning for viruses at `date`" >> $MYLOG
clamscan --recursive --infected --max-filesize=100M --max-scansize=100M --exclude=/boot / >> $MYLOG 2>&1
```

### Setup Firewall

#### Confirm that \`ufw\` is installed

```
ufw --version;
```

#### Setup base rules for \`ufw\`

```
sudo ufw default deny incoming;

# common ports
sudo ufw deny ssh;
sudo ufw deny ftp;
sudo ufw deny smtp;
sudo ufw deny cups;
sudo ufw deny 69;
sudo ufw deny 514;

# samba
sudo ufw deny 137;
sudo ufw deny 138;
sudo ufw deny 139;
sudo ufw deny 445;
```

#### Enable \`ufw\`

If not already done, enable `ufw`:

```
sudo ufw enable;
```

#### Enable \`ufw\` to run at startup

```
sudo systemctl enable ufw;
```

### Secure \`sysctl\`

```
# make an archive of the configuration file
sudo cp --archive /etc/sysctl.conf /etc/sysctl.conf-COPY-$(date +"%Y%m%d%H%M%S");

# edit the configuration file
sudo vim /etc/sysctl.conf;
```

Paste in the following contents:

```
# IP Spoofing protection
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.default.rp_filter = 1

# Ignore ICMP broadcast requests
net.ipv4.icmp_echo_ignore_broadcasts = 1

# Disable source packet routing
net.ipv4.conf.all.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv6.conf.default.accept_source_route = 0

# Block SYN attacks
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 2048
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syn_retries = 5

# Log Martians
net.ipv4.conf.all.log_martians = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1

# Ignore ICMP redirects
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0
net.ipv6.conf.default.accept_redirects = 0

# Ignore Directed pings
net.ipv4.icmp_echo_ignore_all = 1
```

Finally, enable it:

```
sudo sysctl -p;
```

### Secure \`/proc\`

Make a backup of the configuration file and then open it:

```
sudo cp --preserve /etc/fstab /etc/fstab-COPY-$(date +"%Y%m%d%H%M%S");

sudo vim /etc/fstab;
```

Paste the following line **at the end of** the file:

```
proc    /proc   proc    defaults,hidepid=2  0   0
```

Reload the configuration by remounting `/proc`:

```
sudo mount -o remount,hidepid=2 /proc;
```

### Secure the kernel

Backup the configuration file:

```
sudo cp --archive /etc/modprobe.d/blacklist.conf /etc/modprobe.d/blacklist.conf-COPY-$(date +"%Y%m%d%H%M%S")
```

Open the configuration file:

```
sudo vim /etc/modprobe.d/blacklist.conf
```

Add the following lines at the end:

```
# Instruct modprobe to force inactive modules to always fail loading
install cramfs /bin/false
install freevxfs /bin/false
install hfs /bin/false
install hfsplus /bin/false
install jffs2 /bin/false
install udf /bin/false
```

\


### Add some blocklists to \`/etc/hosts\` if you want

1. Checkout the repository at [https://github.com/jmdugan/blocklists](https://github.com/jmdugan/blocklists)
2. Copy and paste the blocklists as needed into your `/etc/hosts` file

## Albert Task Launcher

### Troubleshooting

<details>

<summary>Task exits after hiding launcher</summary>

Open Albert by running `albert` in the CLI.

Trigger Albert using the hotkey, and hide the application again.

The `albert` task should have exited with the following error:

```
[fatal:default] SQL ERROR: INSERT INTO execution (query_id, handler_id, runtime) VALUES (:query_id, :handler_id, :runtime); UNIQUE constraint failed: execution.query_id, execution.handler_id Unable to fetch row  --  [(null)]
```

To resolve this, run:

```
rm ~/.config/albert/core.db
```

Reference: [https://github.com/albertlauncher/albert/issues/1033](https://github.com/albertlauncher/albert/issues/1033)&#x20;

</details>
