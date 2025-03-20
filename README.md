# AWS Hands-on Project 🚀

This project demonstrates how to set up a **highly available AWS architecture** with **EC2 instances, VPC networking, and security best practices**.

## 🔹 Architecture Overview

- **VPC-1 with Public & Private Subnets**
- **VPC-2 for Cross-VPC Communication Testing**
- **EC2-1 (Public) with SSH access**
- **EC2-2 (Private) for internal communication**
- **EC2-8 (Private, in VPC-2) for cross-VPC communication testing**
- **Internet Gateway & Route Tables**
- **VPC Peering (for cross-VPC communication)**

## 🚀 Skills Gained

✅ **AWS Networking (VPC, Subnets, Route Tables, Security Groups)**\
✅ **Linux Server Management (EC2, SSH, key pairs, permissions)**\
✅ **Infrastructure as Code (Terraform - optional)**\
✅ **Security Best Practices (Least Privilege Access, Public vs. Private Subnets)**\
✅ **Cloud Troubleshooting & Debugging**\
✅ **VPC Peering & Cross-VPC Communication**

## 🛠 Setup Instructions

### 1️⃣ **Clone the Repository**

```bash
git clone https://github.com/Sinmisworld/aws-vpc-ec2-architecture.git
cd aws-vpc-ec2-architecture
```

## 2️⃣ **Deploy the Infrastructure Manually Using AWS Console**

### **Step 1: Create VPC-1**

1. Navigate to **AWS Console** > **VPC**.
2. Click **Create VPC** and set:
   - **Name:** `VPC-1`
   - **CIDR Block:** `10.10.0.0/16`
   - Enable **DNS resolution**.
3. Click **Create VPC**.

### **Step 2: Create Subnets in VPC-1**

1. Go to **VPC > Subnets**.
2. Click **Create Subnet**.
3. Select **VPC-1**, then create:
   - **Public Subnet**: `10.10.1.0/24`
   - **Private Subnet**: `10.10.2.0/24`
4. Enable **Auto-Assign Public IP** for the public subnet.

### **Step 3: Create VPC-2**

1. Go to **AWS Console** > **VPC** > **Create VPC**.
2. Set:
   - **Name:** `VPC-2`
   - **CIDR Block:** `8.8.0.0/16`
   - Enable **DNS resolution**.
3. Click **Create VPC**.

### **Step 4: Create Subnet in VPC-2**

1. Go to **VPC > Subnets**.
2. Click **Create Subnet**.
3. Select **VPC-2**, then create:
   - **Private Subnet:** `8.8.8.0/24`

### **Step 5: Create an Internet Gateway for VPC-1**

1. Navigate to **VPC > Internet Gateways**.
2. Click **Create Internet Gateway**.
3. Attach it to **VPC-1**.

### **Step 6: Configure Route Tables**

1. Go to **VPC > Route Tables**.
2. Select the main route table, add a route:
   - **Destination:** `0.0.0.0/0`
   - **Target:** The Internet Gateway.
3. Associate this route table with the **public subnet** in VPC-1.

### **Step 7: Launch EC2 Instances**

1. Go to **EC2 > Instances** > **Launch Instance**.
2. Select Amazon Linux 2 (or Ubuntu).
3. Configure:
   - **EC2-1 (Public in VPC-1)** → Associate with **Public Subnet**.
   - **EC2-2 (Private in VPC-1)** → Associate with **Private Subnet**.
   - **EC2-8 (Private in VPC-2)** → Associate with **Private Subnet in VPC-2**.
4. Add Security Group rules:
   - **EC2-1:** Allow SSH (`22`) from your **IP**.
   - **EC2-2 & EC2-8:** Allow only internal traffic from VPC.

### **Step 8: Set Up VPC Peering**

1. Go to **VPC > Peering Connections**.
2. Click **Create Peering Connection**.
3. Select **VPC-1** and **VPC-2**.
4. Accept the request in VPC-2.
5. Modify route tables:
   - In **VPC-1**, add a route:
     - **Destination:** `8.8.0.0/16`
     - **Target:** VPC Peering Connection.
   - In **VPC-2**, add a route:
     - **Destination:** `10.10.0.0/16`
     - **Target:** VPC Peering Connection.

### **Step 9: Verify Setup**

1. **SSH into EC2-1:**
   ```bash
   ssh -i mykp.pem ec2-user@<EC2-1_PUBLIC_IP>
   ```
2. **Ping EC2-2 from EC2-1:**
   ```bash
   ping 10.10.2.X
   ```
3. **Ping EC2-8 from EC2-1 (Cross-VPC):**
   ```bash
   ping <EC2-8_Private_IP>
   ```

## 🔄 Future Improvements
 - Add an Nginx Web Server to EC2-1 for a simple web app.
 - Implement a Bastion Host for better security.
 - Deploy the architecture with Terraform or AWS CDK.





