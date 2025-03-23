# Local Website Hosting with HTTPS & Auto-Redirect (Ubuntu + Apache)

## **🚀 Objective**
This project demonstrates how to host a website locally on Ubuntu using Apache, enable HTTPS, set up auto-redirect from HTTP to HTTPS, and configure it as a reboot-persistent service. The website is accessible via a local domain name (`segroup2.com.cadt..`).

## **🔧 Prerequisites**
- Ubuntu (or any Linux-based OS).
- Apache installed.
- Git installed.
- SSL certificate generation.

## **📋 Steps**

### **1️⃣ Install Apache & Required Packages**
First, update your system and install Apache and OpenSSL:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apache2 openssl -y
