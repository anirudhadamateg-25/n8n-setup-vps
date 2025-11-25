# n8n-setup-vps Step by Step
This guide walks you through setting up n8n using Docker on a Linux VPS, with firewall protection and domain setup

# Update and Upgrade Your VPS
Update the package list:

sudo apt update
Upgrade the packages:

```
 sudo apt upgrade 
```
Check if a reboot is required:
```
cat /var/run/reboot-required
```

If required, reboot:
```
sudo reboot
```

# Set Up a Firewall with UFW (Uncomplicated Firewall)

A firewall helps protect your server by controlling network traffic.

#Install UFW (If not pre-installed)
Hostinger already includes UFW by default.
```
sudo apt install ufw
```
