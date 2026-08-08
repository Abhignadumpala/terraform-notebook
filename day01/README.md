# 🌱 Day 1 — Introduction to Terraform & Infrastructure as Code

**Date:** Sunday, 12th July 2026

My own notes as I start learning Terraform — written plain and simple, day by day.

---

## What Is Terraform?

Terraform is an **Infrastructure as Code (IaC) tool made by HashiCorp**.

In simple terms: instead of logging into a cloud console and clicking buttons to create servers, storage, or networks, I write down what I want in a file — and Terraform creates it for me automatically.

---

## What Is Infrastructure as Code?

Infrastructure as Code (IaC) means describing infrastructure — servers, networks, storage, databases — as configuration files, instead of setting it up manually through a console.

That configuration file becomes the record of what exists and why, the same way source code is the record of how an application works.

---

## Why This Matters

Doing everything manually through a console has real downsides:
- **No repeatability** — rebuilding the same setup by hand takes time and is easy to get wrong
- **No version history** — console changes leave no trail of what changed or when
- **No easy review** — there's no simple way to check a change before it goes live

With IaC, the config file fixes all three: it can be reused, tracked in Git, and reviewed before anything actually happens.

---

## Terraform vs Similar Tools

- **OpenTofu** — an open-source fork of Terraform, works almost the same, just community-run instead of owned by HashiCorp.
- **Pulumi** — same idea as Terraform, but you write actual code (Python, TypeScript, Go) instead of Terraform's own language.
- **CloudFormation** — AWS's own version, but only works with AWS, no other cloud.
- **Ansible** — a different job entirely: it configures software on servers that already exist, rather than creating the servers themselves.

---

## Installing Terraform

Installed Terraform (latest version) using the [official install guide](https://developer.hashicorp.com/terraform/install).

Verified it worked:
```bash
terraform version
```
```
sri-abhi@sri-abhi-ThinkCentre-M900:~/terraform$ terraform --help
Usage: terraform [global options] <subcommand> [args]

The available commands for execution are listed below.
The primary workflow commands are given first, followed by
less common or more advanced commands.

Main commands:
  init          Prepare your working directory for other commands
  validate      Check whether the configuration is valid
  plan          Show changes required by the current configuration
  apply         Create or update infrastructure
  destroy       Destroy previously-created infrastructure

All other commands:
  console       Try Terraform expressions at an interactive command prompt
  fmt           Reformat your configuration in the standard style
  force-unlock  Release a stuck lock on the current workspace
  get           Install or upgrade remote Terraform modules
  graph         Generate a Graphviz graph of the steps in an operation
  import        Associate existing infrastructure with a Terraform resource
  login         Obtain and save credentials for a remote host
  logout        Remove locally-stored credentials for a remote host
  metadata      Metadata related commands
  modules       Show all declared modules in a working directory
  output        Show output values from your root module
  providers     Show the providers required for this configuration
  query         Search and list remote infrastructure with Terraform
  refresh       Update the state to match remote systems
  show          Show the current state or a saved plan
  stacks        Manage HCP Terraform stack operations
  state         Advanced state management
  taint         Mark a resource instance as not fully functional
  test          Execute integration tests for Terraform modules
  untaint       Remove the 'tainted' state from a resource instance
  version       Show the current Terraform version
  workspace     Workspace management

Global options (use these before the subcommand, if any):
  -chdir=DIR    Switch to a different working directory before executing the
                given subcommand.
  -help         Show this help output or the help for a specified subcommand.
  -version      An alias for the "version" subcommand.
sri-abhi@sri-abhi-ThinkCentre-M900:~/terraform$ 

```

```bash
terraform -help
```
```
sri-abhi@sri-abhi-ThinkCentre-M900:~/terraform$ terraform --help
Usage: terraform [global options] <subcommand> [args]

The available commands for execution are listed below.
The primary workflow commands are given first, followed by
less common or more advanced commands.

Main commands:
  init          Prepare your working directory for other commands
  validate      Check whether the configuration is valid
  plan          Show changes required by the current configuration
  apply         Create or update infrastructure
  destroy       Destroy previously-created infrastructure

All other commands:
  console       Try Terraform expressions at an interactive command prompt
  fmt           Reformat your configuration in the standard style
  force-unlock  Release a stuck lock on the current workspace
  get           Install or upgrade remote Terraform modules
  graph         Generate a Graphviz graph of the steps in an operation
  import        Associate existing infrastructure with a Terraform resource
  login         Obtain and save credentials for a remote host
  logout        Remove locally-stored credentials for a remote host
  metadata      Metadata related commands
  modules       Show all declared modules in a working directory
  output        Show output values from your root module
  providers     Show the providers required for this configuration
  query         Search and list remote infrastructure with Terraform
  refresh       Update the state to match remote systems
  show          Show the current state or a saved plan
  stacks        Manage HCP Terraform stack operations
  state         Advanced state management
  taint         Mark a resource instance as not fully functional
  test          Execute integration tests for Terraform modules
  untaint       Remove the 'tainted' state from a resource instance
  version       Show the current Terraform version
  workspace     Workspace management

Global options (use these before the subcommand, if any):
  -chdir=DIR    Switch to a different working directory before executing the
                given subcommand.
  -help         Show this help output or the help for a specified subcommand.
  -version      An alias for the "version" subcommand.
sri-abhi@sri-abhi-ThinkCentre-M900:~/terraform$ 

```

Also installed the HashiCorp Terraform extension in VS Code, for syntax highlighting and autocomplete.

![Terraform version output](images/terraform-version.png)

---

## Key Terms I Need to Know

**Provider** — a plugin that lets Terraform talk to a specific platform (AWS, Azure, etc).
```hcl
provider "aws" {
  region = "us-east-1"
}
```

**Resource** — the actual thing being created.
```hcl
resource "aws_instance" "my_server" {
  ami           = "ami-0123456789"
  instance_type = "t2.micro"
}
```

**State** — Terraform's record of what it currently manages, stored in `terraform.tfstate`. This is how it knows the difference between "doesn't exist yet" and "already exists."

**Plan** — a preview of exactly what will change, shown before anything actually happens.

**HCL** — HashiCorp Configuration Language, the syntax all of this is written in.

**Module** — a reusable, packaged group of config, so I don't rewrite the same setup every time.

---

## My First Hands-On Example

Used a simple starter example with the `local` and `random` providers — no cloud account needed, no cost involved.

```bash
cd example
terraform init      # download providers, set up the working directory
terraform fmt       # format the code
terraform validate  # check for syntax errors
terraform plan      # preview what will be created
terraform apply     # create it (typed: yes)
cat greeting.txt    # see the file Terraform generated
terraform destroy   # clean it up (typed: yes)
```

**Output:**
```
[paste actual terminal output here]
```

![Terraform apply output](images/terraform-apply.png)

---

## The Core Workflow

```
Write  ──▶  Init  ──▶  Plan  ──▶  Apply  ──▶  Destroy
(.tf)     (init)     (preview)   (create)    (clean up)
```

---

## Extra Things I Looked Into

- Set up tab completion for the CLI: `terraform -install-autocomplete`
- Looked briefly at OpenTofu — nearly identical to Terraform day-to-day
- Looked at the `.terraform.lock.hcl` file — it locks the exact provider versions used, so the same config gives the same result every time, on any machine

---

## Day 1 Wrap-Up

Foundations done — I understand what Terraform is, why IaC matters, and ran my first full `init → plan → apply → destroy` cycle.

Next up: Day 2.
