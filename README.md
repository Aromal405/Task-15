
# Task15: DVWA SQL Injection Labs

## Introduction

This repository contains a detailed report on the installation of the Damn Vulnerable Web Application (DVWA) and the exploitation of its SQL Injection vulnerabilities at low, medium, and high levels. DVWA is an intentionally vulnerable web application designed for security professionals to practice their skills.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [SQL Injection Labs](#sql-injection-labs)
  - [Low-Level SQL Injection](#low-level-sql-injection)
  - [Medium-Level SQL Injection](#medium-level-sql-injection)
  - [High-Level SQL Injection](#high-level-sql-injection)
- [Conclusion](#conclusion)
- [Next Steps](#next-steps)

## Prerequisites

Before installing DVWA, ensure that you have the following software installed on your machine:

- **Web Server**: Apache
- **Database**: MySQL (or MariaDB)
- **PHP**: Version 7.x or later

## Installation

Follow these steps to set up DVWA:

1. **Install Apache, MySQL, and PHP**:
   ```bash
   sudo apt update
   sudo apt install apache2 mariadb-server php php-mysqli php-gd php-xml php-mbstring
   ```

2. **Download DVWA**:
   ```bash
   cd /var/www/html
   git clone https://github.com/digininja/DVWA.git
   ```

3. **Set Permissions**:
   ```bash
   sudo chown -R www-data:www-data /var/www/html/DVWA
   sudo chmod -R 755 /var/www/html/DVWA
   ```

4. **Configure Database**:
   - Start MySQL:
     ```bash
     sudo systemctl start mariadb
     ```
   - Secure the installation:
     ```bash
     sudo mysql_secure_installation
     ```
   - Create a DVWA database:
     ```bash
     mysql -u root -p
     CREATE DATABASE dvwa;
     CREATE USER 'dvwa'@'localhost' IDENTIFIED BY 'password';
     GRANT ALL PRIVILEGES ON dvwa.* TO 'dvwa'@'localhost';
     FLUSH PRIVILEGES;
     EXIT;
     ```

5. **Configure DVWA**:
   - Copy the configuration file:
     ```bash
     cp /var/www/html/DVWA/config/config.inc.php.dist /var/www/html/DVWA/config/config.inc.php
     ```
   - Edit `config.inc.php` to set the database credentials:
     ```php
     $_DVWA[ 'db_user' ] = 'dvwa';
     $_DVWA[ 'db_password' ] = 'password';
     ```

6. **Access DVWA**:
   - Open your browser and navigate to `http://localhost/DVWA/setup.php`.
   - Click on "Create / Reset Database".

7. **Set Security Level**:
   - Log in with username `admin` and password `password`.
   - Set the security level in the DVWA interface.

## SQL Injection Labs

### Low-Level SQL Injection

- **Objective**: Retrieve user information using a vulnerable input.
- **Steps**:
  1. Input a single quote (`'`) to test for errors.
  2. Use the payload: `1' OR '1'='1`.
  3. This should allow access to user data.

### Medium-Level SQL Injection

- **Objective**: Use a time-based attack to retrieve data.
- **Steps**:
  1. Input payload: `1' AND SLEEP(5) AND '1'='1`.
  2. If the application pauses, it confirms the vulnerability.

### High-Level SQL Injection

- **Objective**: Bypass authentication.
- **Steps**:
  1. Use payload: `admin' --` in the login form.
  2. This bypasses the password check and logs in as admin.

## Conclusion

The installation and exploitation of DVWA's SQL Injection vulnerabilities highlight the critical need for secure coding practices in web development. Understanding these vulnerabilities is essential for protecting applications against SQL Injection attacks.

