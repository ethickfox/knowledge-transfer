---
Interview graded: false
Last edited time: 2023-08-23T13:34
Needs Rework: false
Status: Not started
Topic:
  - "[[AWS]]"
---
Basic Template

```JSON
{
  "AWSTemplateFormatVersion" : "version date",

  "Description" : "JSON string",

  "Metadata" : {
    template metadata
  },

  "Parameters" : {
    set of parameters
  },
  
  "Rules" : {
    set of rules
  },

  "Mappings" : {
    set of mappings
  },

  "Conditions" : {
    set of conditions
  },

  "Transform" : {
    set of transforms
  },

  "Resources" : { //mandatory
    set of resources
  },
  
  "Outputs" : {
    set of outputs
  }
}
```

![[Untitled 94.png|Untitled 94.png]]

![[Untitled 1 24.png|Untitled 1 24.png]]

![[Untitled 2 17.png|Untitled 2 17.png]]

![[Untitled 3 17.png|Untitled 3 17.png]]

![[Untitled 4 13.png|Untitled 4 13.png]]

![[Untitled 5 13.png|Untitled 5 13.png]]

![[Untitled 6 13.png|Untitled 6 13.png]]

![[Untitled 7 11.png|Untitled 7 11.png]]

![[Untitled 8 11.png|Untitled 8 11.png]]

![[Untitled 9 11.png|Untitled 9 11.png]]

![[Untitled 10 11.png|Untitled 10 11.png]]

![[Untitled 11 11.png|Untitled 11 11.png]]

![[Untitled 12 11.png|Untitled 12 11.png]]

![[Untitled 13 11.png|Untitled 13 11.png]]

![[Untitled 14 11.png|Untitled 14 11.png]]

![[Untitled 15 10.png|Untitled 15 10.png]]

![[Untitled 16 10.png|Untitled 16 10.png]]

![[Untitled 17 7.png|Untitled 17 7.png]]

Output

![[Untitled 18 7.png|Untitled 18 7.png]]

![[Untitled 19 7.png|Untitled 19 7.png]]

S3

## Your goals

- Explain what CloudFormation template and stack are
- Understand templating basics and change sets
- Know how to deploy applications using CloudFormation
- Be able to explain SSM concepts, its categories and use cases
- Be able to tell about secret management best practices. Name AWS services which can help to manage your secrets.

## Self check:

1. If you don't want to apply updates to CFN template right away, but just check the changes, what do you need?
2. What will you do to deploy several CFN stacks at once?
3. Can you deploy a resource on condition using CFN? If yes, how?
4. How can you use the same template for multiple environments?
5. Where can you view the results of template execution?

![[Untitled 20 6.png|Untitled 20 6.png]]

![[Untitled 21 6.png|Untitled 21 6.png]]

![[Untitled 22 6.png|Untitled 22 6.png]]

With the `DeletionPolicy` attribute you can preserve, and in some cases, backup a resource when its stack is deleted.

- Delete
- Retain
- RetainExceptOnCreate
- Snapshot

  

A load balancer cannot be attached to multiple subnets in the same Availability Zone

## Parameters

![[Untitled 23 6.png|Untitled 23 6.png]]

![[Untitled 24 6.png|Untitled 24 6.png]]

![[Untitled 25 6.png|Untitled 25 6.png]]

## Mappings

![[Untitled 26 5.png|Untitled 26 5.png]]

### AWS Systems Manager (SSM)

AWS Systems Manager (formerly known as SSM) is an AWS service that you can use to view and control your infrastructure on AWS. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources. Systems Manager helps you maintain security and compliance by scanning your managed instances and reporting on (or taking corrective action on) any policy violations it detects.

![[Untitled 27 5.png|Untitled 27 5.png]]

Run Command and Maintenance Windows are each capabilities of Systems Manager.

1. **Configure Systems Manager** – Use the Systems Manager console, SDK, AWS Command Line Interface (AWS CLI), or AWS Tools for Windows PowerShell to configure, schedule, automate, and run actions that you want to perform on your AWS resources.
2. **Verification and processing** – Systems Manager verifies the configurations, including permissions, and sends requests to the AWS Systems Manager SSM Agent running on your instances or servers in your hybrid environment. SSM Agent performs the specified configuration changes.
3. **Reporting** – SSM Agent reports the status of the configuration changes and actions to Systems Manager in the AWS Cloud. Systems Manager then sends the status to the user and various AWS services, if configured.

Systems Manager is comprised of individual capabilities, which are grouped into five categories:

- Operations Management
- Application Management
- Change Management
- Node Management
- Shared Resources

  

## Examples

```JSON
aws cloudformation delete-stack --stack-name MyStack
```

```JSON
aws cloudformation create-stack --stack-name MyStack --template-url https://nk-cloudformation-bucket.s3.eu-north-1.amazonaws.com/s3.json
```

```JSON
aws cloudformation list-stacks
```

```JSON
aws cloudformation describe-stacks --stack-name MyStack
```