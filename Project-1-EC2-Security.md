# ☁️ Project 1: Securing AWS EC2 Instances

<div align="center">

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge&logo=amazon-aws)
![EC2](https://img.shields.io/badge/EC2-Compute-blue?style=for-the-badge&logo=amazon-ec2)
![Security](https://img.shields.io/badge/Security-Hardening-red?style=for-the-badge&logo=security)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

<!-- Animated Server GIF -->
<img src="https://user-images.githubusercontent.com/74038190/212257465-7ce8d493-cac5-494e-982a-5a9deb852c4b.gif" width="400" alt="Server GIF"/>

**A comprehensive hands-on guide to securing AWS EC2 instances**

[Overview](#-introduction) •
[Exercises](#-exercises) •
[Checklist](#-security-checklist) •
[Resources](#-additional-resources)

</div>

---

## 📖 Introduction

<img align="right" src="https://user-images.githubusercontent.com/74038190/212257468-1e9a91f1-b626-4baa-b15d-5c385b1be58e.gif" width="180" alt="Coding GIF"/>

In this project, you'll learn how to secure AWS EC2 instances from the ground up:

- 🔐 SSH Key-based Authentication
- 🛡️ Security Group Configuration
- 🖥️ OS & SSH Hardening
- 📊 CloudWatch Monitoring

| Attribute | Details |
|-----------|---------|
| ⏱️ **Duration** | 60-90 minutes |
| 📚 **Level** | Beginner |
| 💰 **Cost** | Free Tier |

<br clear="right"/>

---

## 📋 Prerequisites

| Requirement | Description |
|-------------|-------------|
| ☁️ AWS Account | [Sign up free](https://aws.amazon.com/free/) |
| 💻 AWS CLI | [Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) |
| 🔑 SSH Client | Built-in or [PuTTY](https://www.putty.org/) |

---

## 🎯 Exercises

### Exercise 1: Launch an EC2 Instance

<img align="right" src="https://user-images.githubusercontent.com/74038190/212281775-b468df30-4edc-4bf8-a4ee-f52e1aaddc86.gif" width="150" alt="Shield GIF"/>

| Step | Action |
|------|--------|
| 1 | Log in to [AWS Console](https://aws.amazon.com/console/) |
| 2 | EC2 Dashboard → Launch Instance |
| 3 | Choose **Amazon Linux 2023** |
| 4 | Select **t2.micro** (free tier) |
| 5 | Configure Security Group |
| 6 | Create & download key pair |

<br clear="right"/>

> ⚠️ **Never allow SSH from `0.0.0.0/0` in production!**

---

### Exercise 2: Connect to Your Instance

<img align="right" src="https://user-images.githubusercontent.com/74038190/229223263-cf2e4b07-2615-4f87-9c38-e37600f8381a.gif" width="180" alt="Terminal GIF"/>

```bash
# Set key permissions
chmod 400 my-key.pem

# Connect via SSH
ssh -i "my-key.pem" ec2-user@<public-dns>
```

**Windows PowerShell:**
```powershell
icacls my-key.pem /inheritance:r
icacls my-key.pem /grant:r "$($env:USERNAME):(R)"
```

<br clear="right"/>

---

### Exercise 3: Harden Your Instance

```bash
# Update system
sudo yum update -y

# Create non-root user
sudo adduser secadmin
sudo usermod -aG wheel secadmin
```

**SSH Hardening** (`/etc/ssh/sshd_config`):

```bash
PermitRootLogin no
PasswordAuthentication no
MaxAuthTries 3
```

```bash
sudo systemctl restart sshd
```

---

### Exercise 4: Security Groups

| Type | Port | Source |
|------|------|--------|
| SSH | 22 | **Your IP only** |

---

### Exercise 5: CloudWatch Monitoring

| Setting | Value |
|---------|-------|
| Metric | CPUUtilization |
| Threshold | > 80% |
| Action | Email alert |

---

## ✅ Security Checklist

| Control | Status |
|---------|--------|
| Non-root user created | ⬜ |
| SSH key-based auth only | ⬜ |
| Root login disabled | ⬜ |
| Security Group restricts SSH | ⬜ |
| Packages updated | ⬜ |
| CloudWatch configured | ⬜ |

---

## 🔧 Troubleshooting

<details>
<summary><strong>❌ Connection Timeout</strong></summary>

Check Security Group allows your IP:
```bash
curl ifconfig.me
```
</details>

<details>
<summary><strong>❌ Permission Denied</strong></summary>

```bash
chmod 400 your-key.pem
```
</details>

---

## 📚 Additional Resources

- [EC2 Security Best Practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security.html)
- [Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)

---

## 👤 Author

<div align="center">

<img src="https://user-images.githubusercontent.com/74038190/235224431-e8c8c12e-6826-47f1-89fb-2ddad83b3abf.gif" width="150" alt="Dev GIF"/>

**Amresh Kumar**

[![GitHub](https://img.shields.io/badge/GitHub-Ak--cybe-181717?style=for-the-badge&logo=github)](https://github.com/Ak-cybe)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Amresh%20Kumar-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/amresh-kumar-7b5ab8326/)

</div>

---

<div align="center">

**⭐ Star this repo if it helped you!**

[🔝 Back to Top](#️-project-1-securing-aws-ec2-instances)

</div>
