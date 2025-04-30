Here’s a detailed answer to each of your 13 questions, explained in a beginner-friendly and real-world way with relevant examples:

---

### 1. **How do you troubleshoot VPC issues?**

✅ **Steps to troubleshoot:**

- **Check Route Tables:** Ensure proper routes are defined (e.g., internet gateway for public subnet).
- **Check Security Groups & NACLs:** Make sure they allow required inbound/outbound traffic.
- **Ping/SSH from Bastion Host:** Helps test connectivity inside VPC.
- **Check Internet Gateway/NAT Gateway:** Confirm correct attachment and route.
- **VPC Flow Logs:** Enable flow logs to monitor traffic at subnet or network interface level.

📌 **Example:** If EC2 can't reach the internet, verify if:
- Subnet has a route to the Internet Gateway.
- EC2 is in a public subnet.
- Security group allows outbound traffic.


Common Issues & Fixes:

Problem: EC2 instances can’t communicate.

Check:

Subnet routes (Route Tables).

NACL/Security Group rules (blocking traffic).

Internet Gateway (IGW) attachment.

Problem: Can’t SSH/RDP into instances.

Check:

Security Group (SG) allows inbound traffic (port 22/3389).

NACL doesn’t block outbound responses.

Example:
If Instance A can’t ping Instance B:

Verify both are in the same subnet (or subnets with proper routing).

Check SGs allow ICMP traffic.

---

### 2. **What is the difference between NACL and SG in VPC?**

| Feature                | Security Group (SG)              | Network ACL (NACL)                |
|------------------------|----------------------------------|-----------------------------------|
| Scope                  | Applied to **EC2 instance**       | Applied to **subnet**             |
| Type                   | **Stateful** (remembers traffic) | **Stateless** (no memory)         |
| Rules                  | Only Allow rules                 | Allow & Deny rules                |
| Order of Evaluation    | No rule order                    | Rules evaluated top to bottom     |

📌 **Use Case:**
- Use **SG** to allow access to EC2 (e.g., SSH on port 22).
- Use **NACL** to restrict access to a subnet (e.g., deny a CIDR block).


NACL vs. Security Group (SG)
Feature	NACL	Security Group (SG)
Layer	Network (Layer 3-4)	Instance (Layer 3-4)
Rule Type	Stateless (allow/deny both ways)	Stateful (allow inbound/outbound)
Scope	Subnet-level	Instance-level
Example	Block all traffic from 10.0.0.0/16	Allow SSH only from your IP
Real-World Analogy:

NACL: Like a firewall at a building entrance (checks everyone entering/exiting).

SG: Like a bouncer at a VIP room (only lets authorized people in/out).


---

### 3. **How do you communicate 4 or 5 VPCs?**

✅ **Methods:**
- **VPC Peering:** Point-to-point connection between VPCs. Need to set up manually between each pair.
- **Transit Gateway (TGW):** Central hub to connect multiple VPCs easily and scalably.

📌 **Example:** For 5 VPCs, TGW is better than creating 10+ peering connections.



Options:

VPC Peering: Direct connection between 2 VPCs (no transitive routing).

❌ Doesn’t scale beyond a few VPCs.

Transit Gateway: Hub-and-spoke model (connects all VPCs centrally).

✅ Scalable (supports 100s of VPCs).

AWS PrivateLink: For private services (e.g., S3, DynamoDB).

Example:
A company with Dev, Prod, and Shared Services VPCs uses Transit Gateway to connect all.


---

### 4. **What is a VPC Endpoint?**

✅ **Definition:** A VPC Endpoint allows private connection from your VPC to AWS services without using the internet.

📌 **Example:** Access S3 from EC2 without internet — use **S3 VPC Endpoint** to route traffic internally.


Purpose: Privately connect your VPC to AWS services (S3, DynamoDB) without using the internet.

Types:

Interface Endpoint (uses ENI for services like EC2 API).

Gateway Endpoint (for S3/DynamoDB, free).

Example:
A bank’s VPC uses a Gateway Endpoint to access S3 securely (no public internet exposure).


---

### 5. **What is Transit Gateway?**

✅ **Definition:** A **Transit Gateway** acts as a central router to connect multiple VPCs and on-premises networks.

📌 **Example:** A company has 10 VPCs. Instead of managing many VPC Peering links, connect all to one Transit Gateway.


Purpose: A hub that connects multiple VPCs, VPNs, and AWS accounts.

Benefits:

Simplifies networking (no complex peering).

Supports cross-region/account connections.

Example:
A global company connects US-VPC, EU-VPC, and on-prem DC via Transit Gateway.




---

### 6. **You created EC2 and S3. How can you attach EC2 to S3? What policies are used?**

✅ **Steps:**
- Attach **IAM Role** to EC2 with **S3 access policy**.
- Use **AmazonS3ReadOnlyAccess** or custom policy for specific buckets.

📌 **Example Policy:**
```json
{
  "Effect": "Allow",
  "Action": ["s3:GetObject"],
  "Resource": ["arn:aws:s3:::mybucket/*"]
}
```

Steps:

Create an IAM Role with S3 permissions (e.g., AmazonS3ReadOnlyAccess).

Attach the role to the EC2 instance.

Example Policy:

json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:GetObject"],
    "Resource": ["arn:aws:s3:::my-bucket/*"]
  }]
}
Real-World Use:
A web server (EC2) reads static assets (images) from S3.


---

### 7. **What is a Trusted Role in IAM?**

✅ **Definition:** A **trusted role** allows **another AWS service or account** to assume that role.

📌 **Example:** EC2 assuming a role to access S3. The trust policy defines that EC2 can assume this role.


Purpose: Allows one AWS entity (e.g., EC2, Lambda) to assume another role.

Example:

An EC2 instance assumes a Backup-Role to access RDS.

A Lambda function assumes a DynamoDB-Role to write data.

Trust Policy (JSON):

json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": { "Service": "ec2.amazonaws.com" },
    "Action": "sts:AssumeRole"
  }]
}




---

### 8. **What is Backend in Terraform?**

✅ **Definition:** Backend is where **Terraform stores the state file**.

📌 **Example:** Use **S3 backend** for shared state, enabling team collaboration.


Purpose: Stores the state file (e.g., terraform.tfstate) remotely (S3, Terraform Cloud).

Why?:

Enables team collaboration.

Prevents local state file corruption.

Example (S3 Backend):

hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/network.tfstate"
    region = "us-east-1"
  }
}



---

### 9. **In Terraform, you created one EC2 instance. Now you want to create 10 instances — how?**

✅ **Use `count` parameter:**
```hcl
resource "aws_instance" "my_ec2" {
  count = 10
  ami           = "ami-xxxxx"
  instance_type = "t2.micro"
}
```

Use count or for_each:

hcl
# Using count
resource "aws_instance" "web" {
  count         = 10  # Creates 10 identical instances
  ami           = "ami-123456"
  instance_type = "t3.micro"
}

# Using for_each (unique instances)
resource "aws_instance" "web" {
  for_each      = toset(["web1", "web2", ..., "web10"])
  ami           = "ami-123456"
  instance_type = "t3.micro"
}





---

### 10. **Terraform is automated, but someone manually changed the resource. How do you troubleshoot?**

✅ **Steps:**
- Run `terraform plan`: Shows difference between state file and actual infrastructure.
- Run `terraform apply`: Syncs actual state to match code.
- Use `terraform refresh`: Updates state file with actual resource status.


Problem: Someone manually edits an AWS resource (e.g., changes EC2 type).
Solution:

Run terraform plan to detect drift.

Use terraform apply to revert changes.

Prevent future issues:

Set terraform apply -lock=true (locks state).

Use remote backends (e.g., S3 + DynamoDB locking).


---

### 11. **What is Deployment and StatefulSet in Kubernetes?**

| Feature        | Deployment                        | StatefulSet                          |
|----------------|-----------------------------------|---------------------------------------|
| Use Case       | Stateless apps (e.g., web apps)   | Stateful apps (e.g., DBs like MongoDB)|
| Pod Names      | Random                            | Stable, persistent (pod-0, pod-1...)  |
| Storage        | Shared                            | Persistent per pod (PVCs)            |

📌 **Example:**
- Use **Deployment** for Nginx.
- Use **StatefulSet** for MySQL.


Feature	Deployment	StatefulSet
Use Case	Stateless apps (e.g., web servers)	Stateful apps (e.g., databases)
Scaling	Pods are interchangeable	Pods have stable IDs (pod-0, pod-1)
Storage	Ephemeral	Persistent (PVCs)
Example:

Deployment: NGINX web servers (any pod can handle requests).

StatefulSet: MySQL cluster (each pod has its own data).



---

### 12. **What work do you do on Kubernetes Cluster?**

✅ Common tasks:
- Deploy applications using `kubectl` or YAML.
- Configure Services, Ingress for routing.
- Set up auto-scaling (HPA).
- Monitor logs and pod health.
- Update images via rolling deployments.
- Manage Secrets and ConfigMaps.



Common Tasks:

Scaling: kubectl scale deployment/web --replicas=5.

Updates: Rolling updates (kubectl set image deployment/web nginx=nginx:1.21).

Monitoring: kubectl top pods.

Troubleshooting: kubectl logs <pod>, kubectl describe pod <pod>.

Example:
A DevOps engineer:

Adds nodes (kubectl get nodes).

Deploys apps (kubectl apply -f app.yaml).

Checks health (kubectl get pods -w).

---

### 13. **Which version of Kubernetes are you using currently?**

✅ Example answer (update with your actual version):
> "I’m currently working with Kubernetes version **1.29.x** on Amazon EKS."



Latest Stable (2024): v1.28 (or newer).

Check Version:

sh
kubectl version --short
Best Practice:

Stay 1-2 versions behind the latest (for stability).

Example: Many enterprises use v1.25-v1.27.





---
📌 Summary Cheat Sheet
Topic	Key Answer
VPC Issues	Check SGs, NACLs, Route Tables, IGW.
NACL vs. SG	NACL=stateless (subnet), SG=stateful (EC2).
VPC Peering	Use Transit Gateway for >2 VPCs.
VPC Endpoint	Private S3/DynamoDB access.
Terraform	Use count/for_each for multiple EC2.
K8s	Deployments=stateless, StatefulSets=DBs.
