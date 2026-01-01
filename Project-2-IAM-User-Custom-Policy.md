# 🛡️ Project 2: Implementing Least Privilege with AWS IAM

<div align="center">

<img src="assets/aws-security-banner.png" width="500" alt="AWS Security Banner"/>

<!-- Animated Shield GIF -->
<img src="https://user-images.githubusercontent.com/74038190/212281775-b468df30-4edc-4bf8-a4ee-f52e1aaddc86.gif" width="300"/>

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge&logo=amazon-aws)
![IAM](https://img.shields.io/badge/Security-IAM-red?style=for-the-badge&logo=security)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Level](https://img.shields.io/badge/Level-Beginner-green?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

**A hands-on guide to implementing the Principle of Least Privilege using AWS IAM custom policies**

[Overview](#-project-overview) •
[Implementation](#-step-by-step-implementation) •
[Testing](#phase-3-cli-configuration--testing) •
[Best Practices](#-security-best-practices-implemented)

</div>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Prerequisites](#-prerequisites)
- [Objectives](#-objectives)
- [Architecture Diagram](#-architecture-diagram)
- [Step-by-Step Implementation](#-step-by-step-implementation)
  - [Phase 1: IAM User Configuration](#phase-1-iam-user-configuration)
  - [Phase 2: Defining the Security Policy](#phase-2-defining-the-security-policy-json)
  - [Phase 3: CLI Configuration & Testing](#phase-3-cli-configuration--testing)
- [Use Case Scenarios](#-use-case-scenarios)
- [Security Best Practices](#-security-best-practices-implemented)
- [Important Security Notes](#️-important-security-notes)
- [Learning Outcomes](#-learning-outcomes)
- [Future Improvements](#-future-improvements)
- [Resources & References](#-resources--references)
- [Author](#-author)

---

## 📖 Project Overview

This project demonstrates the implementation of the **Principle of Least Privilege** in AWS. The goal was to move away from using root accounts or full-access admin users by creating a specialized IAM user with strictly limited permissions.

The project involves creating a custom JSON policy that restricts a user to **only** read and list files from a specific S3 bucket, denying all other actions.

| Attribute | Details |
|-----------|---------|
| ⏱️ **Time to Complete** | 30-45 minutes |
| 📚 **Difficulty Level** | Beginner-Intermediate |
| 💰 **AWS Cost** | Free Tier Eligible |
| 🔧 **Services Used** | IAM, S3, AWS CLI |

---

## 📋 Prerequisites

Before starting this project, ensure you have:

- [ ] AWS Account with Administrator access
- [ ] AWS CLI installed and configured ([Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html))
- [ ] Basic understanding of JSON syntax
- [ ] An existing S3 bucket (or create one for testing)

---

## 🎯 Objectives

| Objective | Description |
|-----------|-------------|
| 🆔 **Identity Management** | Create a dedicated programmatic user (`s3-read-user`) |
| 🔒 **Access Control** | Draft a custom IAM policy using JSON for granular permissions |
| ✅ **Verification** | Authenticate and test permissions using the AWS CLI |

---

## 🏗️ Architecture Diagram

<div align="center">

![IAM Policy Architecture](assets/iam-policy-architecture.png)

*Figure 1: IAM User with Custom Policy - Only allowed S3 bucket access, all others denied*

</div>

This architecture demonstrates:
- **IAM User** with programmatic access credentials
- **Custom Policy** attached with specific S3 permissions
- **Single Bucket Access** - `my-secure-bucket` ✅
- **Other Buckets Denied** - Access blocked ❌

---

## 🛠️ Step-by-Step Implementation

### Phase 1: IAM User Configuration

1. Log in to the **AWS Management Console** as an Administrator
2. Navigate to **IAM Dashboard** → **Users** → **Add user**
3. Configure the user details:

| Setting | Value |
|---------|-------|
| **Username** | `s3-read-user` |
| **Access Type** | Programmatic Access |
| **Permissions** | Custom Policy (created in Phase 2) |

> ⚠️ **Important:** Save the Access Key ID and Secret Access Key securely. You won't be able to view the secret key again after this step!

---

### Phase 2: Defining the Security Policy (JSON)

Instead of attaching a managed policy like `AmazonS3ReadOnlyAccess` (which grants access to *all* buckets), I created an **Inline Policy** to restrict access to a specific resource.

#### Custom Policy Definition

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

#### 🔍 Technical Breakdown

| Component | Purpose | Scope |
|-----------|---------|-------|
| `s3:ListBucket` | Allows listing objects inside the bucket | Bucket level ARN |
| `s3:GetObject` | Allows downloading/reading files | Object level (`/*`) |
| **Resource Restriction** | Policy is locked to `my-secure-bucket` | Prevents access to any other data |

<details>
<summary>💡 <strong>Why two ARN formats?</strong></summary>

<br>

AWS S3 permissions work at two levels:
- **Bucket Level** (`arn:aws:s3:::bucket-name`) - For operations like `ListBucket`
- **Object Level** (`arn:aws:s3:::bucket-name/*`) - For operations like `GetObject`

Both must be specified to allow complete read access!

</details>

---

### Phase 3: CLI Configuration & Testing

<img align="right" src="https://user-images.githubusercontent.com/74038190/229223263-cf2e4b07-2615-4f87-9c38-e37600f8381a.gif" width="180"/>

After generating the credentials, configure the local environment to simulate a developer accessing cloud resources.

<div align="center">

![AWS CLI Demo](assets/aws-cli-demo.png)

*Figure 2: AWS CLI Terminal - Testing IAM Permissions*

</div>

#### Step 1: Configure AWS CLI Profile

```bash
aws configure --profile s3-user
```

When prompted, enter the following:

```
AWS Access Key ID: [Paste Key ID]
AWS Secret Access Key: [Paste Secret Key]
Default region name: us-east-1
Default output format: json
```

#### Step 2: Verify Access (Success Scenario)

```bash
# List objects in the allowed bucket
aws s3 ls s3://my-secure-bucket --profile s3-user
```

**✅ Expected Result:** Successfully listed files

```
2025-01-01 10:00:00        1024 config.json
2025-01-01 10:00:00        2048 data.csv
2025-01-01 10:00:00         512 readme.txt
```

#### Step 3: Security Validation (Failure Scenario)

Test that the policy correctly denies access to other buckets:

```bash
# Attempt to list a different bucket (should fail)
aws s3 ls s3://other-sensitive-bucket --profile s3-user
```

**❌ Expected Result:** Access Denied

```
An error occurred (AccessDenied) when calling the ListObjectsV2 operation: Access Denied
```

---

## 🚀 Use Case Scenarios

This configuration is ideal for:

| Use Case | Description |
|----------|-------------|
| 🔌 **Third-party Applications** | Giving an external reporting tool access to read logs from one specific bucket |
| 👨‍💻 **Developers** | Allowing a frontend developer to fetch assets without admin rights |
| ⚙️ **Microservices** | Services that only need to read configuration files |
| 🔄 **CI/CD Pipelines** | Automated deployment processes requiring read access to artifact buckets |
| 📊 **Analytics Tools** | Read-only access for data visualization tools |

---

## 🔐 Security Best Practices Implemented

| Practice | Description | Status |
|----------|-------------|--------|
| **Least Privilege** | User has 0 permissions by default; only explicit allowances added | ✅ |
| **Resource Constraints** | Policy restricted to specific ARNs, not `*` (all resources) | ✅ |
| **Credential Safety** | Access Keys not hardcoded; used via AWS CLI profiles | ✅ |
| **Separation of Duties** | Created specific user instead of sharing Admin credentials | ✅ |
| **Regular Auditing** | Use CloudTrail to monitor IAM user activity | ✅ |
| **No Root Usage** | Root account not used for day-to-day operations | ✅ |

---

## ⚠️ Important Security Notes

> **🚨 Never commit AWS credentials to version control!**

### Recommended Credential Management Methods

| Method | Best For | Security Level |
|--------|----------|----------------|
| **Environment Variables** | Local development | ⭐⭐ |
| **AWS CLI Profiles** | Developer workstations | ⭐⭐⭐ |
| **IAM Roles** | EC2/Lambda (preferred) | ⭐⭐⭐⭐⭐ |
| **AWS Secrets Manager** | Production applications | ⭐⭐⭐⭐⭐ |
| **AWS SSO** | Enterprise environments | ⭐⭐⭐⭐⭐ |

```bash
# ❌ DON'T: Hardcode credentials
export AWS_ACCESS_KEY_ID="AKIAXXXXXXXXXXXXXXXX"

# ✅ DO: Use profiles or IAM roles
aws s3 ls --profile s3-user
```

---

## 📊 Learning Outcomes

After completing this project, you will understand:

- [x] How to create IAM users with programmatic access
- [x] Writing custom JSON policies for fine-grained access control
- [x] The difference between inline and managed policies
- [x] How to test IAM permissions using AWS CLI
- [x] Implementing the Principle of Least Privilege
- [x] Resource-level permissions vs. service-level permissions

---

## 🔄 Future Improvements

### Roadmap

| Enhancement | Description | Priority |
|-------------|-------------|----------|
| 🔐 **MFA Enforcement** | Add condition in policy to require Multi-Factor Authentication | High |
| 🎭 **IAM Roles** | Transition from IAM Users to IAM Roles for EC2 integration | High |
| 📜 **Policy Versioning** | Implement policy version control using AWS Policy Simulator | Medium |
| 🔔 **CloudWatch Alarms** | Set up alerts for unauthorized access attempts | Medium |
| ⏱️ **Session Policies** | Implement temporary credentials with session policies | Low |
| 🏷️ **Resource Tags** | Add tag-based access control for dynamic environments | Low |

---

## 📚 Resources & References

| Resource | Description |
|----------|-------------|
| 📖 [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) | Official AWS security guidelines |
| 🪣 [S3 Bucket Policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html) | Comprehensive S3 access control |
| 🧪 [AWS Policy Simulator](https://policysim.aws.amazon.com/) | Test policies before deployment |
| 🛡️ [OWASP Cloud Security](https://owasp.org/www-project-cloud-security/) | Cloud security best practices |
| 📋 [CIS AWS Foundations Benchmark](https://www.cisecurity.org/benchmark/amazon_web_services) | Security compliance standards |

---

## 👤 Author

<div align="center">

<img src="https://user-images.githubusercontent.com/74038190/235224431-e8c8c12e-6826-47f1-89fb-2ddad83b3abf.gif" width="150"/>

**Amresh Kumar**

[![GitHub](https://img.shields.io/badge/GitHub-Ak--cybe-181717?style=for-the-badge&logo=github)](https://github.com/Ak-cybe)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Amresh%20Kumar-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/amresh-kumar-7b5ab8326/)

*Cloud Security Enthusiast | AWS Practitioner*

</div>

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**⭐ If this project helped you understand IAM policies, please star the repository!**

[![Star History](https://img.shields.io/github/stars/Ak-cybe/AWS-Security-Projects?style=social)](https://github.com/Ak-cybe/AWS-Security-Projects)

---

Made with ❤️ for the Cloud Security Community

[🔝 Back to Top](#️-project-2-implementing-least-privilege-with-aws-iam)

</div>
