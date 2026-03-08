# PrestaShop Stack Setup & Security Testing

## Overview

This project summarizes the complete setup, configuration, troubleshooting, and security testing process for deploying PrestaShop before and after security configuration.

NOTE: ALL COMMANDS AND INSTALLATION STEPS ARE LINUX-VASED

### Components Used

- `Apache HTTP Server`
- `MariaDB`
- `PrestaShop`
- `ModSecurity`
- `OWASP Core Rule Set (CRS)`
- `OWASP ZAP`

---

## 1. Apache Installation & Configuration

**Install Apache**

```
sudo apt update
sudo apt install apache2 PrestaShop LAMP Stack Setup & Security Testing
```
**Enable & Start Apache**
```
sudo systemctl enable apache2
sudo systemctl start apache2
sudo systemctl status apache2
```
**Enable Required Modules**
```
sudo a2enmod rewrite
sudo systemctl restart apache2
```

![Screenshot 2026-02-09 002914](https://github.com/user-attachments/assets/c110166c-05f0-422a-b986-8a7b5b8847e0)
---
## 2. MariaDB Installation & Configuration
```
sudo apt install mariadb-server mariadb-client
sudo mysql_secure_installation
```

![Screenshot 2026-02-09 004309](https://github.com/user-attachments/assets/1ce92d68-5f16-4148-82c7-8aa395023196)
---
## 3. PrestaShop Installation
**Download & Extract**
```
wget https://download.prestashop.com/download/releases/prestashop_*.zip
unzip prestashop_*.zip -d /var/www/html/prestashop
sudo chown -R www-data:www-data /var/www/html/prestashop
```
**Configure Apache Virtual Host**
```
<VirtualHost *:80>
    ServerName yourdomain.com
    DocumentRoot /var/www/html/prestashop

    <Directory /var/www/html/prestashop/>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
**Enable site**
```
sudo a2ensite prestashop.conf
sudo systemctl reload apache2
```
**Open browser, navigate to:**
```
http://localhost/prestashop
```

![Screenshot 2026-02-09 020902](https://github.com/user-attachments/assets/74ac7ac6-5289-4804-89b7-27b0674b5faf)

---

## 4. OWASP ZAP Installation & Configuration
**Install ZAP**
```
sudo apt install zaproxy
```
**Set target URL to:**
```
http://localhost/prestashop
```
**Run:**

`Spider scan`

`Active scan`

**Review alerts and findings**

![Screenshot 2026-02-09 173645](https://github.com/user-attachments/assets/5e5c419c-ecdc-4fc7-936f-a3c7e0b7e4af)

---
## 5. Install Modsecurity (For Hardening)

**Install and Enable**
```
sudo apt install libapache2-mod-security2
sudo a2enmod security2
sudo systemctl restart apache2
```
**Configure OWASP CRS**
```
sudo cp /usr/share/coreruleset/crs-setup.conf.example \
/usr/share/modsecurity-crs/crs-setup.conf
```
**Enable blocking mode in ModSecurity:**
```
SecRuleEngine On
```
**Validate configuration:**
```
sudo apachectl configtest
```
**Create exclusion file**
```
sudo nano /etc/modsecurity/prestashop-auth-exclusions.conf
```
**Add scoped rule removal**
```
<IfModule security2_module>

    <LocationMatch "/prestashop/(login|authentication|register)">
        SecRuleRemoveById 931100
        SecRuleRemoveById 920350
    </LocationMatch>

</IfModule>
```
![WAFrule](https://github.com/user-attachments/assets/86073fe9-6207-432c-a273-00345a8b4625)

---
## 6. Security Validation (OWASP ZAP)
**After enabling Modsecurity for apache, run Owasp zap again to check for vulnerabilities**

![Screenshot 2026-02-12 042639](https://github.com/user-attachments/assets/fe2b4e27-a9ca-4cac-8085-aafbda967b4e)

---
## 7. Final Results

- **SQL Injection** `Blocked`
- **Login Functionality** `Working`
- **Registration** `Working`
- **WAF Engine** `Active`
- **Audit Logging** `Enabled`

---
## Conclusion
The PrestaShop application was successfully hardened using ModSecurity and OWASP CRS.
Malicious requests are blocked while legitimate users can authenticate without disruption. The system demonstrates proper WAF implementation, tuning, and validation practices.












