# 🛡️ Project 2: Implementing Least Privilege with AWS IAM

<div align="center">

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge&logo=amazon-aws)
![IAM](https://img.shields.io/badge/Security-IAM-red?style=for-the-badge&logo=security)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-green?style=for-the-badge)

<!-- Animated Shield GIF -->
<img src="https://user-images.githubusercontent.com/74038190/212281775-b468df30-4edc-4bf8-a4ee-f52e1aaddc86.gif" width="400" alt="Security GIF"/>

**A hands-on guide to implementing the Principle of Least Privilege**

[Overview](#-project-overview) •
[Implementation](#-step-by-step-implementation) •
[Testing](#phase-3-cli-testing) •
[Best Practices](#-security-best-practices)

</div>

---

## 📖 Project Overview

<img align="right" src="https://user-images.githubusercontent.com/74038190/212284100-561aa473-3905-4a80-b561-0d28506553ee.gif" width="180" alt="Security GIF"/>

This project demonstrates the **Principle of Least Privilege** in AWS:

- 🆔 Create dedicated IAM user
- 📜 Write custom JSON policies
- 🔒 Restrict access to specific resources
- 🧪 Test with AWS CLI

| Attribute | Details |
|-----------|---------|
| ⏱️ **Duration** | 30-45 minutes |
| 📚 **Level** | Beginner-Intermediate |
| 💰 **Cost** | Free Tier |

<br clear="right"/>

---

## 📋 Prerequisites

- [ ] AWS Account with Admin access
- [ ] AWS CLI installed
- [ ] Basic JSON knowledge
- [ ] An S3 bucket for testing

---

## 🛠️ Step-by-Step Implementation

### Phase 1: Create IAM User

<img align="right" src="https://user-images.githubusercontent.com/74038190/212257468-1e9a91f1-b626-4baa-b15d-5c385b1be58e.gif" width="150" alt="Coding GIF"/>

1. Go to **IAM Dashboard** → **Users** → **Add user**
2. Configure:

| Setting | Value |
|---------|-------|
| Username | `s3-read-user` |
| Access Type | Programmatic |

> ⚠️ Save your Access Key ID and Secret Key!

<br clear="right"/>

---

### Phase 2: Create Custom Policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowS3ReadAccess",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::my-secure-bucket",
                "arn:aws:s3:::my-secure-bucket/*"
            ]
        }
    ]
}
```

| Permission | Purpose |
|------------|---------|
| `s3:ListBucket` | List objects in bucket |
| `s3:GetObject` | Download files |

---

### Phase 3: CLI Testing

<img align="right" src="https://user-images.githubusercontent.com/74038190/229223263-cf2e4b07-2615-4f87-9c38-e37600f8381a.gif" width="180" alt="Terminal GIF"/>

**Configure Profile:**
```bash
aws configure --profile s3-user
```

**Test Allowed Access:**
```bash
aws s3 ls s3://my-secure-bucket --profile s3-user
# ✅ Success!
```

**Test Denied Access:**
```bash
aws s3 ls s3://other-bucket --profile s3-user
# ❌ Access Denied (as expected!)
```

<br clear="right"/>

---

## 🚀 Use Cases

| Scenario | Description |
|----------|-------------|
| 🔌 Third-party Apps | Read-only log access |
| 👨‍💻 Developers | Fetch assets only |
| ⚙️ Microservices | Read config files |
| 🔄 CI/CD | Artifact bucket access |

---

## 🔐 Security Best Practices

<div align="center">

<img src="https://user-images.githubusercontent.com/74038190/212284115-f47cd8ff-2ffb-4b04-b5bf-4d1c14c0247f.gif" width="300" alt="Security GIF"/>

</div>

| Practice | Status |
|----------|--------|
| Least Privilege | ✅ |
| Specific Resource ARNs | ✅ |
| No hardcoded credentials | ✅ |
| Separation of duties | ✅ |

---

## ⚠️ Credential Safety

```bash
# ❌ DON'T
export AWS_ACCESS_KEY_ID="AKIA..."

# ✅ DO
aws s3 ls --profile s3-user
```

| Method | Security |
|--------|----------|
| CLI Profiles | ⭐⭐⭐ |
| IAM Roles | ⭐⭐⭐⭐⭐ |
| Secrets Manager | ⭐⭐⭐⭐⭐ |

---

## 📊 Learning Outcomes

- [x] Create IAM users
- [x] Write JSON policies  
- [x] Test with AWS CLI
- [x] Least Privilege principle

---

## 📚 Resources

- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [S3 Bucket Policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html)
- [Policy Simulator](https://policysim.aws.amazon.com/)

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

[🔝 Back to Top](#️-project-2-implementing-least-privilege-with-aws-iam)

</div>
