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

# Install UFW (If not pre-installed)
Hostinger already includes UFW by default.
```
sudo apt install ufw
```
#  Check UFW Status
```
sudo ufw status
```

# Default Firewall Rules
Deny all incoming requests:
```
sudo ufw default deny incoming
```
Allow all outgoing requests:
```
sudo ufw default allow outgoing
```
# Allow SSH Connection (IMPORTANT)
```
sudo ufw allow OpenSSH
```
If you changed your SSH port, allow it explicitly:
```
sudo ufw allow <port-number>
```
# View Configured Rules
```
sudo ufw show added
```
# Enable Firewall
```
sudo ufw enable
```
# Allow HTTP and HTTPS Traffic
```
sudo ufw allow http        # or sudo ufw allow 80/tcp
sudo ufw allow https       # or sudo ufw allow 443/tcp
```
