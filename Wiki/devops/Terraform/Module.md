Любой каталог в конфигурации `terraform` является модулем
```hcl
module "developers" {
  source  = "./teams/developers"
}

```

```
module "iam_iam-user" {
  source  = "terraform-aws-modules/iam/aws//modules/iam-user"
  version = "5.28.0"
}
```