# Terraform Built-In Functions

Terraform ships with a library of built-in functions for strings, collections, numbers, dates, etc. Custom/user-defined functions aren't allowed — only what's built in.

The best way to explore them is live, using `terraform console` — an interactive prompt that evaluates expressions without touching any real infrastructure.

```bash
terraform console
```

```
> upper("terraweek")
"TERRAWEEK"

> merge({a=1}, {b=2})
{
  "a" = 1
  "b" = 2
}

> join("-", ["tws", "terraweek", "2026"])
"tws-terraweek-2026"

> length(["dev", "staging", "prod"])
3

> lookup({env="dev"}, "env", "default-value")
"dev"

> format("Hello, %s!", "Abhigna")
"Hello, Abhigna!"
```

Type `exit` to leave the console.

---

## Common Functions Worth Knowing

| Function | What it does |
|---|---|
| `upper()` / `lower()` | Change string case |
| `merge()` | Combines multiple maps into one |
| `join()` | Combines a list into a single string with a separator |
| `split()` | Opposite of `join` — splits a string into a list |
| `lookup()` | Gets a value from a map by key, with an optional default |
| `length()` | Number of items in a list/map/string |
| `format()` | Printf-style string formatting |
| `contains()` | Checks if a list contains a value (commonly used in `validation` blocks) |

---

## Advanced Expressions

### `for` expression — transform a list/map

```hcl
locals {
  upper_names = [for s in var.names : upper(s)]
}
```

Takes each item in `var.names`, runs it through `upper()`, and builds a new list.

### Conditional expression (ternary-style)

```hcl
instance_type = var.environment == "prod" ? "t3.medium" : "t3.micro"
```

Reads as: *if `environment` equals `"prod"`, use `t3.medium`, otherwise use `t3.micro`.*

### `optional()` inside object types

```hcl
variable "server_config" {
  type = object({
    name = string
    size = optional(number, 2)   # optional, defaults to 2 if not provided
  })
}
```

Lets an object type have fields that aren't strictly required — useful when not every caller needs to set every field.

---

## Related

- [Terraform Variables — Types, Defaults, Validation](./terraform-variables-types-validation.md)
- [Locals & Outputs](./terraform-locals-and-outputs.md)
