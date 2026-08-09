# Terraform Locals & Outputs

## Locals

`locals` compute a value once, based on other values, so the same expression doesn't need repeating everywhere in the config.

```hcl
locals {
  name_prefix = "${var.environment}-terraweek"

  common_tags = merge(
    var.tags,
    {
      ManagedBy = "Terraform"
      Owner     = "Abhigna"
    }
  )
}
```

Used elsewhere like:

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "${local.name_prefix}-bucket"
  tags   = local.common_tags
}
```

### Variable vs Local — the difference

| | Variable | Local |
|---|---|---|
| Set from | Outside — tfvars, `-var` flag, env var | Computed inside the config, from other values |
| Purpose | Input | Derived/calculated value |
| Can be overridden externally? | Yes | No |

---

## Outputs

Outputs expose values after `apply` — printed to the terminal, and readable by other tools or parent modules.

```hcl
output "bucket_name" {
  description = "The name of the created S3 bucket"
  value       = aws_s3_bucket.my_bucket.bucket
}

output "bucket_arn" {
  value = aws_s3_bucket.my_bucket.arn
}
```

Example from a real `apply` run:

```
Outputs:

file_path = "./greeting.txt"
pet_name = "neat-polliwog"
```

Outputs are especially useful for surfacing values I'd otherwise have to dig for inside `terraform.tfstate` manually — like a generated ID, an IP address, or an ARN.

---

## Related

- [HCL Blocks, Arguments & Expressions](./hcl-blocks-arguments-expressions.md)
- [Terraform Variables — Types, Defaults, Validation](./terraform-variables-types-validation.md)
- [Built-In Functions](./terraform-built-in-functions.md)
