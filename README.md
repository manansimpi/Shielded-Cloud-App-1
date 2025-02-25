# Shielded-Cloud-App

## 🟢  Phase 1: Setting Up the Core Infrastructure

### Step 1: Create a Virtual Private Cloud (VPC)
1. Go to AWS Console → VPC → Your VPCs → Create VPC
2. Set the following:
    -  Name: WAF-VPC
    -  IPv4 CIDR Block: 172.16.0.0/16
    -  Leave IPv6 disabled (optional)
    -  Click Create VPC
  
![Screenshot 2025-02-24 001928](https://github.com/user-attachments/assets/3d9b7e61-e292-4933-8475-bd51ee5e4346)

  
### Step 2: Create Public & Private Subnets
1. Go to VPC → Subnets → Create Subnet
2. Public Subnet:
   -  Name: Public-Subnet
   -  VPC: WAF-VPC
   -  CIDR Block: 172.16.1.0/24
3. Private Subnet:
   -  Name: Private-Subnet
   -  VPC: WAF-VPC
   -  CIDR Block: 172.16.2.0/24
     
![Screenshot 2025-02-24 002211](https://github.com/user-attachments/assets/85c4327c-edd4-4608-8766-70220b790b57)

<br><br>

![Screenshot 2025-02-24 002247](https://github.com/user-attachments/assets/a3f94eda-401e-437f-be78-d4430e80be09)



### Step 3: Set Up Internet Gateway & Route Tables
1. Go to VPC → Internet Gateways → Create Internet Gateway
   -  Name it WAF-Project-IGW
   -  Attach it to WAF-VPC VPC
2. Modify Route Table for Public Subnet:
   -  Go to VPC → Route Tables
   -  Select the Route Table associated with our VPC.
   -  Click Edit Routes → Add:
   -  Destination: 0.0.0.0/0
   -  Target: Select WAF-Project-IGW

![Screenshot 2025-02-24 002349](https://github.com/user-attachments/assets/bfe5ed5d-0b7a-47a8-8736-97b74c8674ac)

<br><br>

![Screenshot 2025-02-24 002437](https://github.com/user-attachments/assets/85b6bc6e-6dc9-483c-898e-da183b215e2a)

<br><br>

![Screenshot 2025-02-24 003413](https://github.com/user-attachments/assets/7632d8a7-c769-4e82-b063-f28eccd519f6)


### Step 4: Launch EC2 Instance with Web Application
1. Go to AWS Console → EC2 → Launch Instance
2. Choose Amazon Linux 2 (or Ubuntu if preferred)
3. Under Instance Type, pick t2.micro (free-tier)
4. Network Settings:
    - VPC: Select AWS-WAF
    - Subnet: Select Public-Subnet
    - Enable Auto-Assign Public IP
5. Security Group:
    - Allow HTTP (80) for web traffic
    - Allow SSH (22) for admin access (only from your IP)
6. Launch the Instance and SSH into it to run a script on the Web_server.
       

![Screenshot 2025-02-25 043659](https://github.com/user-attachments/assets/c57a111b-e7a4-48bd-b5ac-bde0e33c8a14)

<br><br>

![Screenshot 2025-02-25 043742](https://github.com/user-attachments/assets/01f44fa5-e245-4682-a505-0a21db3e275f)

<br><br>

![Screenshot 2025-02-25 045059](https://github.com/user-attachments/assets/ac699517-3e2b-4afc-8937-73c371d9a3ae)

<br><br>

![Screenshot 2025-02-25 050245](https://github.com/user-attachments/assets/dd8cbad4-3622-494b-99ef-19ce57505317)

<br><br>

![Screenshot 2025-02-25 050340](https://github.com/user-attachments/assets/a0aaa9e9-3591-4636-b6b8-f4a7af33ab57)


## 🔵 Phase 2: Deploy Application Load Balancer (ALB)









