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
   -  Name: Public_Subnet-2
   -  VPC: WAF-VPC
   -  CIDR Block: 172.16.2.0/24

* Note - Both the subnets should be in different AZ's since it's a requirement of the Load Balancer which is going to be used soon.
     
![Screenshot 2025-02-24 002211](https://github.com/user-attachments/assets/85c4327c-edd4-4608-8766-70220b790b57)

<br><br>

![Screenshot 2025-02-25 071048](https://github.com/user-attachments/assets/8021648a-e139-41ad-92af-458c1d3c3ddf)




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
    - VPC: Select WAF-VPC
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

### Step 5: Create an ALB

1. Go to AWS Console → EC2 → Load Balancers → Create Load Balancer.
2. Choose Application Load Balancer (ALB).
3. Set ALB Configuration:
    - Name: App-Server-Loadbalancer
    - Scheme: Internet-facing
    - VPC: Select WAF-VPC
    - Subnets: Select two public subnets in different AZs

![Screenshot 2025-02-25 060459](https://github.com/user-attachments/assets/ec15f4d6-80e1-40fd-af70-19d7082ef80d)

<br><br>

![Screenshot 2025-02-26 172327](https://github.com/user-attachments/assets/e7d79d7a-2065-4bf3-be69-0b94afa229fc)



### Step 6: Configure the Security Group for ALB
1️. Create or select a security group for ALB

* Inbound Rules:
    - HTTP (Port 80) → Source: Anywhere (0.0.0.0/0)
    - (Optional) If you plan to use HTTPS later, allow HTTPS (Port 443)
    - Outbound Rules:
    - Allow all traffic (0.0.0.0/0)
2️. Attach this security group to ALB



![Screenshot 2025-02-25 060739](https://github.com/user-attachments/assets/243ec87f-6578-4ee2-a2f5-1b182b9b8f2a)

<br><br>

![Screenshot 2025-02-25 060931](https://github.com/user-attachments/assets/0d03c79b-2d2a-4a8a-ab12-4c422ac3493b)

<br><br>

![Screenshot 2025-02-25 061312](https://github.com/user-attachments/assets/8c717c90-ebab-4d94-ab7b-c055fb2f6013)



### Step 7: Configure Listeners & Routing
1. Listener Configuration

    - Protocol: HTTP
    - Port: 80
    - Target Group: Create a new target group
    - Name: MyProject-TG
    - Target Type: Instance
    - Protocol: HTTP
    - Port: 80
    - Health Check Path: /
    - Health Check Protocol: HTTP

2. Register EC2 Instances in Target Group

    - Click "Register Targets"
    - Select your EC2 instances
    - Click "Include as pending below"
    - Click "Register Targets"

![Screenshot 2025-02-25 061312](https://github.com/user-attachments/assets/f371b714-84ed-4ede-bec2-775be246227c)

<br><br>

https://github.com/user-attachments/assets/1a27bee0-ad7d-4189-9445-3c74d4e0316b

<br><br>

![Screenshot 2025-02-25 065951](https://github.com/user-attachments/assets/0e138457-0d24-41aa-aef7-f75605804b91)



### Step 8: Review, Create and Test ALB
        
1.  Review the settings
    - Click "Create Load Balancer"
    - Get the ALB DNS Name from the EC2 → Load Balancers page
    - Visit http://<ALB_DNS_NAME> in a browser

https://github.com/user-attachments/assets/285c6226-8fa8-423e-9b8a-36a7b508d1af

<br><br>

![Screenshot 2025-02-26 182439](https://github.com/user-attachments/assets/a2297058-6110-45b6-933d-4b4b06a5c27d)

<br><br>

![Screenshot 2025-02-26 182541](https://github.com/user-attachments/assets/4c1fb7f2-aa10-4a77-9676-a81d9548c5fe)

