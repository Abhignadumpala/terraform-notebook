# 🌱 TerraWeek Day 1 — Introduction to IaC & Terraform Basics

**Date:** Sunday, 12th July 2026

Following the [TerraWeek Challenge](https://github.com/LondheShubham153/TerraWeek) by TrainWithShubham. Day 1 covers the foundations — why Infrastructure as Code exists, installing Terraform, and running my first `terraform apply`.

---

## Task 1: Understand IaC & Terraform

**What is Infrastructure as Code, and what problems does it solve compared to clicking around a cloud console?**

Infrastructure as Code means defining infrastructure — servers, networks, storage — as configuration files instead of manually clicking through a console. It solves a few real problems: no repeatability (rebuilding the same setup by hand takes time and is error-prone), no version history (console changes leave no trail of what changed or why), and no easy review process before a change goes live. With IaC, the config file becomes the documentation, and changes go through version control the same way application code does.

**What is Terraform, and why is it so popular?**

Terraform is a declarative, provider-agnostic IaC tool — I describe the end state I want, and Terraform works out how to get there, rather than me scripting each step myself. It's popular because of its huge provider ecosystem — the same tool and syntax work across AWS, Azure, Kubernetes, and hundreds of other platforms, so I'm not learning a different tool for every platform I touch.

**Terraform vs alternatives:**
- **OpenTofu** — open-source fork of Terraform, functionally almost identical, community-governed instead of HashiCorp-owned.
- **Pulumi** — same IaC concept, but config is written in actual programming languages (Python, TypeScript, Go) instead of HCL.
- **CloudFormation** — AWS-only, no multi-cloud support, but tightly integrated if working purely within AWS.
- **Ansible** — primarily configuration management (installing software, configuring servers that already exist), not infrastructure provisioning.

---

## Task 2: Install Terraform

Installed Terraform ≥ 1.15 following the [official install guide](https://developer.hashicorp.com/terraform/install).

Verified the install:
```bash
terraform version
```
```
[paste actual output here]
```

```bash
terraform -help
```
```
[paste actual output here]
```

Installed the HashiCorp Terraform extension in VS Code for syntax highlighting and autocomplete.

---

## Task 3: 6 Crucial Terraform Terminologies

**1. Provider** — a plugin that lets Terraform talk to a specific platform's API.
```hcl
provider "aws" {
  region = "us-east-1"
}
```

**2. Resource** — a piece of infrastructure to create.
```hcl
resource "aws_instance" "my_server" {
  ami           = "ami-0123456789"
  instance_type = "t2.micro"
}
```

**3. State** — Terraform's record of what it currently manages, stored in `terraform.tfstate`. This is how Terraform tells the difference between "doesn't exist yet" and "already exists."

**4. Plan** — a preview of exactly what Terraform will create, change, or destroy, shown before anything actually happens.

**5. HCL** — HashiCorp Configuration Language, the syntax all Terraform config is written in.

**6. Module** — a reusable, packaged group of Terraform configuration, so the same setup doesn't need to be rewritten every time.

---

## Task 4: My First Terraform Config

Used the starter code in [`./example`](https://github.com/LondheShubham153/TerraWeek/blob/main/day01/example) — uses the `local` and `random` providers, so no cloud account or cost involved.

```bash
cd example
terraform init      # download providers, initialize the working directory
terraform fmt       # format the code
terraform validate  # check for syntax errors
terraform plan      # preview what will be created
terraform apply     # create the resources (typed: yes)
cat greeting.txt    # see the file Terraform generated
terraform destroy   # clean up (typed: yes)
```

**Output:**
```
[paste actual terminal output here]
```

---

## 🔁 The Core Terraform Workflow

```
Write  ──▶  Init  ──▶  Plan  ──▶  Apply  ──▶  Destroy
(.tf)     (init)     (preview)   (create)    (clean up)
```

---

## Bonus

- Set up tab completion: `terraform -install-autocomplete`
- Looked at OpenTofu — near-identical to Terraform, main difference is the open-source license and community governance instead of HashiCorp ownership.
- Explored `.terraform.lock.hcl` — this locks the exact provider versions used, so the same config produces the same result on any machine, regardless of what version might be available later.

---

## Wrap-Up

Day 1 done — foundations locked in. Next: Day 2.

#TerraWeekChallenge #Terraform #IaC #DevOps
