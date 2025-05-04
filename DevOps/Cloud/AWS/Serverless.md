![Untitled 106.png](Untitled%20106.png)

  

**Amazon API Gateway**

![Untitled 1 33.png](Untitled%201%2033.png)

[Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. APIs act as the "front door" for applications to access data, business logic, or functionality from your backend services. Using API Gateway, you can create RESTful APIs and WebSocket APIs that enable real-time two-way communication applications. API Gateway supports containerized and serverless workloads, as well as web applications.

API Gateway handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, CORS support, authorization and access control, throttling, monitoring, and API version management. API Gateway has no minimum fees or startup costs. You pay for the API calls you receive and the amount of data transferred out and, with the API Gateway tiered pricing model, you can reduce your cost as your API usage scales.

### API Gateway creates RESTful APIs that:

- Are HTTP-based, supports SSL
- Enable stateless client-server communication
- Implement standard HTTP methods such as `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`

### API Gateway creates WebSocket APIs that:

- Adhere to the [WebSocket](https://datatracker.ietf.org/doc/html/rfc6455) protocol, which enables stateful, full-duplex communication between client and server.
- Route incoming messages based on message content.

### Use cases:

There are two kinds of developers who use API Gateway: **API developers** and **App developers**.

An API developer creates and deploys an API to enable the required functionality in API Gateway. The API developer must be an IAM user in the AWS account that owns the API.

An app developer builds a functioning application to call AWS services by invoking a WebSocket or REST API created by an API developer in API Gateway.

The app developer is the customer of the API developer. The app developer doesn't need to have an AWS account, provided that the API either doesn't require IAM permissions or supports authorization of users through third-party federated identity providers supported by Amazon Cognito user pool identity federation. Such identity providers include Amazon, Amazon Cognito user pools, Facebook, and Google.

With API Gateway, you can launch new services faster and with reduced investment so you can focus on building your core business services. API Gateway was built to help you with several aspects of creating and managing APIs:

1. **Metering**. API Gateway helps you define plans that meter and restrict third-party developer access to your APIs. You can define a set of plans, configure throttling, and quota limits on a per API key basis. API Gateway automatically meters traffic to your APIs and lets you extract utilization data for each API key.
2. **Security**. API Gateway provides you with multiple tools to authorize access to your APIs and control service operation access. API Gateway allows you to leverage AWS administration and security tools, such as AWS Identity and Access Management (IAM) and Amazon Cognito, to authorize access to your APIs. API Gateway can verify signed API calls on your behalf using the same methodology AWS uses for its own APIs. Using custom authorizers written as AWS Lambda functions, API Gateway can also help you verify incoming bearer tokens, removing authorization concerns from your backend code.
3. **Resiliency**. To prevent your APIs from being overwhelmed by too many requests, API Gateway throttles requests to your APIs. Specifically, API Gateway sets a limit on a steady-state rate and a burst of request submissions against all APIs in your account. You can configure custom throttling for your APIs. To learn more, see Throttle API requests for better throughput.
4. **Operations Monitoring**. After an API is published and in use, API Gateway provides you with a metrics dashboard to monitor calls to your services. The API Gateway dashboard, through integration with Amazon CloudWatch, provides you with backend performance metrics covering API calls, latency data and error rates. You can enable detailed metrics for each method in your APIs and also receive error, access or debug logs in CloudWatch Logs.
5. **Lifecycle Management**. After an API has been published, you often need to build and test new versions that enhance or add new functionality. API Gateway lets you operate multiple API versions and multiple stages for each version simultaneously so that existing applications can continue to call previous versions after new API versions are published.
6. **Designed for Developers**. API Gateway allows you to quickly create APIs and assign static content for their responses to reduce cross-team development effort and time-to-market for your applications. Teams who depend on your APIs can begin development while you build your backend processes.
7. **Real-Time Two-Way Communication**. Build real-time two-way communication applications such as chat apps, streaming dashboards, and notifications without having to run or manage any servers. API Gateway maintains a persistent connection between connected users and enables message transfer between them.
8. **Route53 integration** You can create Route 53 alias record that routes traffic to the regional or Edge-optimized API endpoint.

**AWS Lambda**

[**AWS Lambda**](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) is a serverless compute service that runs your code in response to events and automatically manages the underlying compute resources for you.

Lambda runs your code on high-availability compute infrastructure and performs all the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling, code and security patch deployment, and code monitoring and logging. All you need to do is supply the code.

The code you run on AWS Lambda is called a “Lambda function.” After you create your Lambda function starts to wait for trigger requests. Each function includes your code as well as some associated configuration information, including the function name and resource requirements. Lambda functions are “stateless,” with no affinity to the underlying infrastructure, so that Lambda can rapidly launch as many copies of the function as needed to scale to the rate of incoming events.

AWS Lambda could be configured for execution in VPC. However, startup delays would be bigger.

CPU allocation for Lambda could be adjusted by changing the amount of allocated memory to the function. If you increase memory for your function - then you would get better CPU power and vice versa.

### Lambda key features

1. **Concurrency and scaling controls.** Concurrency and scaling controls such as concurrency limits and provisioned concurrency give you fine-grained control over the scaling and responsiveness of your production applications.
2. **Functions defined as container images.** Use your preferred container image tooling, workflows, and dependencies to build, test, and deploy your Lambda functions.
3. **Code signing.** Code signing for Lambda provides trust and integrity controls that let you verify that only unaltered code that approved developers have published is deployed in your Lambda functions.
4. **Lambda extensions.** You can use Lambda extensions to augment your Lambda functions. For example, use extensions to more easily integrate Lambda with your favorite tools for monitoring, observability, security, and governance.
5. **Function blueprints.** A function blueprint provides sample code that shows how to use Lambda with other AWS services or third-party applications. Blueprints include sample code and function configuration presets for Node.js and Python runtimes.
6. **Database access.** A database proxy manages a pool of database connections and relays queries from a function. This enables a function to reach high concurrency levels without exhausting database connections.
7. **File systems access.** You can configure a function to mount an Amazon Elastic File System (Amazon EFS) file system to a local directory. With Amazon EFS, your function code can access and modify shared resources safely and at high concurrency.

### Lambda Function URL

AWS Lambda Function URL is used to assign HTTPS endpoints to your Lambda function, AWS Function URLs is a feature that allows calling Lambda function directly using function URLs.

For **new or existing Lambda functions it is possible to add Function URLs**.

Here is how to add Function URLs during creating a new Lambda function in AWS Console:

- Lambda → Functions → Create function → Advanced settings → Enable function URL

**Serverless Application Model  
  
**

The [AWS Serverless Application Model (SAM)](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings. With just a few lines per resource, you can define the application you want and model it using YAML. During deployment, SAM transforms and expands the SAM syntax into AWS CloudFormation syntax, enabling you to build serverless applications faster.

### Why SAM

1. **Single-deployment configuration**. SAM makes it easy to organize related components and resources, and operate on a single stack. You can use SAM to share configuration (such as memory and timeouts) between resources, and deploy all related resources together as a single, versioned entity.
2. **Local debugging and testing**. Use SAM CLI to locally build, test, and debug SAM applications on a Lambda-like execution environment. It tightens the development loop by helping you find & troubleshoot issues locally that you might otherwise identify only after deploying to the cloud.
3. **Deep integration with development tools**. You can use SAM with a suite of tools you love and use.
    - IDEs: [PyCharm](https://aws.amazon.com/pycharm/), [IntelliJ](https://aws.amazon.com/intellij/), [Visual Studio Code](https://aws.amazon.com/visualstudiocode/), [Visual Studio](https://aws.amazon.com/visualstudio/), [AWS Cloud9](https://aws.amazon.com/cloud9/)
    - Build: [CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/)
    - Deploy: [CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/), [Jenkins](https://wiki.jenkins.io/display/JENKINS/AWS+SAM+Plugin)
    - Continuous Delivery Pipelines: [CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/)
    - Discover Serverless Apps & Patterns: [AWS Serverless Application Repository](https://docs.aws.amazon.com/serverlessrepo/latest/devguide/)
4. **Built-in best practices**. You can use SAM to define and deploy your infrastructure as configuration. This makes it possible for you to use and enforce best practices through code reviews. Also, with a few lines of configuration, you can enable safe deployments through CodeDeploy, and can enable tracing using AWS X-Ray.
5. **Extension of AWS CloudFormation**. Because SAM is an extension of AWS CloudFormation, you get the reliable deployment capabilities of AWS CloudFormation. You can define resources by using CloudFormation in your SAM template. Also, you can use the full suite of resources, intrinsic functions, and other template features that are available in CloudFormation.

## Your goals

- Know what API Gateway is, its features and what it’s used for
- Know what AWS Lambda is and how does it differ from running an application on EC2 instance
- Understand AWS Lambda configuration, using CPU allocation, triggers, etc
- What is SAM

## Self-check

1. What is the biggest benefit of AWS Lambda?
2. What ways of triggering a Lambda do you know?
3. What is the contract of Lambda function?
4. What is Lambda pricing?
5. Which code libraries/frameworks are reasonable to use in Lambda?
6. How Lambda instances are reused? How to prepare the Lambda code for that?
7. What programming languages does Lambda support?
8. What is the difference between synchronous and asynchronous Lambda invocations?
9. What is Lambda concurrency?
10. What kinds of Lambda concurrency allocations are there?
11. What AWS resources can Lambda access? How?
12. What are the advantages of API Gateway endpoints over traditional web applications?
13. What are the typical API Gateway use cases? What is the use case for combining API GW with Lambda?
14. What is the use case for combining API GW with Lambda?
15. What is API Gateway pricing?