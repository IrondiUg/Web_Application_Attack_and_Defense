# PrestaShop LAMP Stack Setup & Security Testing

## Overview

This project summarizes the complete setup, configuration, troubleshooting, and security testing process for deploying PrestaShop on a LAMP stack.

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

### Install Apache

```
sudo apt update
sudo apt install apache2 PrestaShop LAMP Stack Setup & Security Testing
```
### Enable & Start Apache
```
sudo systemctl enable apache2
sudo systemctl start apache2
sudo systemctl status apache2
```
### Enable Required Modules
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
