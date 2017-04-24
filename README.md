# Terraplate

Terraplate gives you DRY [Terraform](https://terraform.io) templates and moudules (!)
and an anti bike shedding tool 'convention over configuration' environment for laying out multiple infrastructures over many environments.

Terraplate does not actually run Terraform (or Terragrunt) to create or destroy infrastructure.
It's job is to generate the Terraform code that you run.

Terraplate provides an ansible role wrapper for every Terraform resource. By combining these resoruce roles you can template out
a collection of customized related resources. We maintain a separate repostitory of components the use of which is demonstrated
within these playbooks.

See [Terraplate Components](https://github.com/rjayroach/terraplate-components) for examples

## Dependencies

- [Terraform](https://terraform.io)  ;-)
- [Terragrunt](https://github.com/gruntwork-io/terragrunt) to define variable values in tfvars files
- [Ansible](https://www.ansible.com) to define roles and playbooks to generate your terraform code (minimum version 2.2.0)


## Using

The Terraplate role itself does not generate any specific Terraform module or template. Your roles do that.
The Terraplate role provides tasks that will generate the Terraform artificats based on the values in your role.
Your role needs to define the following:

- vars/main.yml
- files/main.tf
- tasks/main.yml

See the [Terraplate Roles](https://github.com/rjayroach/terraplate-roles) for examples

In order to generate the Terraform code that your role defines you run an ansible playbook. Your playbook needs to:

- set the fact 'package' to a value
- include a role at the top 'terraplate/setup'

After that setup work is done your playbook should include the roles that you have defined.
See the [Terraplate Playbooks](https://github.com/rjayroach/terraplate-playbooks) for examples


## Concepts

### Component

This is an envrionment agnostic definition that:

- wraps the Terraform module defined by your role in a file named 'role'.tf
- includes a remote-state.tf and a provider.tf
- reads playbook defined variables in a hash under the nomenclature package_component_namspace_component_name

### Instance

This is an environment specific definition that:

- includes a tfvars file with the values to feed to the component
- reads playbook defined variables in a hash under the nomenclature package_instance_namspace_instance_name

### Variables

The naming convention for variables allows your roles to define default values for every value that is fed to your Terraform module
but also allows your playbooks to easily override any of those values
