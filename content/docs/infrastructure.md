---
title: "Infrastructure"
weight: 1
# bookFlatSection: false
bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

## ArtBasket Infrastructure Provisioning with Terraform

## Introduction

This document describes the infrastructure provisioning for the ArtBasket project using Terraform. We leverage the AWS ecosystem to host our application, utilizing services like AWS Lambda, RDS, S3, DynamoDB, and Secrets Manager.

## Terraform Overview

Terraform is an infrastructure as code (IaC) tool that allows us to define and manage infrastructure resources in a declarative manner. By using Terraform, we can automate the provisioning of our AWS infrastructure, ensuring consistency and reproducibility.

## AWS Infrastructure Components

### AWS Lambda for Go API Functions

- **Description:** We use AWS Lambda to host our Go-based API functions. Lambda provides a serverless environment, allowing us to run our code without managing servers.
- **Terraform Configuration:**

  ```hcl
  resource "aws_lambda_function" "api_function" {
    function_name = "api_function"
    handler       = "main"
    runtime       = "go1.x"
    role          = aws_iam_role.lambda_exec_role.arn
    source_code_hash = filebase64sha256("path/to/zip/file")
    filename      = "path/to/zip/file"
  }

Notes: Ensure that the Go functions are compiled and packaged into a zip file before deploying them to Lambda.

### RDS for SQL Database

Description: Amazon RDS is used to host our SQL database, providing a managed relational database service.
Terraform Configuration:

```bash
resource "aws_db_instance" "database" {
  allocated_storage    = 20
  engine               = "mysql"
  instance_class       = "db.t2.micro"
  name                 = "artbasket_db"
  username             = "admin"
  password             = var.db_password
  publicly_accessible  = false
}

```

Notes: The database password is stored in a Terraform variable for security purposes.

### S3 Bucket for Image Storage

Description: Amazon S3 is used to store images and other static assets for the application.
Terraform Configuration:

```bash
resource "aws_s3_bucket" "image_bucket" {
  bucket = "artbasket-images"
  acl    = "private"
}

```

Notes: The bucket is set to private to ensure that the images are not publicly accessible.

### DynamoDB for Server Logs

Description: Amazon DynamoDB is used as a NoSQL database for storing server logs and other non-relational data.
Terraform Configuration:

```bash
resource "aws_dynamodb_table" "logs_table" {
  name           = "server_logs"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "log_id"
  attribute {
    name = "log_id"
    type = "S"
  }
}

```

Notes: DynamoDB is chosen for its scalability and performance in handling unstructured data.

### Secrets Manager for Sensitive Data

Description: AWS Secrets Manager is used to securely store and access sensitive information like database passwords.
Terraform Configuration:

```bash
resource "aws_secretsmanager_secret" "db_password" {
  name = "db_password"
}
resource "aws_secretsmanager_secret_version" "db_password_version" {
  secret_id     = aws_secretsmanager_secret.db_password.id
  secret_string = "{\"password\":\"your_password_here\"}"
}


```

Notes: The actual password should be replaced with a secure value and not hard-coded in the Terraform configuration.

## Conclusion

By using Terraform, we can efficiently provision and manage the infrastructure for the ArtBasket project in the AWS ecosystem. This approach ensures that our infrastructure is consistently deployed and easily maintainable.
