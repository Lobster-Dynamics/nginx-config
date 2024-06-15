# Nginx config for serving the backend of the app
The server is running on ubuntu 22.04 arm64 on oracle cloud

## Installation
1. Install Nginx
```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
```

2. Check if nginx was installed successfully
```bash
sudo nginx -v
```

3. Start Nginx
```bash
sudo systemctl start nginx.service
```

4. Check if Nginx is running
```bash
sudo systemctl status nginx
```

5. Add IP tables to allow traffic on port 80 and 443
```bash
sudo iptables -I INPUT -p tcp --dport 80 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -I OUTPUT -p tcp --sport 80 -m conntrack --ctstate ESTABLISHED -j ACCEPT

sudo iptables -I INPUT -p tcp --dport 443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -I OUTPUT -p tcp --sport 443 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```

6. Install Nginx server string in the HTTP header

Note: Tested in Ubuntu 22.04, not sure if it's available in other distros
```bash
sudo apt install libnginx-mod-http-headers-more-filter
```

7. Copy nginx-config
```bash
sudo rm -rf /etc/nginx/*
sudo cp -r /home/ubuntu/nginx-config/* /etc/nginx/
```

8. Restart Nginx
```bash
sudo systemctl restart nginx
```

9. Check if Nginx is running
```bash
sudo systemctl status nginx
```

10. Add sites-available to sites-enabled
```bash
sudo mkdir /etc/nginx/sites-enabled
sudo ln -s /etc/nginx/sites-available/* /etc/nginx/sites-enabled
```

11. Install Snapd
```bash
sudo apt install snapd
```

12. Remove certbot-auto and any Certbot OS packages
```bash
sudo apt-get remove certbot
sudo dnf remove certbot
sudo yum remove certbot
```

13. Install Certbot
```bash
sudo snap install --classic certbot
```

14. Prepare the Certbot command
```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

15. Run Certbot
```bash
sudo certbot --nginx
```

16. Test the renewal process
```bash
sudo certbot renew --dry-run
```

17. Install custom error pages
```bash
git clone https://github.com/alexphelps/server-error-pages.git custom_error_pages
sudo cp custom_error_pages/_site/*.html /usr/share/nginx/html/
rm -rf custom_error_pages
```
