# Terraform Variable Precedence

If the same variable is set in more than one place, Terraform needs a rule to decide which value actually wins. Highest precedence wins:

```
-var / -var-file  ▶  *.auto.tfvars  ▶  terraform.tfvars  ▶  TF_VAR_ env vars  ▶  default
```

## In Plain Terms

| Priority | Source | Notes |
|---|---|---|
| 1 (highest) | `-var` flag / `-var-file` on the CLI | Wins over everything else |
| 2 | `*.auto.tfvars` files | Auto-loaded by Terraform without needing to be referenced |
| 3 | `terraform.tfvars` | The conventional default tfvars filename, auto-loaded |
| 4 | `TF_VAR_xxx` environment variables | e.g. `export TF_VAR_environment=prod` |
| 5 (lowest) | `default` in the `variable` block | Fallback if nothing else is set |

## Example — Same Variable, Three Ways

```bash
# 1. Command line flag (highest priority)
terraform apply -var 'container_name=tws-web'
```

```hcl
# 2. terraform.tfvars file
container_name = "tws-web"
```

```bash
# 3. Environment variable
export TF_VAR_container_name=tws-web
```

If all three are set at once, the `-var` flag wins.

## `-var` vs `terraform.tfvars`

| `-var` | `terraform.tfvars` |
|---|---|
| Passes variables through the command line | Stores variables in a file |
| Good for testing or one-off runs / CI pipelines | Best for reusable, repeatable projects |
| Must be supplied every single run | Loaded automatically — no flags needed |

## Related

- [Terraform Variables — Types, Defaults, Validation](./terraform-variables-types-validation.md)
