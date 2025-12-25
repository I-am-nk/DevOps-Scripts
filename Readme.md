# 📦 EC2 to S3 Backup Script Automation Using Cron Job

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
```
EC2 (Ubuntu)
└── Backup Script (Bash)
└── Cron Job
└── IAM Role
└── Amazon S3

```
![Untitled design](https://github.com/user-attachments/assets/de1a56b8-4780-4223-9f9f-535335c5138f)

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

- **Bucket Name:** 
- Block all public access ✅
- Enable versioning (recommended)

> No folders need to be created manually in S3.
<img width="1912" height="968" alt="image" src="https://github.com/user-attachments/assets/3a48e6fa-2ef1-4288-9282-1f99089d34dd" />

---

## 2️⃣ Create IAM Role for EC2

### Role Details
- **Role Name:** `EC2-S3-Backup-Role`
- **Trusted Entity:** EC2

### Attach AWS Managed Policy
For simplicity and learning:
- `AmazonS3FullAccess`

> In production, replace this with a least-privilege custom policy.
<img width="1908" height="973" alt="image" src="https://github.com/user-attachments/assets/c3c13f3d-1d6f-45e5-9ae8-54ddaa5a66f8" />

---

## 3️⃣ Create EC2 Instance

- **Instance Name:** `Ec2-s3-backup`
- **AMI:** Ubuntu 22.04
- **Instance Type:** t2.micro
- **IAM Role:** Attach `EC2-S3-Backup-Role`
- Allow SSH (port 22)

---
<img width="1919" height="596" alt="image" src="https://github.com/user-attachments/assets/f8914345-c016-4fa0-b5d4-6cacaf01d21d" />

## 4️⃣ Connect to EC2 Instance

SSH into the instance:

```
ssh -i my-key.pem ubuntu@<public IP>
```

## 5️⃣ Create Required Directories on EC2
### Data directory (source to back up)

```
mkdir -p /home/ubuntu/data
```
Create 1–2 files inside this directory — these are the files that will be backed up to the S3 bucket.

### Scripts directory

```
mkdir -p /home/ubuntu/scripts
```


---

## 6️⃣ Install AWS CLI v2

```
sudo apt update
sudo apt install unzip -y

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
unzip awscliv2.zip
sudo ./aws/install
```

Verify installation:

```
aws --version
aws s3 ls
```

## 7️⃣ Create Backup Script

Create the script file:

```
vim /home/ubuntu/scripts/backup.sh
```

👉 Use the Script which is present in this repository and paste it over there. 

Make it executable:
```
chmod +x /home/ubuntu/scripts/backup.sh
```


---

## 8️⃣ Test Backup Manually

Run the script:
```
/home/ubuntu/scripts/backup.sh
```


Check the S3 bucket:

Open the AWS Console and check inside S3 bucket you will see backup folder.





## 9️⃣ Automate Using Cron Job

Edit the crontab:
```
crontab -e
```


Select the **nano** editor when prompted, then paste this line:
```
* * * * * /home/ubuntu/scripts/backup.sh
```

This runs the backup every minute (useful for testing).

Nano shortcuts:

- `Ctrl + S` – Save current file  
- `Ctrl + X` – Exit nano

---

## 📦 Backup Output in S3

Example S3 layout:

```
ec2-backup-iamnk/
├── backup-2025-12-25_14-30-10.tar.gz
├── backup-2025-12-25_14-31-10.tar.gz
```
<img width="1919" height="967" alt="image" src="https://github.com/user-attachments/assets/59bbb574-f26f-4f14-8417-94196c4f1f38" />


Each backup file contains:

- The compressed `/home/ubuntu/data` directory  
- A timestamped filename  
- The full directory structure of the data directory

---

## 👨‍💻 Author
**Nandkishor Khandare**  
Cloud & DevOps / SRE Engineer  

## 📬 **Contact**: 
[LinkedIn](https://www.linkedin.com/in/nandkishor-khandare-616492215/) | [Email](nandkishor.k6e@gmail.com) | [Twitter(X)](https://x.com/devops_nk) 
