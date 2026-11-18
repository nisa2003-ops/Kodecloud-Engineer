# Terraform Task: AWS Key Pair Creation

## Problem Statement 01

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create a key pair using Terraform with the following requirements:
*   Name of the key pair should be `xfusion-kp`.
*   Key pair type must be `rsa`.
*   The private key file should be saved under `/home/bob/xfusion-kp.pem`.
*   The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

---

## Solution

### File Path: `/home/bob/terraform/main.tf`

```hcl
# 1. Generate an RSA Private Key
resource "tls_private_key" "rsa_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

# 2. Create the AWS Key Pair using the generated public key
resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.rsa_key.public_key_openssh
}

# 3. Save the Private Key locally under /home/bob/xfusion-kp.pem
resource "local_file" "private_key" {
  content         = tls_private_key.rsa_key.private_key_pem
  filename        = "/home/bob/xfusion-kp.pem"
  file_permission = "0400"
}
```

---

## Steps to Execute

1. **Navigate to the Terraform working directory:**
   ```bash
   cd /home/bob/terraform
   ```

2. **Initialize Terraform:**
   ```bash
   terraform init
   ```

3. **Apply the configuration:**
   ```bash
   terraform apply -auto-approve
   ```

4. **Verify key file generation:**
   ```bash
   ls -la /home/bob/xfusion-kp.pem
   ```

## Problem Statement 02

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.
Use Terraform to create a security group under the default VPC with the following requirements:
1) The name of the security group must be datacenter-sg.
2) The description must be Security group for Nautilus App Servers.
3) Add an inbound rule of type HTTP, with a port range of 80, and source CIDR range 0.0.0.0/0.
4) Add another inbound rule of type SSH, with a port range of 22, and source CIDR range 0.0.0.0/0.
   Ensure that the security group is created in the us-east-1 region using Terraform. The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.
   Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

---

## Solution

### File Path: `/home/bob/terraform/main.tf`

```hcl
# Fetch the default VPC ID dynamically
data "aws_vpc" "default" {
  default = true
}

resource "aws_security_group" "datacenter_sg" {
  name        = "datacenter-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "HTTP Inbound"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "SSH Inbound"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

---

## Steps to Execute

1. **Navigate to the Terraform working directory:**
   ```bash
   cd /home/bob/terraform
   ```

2. **Initialize Terraform:**
   ```bash
   terraform init
   ```

3. **Apply the configuration:**
   ```bash
   terraform apply -auto-approve
   ```

## Problem Statement 03

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Create a VPC named devops-vpc in region us-east-1 with any IPv4 CIDR block through terraform.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

---

## Solution

### File Path: `/home/bob/terraform/main.tf`

```hcl
resource "aws_vpc" "devops_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "devops-vpc"
  }
}
```

---

## Steps to Execute

1. **Navigate to the Terraform working directory:**
   ```bash
   cd /home/bob/terraform
   ```

2. **Initialize Terraform:**
   ```bash
   terraform init
   ```

3. **Apply the configuration:**
   ```bash
   terraform apply -auto-approve
   ```

## Problem Statement 04

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud.Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

Create a VPC named xfusion-vpc in us-east-1 region with 192.168.0.0/24 IPv4 CIDR using terraform.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.

---

## Solution

### File Path: `/home/bob/terraform/main.tf`

```hcl
resource "aws_vpc" "xfusion_vpc" {
  cidr_block = "192.168.0.0/24"

  tags = {
    Name = "xfusion-vpc"
  }
}
```

---

## Steps to Execute

1. **Navigate to the Terraform working directory:**
   ```bash
   cd /home/bob/terraform
   ```

2. **Initialize Terraform:**
   ```bash
   terraform init
   ```

3. **Apply the configuration:**
   ```bash
   terraform apply -auto-approve
   ```