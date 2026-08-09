# HCL: Blocks, Arguments & Expressions

HCL (HashiCorp Configuration Language) is the syntax every `.tf` file is written in. This covers the building blocks of that syntax.

---

## The Anatomy of a Block

Every block in Terraform follows the same basic shape:

```hcl
block_type "label_one" "label_two" {
  argument = value
}
```

Example:

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "abhigna-terraform-lab-2026"
}
```

| Part | Name | Meaning |
|---|---|---|
| `resource` | block type | What kind of block this is (`resource`, `variable`, `provider`, `output`, `locals`, `terraform`, etc.) |
| `"aws_s3_bucket"` | label one | For a resource, this is the resource *type* — maps to something a provider knows how to create |
| `"my_bucket"` | label two | The local name — how I refer to this resource elsewhere in my own code. Never seen by the actual cloud provider |
| `bucket = "..."` | argument | A `name = value` line that configures the block |

Not every block type needs the same number of labels:

| Block type | Labels needed |
|---|---|
| `resource "type" "name" { }` | 2 |
| `provider "aws" { }` | 1 |
| `variable "environment" { }` | 1 |
| `terraform { }` | 0 |
| `locals { }` | 0 |

---

## Argument vs Block

**An argument** is a simple `name = value` line inside a block — assigns a single value (or list/map) to a setting.

```hcl
bucket = "abhigna-terraform-lab-2026"   # argument
```

**A block** is a `{ }` structure that can contain more arguments *or* nested blocks. Blocks can repeat, arguments can't (within the same block).

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "abhigna-terraform-lab-2026"   # argument

  versioning {                             # nested block
    enabled = true                         # argument inside that nested block
  }
}
```

**Quick test:** if it has `{ }` after it, it's a block. If it's just `name = value`, it's an argument.

---

## Expressions

Expressions let me compute or reference values instead of hardcoding everything.

### String interpolation

Embeds a value inside a string using `${ }`:

```hcl
content = "Hello! Your infra pet name is: ${random_pet.name.id}"
```

Note: in modern Terraform, `${...}` is only strictly needed when *mixing* literal text with a value. A bare reference on its own doesn't need the wrapper:

```hcl
value = random_pet.name.id   # no ${} needed here
```

### References

Point to an attribute of another resource, variable, local, or data source:

```hcl
resource.name.attribute
var.environment
local.name_prefix
data.aws_ami.example.id
path.module        # built-in reference to the current module's filesystem path
```

### Operators

Standard comparison/logic operators:

```hcl
var.count > 1
var.environment == "prod"
var.a && var.b
```

---

## Related

- [Terraform Variables — Types, Defaults, Validation](./terraform-variables-types-validation.md)
- [Locals & Outputs](./terraform-locals-and-outputs.md)
- [Built-In Functions](./terraform-built-in-functions.md)
