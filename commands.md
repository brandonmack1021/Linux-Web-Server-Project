# Linux Web Server Project - Step 0

## Goal
Document the creation of an AWS EC2 Ubuntu instance to use for the Linux Web Server Project.

---

## Steps to Launch EC2 Instance

1. Go to AWS EC2 -> Launch instance
2. Select **Ubuntu Server 24.04 LTS**
3. Choose instance type: **t3.micro** (free tier)
4. Create or select key pair
   - Name: 'linux-web-server-project'
   - Key type: **RSA**
   - Private key file format: '.pem' (for Windows 10 and above)
   - Download the '.pem' file and keep it safe
5. Configure instance settings:
   - Default VPC/subnet
   - Auto-assign public IP:Yes
6. Configure Security Group:
   - Allow SSH (port 22) from anywhere (0.0.0.0/0)
7. Add storage: 15Gib gp3
8. Launch instance

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

   

