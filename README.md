# n8n-Setup-VPS Step by Step
This guide walks you through setting up n8n using Docker on a Linux VPS, with firewall protection and domain setup

### Update and Upgrade Your VPS
Update the package list:
```
sudo apt update
```
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
---
# Set Up a Firewall with UFW (Uncomplicated Firewall)

A firewall helps protect your server by controlling network traffic.

### Install UFW (If not pre-installed)
Hostinger already includes UFW by default.
```
sudo apt install ufw
```
###  Check UFW Status
```
sudo ufw status
```

### Default Firewall Rules
Deny all incoming requests:
```
sudo ufw default deny incoming
```
Allow all outgoing requests:
```
sudo ufw default allow outgoing
```
### Allow SSH Connection (IMPORTANT)
```
sudo ufw allow OpenSSH
```
If you changed your SSH port, allow it explicitly:
```
sudo ufw allow <port-number>
```
### View Configured Rules
```
sudo ufw show added
```
### Enable Firewall
```
sudo ufw enable
```
### Allow HTTP and HTTPS Traffic
```
sudo ufw allow http        # or sudo ufw allow 80/tcp
sudo ufw allow https       # or sudo ufw allow 443/tcp
```
---
# Install Docker
Official guide: Docker Engine Install for Ubuntu

Add Docker’s Official GPG Key and Repo
```
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

```
### Install Docker Engine
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### Verify installation
```
sudo docker run hello-world
```
### Check version
```
docker --version
```
# Set Up Domain
Buy a domain from any registrar.
Add A Records in your DNS settings:
### Example for n8n.codeproudly.com
```
A Record (Main) 
Name: n8n 
Value: Your VPS IP 
TTL: 60

---

A Record (www subdomain) 
Name: www.n8n 
Value: Your VPS IP 
TTL: 60  
Verify DNS: 
nslookup n8n.codeproudly.com 
```

### Clone n8n Starter
# Install git:
```
sudo apt install git
```
# Clone repo:
```
git clone https://github.com/anirudhadamateg-25/n8n-setup-vps.git
```
```
cd n8n_starter
```
Edit .env file with your domain and settings.

---

## Run n8n with Docker Compose
```
docker compose up -d
```
This sets up n8n on localhost:5678.

---

# Reverse Proxy with Caddy
Visit: Caddy Install Docs

### For Ubuntu
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update

sudo apt install caddy
```

### Start and enable Caddy:
```
sudo systemctl enable caddy

sudo systemctl start caddy
```

### Configure Caddy
Edit the Caddyfile:
```
cd /etc/caddy
sudo nano Caddyfile
```

### Paste and modify the following:
```
n8n.thapatechnical.in {
    reverse_proxy localhost:5678
}

www.n8n.thapatechnical.in {
    redir https://n8n.thapatechnical.in{uri}
}

``` 
Save and exit (Ctrl + O, Enter, Ctrl + X)

### Restart Caddy:
```
sudo systemctl restart caddy
```

### Access Your n8n Instance
Visit: https://codeproudly.com in your browser — your instance is now live! 🎉

