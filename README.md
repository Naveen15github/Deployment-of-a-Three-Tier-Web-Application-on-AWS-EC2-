# Deploying a Three-Tier Web Application on AWS EC2

This guide provides a **step-by-step walkthrough** for deploying a secure and scalable three-tier architecture using AWS EC2 instances.  
*VPC creation steps are omitted—you can use the default VPC provided by AWS.*

---

## 🏗️ Overview: What is a Three-Tier Architecture?

A **three-tier architecture** splits your application into three layers:
- **Presentation Tier** (Web Server) 🌐: Handles user requests and serves static content.
- **Application Tier** (App Server) ⚙️: Processes business logic and connects web requests to the database.
- **Data Tier** (Database Server) 🗄️: Stores persistent data securely.

---

## 📝 Prerequisites

- AWS Account
- SSH key pair for EC2 access
- Basic knowledge of the AWS Console
- Source code for web, app, and database tiers

---

## 🛠️ Step-by-Step Deployment

### 1️⃣ Set Up Security Groups

- **Web Server SG:** Allow inbound HTTP (80), HTTPS (443), and SSH (22).
- **App Server SG:** Allow inbound traffic only from the Web Server SG (custom ports, e.g., 3000, 5000), and SSH (22).
- **DB Server SG:** Allow inbound traffic only from the App Server SG (DB port, e.g., 3306 for MySQL, 5432 for PostgreSQL), and SSH (22).

*Screenshot Example: Creating a Security Group*

![Create Security Group Screenshot](https://github.com/Naveen15github/Deployment-of-a-Three-Tier-Web-Application-on-AWS-EC2-/blob/eca1cd7dd9d42d3483dc1fe16d705a833de7d7c9/Screenshot%20(28).png)

---

### 2️⃣ Create EC2 Instances

Follow these steps to create an EC2 instance for any tier (Web/App/DB):

#### **Step 1: Launch EC2 Instance**
1. Go to the AWS Console → **EC2** → **Instances** → "Launch Instance".
2. Enter an instance name (e.g., `web-server`, `app-server`, `db-server`).
3. Select an Amazon Machine Image (AMI) (e.g., Ubuntu Server 22.04 LTS or Amazon Linux 2).
4. Choose an instance type (e.g., t2.micro for free tier).
5. Select an existing key pair or create a new one (**download the .pem file and keep it safe!**).
6. In the **Network settings**, select the default VPC and appropriate subnet.
7. Attach the relevant **Security Group** (see step 1).
8. Click **Launch Instance**.

#### **Step 2: Check Instance Status**
- Wait until the instance state is **"running"**.
- Copy the **Public IPv4 address** for your instance (used for SSH).

*Screenshot Example: Instance Running Status*

![Instance Running Screenshot](https://github.com/Naveen15github/Deployment-of-a-Three-Tier-Web-Application-on-AWS-EC2-/blob/254b71186353fc7d030004ccedf5b46e96f65b0b/Screenshot%20(29).png)

---

### 3️⃣ Connect to EC2 via Command Prompt (SSH)

#### **Windows**
1. Open **Command Prompt** or **PowerShell**.
2. Navigate to the folder containing your `.pem` key:
   ```sh
   cd C:\path\to\your\key\
   ```
3. Use the following command to connect:
   ```sh
   ssh -i your-key.pem ubuntu@<Public-IP>
   ```
   - Use `ec2-user@<Public-IP>` for Amazon Linux.
   - If you get a permissions error, run:
     ```sh
     icacls your-key.pem /inheritance:r
     icacls your-key.pem /grant:r "%username%:R"
     ```

#### **Mac/Linux**
1. Open **Terminal**.
2. Go to the directory with your `.pem` key:
   ```sh
   cd ~/path/to/your/key/
   chmod 400 your-key.pem
   ```
3. Connect to your instance:
   ```sh
   ssh -i your-key.pem ubuntu@<Public-IP>
   ```
   - Use `ec2-user@<Public-IP>` for Amazon Linux.

*Screenshot Example: SSH Connection from Command Prompt*

![SSH Connection Screenshot](https://github.com/Naveen15github/Deployment-of-a-Three-Tier-Web-Application-on-AWS-EC2-/blob/55c38ff189f6b13c5806905c89cdc67fbf9f0aee/Screenshot%20(30).png)

---

### 4️⃣ Initial Setup on Each Instance

**Update packages:**
- Ubuntu/Debian:
  ```sh
  sudo apt update && sudo apt upgrade -y
  ```
- Amazon Linux:
  ```sh
  sudo yum update -y
  ```


---

### 5️⃣ How to Install Node.js on EC2 via Command Prompt

**For Ubuntu/Debian:**

1. **Update package list:**
   ```sh
   sudo apt update
   ```
2. **Install Node.js and npm:**
   ```sh
   sudo apt install -y nodejs npm
   ```
3. **Check installation:**
   ```sh
   node -v
   npm -v
   ```

**For Amazon Linux 2:**

1. **Install Node.js (LTS version):**
   ```sh
   sudo yum update -y
   sudo yum install -y gcc-c++ make
   curl -sL https://rpm.nodesource.com/setup_18.x | sudo -E bash -
   sudo yum install -y nodejs
   ```
2. **Check installation:**
   ```sh
   node -v
   npm -v
   ```

---

Continue with software installation and configuration for each tier as described above.

---
## Running website
![website Screenshot](https://github.com/Naveen15github/Deployment-of-a-Three-Tier-Web-Application-on-AWS-EC2-/blob/212b23e4e44635e8da79a7ad10fa242fcacbcecb/Screenshot%20(27).png)
## 📁 Example Project Structure

```
.
├── web-server/
│   └── index.html
├── app-server/
│   └── app.js
│   └── .env
├── db-server/
│   └── db_setup.sql
├── screenshots/
│   └── create-security-group.png
│   └── launch-ec2-instance.png
│   └── instance-running-status.png
│   └── ssh-connection.png
│   └── update-packages.png
│   └── install-nodejs.png
└── README.md
```

---

## 📚 Useful Resources

- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
- [Supabase Docs](https://supabase.com/docs)

---

## 💡 Pro Tips

- Use environment variables for secrets and keys.
- Always apply the principle of least privilege on Security Groups.
- Regularly update and patch your instances.
- Clean up unused resources to avoid charges.

---

**Questions or feedback? Feel free to contribute or open an issue!** ✨
