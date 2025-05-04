---
Interview graded: true
Last edited time: 2023-08-07T15:01
Needs Rework: false
Status: Not started
Topic:
  - "[AWS](AWS.md)"
---
To assume a role, we use the Security Token Service (STS) that gives us temporary credentials to use the role. Why would we need separate credentials? When you assume a role, you get credentials, which you can use to make API-calls with. These credentials let you act as the role until they expire. They’re separate from your original credentials, so you can easily use both at the same time for different API calls. Temporary Credentials also look different from the long-term credentials.

The API call we need to make in order to assume the role is the sts:AssumeRole action. We need to specify the ARN of the role we want to assume as well as a session name. The session name will be visible in CloudTrail and is part of what makes it transparent who assumed a role. Optionally we can also specify how long the credentials should be valid. The upper limit in IAM is 72 hours, but you can specify a lower boundary for each role.

In order for this to work, the principal that assumes the role needs the sts:AssumeRole permission for said role in its identity policy and the principal needs to be listed in the trust relationship of the role. If either of them is missing the call fails. When the permissions are set up correctly, the STS response contains a credentials object with four pieces of information:

- Access Key Id
- Secret Access Key
- Security Token
- Expiration - a timestamp that tells you when the credentials expire

AWS Identity and Access Management (IAM) helps you securely control access to Amazon Web Services (AWS) and your account resources. IAM can also keep your account credentials private. With IAM, you can create multiple IAM users under the umbrella of your AWS account or enable temporary access through identity federation with your corporate directory. In some cases, you can also enable access to resources across AWS accounts.

- **Fine-grained access control to AWS resources** - IAM enables [your users](https://aws.amazon.com/iam/features/manage-users/) to [control access](https://aws.amazon.com/iam/features/manage-permissions/) to AWS service APIs and to specific resources. IAM also enables you to add [specific conditions](https://aws.amazon.com/iam/features/manage-permissions/) such as time of day to control how a user can use AWS, their originating IP address, whether they are using SSL, or whether they have authenticated with a [multi-factor authentication device](https://aws.amazon.com/iam/features/mfa/). IAM is secure by default; users have no access to AWS resources until permissions are explicitly granted.
- **Temporary credentials** - In addition to defining access permissions directly to users and groups, IAM lets you create roles. Roles allow you to define a set of permissions and then let authenticated users or EC2 instances assume them, increasing your security posture by granting temporary access to the resources you define.
- **Multi-factor authentication for highly privileged users** - protect your AWS environment by using [AWS MFA](https://aws.amazon.com/iam/features/mfa/), a security feature available at no extra cost that augments user name and password credentials. MFA requires users to prove physical possession of a hardware MFA token or MFA-enabled mobile device by providing a valid MFA code.
- **Analyze access** - IAM helps you [analyze access](https://aws.amazon.com/iam/features/analyze-access/) across your AWS environment. Your security teams and administrators can quickly validate that your policies only provide the intended public and cross-account access to your resources. You can also easily identify and refine your policies to allow access to only the services being used. This helps you to better adhere to the principle of least privilege.
- **Integrate with your corporate directory** - IAM can be used to grant your employees and applications [federated access](https://aws.amazon.com/identity/federation/) to the AWS Management Console and AWS service APIs, using your existing identity systems such as Microsoft Active Directory. You can use any [identity management](http://docs.aws.amazon.com/IAM/latest/UserGuide/IdP-solution-providers.html) solution that supports [SAML 2.0](https://aws.amazon.com/identity/saml/), or feel free to use one of our federation samples ([AWS Console SSO](http://aws.amazon.com/code/4001165270590826) or [API federation](http://aws.amazon.com/code/1288653099190193)).
- **Seamlessly integrated into AWS services** - IAM is integrated into most AWS services. This provides the ability to define access controls from one place in the AWS Management Console that will take effect throughout your AWS environment.

# AM Identities

The AWS account root user or an IAM administrator for the account can create IAM identities. An IAM identity provides access to an AWS account. A user group is a collection of IAM users managed as a unit. An IAM identity represents a user, and can be authenticated and then authorized to perform actions in AWS. Each IAM identity can be associated with one or more policies. Policies determine what actions a user, role, or member of a user group can perform, on which AWS resources, and under what conditions.

### IAM users

An [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) is an entity that you create in AWS. The IAM user represents the person or service who uses the IAM user to interact with AWS. A primary use for IAM users is to give people the ability to sign in to the AWS Management Console for interactive tasks and to make programmatic requests to AWS services using the API or CLI. A user in AWS consists of a name, a password to sign into the AWS Management Console, and up to two access keys that can be used with the API or CLI. When you create an IAM user, you grant it permissions by making it a member of a user group that has appropriate permission policies attached (recommended), or by directly attaching policies to the user. You can also clone the permissions of an existing IAM user, which automatically makes the new user a member of the same user groups and attaches all the same policies. To add IAM users to your IAM account, see [Creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html).

### IAM user groups

An [IAM user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) is a collection of IAM users. You can use user groups to specify permissions for a collection of users, which can make those permissions easier to manage for those users. For example, you could have a user group called Admins and give that user group the types of permissions that administrators typically need. Any user in that user group automatically has the permissions that are assigned to the user group. If a new user joins your organization and should have administrator privileges, you can assign the appropriate permissions by adding the user to that user group. Similarly, if a person changes jobs in your organization, instead of editing that user's permissions, you can remove him or her from the old user groups and add him or her to the appropriate new user groups. A user group cannot be identified as a `Principal` in a resource-based policy. A user group is a way to attach policies to multiple users at one time. When you attach an identity-based policy to a user group, all of the users in the user group receive the permissions from the user group. For more information about these policy types, see [Identity-based policies and resource-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html).

### IAM roles

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is very similar to a user, in that it is an identity with permission policies that determine what the identity can and cannot do in AWS. However, a role does not have any credentials (password or access keys) associated with it. Instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it. An IAM user can assume a role to temporarily take on different permissions for a specific task. A role can be assigned to a federated user who signs in by using an external identity provider instead of IAM. AWS uses details passed by the identity provider to determine which role is mapped to the [federated user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html).

### IAM role assuming

An [IAM role assuming](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) is an AWS IAM feature that allows a trusted entity to assume temporary permissions for a role. The trusted entity can be an IAM user or an AWS service, such as an EC2 instance or Lambda function. To assume a role, the trusted entity must first request permission to assume the role by calling the AWS Security Token Service (STS) AssumeRole API action. The AssumeRole request includes the ARN (Amazon Resource Name) of the role to be assumed and an optional set of policy permissions that the role session requires. If the request is approved, the STS returns temporary security credentials consisting of an access key, a secret access key, and a security token. These temporary credentials can be used to access AWS resources that the role has been granted permissions to, and the permissions are limited to the policies attached to the role. The temporary credentials are valid for a limited time, typically up to one hour, after which they expire and the permissions are revoked. This helps to minimize the risk of unauthorized access and ensures that permissions are only granted when needed. The Assume Role feature provides a way to delegate access to AWS resources without sharing long-term access keys or passwords, and can be used to enforce the principle of least privilege and improve the security and auditability of AWS resource access.

### IAM role passing

In AWS [Role Passing](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) refers to the ability of a user or service to assume the permissions and privileges of an AWS Identity and Access Management (IAM) role. It is a way to delegate access to AWS resources across accounts, services, or systems without requiring long-term credentials such as access keys or passwords. The purpose of AWS IAM Role passing is to enable secure and flexible access to AWS resources for different entities such as users, services, and applications. Here are some of the main purposes of role passing in AWS IAM:

1. Secure access: Role passing enables entities to assume roles with temporary credentials, which reduces the risk of credentials being compromised or abused.
2. Access control: IAM roles can be used to control access to AWS resources based on policies that define who can assume the role and what actions they can perform.
3. Cross-account access: Role passing can be used to grant access to AWS resources across different AWS accounts, without the need to share long-term credentials.
4. Federated access: Role passing enables federated access to AWS resources, which allows external identities, such as users from other identity providers, to assume an IAM role and access AWS resources.
5. Least privilege: IAM roles can be used to grant users and services the minimum permissions needed to perform their tasks, reducing the risk of accidental or intentional misuse of AWS resources.

  

**Types of AWS credentials**

- **Username and password**
- **Access keys**
- **Multi-Factor Authentication (MFA)**

  

### Temporary credentials in IAM

[Temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) are primarily used with IAM roles, but there are also other uses. You can request temporary credentials that have a more restricted set of permissions than your standard IAM user. This prevents you from accidentally performing tasks that are not permitted by the more restricted credentials. A benefit of temporary credentials is that they expire automatically after a set period of time. You have control over the duration that the credentials are valid.

  

### IAM policies

An [IAM policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) is a set of permissions that controls access to AWS resources. It specifies which actions are allowed or denied on which resources, and it can be attached to IAM users, groups, or roles to grant or restrict access to AWS services and resources. For more information about policies types, please follow the [link](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)

Example of an identity based policy:

```Java
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "ec2:*",
            "Resource": "*",
            "Effect": "Allow",
            "Condition": {
                "StringEquals": {
                    "ec2:Region": "us-east-2"
                }
            }
        }
    ]
}
```

Example of a resource based policy:

```Java
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Action": "s3:PutObject",
			"Principal": {
				"AWS": "arn:aws:iam::<account-id>:root"
			},
			"Resource": "arn:aws:s3:::mybucket/*",
			"Effect": "Allow"
		}
	]
}
```

![Untitled 114.png](Untitled%20114.png)

A policy is an entity in AWS that, when attached to an identity or resource, defines their permissions. AWS evaluates these policies when a principal, such as a user, makes a request. If you specify multiple conditions, or multiple keys in a single condition, IAM evaluates them using a logical AND operation. If you specify a single condition with multiple values for one key, IAM evaluates the condition using a logical OR operation. For a permission to be granted, all conditions must be met.

Policy types, statements and syntax could be found [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)

Please review core IAM Policy elements

![Untitled 1 38.png](Untitled%201%2038.png)

### Assume IAM role

IAM > Roles > Trust relationships

```Java
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<account-id>:user/S3User"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

To assume a role, we use the Security Token Service (STS) that gives us temporary credentials to use the role. Why would we need separate credentials? When you assume a role, you get credentials, which you can use to make API-calls with. These credentials let you act as the role until they expire. They’re separate from your original credentials, so you can easily use both at the same time for different API calls. Temporary Credentials also look different from the long-term credentials.

The API call we need to make in order to assume the role is the sts:AssumeRole action. We need to specify the ARN of the role we want to assume as well as a session name. The session name will be visible in CloudTrail and is part of what makes it transparent who assumed a role. Optionally we can also specify how long the credentials should be valid. The upper limit in IAM is 72 hours, but you can specify a lower boundary for each role.

In order for this to work, the principal that assumes the role needs the sts:AssumeRole permission for said role in its identity policy and the principal needs to be listed in the trust relationship of the role. If either of them is missing the call fails. When the permissions are set up correctly, the STS response contains a credentials object with four pieces of information:

- Access Key Id
- Secret Access Key
- Security Token
- Expiration - a timestamp that tells you when the credentials expire

![Untitled 2 28.png](Untitled%202%2028.png)

### PassRole

IAM > User > Permissions

![Untitled 3 23.png](Untitled%203%2023.png)

```Java
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EC2",
            "Effect": "Allow",
            "Action": [
                "iam:GetRole",
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::<your-account-id>:role/EC2RoleWithLimitation"
        }
    ]
}
```

The PassRole permission (not action, even though it's in the Action block!) is the additional layer of checking required to secure this.  
By giving a role or user the iam:PassRole permission, you are saying "this entity (principal) is allowed to assign AWS roles to resources and services in this account".  
You can limit which roles a user or service can pass to others by specifying the role ARN(s) in the Resource field of the policy that grants them iam:PassRole:  
Without PassRole we could set administrator IAM role for new Instance  

![Untitled 4 18.png](Untitled%204%2018.png)

With PassRole we can set only allowed IAM role for new Instance

![Untitled 5 18.png](Untitled%205%2018.png)

### IAM policy simulator

With the IAM policy simulator, you can test and troubleshoot identity-based policies, IAM permissions boundaries, AWS Organizations service control policies, and resource-based policies. The policy simulator does not make an actual AWS service request, so you can safely test requests that might make unwanted changes to your live AWS environment. The only result returned is whether the requested action would be allowed or denied.

### IAM Access Analyzer

It continuously monitors policies for changes where you no longer need to rely on intermittent manual checks in order to catch issues as policies are added or updated. Using IAM Access Analyzer, you can proactively address any resource policies that violate their security and governance best practices around resource sharing and protect their resources from unintended access. IAM Access Analyzer delivers comprehensive, detailed findings through the IAM, Amazon S3, and AWS Security Hub consoles and also through its APIs. Findings can also be exported as a report for auditing purposes. IAM Access Analyzer findings provide definitive answers of who has public and cross-account access to AWS resources from outside an account.

### [**AWS IAM Identity Center (successor to AWS Single Sign-On)**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) 

Helps to manage large amount of users and use other identity provider (Microsoft Active Directory)

### Key points

---

- Policies are expressed in JSON [format](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_grammar.html).
- Use an [instance profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) to pass an IAM role to an EC2 instance (see picture below).
- By default IAM role has "Maximum session duration" 1 hour, it could be changed, but not for role chaining using CLI or API.
- To configure many AWS services, you must [pass an IAM role to the service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html).
- Resource-based policies are attached to a resource. For example, you can attach resource-based policies to Amazon S3 buckets, Amazon SQS queues, and AWS Key Management Service encryption keys. For a list of services that support resource-based policies, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html).

  

## Self-check

1. What is the difference between IAM identity and IAM principle?
    
    The principal is defined in IAM policies to grant or deny access, and the identity is used for authentication and authorization purposes.
    
    _**For example,**_ an IAM user named “Shristi” is a principal, and her IAM username shristi@example.com is her identity. The policy statement can grant or deny permissions to the principal (Shristi), and when Shristi authenticates with her username and password, her identity is verified.
    
2. Which type of request to AWS is recorded by CloudTrail?
    
    Events include actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs.
    
    When activity occurs in your AWS account, that activity is recorded in a CloudTrail event. You can easily view recent events in the CloudTrail console by going to Event history
    
3. What's the difference between customer managed IAM policy and AWS managed IAM policy?
    
    An AWS managed policy is a standalone policy that is created and administered by AWS. A customer managed policy is a standalone policy that you administer in your own AWS account. An inline policy is a policy that's embedded in an IAM identity (a user, group, or role).
    
4. How does inline policy differ from managed policy?
    
    An inline policy is a policy that's embedded in an IAM identity (a user, group, or role).
    
5. What are the limits for inline IAM policy?
    
      
    
6. What are the best practices when working with permissions?
7. The AWS CLI credentials and configuration settings take precedence in which order?(name first 3)
    1. [**Command line options**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-options.html) – Overrides settings in any other location, such as the `-region`, `-output`, and `-profile` parameters.
    2. [**Environment variables**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) – You can store values in your system's environment variables.
    3. [**Assume role**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html) – Assume the permissions of an IAM role through configuration or the [`aws sts assume-role`](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sts/assume-role.html) command.
    4. [**Assume role with web identity**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html) – Assume the permissions of an IAM role using web identity through configuration or the [`aws sts assume-role`](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sts/assume-role.html) command.
    5. [**AWS IAM Identity Center (successor to AWS Single Sign-On)**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) – The IAM Identity Center credentials are stored in the `config` file and are updated when you run the `aws configure sso` command. The `config` file is located at `~/.aws/config` on Linux or macOS, or at `C:\Users\``_USERNAME_``\.aws\config` on Windows.
    6. [**Credentials file**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) – The `credentials` and `config` file are updated when you run the command `aws configure`. The `credentials` file is located at `~/.aws/credentials` on Linux or macOS, or at `C:\Users\``_USERNAME_``\.aws\credentials` on Windows.
    7. [**Custom process**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sourcing-external.html) – Get your credentials from an external source.
    8. [**Configuration file**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) – The `credentials` and `config` file are updated when you run the command `aws configure`. The `config` file is located at `~/.aws/config` on Linux or macOS, or at `C:\Users\``_USERNAME_``\.aws\config` on Windows.
    9. [**Amazon EC2 instance profile credentials**](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) – You can associate an IAM role with each of your Amazon Elastic Compute Cloud (Amazon EC2) instances. Temporary credentials for that role are then available to code running in the instance. The credentials are delivered through the Amazon EC2 metadata service. For more information, see [IAM Roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) in the _Amazon EC2 User Guide for Linux Instances_ and [Using Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the _IAM User Guide_.
    10. [**Container credentials**](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html) – You can associate an IAM role with each of your Amazon Elastic Container Service (Amazon ECS) task definitions. Temporary credentials for that role are then available to that task's containers. For more information, see [IAM Roles for Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html) in the _Amazon Elastic Container Service Developer Guide_.
8. What is the allow/deny priority order when policies are configured on different levels (group, user, etc.)?
    1. **Identity-based policies** – Identity-based policies are attached to an IAM identity (user, group of users, or role) and grant permissions to IAM entities (users and roles). If only identity-based policies apply to a request, then AWS checks all of those policies for at least one `Allow`.
    2. **Resource-based policies** – Resource-based policies grant permissions to the principal (account, user, role, and session principals such as role sessions and IAM federated users ) specified as the principal. The permissions define what the principal can do with the resource to which the policy is attached. If resource-based policies and identity-based policies both apply to a request, then AWS checks all the policies for at least one `Allow`. When resource-based policies are evaluated, the principal ARN that is specified in the policy determines whether implicit denies in other policy types are applicable to the final decision.
    3. **IAM permissions boundaries** – Permissions boundaries are an advanced feature that sets the maximum permissions that an identity-based policy can grant to an IAM entity (user or role). When you set a permissions boundary for an entity, the entity can perform only the actions that are allowed by both its identity-based policies and its permissions boundaries. In some cases, an implicit deny in a permissions boundary can limit the permissions granted by a resource-based policy. To learn more, see [Determining whether a request is allowed or denied within an account](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html#policy-eval-denyallow) later in this topic.
    4. **AWS Organizations service control policies (SCPs)** – Organizations SCPs specify the maximum permissions for an organization or organizational unit (OU). The SCP maximum applies to principals in member accounts, including each AWS account root user. If an SCP is present, identity-based and resource-based policies grant permissions to principals in member accounts only if those policies and the SCP allow the action. If both a permissions boundary and an SCP are present, then the boundary, the SCP, and the identity-based policy must all allow the action.
    5. **Session policies** – Session policies are advanced policies that you pass as parameters when you programmatically create a temporary session for a role or federated user. To create a role session programmatically, use one of the `AssumeRole*` API operations. When you do this and pass session policies, the resulting session's permissions are the intersection of the IAM entity's identity-based policy and the session policies. To create a federated user session, you use the IAM user access keys to programmatically call the `GetFederationToken` API operation. A resource-based policy has a different effect on the evaluation of session policy permissions. The difference depends on whether the user or role's ARN or the session's ARN is listed as the principal in the resource-based policy. For more information, see [Session policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_session).

## Examples

Configure profile

```Java
aws configure
```

Set profile during command evaluation

```Bash
aws some_command --profile FullS3
```

Set command for session

```Bash
export AWS_PROFILE=FullS3
```

List all local profiles

```Bash
aws configure list-profiles
```

[https://git.epam.com/epmc-acm-public/aws-associate-training/-/blob/master/materials/security_identity_&_compliance/README.md](https://git.epam.com/epmc-acm-public/aws-associate-training/-/blob/master/materials/security_identity_&_compliance/README.md)

  

## Secrets Manager

AWS Secrets Manager helps you manage, retrieve, and rotate database credentials, application credentials, OAuth tokens, API keys, and other secrets throughout their lifecycles. [Many AWS services that use secrets](https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating.html) store them in Secrets Manager.

## Your goals

- Be able to describe identities in IAM
- Understand standard API request flow
- Know what Amazon Resource Name (ARN) is
- Learn about different types of IAM policies and what they consist of
- Be able to tell what types of AWS credentials exist
- Know about IAM Access Analyzer and IAM Policy Simulator

## Self-check

1. What is the difference between IAM identity and IAM principle?
2. Which type of request to AWS is recorded by CloudTrail?
3. What's the difference between customer managed IAM policy and AWS managed IAM policy?
4. How does inline policy differ from managed policy?
5. What are the limits for inline IAM policy?
6. What are the best practices when working with permissions?
7. The AWS CLI credentials and configuration settings take precedence in which order?(name first 3)
8. What is the allow/deny priority order when policies are configured on different levels (group, user,  
    etc.)?