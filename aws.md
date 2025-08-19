
# AWS ☁️

Amazon Web Services (AWS) is the **oldest and most popular cloud platform**, providing computing, storage, networking, and managed services.

AWS relies on **virtualization**:

* **Type A:** Virtualization on hardware.
* **Type B:** If the base machine fails, the VM also fails.

👉 Note: **NFS = EFS** (Amazon Elastic File System is similar to Network File System).

## 🔹 AWS Basics

* **Region** → Geographical area (e.g., `us-east-1`, `ap-south-1`)
* **Availability Zone (AZ)** → Data centers within a region
* **IAM** → Identity and Access Management (users, roles, policies)

---

## 🔹 EC2 (Elastic Compute Cloud)

EC2 provides scalable virtual machines in the cloud.

### Common Commands (using AWS CLI)

```bash
# Configure AWS CLI
aws configure

# Launch an EC2 instance
aws ec2 run-instances \
  --image-id ami-12345678 \
  --count 1 \
  --instance-type t2.micro \
  --key-name my-key \
  --security-group-ids sg-12345678 \
  --subnet-id subnet-12345678

# List running instances
aws ec2 describe-instances

# Start an instance
aws ec2 start-instances --instance-ids i-1234567890abcdef0

# Stop an instance
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Terminate an instance
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
```

---

## 🔹 S3 (Simple Storage Service)

Object storage to store and retrieve files.

### Common Commands

```bash
# Create a bucket
aws s3 mb s3://mybucketname

# List buckets
aws s3 ls

# Upload a file
aws s3 cp myfile.txt s3://mybucketname/

# Download a file
aws s3 cp s3://mybucketname/myfile.txt .

# Sync local folder with bucket
aws s3 sync ./local-folder s3://mybucketname/

# Delete file
aws s3 rm s3://mybucketname/myfile.txt

# Delete bucket (only if empty)
aws s3 rb s3://mybucketname
```

---

## 🔹 VPC (Virtual Private Cloud)

Custom private network for your AWS resources.

### Key Components

* **VPC** → Main private network
* **Subnets** → Divide VPC into smaller sections (public/private)
* **Route Table** → Routing rules for subnets
* **Internet Gateway (IGW)** → Provides internet access
* **Security Group** → Firewall rules for EC2 instances
* **NACL (Network ACL)** → Controls traffic at subnet level

### Common Commands

```bash
# Create a VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Create a subnet
aws ec2 create-subnet --vpc-id vpc-12345678 --cidr-block 10.0.1.0/24

# Create an Internet Gateway
aws ec2 create-internet-gateway

# Attach IGW to VPC
aws ec2 attach-internet-gateway --vpc-id vpc-12345678 --internet-gateway-id igw-12345678

# Create a route table
aws ec2 create-route-table --vpc-id vpc-12345678

# Add a route to the internet
aws ec2 create-route \
  --route-table-id rtb-12345678 \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id igw-12345678

# Associate subnet with route table
aws ec2 associate-route-table \
  --subnet-id subnet-12345678 \
  --route-table-id rtb-12345678
```

---

## 🔹 Elastic Load Balancers (ELB)

**Definition:** A Load Balancer distributes incoming traffic across multiple servers to ensure high availability and fault tolerance.

### Types of Load Balancers in AWS:

1. **Application Load Balancer (ALB) – Layer 7**

   * Works at application layer (HTTP/HTTPS).
   * Supports **routing based on host, path, headers, query parameters**.
   * Advanced features: WebSockets, ECS/EKS integration.

2. **Network Load Balancer (NLB) – Layer 4**

   * Works at transport layer (TCP/UDP/TLS).
   * Handles **high-performance & low-latency applications**.
   * Supports **millions of requests/second** with static IPs.

3. **Gateway Load Balancer (GWLB) – Layer 3**

   * Works at network layer (IP level).
   * Integrates **third-party firewalls and intrusion detection** systems.

4. **Classic Load Balancer (CLB) – Legacy**

   * Works at Layer 4 & 7 but lacks modern features.
   * Used only in **legacy apps**.

---

## 🔹 Load Balancing Policies

Load balancing policies define how traffic is distributed across servers.

1. **Round Robin**

   * Requests are distributed in order, one by one.
   * Best for servers with similar capacity.

2. **Least Connections**

   * Routes traffic to the server with the **fewest active connections**.
   * Best for **uneven workloads**.

3. **IP Hash (Source IP Affinity)**

   * Uses **client IP** to always send traffic to the same server.
   * Useful for **sticky sessions** (e.g., user login persistence).

---

## 🔹 Amazon EC2 Auto Scaling 

**Definition:** Automatically adjusts the number of EC2 instances based on demand.

* Ensures **high availability, cost-efficiency, and performance**.

### Scaling Methods:

* **Dynamic Scaling** → For unpredictable traffic.
* **Predictive Scaling** → For recurring usage patterns.
* **Scheduled Scaling** → For fixed-time scaling needs.
* **Simple Scaling** → Basic rule-based scaling.

---

## 🔹 Amazon EFS (Elastic File System)

**Definition:** A scalable, fully managed **shared file storage** for use with EC2 instances.

### EFS Installation on Amazon Linux:

```bash
yum install amazon-efs-utils -y
sudo mount -t efs -o tls fs-053c353ed1870c466:/ /mnt
```

### EFS Installation on RedHat Linux:

```bash
yum install nfs* -y
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport \
fs-053c353ed1870c466.efs.ap-south-1.amazonaws.com:/ efs
```

### Permanent Mounting (fstab entry):

```fstab
fs-053c353ed1870c466.efs.ap-south-1.amazonaws.com:/ /mnt nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 0 0
```

Then run:

```bash
mount -a
```

---

## 🔹 CloudWatch

**Definition:** A monitoring service for AWS resources and applications.

👉 To simulate CPU stress for testing:

```bash
# Install stress package on RHEL 9
sudo rpm -ivh https://dl.fedoraproject.org/pub/epel/9/Everything/x86_64/Packages/s/stress-1.0.4-29.el9.x86_64.rpm

# Run stress test
stress --cpu 30 --timeout 500 &
```

---

## 🔹 Amazon RDS (Relational Database Service)

**Definition:** A managed database service that supports MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server.

### Setup on EC2 (MariaDB client):

```bash
yum install mariadb-server -y
```

👉 Add **EC2 instance security group ID** into the **RDS security group** for connectivity.

### Connect to RDS from EC2:

```bash
mysql -h database-1.cdmm2gs4ius9.ap-south-1.rds.amazonaws.com -u admin -p
```

---


