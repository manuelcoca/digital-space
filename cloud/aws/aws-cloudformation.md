# AWS CloudFormation

## CloudFormation - Why and What?

Complex cloud infrastructure can be hard to manage. With CloudFormation, infrastructure within AWS cloud can be managed and deployed easily by reuseable templates written in YAML or JSON (infrastructure as code).

* Templates will provision resources called a Stack
* The stack can be updated by deploying changes in the template
* Templates can be reused to deploy stack in different regions
  * To make templates reuseable, use the parameters, mappings and conditions section so the stack can be customized when created

## Templates

A template consists of different objects:

* Description - Describe purpose etc. of the template
* Metadata - Details can metadata of resources
* Parameters - Customize resources with parameters to pass
* Mappings - Map values based on a key. Example: Map specific values by region
* Conditions - Create conditions and rules. Example: Only create next resource if another specific resource was created
* Transforms - Transform objects enables reuse of template components
* Resource - Defines the resource which should be created
