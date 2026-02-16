# PrestaShop LAMP Stack Setup & Security Testing

## Overview

This project summarizes the complete setup, configuration, troubleshooting, and security testing process for deploying PrestaShop on a LAMP stack before and after security configuration.

### Components Used

- Apache HTTP Server
- MariaDB
- PHP
- PrestaShop
- ModSecurity
- OWASP Core Rule Set (CRS)
- OWASP ZAP

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
---
## 2. MariaDB Installation & Configuration
```
sudo apt install mariadb-server mariadb-client
sudo mysql_secure_installation
```
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
Review alerts and findings

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
---
## 6. Security Validation (OWASP ZAP)
**After enabling Modsecurity for apache, run Owasp zap again to check for vulnerabilities**

***screenshot after***


## Final Results

- **SQL Injection** `Blocked`
- **Login Functionality** `Working`
- **Registration** `Working`
- **WAF Engine** `Active`
- **Audit Logging** `Enabled`

---
## Conclusion
The PrestaShop application was successfully hardened using ModSecurity and OWASP CRS.
Malicious requests are blocked while legitimate users can authenticate without disruption. The system demonstrates proper WAF implementation, tuning, and validation practices.












