# MultiCloud, DevOps & AI Challenge - Day 1 - Experienced

<aside>
💡 Remember to use the class replay as a reference.
</aside>

## Streamlined Guide: Using Claude as AI Assistant to Terraform

### Step 1: Use Claude to Generate Terraform Code

1. Start a conversation with Claude.
2. Ask Claude to create Terraform code for an S3 bucket. Use a prompt like:
   > "Please provide Terraform code to create an S3 bucket in AWS with a unique name."
3. Claude should generate code similar to this:

```hcl
provider "aws" {
  region = "us-west-2"  # Replace with your desired region
}

resource "random_pet" "bucket_name" {
  length    = 2
  separator = "-"
}

resource "aws_s3_bucket" "example_bucket" {
  bucket = "my-unique-bucket-${random_pet.bucket_name.id}"

  # Enable versioning
  versioning {
    enabled = true
  }

  # Enable server-side encryption
  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }

  # Optional: Add tags
  tags = {
    Name        = "My Unique Bucket"
    Environment = "Development"
  }
}

# Optional: Block public access
resource "aws_s3_bucket_public_access_block" "bucket_access_block" {
  bucket = aws_s3_bucket.example_bucket.id

  block_public_acls   = true
  block_public_policy = true
  ignore_public_acls  = true
  restrict_public_buckets = true
}

output "bucket_name" {
  value = aws_s3_bucket.example_bucket.id
}
```

. Save this code for use in Step 5

### Step 2: Create IAM Role for EC2

1. Log in to the AWS Management Console.
   ![AWS Console Login](images/login.png)

2. Navigate to the IAM dashboard.
   ![IAM Dashboard](images/iam-dashboard.png)

3. Click "Roles" in the left sidebar, then "Create role".
   ![Create Role](images/roles.png)

4. Choose "AWS service" as the trusted entity type and "EC2" as the use case.
   ![AWS Servive](images/aws-service.png)

5. Search for and attach the "AdministratorAccess" policy
   ![Attach Policy](images/AdministratorAccess.png)
   Note: In a production environment, use a more restricted policy.

6. Name the role "EC2Admin" and provide a description.
   ![Role Name](images/Ec2Admin.png)
   Review and create the role.

### Step 3: Launch EC2 Instance

1. Go to the EC2 dashboard in the AWS Management Console.

2. Click "Launch Instance".
   ![Launch Instance](images/ec2dashboard.png)

3. Choose an Amazon Linux 2 AMI.
   ![AMI](images/ami.png)

4. Select a t2.micro instance type.
   ![t2](images/t2.png)

5. Configure instance details:

   - Network: Default VPC
   - Subnet: Any available
   - Auto-assign Public IP: Enable
   - IAM role: Select "EC2Admin"
     ![default](images/default.png)
     ![EC2Admin-role](images/EC2Admin-role.png)

6. Keep default storage settings.

7. Add a tag: Key="Name", Value="workstation".
   ![tag](images/tag.png)

8. Create a security group allowing SSH access from EC2 Connect IP.
   ![security](images/sg.png)

9. Review and launch, selecting or creating a key pair.
   ![sucess](images/sucess.png)
