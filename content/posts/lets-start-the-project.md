---
author: "Hasan Ozgan"
date: 2020-04-25
title: Let's start the project
draft: false
---

## Introduction

I've got a domain for that project. Domain is `ac.tivi.st`. I think so good that is good name for a calendar/event application project.
I'll use AWS platform for cloud requirement. Besides, each service's project structure will be clean architecture style.

As the first step, I want to create a simple working version.

- I've bought domain (tivi.st)
- I've created an AWS account.
- I've created a terraform.io account for terraform state. (terraform is a provision and management tool for cloud)
- I've installed terraform, aws-cli tools.


## Domain Setup with Terraform

Firstly, I created new repo for infrastructure. After I applied, terraform project layout like below;

```
.
└── aws
    ├── modules
    │   ├── cognito-userpool
    │   ├── eks-cluster
    │   └── vpc
    └── providers
        ├── 010-route53
        ├── 020-vpc
        ├── 030-eks
        └── 040-cognito-userpool
```

First I've created DNS setup for `tivi.st` domain.

```terraform
resource "aws_route53_zone" "main" {
  name = "tivi.st"
}

resource "aws_route53_record" "blog" {
  zone_id = aws_route53_zone.main.zone_id
  name    = "hack"
  type    = "CNAME"
  ttl     = "1800"
  records = ["ac-tivi-st.github.io."]
}

output "namespaces" {
  description = "domain ns"
  value       = join(",", aws_route53_zone.main.name_servers)
}
```

The terraform supports workspaces for environments (dev, staging, prod etc...)

Firstly, you need login to terraform.io. After for remote state management you need below lines.

```terraform
terraform {
    backend "remote" {
        organization = "activist"
        workspaces {
            prefix = "route53-"
        }
    }
}
```

#### terraform commands:

```bash
$ terraform login
$ terraform workspace new <ENV>
$ terraform init
$ terraform plan
$ terraform apply
```

## Github Pages

I've created new repo in my github account (https://github.com/ac-tivi-st/blog)

## Next Steps

I'll continue with kubernetes setup for simple project.
