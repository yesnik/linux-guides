# Terraform

[Terraform](https://www.terraform.io/) is an infrastructure as code (IaC) tool that lets us build, change, and version cloud and on-prem resources safely and efficiently. 

- It helps to manage infrastructure with config files rather than through a graphical user interface.
- **HCL** (HashiCorp Configuration Language) - is used to describe infrastructure in config files. It's declarative. It describes the desired end state of our infrastructure.
  Terraform calculate dependencies between resources to create or destroy them in the correct order.
- It can manage infrastructure in multiple cloud platforms.
- Terraform state allows us to track resources changes throughout our deployment.
- Terraform creates and manages resources on cloud platforms and other services through their APIs.
- *Providers* enable Terraform to work with virtually any platform or service with an accessible API. Examples of providers: AWS, Azure, Kubernetes, etc.

## Deploy infrastructure steps

- Scope: identify the infrastructure for our project
- Author: write configuration to define our infrastructure
- Initialize: install the required Terraform Providers (Plugins)
- Plan: preview the changes Terraform will make to match our configuration
- Apply: make the changes to our infrastructure

**State file** - Terraform uses it to determine what changes to make to our infrastructure so that it will match our configuration.
 
