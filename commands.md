# Linux Web Server Project - Step 0

## Goal
Document the creation of an AWS EC2 Ubuntu instance to use for the Linux Web Server Project.

---

## Steps to Launch EC2 Instance

1. Go to AWS EC2 -> Launch instance

2. Select **Ubuntu Server 24.04 LTS**
 
5. Choose instance type: **t3.micro** (free tier)
   
6. Create or select key pair
   - Name: 'linux-web-server-project'
   - Key type: **RSA**
   - Private key file format: '.pem' (for Windows 10 and above)
   - Download the '.pem' file and keep it safe
  
7. Configure instance settings:
   - Default VPC/subnet
   - Auto-assign public IP: Yes
  
8. Configure Security Group:
   - Allow SSH (port 22) from anywhere (0.0.0.0/0)
  
9. Add storage: 15Gib gp3
    
11. Launch instance

**Notes** 
- This instance willl be used to practice Linux commands and deploy a web server.
- **Do not commit your '.pem' key to GitHub** keep it private.


# Linux Web Server Project - Step 1

## Goal
Connect to the EC2 Ubuntu instance, update system packages, and install the Nginx web server.

---

## Steps to Connect to EC2 instance via SSH (Windows Powershell)

1. Open up **Windows Powershell**
2. Navigate to the directory where the '.pem' key file is stored:
   
```powershell
cd C:\Users\<YourUsername>\Desktop
```

3. Connect to the EC2 instance using SSH:
```bash
ssh -i linux-web-server-project.pem ubuntu@<instance-public-ipv4>
```

5. Update Ubuntu Packages
```bash
sudo apt update && sudo apt upgrade -y
```

6. Install Nginx Web Server
```bash
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

8. Verify Nginx Service Status
```bash
sudo systemctl status nginx
```

**Notes**
- You start in the /home/ubuntu directory after connecting via SSH
- Directory changes are not required for installing software
- Never upload your .pem key to GitHub


# Linux Web Server Project - Step 2

## Goal
Allow HTTP traffic to the EC2 instance and verify the Nginx web server is accessible form a web browser.

---

## Steps to Configure Firewall and Test Nginx

1. Check UFW (Uncomplicated Firewall) status
```bash
sudo ufw status
```
* By default, UFW may be inactive on Ubuntu
* We will explicitly allow web traffic (port 80)

2. Allow SSH Traffic (So you dont lock yourself out)
```bash
sudo ufw allow ssh
```

3. Allow HTTP traffic (port 80)
```bash
sudo ufw allow http
```
* This allows inbound web traffic required for Nginx
  
4. Enable the UFW firewall
```bash
sudo ufw allow http
```
* Press y when prompted to continue
* This activates your firewall rules
  
5. Verify firewall rules 
```bash
sudo ufw status
```
* you should see something like
  *80/tcp ALLOW Anywhere
  *22/tcp ALLOW Anywhere
  
6. Verify AWS security group allows HTTP
* Go to AWS EC2 -> Security Groups
* Edit inbound rules
* Ensure HTTP (port 80) is allowed from 0.0.0.0/0
  
7. Test Nginx in a web browser
* Copy your EC2 Public IPv4 address
* Open a web browser
* Navigate to:
```
http://<instance-public-ipv4>
```
*you should see the default Nginx welcome page

**Notes**
- Port 22 (SSH) allows remote access)
- Port 80 (HTTP) allows web traffic
- Both AWS security groups and UFW must allow traffic
- If the page does not load:
  - Confirm Nginx isn running (sudo systemctl status nginx)
  - Verify port 80 is allowed in AWS and UFW

