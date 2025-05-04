[CLF-02 Cheatsheet](CLF-02%20Cheatsheet.md)

  

## **Domain 1: Cloud Concepts**

**6 advantages of cloud computing**

- **Trade fixed expense for variable expense** – Instead of having to invest heavily in data centers and servers before you know how you’re going to use them, you can pay only when you consume computing resources, and pay only for how much you consume.
- **Benefit from massive economies of scale** – By using cloud computing, you can achieve a lower variable cost than you can get on your own. Because usage from hundreds of thousands of customers is aggregated in the cloud, providers such as AWS can achieve higher economies of scale, which translates into lower pay as-you-go prices.
- **Stop guessing capacity** – Eliminate guessing on your infrastructure capacity needs. When you make a capacity decision prior to deploying an application, you often end up either sitting on expensive idle resources or dealing with limited capacity. With cloud computing, these problems go away. You can access as much or as little capacity as you need, and scale up and down as required with only a few minutes’ notice.
- **Increase speed and agility** – In a cloud computing environment, new IT resources are only a click away, which means that you reduce the time to make those resources available to your developers from weeks to just minutes. This results in a dramatic increase in agility for the organization, since the cost and time it takes to experiment and develop is significantly lower.
- **Stop spending money running and maintaining data centers** – Focus on projects that differentiate your business, not the infrastructure. Cloud computing lets you focus on your own customers, rather than on the heavy lifting of racking, stacking, and powering servers.
- **Go global in minutes** – Easily deploy your application in multiple regions around the world with just a few clicks. This means you can provide lower latency and a better experience for your customers at minimal cost.

### AWS Well-Architected Framework

**1.** **Scalability**

- **Scaling Horizontally** – an increase in the number of resources
- **Scaling Vertically** – an increase in the specifications of an individual resource

**2.** **Disposable Resources Instead of Fixed Servers**

- **Instantiating Compute Resources** – automate setting up of new resources along with their configuration and code
- **Infrastructure as Code** – AWS assets are programmable. You can apply techniques, practices, and tools from software development to make your whole infrastructure reusable, maintainable, extensible, and testable.

**3.** **Automation**

- **Serverless Management and Deployment** – being serverless shifts your focus to automation of your code deployment. AWS handles the management tasks for you.
- **Infrastructure Management and Deployment** – AWS automatically handles details, such as resource provisioning, load balancing, auto scaling, and monitoring, so you can focus on resource deployment.
- **Alarms and Events** – AWS services will continuously monitor your resources and initiate events when certain metrics or conditions are met.

**4.** **Loose Coupling**

- **Well-Defined Interfaces** – reduce interdependencies in a system by allowing various components to interact with each other only through specific, technology agnostic interfaces, such as RESTful APIs.
- **Service Discovery** – applications that are deployed as a set of smaller services should be able to be consumed without prior knowledge of their network topology details. Apart from hiding complexity, this also allows infrastructure details to change at any time.
- **Asynchronous Integration** – interacting components that do not need an immediate response and where an acknowledgement that a request has been registered will suffice, should integrate through an intermediate durable storage layer.
- **Distributed Systems Best Practices** – build applications that handle component failure in a graceful manner.

**5.** **Services, Not Servers**

- **Managed Services** – provide building blocks that developers can consume to power their applications, such as databases, machine learning, analytics, queuing, search, email, notifications, and more.
- **Serverless Architectures** – allow you to build both event-driven and synchronous services without managing server infrastructure, which can reduce the operational complexity of running applications.

**6.** **Databases**

- Choose the Right Database Technology for Each Workload
- **Relational Databases** provide a powerful query language, flexible indexing capabilities, strong integrity controls, and the ability to combine data from multiple tables in a fast and efficient manner.
- **NoSQL Databases** trade some of the query and transaction capabilities of relational databases for a more flexible data model that seamlessly scales horizontally. It uses a variety of data models, including graphs, key-value pairs, and JSON documents, and are widely recognized for ease of development, scalable performance, high availability, and resilience.
- **Data Warehouses** are a specialized type of relational database, which is optimized for analysis and reporting of large amounts of data.
- **Graph Databases** uses graph structures for queries.
    - Search Functionalities
        - Search is often confused with query. A query is a formal database query, which is addressed in formal terms to a specific data set. Search enables datasets to be queried that are not precisely structured.
        - A search service can be used to index and search both structured and free text format and can support functionality that is not available in other databases, such as customizable result ranking, faceting for filtering, synonyms, and stemming.

**7.** **Managing Increasing Volumes of Data**

- **Data Lake** – an architectural approach that allows you to store massive amounts of data in a central location so that it’s readily available to be categorized, processed, analyzed, and consumed by diverse groups within your organization.

**8.** **Removing Single Points of Failure**

- **Introducing Redundancy**
    - **Standby redundancy –** when a resource fails, functionality is recovered on a secondary resource with the failover process. The failover typically requires some time before it completes, and during this period the resource remains unavailable. This is often used for stateful components such as relational databases.
    - **Active redundancy** – requests are distributed to multiple redundant compute resources. When one of them fails, the rest can simply absorb a larger share of the workload.
- **Detect Failure** – use health checks and collect logs
- **Durable Data Storage**
    - **Synchronous replication** – only acknowledges a transaction after it has been durably stored in both the primary storage and its replicas. It is ideal for protecting the integrity of data from the event of a failure of the primary node.
    - **Asynchronous replication** – decouples the primary node from its replicas at the expense of introducing replication lag. This means that changes on the primary node are not immediately reflected on its replicas.
    - **Quorum-based replication** – combines synchronous and asynchronous replication by defining a minimum number of nodes that must participate in a successful write operation.
- **Automated Multi-Data Center Resilience** – utilize AWS Regions and Availability Zones (Multi-AZ Principle). (See Disaster Recovery section)
- **Fault Isolation and Traditional Horizontal Scaling** – Shuffle Sharding

**9.** **Optimize for Cost**

- **Right Sizing** – AWS offers a broad range of resource types and configurations for many use cases.
- **Elasticity** – save money with AWS by taking advantage of the platform’s elasticity.
- **Take Advantage of the Variety of Purchasing Options** – Reserved Instances vs Spot Instances (See AWS Pricing)

**10.** **Caching**

- **Application Data Caching** – store and retrieve information from fast, managed in-memory caches.
- **Edge Caching** – serves content by infrastructure that is closer to viewers, which lowers latency and gives high, sustained data transfer rates necessary to deliver large popular objects to end users at scale.

**11.** **Security**

- **Use AWS Features for Defense in Depth** – secure multiple levels of your infrastructure from network down to application and database.
- **Share Security Responsibility with AWS** – AWS handles the security OF the Cloud while customers handle security IN the Cloud.
- **Reduce Privileged Access** – implement the Principle of Least Privilege controls.
- **Security as Code** – firewall rules, network access controls, internal/external subnets, and operating system hardening can all be captured in a template that defines a _Golden Environment_.
- **Real-Time Auditing** – implement continuous monitoring and automation of controls on AWS to minimize exposure to security risks.

**12.** **Cloud Architecture Best Practices**

There are various best practices that you can follow which can help you build an application in the AWS cloud. The notable ones are:

1. **Decouple your components**– the key concept is to build components that do not have tight dependencies on each other so that if one component were to fail for some reason, the other components in the system will continue to work. This is also known as loose coupling. This reinforces the Service-Oriented Architecture (SOA) design principle that the more loosely coupled the components of the system are, the better and more stable it scales.
2. **Think parallel** This internalizes the concept of parallelization when designing architectures in the cloud. It encourages you to implement parallelization whenever possible and to also automate the processes of your cloud architecture.
3. **Implement elasticity**This principle is implemented by automating your deployment process and streamlining the configuration and build process of your architecture. This ensures that the system can scale in and scale out to meet the demand without any human intervention.
4. **Design for failure** – This concept encourages you to be a pessimist when designing architectures in the cloud and assumes that the components of your architecture will fail. This reinforces you to always design your cloud architecture to be highly available and fault-tolerant.

## **Domain 2: Security and Compliance**

## **Domain 3: Technology**

## **Domain 4: Billing and Pricing**

  

### APIs

###   
AWS Cloud Adoption Framework (AWS CAF)  

  
  

### AWS Compliance

**Principals** are a person or application that uses the AWS account root user, an IAM user, or an IAM role, to sign in and make requests to AWS.

**Identities** are the IAM resource objects that are used to identify and group. You can attach a policy to an IAM identity. These include users, groups, and roles.

### Compute

Use the Limits page in the Amazon EC2 console to request an increase in the limits for resources provided by Amazon EC2 or Amazon VPC on a per-Region basis.

### Cost management  
  

### Databases  
  

### Amazon EC2 instance types (for example, Reserved, On-Demand, Spot)  
  

### AWS global infrastructure (for example, AWS Regions, Availability Zones)  
  

### Infrastructure as code (IaC)

###   
AWS Knowledge Center  
Machine learning  
Management and governance  

  
  
**Migration and data transfer**

  

###   
Network services  

**Network ACL -** The NACL acts as a firewall, but at the subnet level, not the instance level. A network access control list (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.

**Security Group** - A security group acts as a virtual firewall for your instance to control inbound and outbound traffic. When you launch an instance in a VPC, you can assign up to five security groups to the instance. Security groups act at the instance level, not the subnet level. Therefore, each instance in a subnet in your VPC can be assigned to a different set of security groups.

AWS Partner Network  
AWS Prescriptive Guidance  
AWS Pricing Calculator  

###   
AWS Professional Services  

**Amazon WorkSpaces** provides a Desktop as a Service (DaaS) solution.

  

  
AWS re:Post  
AWS SDKs  
  

### AWS Security

**Amazon Macie** is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS.

**Amazon Inspector** is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. Amazon Inspector automatically assesses applications for exposure, vulnerabilities, and deviations from best practices.

You must provide your **AWS access keys** to make programmatic calls to AWS or to use the AWS Command Line Interface or AWS Tools for PowerShell.

  

  
AWS shared responsibility model  
  

### AWS Solutions Architects  
  
**Cloud Computing Models**

- **Infrastructure as a Service (IaaS)  
      
    **Infrastructure as a Service, sometimes abbreviated as IaaS, contains the basic building blocks for cloud IT and typically provide access to networking features, computers (virtual or on dedicated hardware), and data storage space. Infrastructure as a Service provides you with the highest level of flexibility and management control over your IT resources and is most similar to existing IT resources that many IT departments and developers are familiar with today.
- **Platform as a Service (PaaS)  
      
    **Platforms as a service remove the need for organizations to manage the underlying infrastructure (usually hardware and operating systems) and allow you to focus on the deployment and management of your applications. This helps you be more efficient as you don’t need to worry about resource procurement, capacity planning, software maintenance, patching, or any of the other undifferentiated heavy lifting involved in running your application.
- **Software as a Service (SaaS)  
      
    **Software as a Service provides you with a completed product that is run and managed by the service provider. In most cases, people referring to Software as a Service are referring to end-user applications. With a SaaS offering you do not have to think about how the service is maintained or how the underlying infrastructure is managed; you only need to think about how you will use that particular piece of software. A common example of a SaaS application is web-based email where you can send and receive email without having to manage feature additions to the email product or maintaining the servers and operating systems that the email program is running on.

Storage  
AWS Support Center  

###