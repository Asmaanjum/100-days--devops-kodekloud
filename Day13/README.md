# Day 13 : iptables installation and configuration

## task : install iptables and block incoming port 6000 on all app servers except from the load balancer (LBR) host

### step 1 : find LBR IP address
'''getent hosts stlb01'''

WHY : since ips are dynamic in kodekloud , we resolved the hostname to get the actual ip of the LBR host.
RESULT : 10.244.247.221 

### step 2 : install iptables on each app server
'''yum install -y iptables-nft'''

WHY iptables-nft : on RHEL/Rockey 9, the traditionaliptables binary is not installed by default. the iptables-nft package provides iptables command as compatibility layer over nftables
ISSUE FACED : 
-'yum install iptables-services' installed but binary was missing
-'systemctl start iptables' failed with exit code 5
-'iptables' command not found in PATH
-binary was at /usr/sbin/iptables but not in root PATH
RESULT : install iptables-nft which provides the actual binary at /usr/sbin/iptables

### step 3 : add firewall rules
'''/usr/sbin/iptables -I INPUT -p tcp --dport 6000 -s 10.244.247.221 -j ACCEPT
/usr/sbin/iptables -A INPUT -p tcp --dport 6000 -j DROP'''

WHY each flag:
|     flag                |       meaning                      |
|-------------------------|------------------------------------|
| -I INPUT                | insert rule at top of input chain  |
| -A INPUT                | Append rule at bottom              |
| -p tcp                  | protocol = TCP                     |
| --dport 6000            | destination port 6000              |
| -s <IP>                 | source IP (LBR only)               |
| -j ACCEPT               | Allow the packet                   |
| -j DROP                 | block the packet                   |

### step 4 : verify rules 
'''/usr/sbin/iptables -L INPUT -n -v'''

### step 5 : persist rules after reboot
'''/usr/sbin/iptables-save > /etc/sysconfig/iptables
systemctl enable iptables'''

WHY : without saving ,rules are lost after reboot. iptables-save writes rules to file.
systemctl enable makes iptables restore them on boot


### RESULT :
port 6000 is now blocked on all 3 app servers for everyone EXCEPT the LBR host (10.244.247.221) rules persist across reboots via /etc/sysconfig/iptables
