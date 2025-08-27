## Day 15 – Setup SSL for Nginx 🔐🌐

### 🔎 What is SSL?
**SSL (Secure Sockets Layer)** ensures **encrypted communication** between the web server and clients (HTTPS).  
With Nginx, SSL/TLS certificates can be set up using **self-signed certificates** (for testing) or **Let's Encrypt** (for production).

---

## 🐧 Install Nginx

### On Ubuntu/Debian:
```bash
sudo apt update -y
sudo apt install nginx -y
```
### On CentOS/RHEL:
```bash
sudo yum install epel-release -y
sudo yum install nginx -y
```
##### 📝 Generate a Self-Signed SSL Certificate
```bash
sudo mkdir -p /etc/nginx/ssl
cd /etc/nginx/ssl

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout nginx-selfsigned.key \
  -out nginx-selfsigned.crt
```
👉 You’ll be asked details like Country, State, Domain Name.
For Common Name (CN), enter your server’s domain name or IP.

#### ⚙️ Configure Nginx for SSL
```bash
Edit the default site configuration:

sudo vi /etc/nginx/sites-available/default    # Ubuntu/Debian
sudo vi /etc/nginx/conf.d/default.conf        # CentOS/RHEL
```
##### Update server block:
```bash
server {
    listen 443 ssl;
    server_name your_domain_or_ip;

    ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;

    location / {
        root /var/www/html;
        index index.html;
    }
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name your_domain_or_ip;
    return 301 https://$host$request_uri;
}
```
##### 🚀 Restart Nginx
```bash
sudo systemctl restart nginx
sudo systemctl enable nginx
```
##### ✅ Test SSL

Open browser and visit:
```bash
https://your_server_ip
```
You should see your Nginx welcome page with HTTPS 🔒 (self-signed certs may show a security warning).

### 📌 Summary

###### Install Nginx (apt or yum)

###### Generate SSL certificate with OpenSSL

###### Configure Nginx server block for SSL

###### Restart Nginx & access via https://

👉 For production, use Let’s Encrypt + Certbot to get a free trusted SSL certificate. Or use paid certificates.
