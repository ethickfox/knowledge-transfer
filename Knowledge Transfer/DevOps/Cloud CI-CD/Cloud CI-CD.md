---
Interview graded: false
Last edited time: 2023-09-12T17:37
Needs Rework: false
Status: Not started
Topic:
  - "[[AWS]]"
---
![[/Untitled 54.png|Untitled 54.png]]

**Continuous integration** is a DevOps software development practice where developers regularly merge their code changes into a central repository, after which automated builds and tests are run. Continuous integration most often refers to the build or integration stage of the software release process and entails both an automation component (e.g. a CI or build service) and a cultural component (e.g. learning to integrate frequently).

  

**Continuous delivery** is a software development practice where code changes are automatically prepared for a release to production.

With continuous delivery, every code change is built, tested, and then pushed to a non-production testing or staging environment. There can be multiple, parallel test stages before a production deployment. The difference between **continuous delivery** and **continuous deployment** is the presence of a manual approval to update to production. With continuous deployment, a release to production happens automatically without explicit approval.

![[Untitled 1 15.png|Untitled 1 15.png]]

**CodeCommit** is a secure, highly scalable, managed source control service that hosts private Git repositories. It provides:

- Integration with other AWS services
- Pull requests and approval templates
- Access control

**CodeBuild** is a fully managed build service in the cloud. It's features include:

- Fully managed – CodeBuild eliminates the need to set up, patch, update, and manage your own build servers.
- On demand – CodeBuild scales on demand to meet your build needs. You pay only for the number of build minutes you consume.
- Out of the box – CodeBuild provides preconfigured build environments for the most popular programming languages. All you need to do is point to your build script to start your first build.

**CodeDeploy** is a deployment service that automates application deployments to Amazon EC2 instances, on-premises instances, serverless Lambda functions, or Amazon ECS services. You can deploy:

- Code
- Serverless AWS Lambda functions
- Web and configuration files
- Executables
- Packages
- Scripts
- Multimedia files  
      
    

**CodePipeline** is a continuous delivery service you can use to model, visualize, and automate the steps required to release your software. It uses other AWS Services as building blocks to automate a workflow.

**App Runner** is an AWS service released on the 18th of May 2021 with description as the simplest way to build and run your containerized web application in AWS. It automatically builds, deploys applications, and distributes traffic with encryption. App Runner can scale up or down automatically to meet your traffic needs.

CI/CD field has a wide variety of tools to choose from. The most well-known solutions include:

- Jenkins
- GitLab
- TravisCI
- GitHub Actions
- TeamCity
- Azure DevOps

  

![[/Untitled 2 11.png|Untitled 2 11.png]]

1. As input, you must provide CodeBuild with a build project. A build project includes information about how to run a build, including where to get the source code, which build environment to use, which build commands to run, and where to store the build output. A build environment represents a combination of operating system, programming language runtime, and tools that CodeBuild uses to run a build.
2. CodeBuild uses the build project to create the build environment.
3. CodeBuild downloads the source code into the build environment and then uses the build specification (buildspec), as defined in the build project or included directly in the source code. A buildspec is a collection of build commands and related settings, in YAML format, that CodeBuild uses to run a build.
4. If there is any build output, the build environment uploads its output to an S3 bucket. The build environment can also perform tasks that you specify in the buildspec (for example, sending build notifications to an Amazon SNS topic).
5. While the build is running, the build environment sends information to CodeBuild and Amazon CloudWatch Logs.
6. While the build is running, you can use the AWS CodeBuild console, AWS CLI, or AWS SDKs to get summarized build information from CodeBuild and detailed build information from Amazon CloudWatch Logs. If you use AWS CodePipeline to run builds, you can get limited build information from CodePipeline.

CodeDeploy primary components include:

- [Application](https://docs.aws.amazon.com/codedeploy/latest/userguide/applications.html) is name that uniquely identifies the application you want to deploy. CodeDeploy uses this name, which functions as a container, to ensure the correct combination of revision, deployment configuration, and deployment group are referenced during a deployment.
- [Compute platform](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps.html) is a platform on which CodeDeploy deploys an application. There are three compute platforms: EC2/On-Premise, AWS Lambda, Amazon ECS.
- [Deployment configuration](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html) is set of deployment rules and deployment success and failure conditions used by CodeDeploy during a deployment.
- [Deployment group](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups.html) is a set of individual instances. A deployment group contains individually tagged instances, Amazon EC2 instances in Amazon EC2 Auto Scaling groups, or both.
- [Deployment type](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html#welcome-deployment-overview) is a method used to make the latest application revision available on instances in a deployment group. There are two deployment types: In-place and Blue/Green.
- [[gettin]] is an IAM role that you attach to your Amazon EC2 instances. This profile includes the permissions required to access the Amazon S3 buckets or GitHub repositories where the applications are stored.
- [Revision](https://docs.aws.amazon.com/codedeploy/latest/userguide/application-revisions.html) is a version of your application.
    - An AWS Lambda deployment revision is a YAML- or JSON-formatted file that specifies information about the Lambda function to deploy. AWS Lambda revisions can be stored in Amazon S3 buckets.
    - An EC2/On-Premises deployment revision is an archive file that contains source content (source code, webpages, executable files, and deployment scripts) and an application specification file (AppSpec file). EC2/On-Premises revisions are stored in Amazon S3 buckets or GitHub repositories.
    - For Amazon S3, a revision is uniquely identified by its Amazon S3 object key and its ETag, version, or both. For GitHub, a revision is uniquely identified by its commit ID.
- Target revision is the most recent version of the application revision that you have uploaded to your repository and want to deploy to the instances in a deployment group. In other words, the application revision currently targeted for deployment. This is also the revision that is pulled for automatic deployments.
- [Service role](https://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-create-service-role.html) is an IAM role that grants permissions to an AWS service so it can access AWS resources. The policies you attach to the service role determine which AWS resources the service can access and the actions it can perform with those resources. For CodeDeploy, a service role is used for the following:
    - To read either the tags applied to the instances or the Amazon EC2 Auto Scaling group names associated with the instances. This enables CodeDeploy to identify instances to which it can deploy applications.
    - To perform operations on instances, Amazon EC2 Auto Scaling groups, and Elastic Load Balancing load balancers.
    - To publish information to Amazon SNS topics so that notifications can be sent when specified deployment or instance events occur.
    - To retrieve information about CloudWatch alarms to set up alarm monitoring for deployments.
- [AppSpec file](https://docs.aws.amazon.com/codedeploy/latest/userguide/application-specification-files.html) is a YAML-formatted or JSON-formatted file. The AppSpec file is used to manage each deployment as a series of lifecycle event hooks, which are defined in the file.

![[/Untitled 3 11.png|Untitled 3 11.png]]

## Your goals

- Understand basic concepts of CI/CD
- Be familiar with
    - CodeCommit
    - CodeBuild
    - CodeDeploy
    - CodePipeline
- Know available deployment strategies
- Be familiar with available CodePipeline actions

## Self-check:

- How to set up different deployment strategies (blue-green, canary) via the AWS CodeDeploy?
- What is application, revision, deployment, deployment configuration, and deployment group in  
    terms of AWS CodeDeploy?  
    
- What is stage, action and transition in CodePipeline?
- What is pricing for the AWS CodeCommit/AWS CodeDeploy/AWS CodePipeline services?
- What is CI?
- What is CD (delivery)?
- What is CD (deployment)?
- What CodePipeline best practices do you know?
- How easy is it to migrate from more traditional services like GitHub/GitLab onto CodeCommit?
- How would you compare CodeCommit to Git?
- What are the key elements of an app spec in CodeDeploy?
- What are the key elements of a build spec in CodeBuild?
- What AWS services may aid CodeBuild execution?
- What 3rd party tools and other AWS services may participate CodePipeline execution?