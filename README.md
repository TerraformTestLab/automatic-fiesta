# automatic-fiesta

A Terraform workspace that provisions an AWS S3 bucket using Doormat for credential management.

## Overview

This Terraform configuration demonstrates the use of Hashicorp's Doormat provider to securely manage AWS credentials. It provisions an S3 bucket with configurable naming, environment tags, and additional custom tags.

## Prerequisites

- Terraform >= 1.0
- AWS account with appropriate IAM permissions
- Doormat credentials provider access
- Terraform Cloud account with `SujaysTerraformLab` organization

## Configuration

### Providers

- **AWS Provider**: Version ~> 5.0
  - Region: us-east-1
  - Credentials: Managed via Doormat provider
  
- **Doormat Provider**: Version ~> 0.0.2
  - Provides temporary AWS credentials for the specified IAM role

### Resources

#### S3 Bucket

- **Name**: Configurable via `bucket_name` variable
- **Tags**: Automatically includes `Environment` and `ManagedBy` tags, plus any additional custom tags

## Variables

| Variable | Description | Type | Default |
| --- | --- | --- | --- |
| `bucket_name` | Name of the S3 bucket | string | Required |
| `environment` | Environment name (e.g., dev, staging, prod) | string | dev |
| `tags` | Additional tags to apply to the S3 bucket | map(string) | {} |

## Outputs

| Output | Description |
| --- | --- |
| `bucket_name` | Name of the created S3 bucket |
| `bucket_arn` | ARN of the created S3 bucket |

## Usage

1. Set up your Terraform Cloud workspace connection
2. Configure the required variables (at minimum, `bucket_name`)
3. Run `terraform plan` to review changes
4. Run `terraform apply` to create the S3 bucket

### Example terraform.tfvars

```hcl
bucket_name = "my-application-bucket"
environment = "dev"
tags = {
  Team    = "Platform"
  Project = "DataPipeline"
}
```

## Security

- AWS credentials are managed through Doormat, providing temporary credential rotation
- The IAM role used is scoped to: `arn:aws:iam::xxxxxxxxxxxx:role/sample_dev-custom_role`
- S3 bucket configuration includes environment and managed-by tags for tracking

## License

See LICENSE file for details
