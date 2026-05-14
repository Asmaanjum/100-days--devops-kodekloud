# Day 03 - Disable Direct Root SSH Login

## server i worked on 
| server | user  |
|--------|-------|
|stapp01 | tony  |
|stapp02 |steve  |
|stapp03 |banner |

## commands used
# SSH into each server from jump host
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# change permitrootlogin to no
sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/'  /etc/ssh/sshd_config

# verify the change
sudo grep PermitRootLogin /etc/ssh/sshd_config

# restart ssh service
sudo systemctl restart sshd
