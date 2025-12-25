# 📦 EC2 to S3 Backup Automation Using Cron Job

## 📖 Project Description
This project demonstrates how to automate **file-level backups from an AWS EC2 instance to an S3 bucket** using a **Bash script and cron job**.

The solution uses:
- **IAM Role (no access keys)**
- **AWS CLI**
- **Cron job automation**
- **Compressed, timestamped backups**
- **Amazon S3 for durable storage**

---

## 🏗️ Architecture Overview
EC2 (Ubuntu)
└── Backup Script (Bash)
└── Cron Job
└── IAM Role
└── Amazon S3


---

## 🛠️ Prerequisites
- AWS account
- IAM user with AdministratorAccess (for setup)
- Basic Linux & AWS knowledge
- Ubuntu EC2 instance

---

## 🚀 Step-by-Step Implementation

---

## 1️⃣ Create S3 Bucket
Create an S3 bucket to store backups.

- **Bucket Name:** `ec2-backup-iamnk`
- Block all public access ✅
- Enable versioning (recommended)

> No folders need to be created manually in S3.

---

## 2️⃣ Create IAM Role for EC2

### Role Details
- **Role Name:** `EC2-S3-Backup-Role`
- **Trusted Entity:** EC2

### Attach AWS Managed Policy
For simplicity and learning:
- `AmazonS3FullAccess`

> In production, replace this with a least-privilege custom policy.

---

## 3️⃣ Create EC2 Instance

- **Instance Name:** `Ec2-s3-backup`
- **AMI:** Ubuntu 22.04
- **Instance Type:** t2.micro
- **IAM Role:** Attach `EC2-S3-Backup-Role`
- Allow SSH (port 22)

---

## 4️⃣ Connect to EC2 Instance

```bash
ssh -i pemfile ubuntu@<EC2-PUBLIC-IP>


5️⃣ Create Required Directories on EC2
Data directory (source to backup)

mkdir -p /home/ubuntu/data

Create 1-2 files here the same files we are taking as a backup to the s3 bucket

Scripts directory   

mkdir -p /home/ubuntu/scripts


6️⃣ Install AWS CLI v2

sudo apt update
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
unzip awscliv2.zip
sudo ./aws/install

Verify:
aws --version
aws s3 ls

7️⃣ Create Backup Script

Create the script:

vim /home/ubuntu/scripts/backup.sh

Paste the Script over there.

Make it executable:
chmod +x /home/ubuntu/scripts/backup.sh


8️⃣ Test Backup Manually
/home/ubuntu/scripts/backup.sh


Check S3 bucket:

aws s3 ls s3://ec2-backup-iamnk

9️⃣ Automate Using Cron Job

Edit crontab:

crontab -e

select the nano editor 

paste it in crontab 

* * * * * /home/ubuntu/scripts/backup.sh
This runs the backup every minute (for testing).

Ctrl+S   	Save current file
Ctrl+X	Close buffer, exit from nano

📦 Backup Output in S3

ec2-backup-iamnk/
 ├── backup-2025-12-25_14-30-10.tar.gz
 ├── backup-2025-12-25_14-31-10.tar.gz


Each file contains:

Compressed data directory

Timestamped version

Full directory structure


