# Terraform Task: AWS Key Pair Creation

## Problem Statement

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