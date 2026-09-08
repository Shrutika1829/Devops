# Terraform Interview Questions & Answers — DevOps Engineer (1 Year Experience)

> A practical interview-preparation guide for Terraform, focused on the questions most commonly asked for a DevOps Engineer with around 1 year of experience.

---

## Table of Contents

1. [How to Answer Terraform Interview Questions](#how-to-answer-terraform-interview-questions)
2. [Terraform Fundamentals](#1-terraform-fundamentals)
3. [Terraform Commands](#2-terraform-commands)
4. [Terraform State & Backend](#3-terraform-state--backend)
5. [Variables, Locals & Outputs](#4-variables-locals--outputs)
6. [Dependencies](#5-dependencies)
7. [Modules](#6-modules)
8. [Lifecycle Management](#7-lifecycle-management)
9. [Import & Drift](#8-import--drift)
10. [Workspaces](#9-workspaces)
11. [Secrets & Security](#10-secrets--security)
12. [Git & Terraform](#11-git--terraform)
13. [Terraform + CI/CD](#12-terraform--cicd)
14. [Troubleshooting Scenarios](#13-troubleshooting-scenarios)
15. [Top 20 Must-Prepare Questions](#14-top-20-must-prepare-questions)
16. [Rapid Revision Cheat Sheet](#15-rapid-revision-cheat-sheet)

---

# How to Answer Terraform Interview Questions

For a 1-year DevOps interview, do not give only a textbook definition. A strong answer usually follows:

### PREP Technique

**P — Point:** Give the direct answer first.  
**R — Reason:** Explain why it is used.  
**E — Example:** Give a practical example.  
**P — Practical/Project:** Explain how you would use it in a real DevOps environment.

Example:

> **Interviewer:** What is Terraform?
>
> **Answer:** Terraform is an Infrastructure as Code tool used to provision and manage infrastructure using configuration files. We use it because it makes infrastructure repeatable, version-controlled and automated. For example, instead of manually creating an EC2 instance, VPC and security group from the AWS console, I can define them in Terraform and provision them using `terraform apply`. In a DevOps environment, I would store the Terraform code in Git and run plan/apply through a CI/CD pipeline with proper approval for production.

### STAR Technique

Use **STAR** mainly for questions such as:

- "Tell me about a problem you solved using Terraform."
- "Tell me about a Terraform incident."
- "How did you use Terraform in your project?"

**S — Situation:** What was the problem?  
**T — Task:** What were you responsible for?  
**A — Action:** What did you do?  
**R — Result:** What was the outcome?

> **Important:** Do not claim a Terraform project you have not actually worked on. If you are learning Terraform, say "In my practice project..." rather than presenting it as production experience.

---

# 1. Terraform Fundamentals

## Q1. What is Terraform?

### Interview Answer — PREP

**Point:**  
Terraform is an **Infrastructure as Code (IaC)** tool developed by HashiCorp. It allows us to define, provision and manage infrastructure using configuration files.

**Reason:**  
We use Terraform to automate infrastructure creation, maintain consistency and keep infrastructure configuration in version control.

**Example:**  
For AWS, I can define resources such as VPC, subnet, EC2, security groups, ALB and RDS in Terraform.

**Practical:**  
A typical workflow is:

```bash
terraform init
terraform plan
terraform apply
```

Instead of manually creating infrastructure from the AWS console, Terraform can create it from code.

---

## Q2. Why do we use Terraform?

### Interview Answer — PREP

**Point:**  
We use Terraform to automate and manage infrastructure as code.

**Reason:**  
The major benefits are:

- Automation
- Repeatability
- Version control
- Consistency
- Reusability
- Dependency management
- Easier infrastructure changes
- Support for multiple cloud/service providers

**Example:**  
If I need the same VPC and application infrastructure in development, staging and production, I can use reusable Terraform code rather than manually creating every resource.

**Practical:**  
I can store the Terraform code in Git and run it through Jenkins or another CI/CD pipeline.

---

## Q3. What is Infrastructure as Code?

### Interview Answer — PREP

**Point:**  
Infrastructure as Code means managing infrastructure through code or configuration files instead of manually creating resources.

**Reason:**  
It makes infrastructure repeatable, reviewable and version-controlled.

**Example:**

```text
Terraform Code
      |
      v
Terraform Provider
      |
      v
AWS API
      |
      v
VPC / EC2 / ALB / RDS
```

**Practical:**  
If an environment needs to be recreated, I can use the same Terraform configuration instead of manually rebuilding it.

---

## Q4. Terraform vs AWS CloudFormation?

| Terraform | CloudFormation |
|---|---|
| Developed by HashiCorp | Developed by AWS |
| Multi-cloud | Primarily AWS |
| Uses HCL | Uses YAML/JSON |
| Uses providers | AWS-native service |
| Has Terraform state | Uses CloudFormation stacks |
| Can manage many providers | Strong AWS integration |

### Interview Answer

> Terraform is a third-party IaC tool that supports multiple providers such as AWS, Azure, GCP and Kubernetes. CloudFormation is AWS's native IaC service. If an organization works across multiple platforms, Terraform can be a strong choice. If the organization is heavily AWS-focused and wants deep AWS-native integration, CloudFormation is also a good option.

---

## Q5. What is a Terraform provider?

### Interview Answer — PREP

**Point:**  
A provider is a Terraform plugin that allows Terraform to communicate with an external API or platform.

**Reason:**  
Terraform itself does not directly know how to create AWS, Azure or Kubernetes resources. The provider handles communication with those APIs.

**Example:**

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

**Practical:**  
The AWS provider allows Terraform to create and manage AWS resources such as EC2, VPC, S3, IAM and RDS.

---

## Q6. What is a Terraform resource?

### Interview Answer — PREP

**Point:**  
A resource represents an infrastructure object that Terraform creates or manages.

**Example:**

```hcl
resource "aws_instance" "web" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t3.micro"
}
```

Here:

- `aws_instance` = resource type
- `web` = local Terraform resource name

**Practical:**  
Terraform tracks this EC2 instance in its state and compares the desired configuration with the current infrastructure.

---

## Q7. What is a Terraform data source?

### Interview Answer — PREP

**Point:**  
A data source retrieves information about existing infrastructure or external data; it does not create the resource.

**Example:**

```hcl
data "aws_vpc" "existing" {
  id = "vpc-123456"
}
```

Then I can reference:

```hcl
data.aws_vpc.existing.id
```

**Key difference:**

```text
Resource    -> Creates/manages infrastructure
Data source -> Reads existing information
```

---

# 2. Terraform Commands

## Q8. What is `terraform init`?

### Interview Answer

> `terraform init` initializes a Terraform working directory. It downloads the required providers and modules and initializes the configured backend.

Example:

```bash
terraform init
```

I normally run it before planning or applying a new Terraform configuration.

---

## Q9. What is `terraform plan`?

### Interview Answer

> `terraform plan` compares the desired configuration with the current state and infrastructure information and shows the changes Terraform intends to make. It is a preview and does not normally make those changes.

Example:

```bash
terraform plan
```

Typical output may show:

```text
Plan: 2 to add, 1 to change, 0 to destroy.
```

I use the plan to review changes before applying them, especially in production.

---

## Q10. What is `terraform apply`?

### Interview Answer

> `terraform apply` executes the Terraform plan and makes the required infrastructure changes.

Typical workflow:

```bash
terraform init
terraform plan
terraform apply
```

In production, I would normally review the plan and follow the organization's approval process before applying changes.

---

## Q11. What is `terraform destroy`?

### Interview Answer

> `terraform destroy` removes resources managed by the current Terraform configuration and state.

```bash
terraform destroy
```

I would never run it casually against production because it can cause destructive changes.

---

## Q12. What is `terraform validate`?

### Interview Answer

> `terraform validate` checks whether the Terraform configuration is syntactically and structurally valid.

```bash
terraform validate
```

It is useful as an early validation step in CI/CD.

---

## Q13. What is `terraform fmt`?

### Interview Answer

> `terraform fmt` automatically formats Terraform files according to Terraform's standard formatting.

```bash
terraform fmt
```

I can include it in a CI pipeline to maintain consistent code formatting.

---

## Q14. What is `terraform show`?

### Interview Answer

> `terraform show` displays the Terraform state or a saved plan in a human-readable format.

Examples:

```bash
terraform show
```

or:

```bash
terraform show planfile
```

---

# 3. Terraform State & Backend

## Q15. What is Terraform state?

### Interview Answer — PREP

**Point:**  
Terraform state is the record Terraform uses to track the infrastructure resources it manages.

**Reason:**  
Terraform needs to know the relationship between the configuration and real infrastructure so it can determine what needs to be created, changed or destroyed.

**Example:**

```text
Terraform Configuration
        |
        v
Terraform State
        |
        v
Real Infrastructure
```

The default local state file is:

```text
terraform.tfstate
```

**Practical:**  
In a team environment, I would store state in a secure remote backend instead of relying on local state.

---

## Q16. Why is Terraform state important?

### Interview Answer

> State is important because Terraform uses it to track managed resources, resource IDs, attributes and relationships. It helps Terraform calculate the difference between the desired configuration and the current infrastructure.

For example, if Terraform already manages an EC2 instance, state helps Terraform know which AWS instance corresponds to the Terraform resource.

---

## Q17. Should we store `terraform.tfstate` in Git?

### Interview Answer

> Generally, no. Terraform state should not normally be committed to Git because it can contain sensitive information and local state is not suitable for concurrent team operations.

For a team, I would use a remote backend with appropriate access controls, encryption, versioning/backup and locking where supported.

---

## Q18. What is a remote backend?

### Interview Answer — PREP

**Point:**  
A backend determines where Terraform stores its state.

**Reason:**  
A remote backend allows a team to share state and provides better collaboration and operational control.

**Example:**

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "ap-south-1"
  }
}
```

**Practical:**  
For AWS, S3 is commonly used as the remote state store. Depending on the Terraform/AWS setup, state locking can be configured using supported locking mechanisms such as S3 native locking; older setups commonly used DynamoDB-based locking.

---

## Q19. Why do we need state locking?

### Interview Answer

> State locking prevents conflicting Terraform operations from modifying the same state at the same time.

For example, if two engineers run `terraform apply` simultaneously against the same state, concurrent changes could cause problems. Locking helps ensure only the appropriate operation modifies the state at a time.

---

## Q20. What happens if two developers run `terraform apply` at the same time?

### Interview Answer — Scenario

> If both operations use the same remote state and locking is configured, one operation should acquire the state lock while the other waits or fails depending on the situation. This prevents both operations from modifying the same state concurrently.
>
> If state locking is not configured, concurrent operations can lead to race conditions and state problems. Therefore, for team environments I would use a proper remote backend with locking.

---

# 4. Variables, Locals & Outputs

## Q21. What are Terraform variables?

### Interview Answer

> Variables are inputs that make Terraform configurations reusable and configurable without changing the main resource definitions.

Example:

```hcl
variable "instance_type" {
  type    = string
  default = "t3.micro"
}
```

Usage:

```hcl
resource "aws_instance" "web" {
  instance_type = var.instance_type
}
```

---

## Q22. How can you pass Terraform variable values?

Common methods include:

### Command line

```bash
terraform apply -var="environment=prod"
```

### `terraform.tfvars`

```hcl
environment = "prod"
```

### Auto-loaded variable files

```text
terraform.tfvars
*.auto.tfvars
```

### Environment variable

```bash
export TF_VAR_environment=prod
```

---

## Q23. What is `terraform.tfvars`?

### Interview Answer

> `terraform.tfvars` is a commonly used variable values file. Terraform automatically loads `terraform.tfvars` and matching `*.auto.tfvars` files.

Example:

```hcl
region        = "ap-south-1"
instance_type = "t3.micro"
environment   = "dev"
```

I would avoid committing sensitive variable files containing secrets.

---

## Q24. What is the difference between variable and local?

### Interview Answer — PREP

**Point:**  
A variable is an input to Terraform, while a local is an internally defined reusable value.

**Example:**

```hcl
variable "environment" {
  type = string
}

locals {
  common_name = "myapp-${var.environment}"
}
```

**Simple way to remember:**

```text
Variable -> Input
Local    -> Internal reusable value
```

---

## Q25. What is a Terraform output?

### Interview Answer

> An output exposes useful information from Terraform after resources are created.

Example:

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

After apply, Terraform can display the EC2 public IP.

Outputs are useful for exposing:

- EC2 IP
- ALB DNS
- VPC ID
- RDS endpoint
- Resource IDs

---

# 5. Dependencies

## Q26. How does Terraform understand resource dependencies?

### Interview Answer — PREP

**Point:**  
Terraform builds a dependency graph based on references between resources.

**Example:**

```hcl
resource "aws_instance" "web" {
  subnet_id = aws_subnet.public.id
}
```

Terraform sees that the EC2 instance references the subnet, so the subnet must exist before the EC2 instance can be created.

Conceptually:

```text
VPC
 |
 v
Subnet
 |
 v
EC2
```

---

## Q27. What is `depends_on`?

### Interview Answer

> `depends_on` explicitly tells Terraform that one resource depends on another when the dependency cannot be inferred automatically.

Example:

```hcl
resource "aws_instance" "web" {
  depends_on = [
    aws_iam_role_policy.example
  ]
}
```

I use it only when necessary because Terraform's implicit dependencies are generally preferable.

---

# 6. Modules

## Q28. What is a Terraform module?

### Interview Answer — PREP

**Point:**  
A Terraform module is a reusable collection of Terraform configuration files.

**Reason:**  
Modules help reduce duplicate code and standardize infrastructure.

**Example:**

```text
modules/
├── vpc/
├── ec2/
├── rds/
└── security-group/
```

Usage:

```hcl
module "vpc" {
  source = "./modules/vpc"
}
```

**Practical:**  
Instead of writing VPC code separately for every environment, I can create a reusable VPC module and pass environment-specific inputs.

---

## Q29. Why do we use Terraform modules?

### Interview Answer

> We use modules for reusability, standardization, maintainability and reducing duplicate code.

For example, an organization can have a standard VPC module and reuse it across multiple environments.

---

## Q30. What is the difference between root module and child module?

### Interview Answer

> The root module is the Terraform configuration from which I run Terraform commands. A child module is a reusable module called by the root module.

Example:

```hcl
module "vpc" {
  source = "./modules/vpc"
}
```

Here the main configuration is the root module and `./modules/vpc` is a child module.

---

# 7. Lifecycle Management

## Q31. What is Terraform lifecycle?

### Interview Answer

> The `lifecycle` block allows us to control how Terraform creates, updates and destroys resources.

Important lifecycle arguments include:

```hcl
lifecycle {
  create_before_destroy = true
  prevent_destroy       = true
  ignore_changes        = [
    tags
  ]
}
```

---

## Q32. What is `create_before_destroy`?

### Interview Answer

> `create_before_destroy` tells Terraform to create a replacement resource before destroying the old one when replacement is required.

Example:

```hcl
lifecycle {
  create_before_destroy = true
}
```

It can help reduce downtime, but the resource and architecture must support having old and new resources coexist.

---

## Q33. What is `prevent_destroy`?

### Interview Answer

> `prevent_destroy` prevents Terraform from destroying a resource through Terraform.

Example:

```hcl
lifecycle {
  prevent_destroy = true
}
```

This can be useful for critical resources such as production databases.

---

## Q34. What is `ignore_changes`?

### Interview Answer

> `ignore_changes` tells Terraform to ignore changes to specified resource attributes.

Example:

```hcl
lifecycle {
  ignore_changes = [
    tags
  ]
}
```

It can be useful when another system is intentionally managing a specific attribute.

---

# 8. Import & Drift

## Q35. What is Terraform import?

### Interview Answer — PREP

**Point:**  
Terraform import brings an existing infrastructure resource under Terraform management by associating it with a Terraform resource in state.

**Example:**

```bash
terraform import aws_instance.web i-123456789
```

**Practical:**  
If an EC2 instance was created manually in AWS but now needs to be managed through Terraform, I can first write the appropriate resource configuration and then import the existing instance.

**Important:**  
Import does not automatically mean the Terraform configuration is perfect. I still need to ensure the configuration correctly represents the imported resource.

---

## Q36. Can Terraform import automatically generate perfect Terraform code?

### Interview Answer

> No. Import primarily associates an existing resource with Terraform state. I still need to create and review the Terraform configuration. Terraform has configuration-generation capabilities in some workflows, but generated configuration should be reviewed and cleaned up before being treated as production code.

---

## Q37. What is Terraform drift?

### Interview Answer — PREP

**Point:**  
Drift occurs when the real infrastructure differs from what Terraform configuration expects.

**Example:**

```text
Terraform:
instance_type = t3.micro

AWS:
instance_type = t3.large
```

If the resource is managed by Terraform, a subsequent plan may detect the difference.

**Practical:**  
I would investigate why the change happened and then decide whether to update Terraform, revert the manual change, or intentionally manage the attribute differently.

---

# 9. Workspaces

## Q38. What are Terraform workspaces?

### Interview Answer

> Terraform workspaces allow multiple state instances to be associated with the same Terraform configuration.

Example:

```bash
terraform workspace list
terraform workspace select dev
```

Possible workspaces:

```text
dev
stage
prod
```

However, for complex production environments, I would not automatically choose workspaces. Separate configurations/directories and clearly separated state are often easier to control.

---

# 10. Secrets & Security

## Q39. How do you manage secrets in Terraform?

### Interview Answer — PREP

**Point:**  
I avoid hard-coding secrets in Terraform files.

**Reason:**  
Terraform configuration and state can be stored, logged or accessed by multiple systems.

**Approaches:**

- AWS Secrets Manager
- AWS Systems Manager Parameter Store
- CI/CD secret stores
- Environment variables
- Sensitive Terraform variables

Example:

```hcl
variable "db_password" {
  sensitive = true
}
```

**Important:**  
`sensitive = true` mainly prevents Terraform from displaying the value in normal CLI output. It does not guarantee the secret is absent from state. Therefore, state must also be secured.

---

## Q40. How would you secure Terraform state?

### Interview Answer

I would:

1. Use a remote backend.
2. Enable encryption at rest.
3. Restrict IAM permissions.
4. Enable versioning/backup where appropriate.
5. Use state locking where supported.
6. Avoid exposing state publicly.
7. Restrict who can read state because it may contain sensitive values.
8. Never commit state to Git.

---

# 11. Git & Terraform

## Q41. What Terraform files should be committed to Git?

### Interview Answer

Generally, I would commit:

```text
*.tf
modules/
.terraform.lock.hcl
*.tfvars.example
```

I would normally exclude:

```text
.terraform/
terraform.tfstate
terraform.tfstate.*
*.tfstate
```

I would also avoid committing real secret-containing `.tfvars` files.

---

## Q42. Why should we commit `.terraform.lock.hcl`?

### Interview Answer

> `.terraform.lock.hcl` records the selected provider versions and checksums. Committing it helps ensure consistent provider installation across developers and CI/CD environments.

---

# 12. Terraform + CI/CD

## Q43. How would you integrate Terraform with Jenkins?

### Interview Answer — PREP

**Point:**  
I would integrate Terraform into Jenkins as a pipeline with validation, planning, approval and deployment stages.

**Example pipeline:**

```text
Developer
    |
    v
Git Repository
    |
    v
Jenkins
    |
    +--> terraform fmt
    |
    +--> terraform validate
    |
    +--> terraform init
    |
    +--> terraform plan
    |
    +--> Approval
    |
    +--> terraform apply
    |
    v
AWS
```

**Practical:**  
For production, I would keep `terraform plan` as a reviewable artifact and require the organization's approval before apply.

---

## Q44. Should `terraform apply` run automatically on every Git commit?

### Interview Answer

> I would not blindly auto-apply production changes on every commit. A safer approach is to run formatting, validation and plan automatically, review the plan, and then use an approval gate before production apply. Lower environments may use more automation depending on the organization's deployment policy.

---

# 13. Troubleshooting Scenarios

## Q45. Terraform plan says it wants to destroy an important production resource. What will you do?

### Interview Answer — STAR/Problem-Solving

**Situation:**  
Terraform plan shows that a critical production resource will be destroyed.

**Task:**  
My responsibility is to prevent an unintended destructive change.

**Action:**

1. I would not immediately run `terraform apply`.
2. I would inspect the exact plan output.
3. I would identify which configuration change is causing replacement/destruction.
4. I would check recent Git changes.
5. I would compare Terraform state with the actual AWS resource.
6. I would check whether the resource has changed manually.
7. I would check lifecycle settings.
8. If required, I would use `prevent_destroy` for appropriate critical resources.
9. I would get the required approval before making a production change.

**Result:**

> This approach helps prevent accidental production deletion and ensures the root cause is understood before applying the change.

---

## Q46. Two developers run Terraform apply simultaneously. What will you do?

### Interview Answer

> First I would check whether both operations are using the same remote backend and state. If state locking is configured, one operation should hold the lock while the other waits or fails. I would avoid manually forcing changes without understanding the lock situation. For a team environment, I would ensure remote state and locking are properly configured.

---

## Q47. Terraform state file is accidentally deleted. What will you do?

### Interview Answer — Scenario

> First I would determine whether the project uses a remote backend. If it does, I would recover the state from the remote backend/version history or backup mechanism. I would not immediately recreate the infrastructure.
>
> If the state cannot be recovered, I would carefully map existing infrastructure back into Terraform using `terraform import` where appropriate, then run `terraform plan` and reconcile differences.

---

## Q48. Someone manually changes an AWS resource managed by Terraform. What happens?

### Interview Answer

> This creates infrastructure drift. During a subsequent refresh/plan, Terraform can detect differences between the managed configuration and the actual infrastructure. Terraform may then propose changing the resource back to the declared configuration.
>
> I would first determine whether the manual change was intentional. If it was intentional, I would update the Terraform configuration or management approach rather than repeatedly making manual changes.

---

## Q49. Terraform wants to recreate an EC2 instance instead of updating it. Why?

### Interview Answer

> Some resource attributes cannot be changed in place. When such an attribute changes, Terraform may mark the resource for replacement.

Example plan:

```text
-/+ aws_instance.web
```

This means Terraform plans to replace the resource.

I would inspect the plan to identify the exact attribute causing the replacement and then assess the impact before applying.

---

## Q50. How would you avoid downtime when Terraform replaces a resource?

### Interview Answer

> It depends on the architecture. If the resource supports it, I can use `create_before_destroy`.

```hcl
lifecycle {
  create_before_destroy = true
}
```

For production applications, I would also prefer architecture-level high availability such as an ALB with an Auto Scaling Group, rolling replacement or blue/green deployment where appropriate.

---

## Q51. An existing AWS resource needs to be managed by Terraform. What will you do?

### Interview Answer

I would:

1. Write the Terraform resource configuration.
2. Verify the resource details.
3. Import the existing resource.

Example:

```bash
terraform import aws_instance.web i-123456789
```

4. Run:

```bash
terraform plan
```

5. Reconcile the configuration until the plan represents the desired state.

---

## Q52. Terraform creates resources in the wrong AWS region. How will you troubleshoot?

### Interview Answer

I would check:

1. AWS provider configuration.
2. Provider aliases.
3. `AWS_REGION` / `AWS_DEFAULT_REGION`.
4. CI/CD environment variables.
5. AWS credentials/account.
6. Module provider configuration.
7. Whether the resource itself is regional.

Example:

```hcl
provider "aws" {
  region = "ap-south-1"
}
```

I would also verify the AWS account and region in which Terraform is authenticated.

---

## Q53. Terraform apply fails halfway through. What happens?

### Interview Answer

> Terraform may have already created or modified some resources before the failure. Successful operations are reflected in state, while the failed operation needs investigation.

My approach:

```text
Read error
   ↓
Identify root cause
   ↓
Fix issue
   ↓
terraform plan
   ↓
Review
   ↓
terraform apply
```

I would not blindly rerun commands without understanding the error.

---

## Q54. Terraform says "No changes", but AWS infrastructure is different. What will you check?

### Interview Answer

I would check:

1. Whether I am using the correct Terraform state.
2. Whether I am authenticated to the correct AWS account.
3. Whether the provider region is correct.
4. Whether the resource is actually managed by this state.
5. Whether `ignore_changes` is configured.
6. Whether the actual difference is an attribute Terraform does not manage.
7. Whether the state is up to date.

The key point is to compare:

```text
Configuration
     ↕
State
     ↕
Actual AWS infrastructure
```

---

## Q55. Terraform says a resource already exists. What could be the reason?

### Interview Answer

> A common reason is that the resource already exists in AWS but is not represented in the current Terraform state.

If the resource should be managed by Terraform, I would verify the configuration and then use:

```bash
terraform import
```

After importing, I would run:

```bash
terraform plan
```

to reconcile the configuration and actual resource.

---

# 14. Top 20 Must-Prepare Questions

If you have limited interview preparation time, master these first:

| Priority | Question |
|---|---|
| ⭐⭐⭐ | What is Terraform? |
| ⭐⭐⭐ | Why do we use Terraform? |
| ⭐⭐⭐ | Terraform vs CloudFormation |
| ⭐⭐⭐ | What is a provider? |
| ⭐⭐⭐ | What is a resource? |
| ⭐⭐⭐ | What is a data source? |
| ⭐⭐⭐ | What is `terraform init`? |
| ⭐⭐⭐ | What is `terraform plan`? |
| ⭐⭐⭐ | What is `terraform apply`? |
| ⭐⭐⭐ | What is Terraform state? |
| ⭐⭐⭐ | Why use remote state? |
| ⭐⭐⭐ | What is state locking? |
| ⭐⭐⭐ | What is a backend? |
| ⭐⭐⭐ | What are variables? |
| ⭐⭐⭐ | What are outputs? |
| ⭐⭐⭐ | What are Terraform modules? |
| ⭐⭐⭐ | What is `depends_on`? |
| ⭐⭐⭐ | What is Terraform lifecycle? |
| ⭐⭐⭐ | What is Terraform import? |
| ⭐⭐⭐ | What is Terraform drift? |

---

# 15. Rapid Revision Cheat Sheet

## Core Terraform Workflow

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
```

---

## Important Terraform Files

```text
main.tf              -> Main resources/configuration
variables.tf         -> Variable definitions
terraform.tfvars     -> Variable values
outputs.tf           -> Outputs
providers.tf         -> Provider configuration
versions.tf          -> Terraform/provider requirements
.terraform.lock.hcl  -> Provider dependency lock file
terraform.tfstate    -> Terraform state
```

---

## Terraform Concepts

```text
Provider
   ↓
Resource
   ↓
State
   ↓
Plan
   ↓
Apply
```

---

## Resource vs Data Source

```text
Resource
    ↓
Create / manage infrastructure

Data Source
    ↓
Read existing information
```

---

## Variable vs Local

```text
Variable
    ↓
Input

Local
    ↓
Internal reusable value
```

---

## Terraform State

Remember:

```text
Configuration
      +
State
      +
Real Infrastructure
      ↓
Terraform Plan
```

---

## Module

```text
Reusable Terraform code
        ↓
Less duplication
        ↓
Standardization
        ↓
Easier maintenance
```

---

## Lifecycle

```hcl
lifecycle {
  create_before_destroy = true
  prevent_destroy       = true
  ignore_changes        = [...]
}
```

---

## Import

```bash
terraform import RESOURCE_TYPE.NAME RESOURCE_ID
```

Example:

```bash
terraform import aws_instance.web i-123456789
```

Then:

```bash
terraform plan
```

---

# Final Interview Strategy

For a **1-year DevOps Engineer** interview, I would prepare Terraform in this order:

### Level 1 — Must Know

- Terraform definition
- IaC
- Providers
- Resources
- Data sources
- `init`
- `plan`
- `apply`
- `destroy`
- Variables
- Outputs

### Level 2 — Interview Favorites

- State
- Remote backend
- State locking
- Modules
- `depends_on`
- Lifecycle
- Import
- Drift
- Workspaces
- `.terraform.lock.hcl`

### Level 3 — Practical DevOps

- Terraform + Git
- Terraform + Jenkins
- Remote state
- Secrets
- Production approvals
- CI/CD pipeline
- Environment separation
- Troubleshooting

### Level 4 — Scenario Questions

Be ready to answer:

```text
Terraform wants to destroy production DB
        ↓
State file deleted
        ↓
Manual AWS change
        ↓
Resource already exists
        ↓
Wrong AWS region
        ↓
No changes but infrastructure differs
        ↓
EC2 replacement
        ↓
Apply fails halfway
        ↓
Two engineers run apply
```

---

# One-Line Interview Formula

When answering Terraform questions, remember:

> **Definition → Why → Example → Practical usage**

For troubleshooting:

> **Problem → Investigation → Action → Validation → Result**

For experience/project questions:

> **Situation → Task → Action → Result (STAR)**

This keeps your answers structured, confident and easy for the interviewer to follow.
