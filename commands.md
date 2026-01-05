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

4. Update Ubuntu Packages.
```bash
sudo apt update && sudo apt upgrade -y
```

5. Install Nginx Web Server
```bash
sudo apt install nginx -y
```
```bash
sudo systemctl start nginx
```
```bash
sudo systemctl enable nginx
```

6. Verify Nginx Service Status.
```bash
sudo systemctl status nginx
```

**Notes**
- You start in the /home/ubuntu directory after connecting via SSH.
- Directory changes are not required for installing software.
- Never upload your .pem key to GitHub.


# Linux Web Server Project - Step 2

## Goal
Allow HTTP traffic to the EC2 instance and verify the Nginx web server is accessible form a web browser.

---

## Steps to Configure Firewall and Test Nginx

1. Check UFW (Uncomplicated Firewall) status.
```bash
sudo ufw status
```
* By default, UFW may be inactive on Ubuntu.
* We will explicitly allow web traffic (port 80).

2. Allow SSH Traffic (So you dont lock yourself out).
```bash
sudo ufw allow ssh
```

3. Allow HTTP traffic (port 80).
```bash
sudo ufw allow http
```
* This allows inbound web traffic required for Nginx.
  
4. Enable the UFW firewall.
```bash
sudo ufw enable
```
* Press y when prompted to continue.
* This activates your firewall rules.
  
5. Verify firewall rules 
```bash
sudo ufw status
```
* you should see something like.
  *80/tcp ALLOW Anywhere.
  *22/tcp ALLOW Anywhere.
  
6. Verify AWS security group allows HTTP.
* Go to AWS EC2 -> Security Groups.
* Edit inbound rules.
* Ensure HTTP (port 80) is allowed from 0.0.0.0/0.
  
7. Test Nginx in a web browser.
* Copy your EC2 Public IPv4 address.
* Open a web browser.
* Navigate to:
```
http://<instance-public-ipv4>
```
*you should see the default Nginx welcome page.

**Notes**
- Port 22 (SSH) allows remote access).
- Port 80 (HTTP) allows web traffic.
- Both AWS security groups and UFW must allow traffic.
- If the page does not load:
  - Confirm Nginx isn't running (sudo systemctl status nginx).
  - Verify port 80 is allowed in AWS and UFW.


# Linux Web Server Project - Step 3

##Goal
Deploy a custom webpage on the EC2 Nginx web server and verify it is accessible in a browser.

--- 

## Steps to Deploy a Custom Web page

1. Navigate to the Nginx web directory.
```bash
cd /var/www/html
```
* This is where Nginx serves web pages from.
* The default page index.nginx-debian.html lives here.

2. Create a new index.html file.
```bash
sudo nano index.html
```
* This will open the Nano text editor.
* You will create your custom webpage.

3. Add basic HTML content
* Paste following into Nano:
```html
<!DOCTYPE html>
<html>
<html>
  <title>My Linux Web Server</title>
</head>
<body>
  <h1>Hello from my linux Web Server!</h1>
  <p>This is my first deployed webpage using Nginx.</p>
</body>
</html>
```
* h1 is the main heading.
* p is a paragraph.
* you can change the text if you want.

4. Save and exit Nano.
   * Press CTRL + 0 -> Enter -> Saves the file.
   * Press CTRL + X -> exits Nano.

5. Verify the Webpage in a browser.

* Open a web Browser.
* Navigate to:
```
http://<instance-public-ipv4>
```
* you should see your custom webpage with the heading and paragraph.
* If you dont see any changes try refreshing your browser.

6. Restart Nginx (if necessary).
```bash
sudo systemctl restart nginx
```
* Usually optional, but ensures the new page is served.

**Notes** 
- The default Nginx page will be replaced by your index.html.
- This is a static HTML, no programming required.
- Your webpage is served via port 80 - make sure firewall and AWS security group are still allowing HTTP traffic.


# Linux Web Server Project - Step 4

##Goal
Perform final validation, basic security hardening, and confirm the web server is production-ready.

--- 

## Steps to finalize the project

1. Verify Nginx service is running
```bash
sudo systemctl status nginx
```
* Confirms the web server is active and enabled.

2. Verify firewall status
```bash
sudo ufw status
```
* Ensure SSH (22) and HTTP (80) are allowed.
* Confirms firewall is enabled and functioning.

3. Check listening ports
```bash
sudo ss -tuln
```
* Verifies the server is listening on:
  * Port 22 (SSH).
  * Port 80 (HTTP).

4. Confirm webpage is accessible
* Open a web browser.
* Navigate to:
```
http://<instance-public-ipv4>
```
* Confirm the custom webpage loads succesfully.

5. Review system uptime and disk usage
```bash
uptime
```
```bash
df -h
```
* Confirms system stability and available storage.

6. Optional: Remove default Nginx page
```bash
sudo rm /var/www/html/index.nginx-debian.html
```
* Prevents serving unused default Nginx page.

**Final Notes**
- This project demonstrates:
  - Linux system administration.
  - Secure remote access (SSH).
  - Firewall configuration (UFW + AWS Security Groups).
  - Web server deployment (Nginx).
- All steps are documented for repeatability.
- No sensitive information is stored in the repository.

  

