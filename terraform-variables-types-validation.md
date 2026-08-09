# Terraform Variables: Types, Defaults, Validation & Sensitive

Variables make configs reusable instead of hardcoded. By convention they live in `variables.tf`, though that filename isn't required — Terraform reads all `.tf` files in a folder together.

---

## Primitive Types

```hcl
variable "environment" {
  description = "Deployment environment"
  type        = string
  default     = "dev"
}

variable "instance_count" {
  type    = number
  default = 2
}

variable "enable_monitoring" {
  type    = bool
  default = true
}
```

---

## Collection Types

```hcl
variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a", "us-east-1b"]
}

variable "tags" {
  type = map(string)
  default = {
    Owner   = "Abhigna"
    Project = "TerraWeek"
  }
}

variable "unique_names" {
  type    = set(string)
  default = ["web", "api", "db"]
}
```

| Type | Behaviour |
|---|---|
| `list` | Ordered, can contain duplicates |
| `map` | Key/value pairs, like a dictionary |
| `set` | Unordered, no duplicates allowed |

---

## Structural Types

```hcl
variable "server_config" {
  type = object({
    name = string
    size = number
    tags = list(string)
  })
  default = {
    name = "web-server"
    size = 2
    tags = ["dev", "web"]
  }
}

variable "network_settings" {
  type = tuple([string, number, bool])
  default = ["10.0.0.0/16", 8080, true]
}
```

- `object` — a fixed set of named fields, each with its own type (like a mini schema)
- `tuple` — a fixed-length list where each position has its own specific type

---

## Validation Block

Restricts what values are actually allowed for a variable:

```hcl
variable "environment" {
  description = "Deployment environment"
  type        = string
  default     = "dev"

  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "environment must be one of: dev, staging, prod."
  }
}
```

If someone sets `environment = "test"`, `terraform plan` fails immediately with the custom error message — catches mistakes before anything gets created.

---

## Sensitive Flag

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

Terraform hides this value from `plan`/`apply` output (shows `(sensitive value)` instead) and from `terraform console`.

**Important caveat:** it's still stored in **plaintext** inside `terraform.tfstate`. `sensitive` only hides it from terminal output — it is not encryption. Real secrets should come from something like AWS Secrets Manager, SSM Parameter Store, or environment variables — not hardcoded defaults.

---

## Related

- [HCL Blocks, Arguments & Expressions](./hcl-blocks-arguments-expressions.md)
- [Locals & Outputs](./terraform-locals-and-outputs.md)
- [Variable Precedence](./terraform-variable-precedence.md)
