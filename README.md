# AWS VPC Architecture: Secure S3 Backup via Gateway Endpoint

## 📌 Project Overview
This project demonstrates a secure, enterprise-grade AWS VPC network architecture. The goal is to allow a **Private EC2 instance** (with no public IP and no internet access) to securely transfer backups to an **Amazon S3 bucket** using an **AWS VPC Gateway Endpoint**. 

All data traffic flows entirely within the AWS private backbone network, eliminating exposure to the public internet and avoiding the costs of a NAT Gateway.

---

## 🏗️ Architecture Highlights

- **Custom VPC:** Configured with Public and Private subnets across availability zones.
- **Bastion Host (Public EC2):** Acts as a secure jump box to access the private instance.
- **Private Instance (Private EC2):** Isolated instance with no Internet Gateway route.
- **VPC Gateway Endpoint (`com.amazonaws.us-east-1.s3`):** Route table-associated endpoint providing direct, private access to S3.
- **IAM Role (`AmazonS3FullAccess`):** Least-privilege IAM configuration attached to the EC2 instance.

---

## 🛠️ Step-by-Step Implementation & Commands

### 1. SSH into Public Instance (Bastion Host)
Connect to the public EC2 instance from local terminal (MobaXterm/SSH):
```bash
ssh -i "your-key.pem" ec2-user@<PUBLIC_EC2_PUBLIC_IP>

nano betakey.pem
# Paste key content, then save (Ctrl+O -> Enter -> Ctrl+X)

chmod 400 betakey.pem
ssh -i betakey.pem ec2-user@10.0.2.65

# List all S3 buckets
aws s3 ls

# Create a test backup file
echo "My First Secure S3 Backup via VPC Endpoint" > backup.txt

# Upload backup to the private S3 bucket
aws s3 cp backup.txt s3://myy-privte-bucket-123/

# Verify uploaded backup
aws s3 ls s3://myy-privte-bucket-123/
