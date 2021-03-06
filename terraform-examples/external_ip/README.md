# External IP Example

This terraform example shows how the bucket constraint works. The relevant files include:

* validator/external_ip.rego
* policies/templates/external_ip.yaml
* policies/constraints/external_ip.yaml

**TL;DR** `terraform-validator` will execute against all constraint YAML files in the `policies/constraints` directory. Constraints implement templates, which are in the `policies/templates` directory. Templates make use of the [Rego Language](https://www.openpolicyagent.org/docs/latest/how-do-i-write-policies/#what-is-rego) for conditional assertions in the `validator` directory. These Rego files are copied as text into the template YAML files when running `make build`.

## Running the Test

```
# Ensure terraform-validator is installed
terraform plan -out tfplan.out
terraform-validator validate tfplan.out --policy-path=../..
```

You should get some output like this:

```
Found Violations:

Constraint forbid-external-ip on resource //compute.googleapis.com/projects/PROJECT/zones/us-east1-b/instances/test-instance: //compute.googleapis.com/projects/PROJECT/zones/us-east1-b/instances/test-instance is not allowed to have an external IP.
```


## Developing With Rego

1. Install `opa` from https://www.openpolicyagent.org/docs/latest/get-started
2. Make changes to the .rego file
3. Run `make build` to copy the rego source into the policies/templates file as an INLINE
4. Re-run `terraform-validator`
