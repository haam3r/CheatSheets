# Inital configurations

## System hardening

### Login.defs

In `/etc/login.defs` set options:

```bash
`LOG_OK_LOGINS  yes`
`UMASK  027`
```

With these we start logging succesful authentications as well and set the UMASK value to be more secure than the legacy default.


### Fstab

In `/etc/fstab` add line:

```bash
tmpfs     /run/shm     tmpfs     defaults,noexec,nosuid     0     0
```

This line helps secure shared memory.

### Sysctl

In `/etc/sysctl.conf` set options:

```bash
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
# Ignore send redirects
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0
# Block SYN attacks
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_max_syn_backlog = 2048
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syn_retries = 5
# Log Martians
net.ipv4.conf.all.log_martians = 1
net.ipv4.icmp_ignore_bogus_error_responses = 1
# Ignore ICMP redirects
net.ipv4.conf.all.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
net.ipv4.conf.default.accept_redirects = 0 
net.ipv6.conf.default.accept_redirects = 0
# Ignore Directed pings
net.ipv4.icmp_echo_ignore_all = 1
```

Reload sysctl with changes: `sudo sysctl -p`

### Host.conf

In `/etc/host.conf` set options:

```bash
order bind,hosts
nospoof on
```

## SSH

SSH server and client configs handled by salt.
Take a look at: <https://stribika.github.io/2015/01/04/secure-secure-shell.html>

## IPTables and Fail2Ban

Handled by salt.

----------

## Sources

[How to secure an Ubuntu 16.04 LTS server - Part 1 The Basics][1]

[1]: https://www.thefanclub.co.za/how-to/how-secure-ubuntu-1604-lts-server-part-1-basics
