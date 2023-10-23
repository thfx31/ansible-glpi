# Deploy GLPI with Ansible

install_glpi.yml playbook will install and configure :
- apache2 (SSL,UFW and fail2ban)
- mariadb-server
- glpi 10

Once MariaDB and Apache2 are up and running, this role performs the following actions:
Download GLPI 10
Create a system user and configure the database for the WebApp
Configure and enables a Website in Apache2 to access GLPI
Configure HTTPS and generate self-signed certificates 


## Requirements
None

## Tested plateform
- Ubuntu 22.04
- Debian 12
- Debian 11

## Role variables
Multiple variables are necessary in order to properly configure GLPI.

```
extra_packages:
- php-curl
- php-gd
- php-mbstring
- php-intl
- php-xml
- php-ldap
- php-zip
- php-bz2
- python3-cryptography

# Apache2 vars
http_host: glpi.deploy
http_conf: 001-glpi.conf
http_port: 80
https_port: 443

# SSL vars
ssl_csr_directory: /etc/ssl/csr
ssl_csr_email: youremail
ssl_crt_directory: /etc/ssl/certs
ssl_privkey_directory: /etc/ssl/private

# Database vars
glpi_db_host: "localhost"
glpi_db_port: "3306"
glpi_db_root_password: "root_password"
glpi_db_name: "glpi db name"
glpi_db_user: "admin"
glpi_db_password: "db password"

# GLPI vars
glpi_install_path: /var/www/glpi
```
The variables above can be configured in group_vars. As far as the credentials are concerned, these should be kept in a separate secret vars_file encrypted with ansible-vault.

## Exemple playbook
Here is a simple example playbook to use this role:

```
---
- hosts: glpi-servers
  
  roles:
    - web
    - database
    - glpi
```
