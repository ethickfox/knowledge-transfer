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

![89.png](../../_img/89.png)

![10.png](../../_img/10.png)

![39.png](../../_img/39.png)

![152.png](../../../Software_Architecture/_img/152.png)

![48.png](../../_img/48.png)

![30.png](../../../Software_Architecture/_img/30.png)

![44.png](../../../Software_Architecture/_img/44.png)

![9.png](../../../JavaScript/_img/9.png)

![112.png](../../_img/112.png)

![62.png](../../_img/62.png)

![61.png](../../_img/61.png)

![31.png](../../_img/31.png)

![60.png](../../_img/60.png)

![Untitled3.png](Software_Architecture/Communication/_img/Untitled3.png)

![54.png](../../_img/54.png)

![Untitled 15 10.png](Untitled%2015%2010.png)

![29.png](../../_img/29.png)

![26.png](../../_img/26.png)

Output

![24.png](../../_img/24.png)

![64.png](../../../Software_Architecture/_img/64.png)

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

![141.png](../../../Software_Architecture/_img/141.png)

![21.png](../../_img/21.png)

![69.png](../../_img/69.png)

With the `DeletionPolicy` attribute you can preserve, and in some cases, backup a resource when its stack is deleted.

- Delete
- Retain
- RetainExceptOnCreate
- Snapshot

  

A load balancer cannot be attached to multiple subnets in the same Availability Zone

## Parameters

![67.png](../../_img/67.png)

![64.png](../../_img/64.png)

![108.png](../../_img/108.png)

## Mappings

![102.png](../../_img/102.png)

### AWS Systems Manager (SSM)

AWS Systems Manager (formerly known as SSM) is an AWS service that you can use to view and control your infrastructure on AWS. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources. Systems Manager helps you maintain security and compliance by scanning your managed instances and reporting on (or taking corrective action on) any policy violations it detects.

![115.png](../../_img/115.png)

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