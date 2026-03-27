# ajira-mentorship-project
This repository demonstrates the setup and configuration of an AWS EC2 instance. It includes step-by-step documentation and networking configurations to deploy and maintain a scalable cloud environment.

# AWS S3 Security Project: Data Exfiltration & Monitoring with CloudTrail

##  Project Overview
This project demonstrates how misconfigured AWS Identity and Access Management (IAM) permissions can lead to unauthorized access and data exfiltration from Amazon S3, and how AWS CloudTrail can be used to detect and monitor such activity.

---

##  Objectives
- Simulate a real-world cloud security misconfiguration
- Demonstrate data exfiltration using compromised credentials
- Monitor activity using AWS CloudTrail
- Apply least privilege to mitigate the vulnerability

---

##  Architecture

Components used:
- IAM Admin User (`cloud-admin`)
- IAM Attacker User (`attacker-user`)
- S3 Bucket (Sensitive Data)
- S3 Bucket (CloudTrail Logs)
- AWS CloudTrail (Monitoring)
- Kali Linux (Attacker Machine)

---

## ⚙️ Setup

### 1. IAM Configuration
- Created IAM admin user (`cloud-admin`)
- Created attacker user (`attacker-user`) with:
  - Programmatic access
  - `AmazonS3FullAccess` policy (intentional misconfiguration)

### 2. S3 Buckets
- `mentorship-project-buckt`
  - Stored file: `confidential.txt`
- `cloudtrail-logs-project-files`
  - Stores CloudTrail logs

### 3. CloudTrail
- Trail created: `s3-monitoring-project`
- Enabled:
  - Management events
  - **S3 Data events (Read & Write)**
- Logs stored in logs bucket

---

##  Attack Simulation (Data Exfiltration)

### Configure AWS CLI (Kali Linux)
```bash
aws configure
```

### List Buckets
```bash
aws s3 ls
```

### Access Target Bucket
```bash
aws s3 ls s3://mentorship-project-buckt
```

### Download Sensitive File
```bash
aws s3 cp s3://mentorship-project-buckt/confidential.txt stolen-data.txt
```

✅ Result: mentorship-project-buckt files successfully downloaded

---

## 🔍 Detection with CloudTrail

- Navigated to **CloudTrail → Event History**
- Filtered events:
  - `ListBucket`

### Observations:
- Event Name: `GetObject`
- User: `attacker-user`
- Resource: S3 bucket
- Source IP: Attacker machine

 This confirms unauthorized access and data exfiltration

---

##  Remediation (Least Privilege)

- Removed `AmazonS3FullAccess` from attacker-user
- Applied restricted permissions OR removed access entirely

### Validation
```bash
aws s3 cp s3://mentorship-project-buckt/confidential.txt test.txt
```

❌ Result: Access Denied

---

##  Key Learnings
- Misconfigured IAM policies can expose sensitive data
- CloudTrail is essential for visibility and auditing
- Principle of least privilege is critical in cloud security
- External attackers can exploit leaked credentials

---

##  Future Improvements
- Integrate CloudWatch alerts for real-time detection
- Add IAM policy conditions (IP restriction)
- Enable MFA for sensitive operations

---

## 🧹 Cleanup
- Deleted or rotated access keys used in the simulation
- Removed unnecessary permissions

---

##  Conclusion

This project demonstrates a complete cloud security lifecycle:
**Setup → Misconfiguration → Attack → Detection → Remediation → Validation**

It highlights the importance of proper IAM configuration and monitoring in securing cloud environments.

