# Shielded-Cloud-App

## 🟢  Phase 1: Setting Up the Core Infrastructure

### Step 1: Create a Virtual Private Cloud (VPC)
1. Go to AWS Console → VPC → Your VPCs → Create VPC
2. Set the following:
    -  Name: AWS-WAF
    -  IPv4 CIDR Block: 172.16.0.0/16
    -  Leave IPv6 disabled (optional)
    -  Click Create VPC
  
### Step 2: Create Public & Private Subnets
1. Go to VPC → Subnets → Create Subnet
2. Public Subnet:
   -  Name: Public-Subnet
   -  VPC: AWS-WAF
   -  CIDR Block: 172.16.1.0/24
3. Private Subnet:
   -  Name: Private-Subnet
   -  VPC: AWS-WAF
   -  CIDR Block: 172.16.2.0/24
  

### Step 3: Set Up Internet Gateway & Route Tables
1. Go to VPC → Internet Gateways → Create Internet Gateway
   -  Name it WAF-Project-IGW
   -  Attach it to AWS-WAF
2. Modify Route Table for Public Subnet:
Go to VPC → Route Tables
Select the Main Route Table
Click Edit Routes → Add:
Destination: 0.0.0.0/0
Target: Select WAF-Project-IGW
