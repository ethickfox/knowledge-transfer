The management of essential technical operations—hardware, software, networking in physical and virtual environments—aiming to minimize downtime and maintain business productivity.

**System Infrastructure Options**

![Untitled 93.png](../Software_Architecture/_img/Untitled%2093.png)

**Physical Infrastructure  
  
**Traditional physical on-premises data centers represent the most basic IT infrastructure.  
  
**Benefit**

- Sufficient security — Companies own and control them privately, so sensitive data won’t be exposed to a threat.

**Drawback**

- Under-utilization — In typical physical data centers, server utilization is less than 20%.

**When to Use Physical Infrastructure**

When a team needs to develop software that uses custom hardware (e.g., automotive or medical equipment), and virtualization is not available because of the characteristics of custom hardware.

  

**On-Premises Virtualization  
  
**Virtual infrastructure provides the same IT capabilities as physical infrastructure but uses software components to create an IT environment. For example, modernized data centers provide end-to-end virtualization (running a virtual computer system that is not tied to the actual hardware, such as more than one OS running on the same machine) and consolidation with physical servers. To order a virtual machine, users must contact data center support.

**Benefits**

- Sufficient utilization — The utilization level for virtual infrastructure is about 50% due to the more efficient use of IT assets; one physical server is used for several virtual machines. Capital costs are lower since the company does not need to invest in on-site resources.
- Automation — Virtualization allows automatic updates to the hardware and software.

**Drawbacks**

- Difficult management — Your team must manage a hypervisor (a host operating system) and many virtual machines (guest operating systems).
- Costly scalability — Costs of scaling **instances** can be extremely high.

**When to Use On-Premises Virtualization**

When a team does not have the competency level required to use a private cloud and needs to connect custom hardware or software to specific devices (such as a car’s onboard computer).

  

**Private Clouds  
  
**A private cloud is a shared pool of computing and networking resources that an organization owns and uses for numerous projects. Private clouds help businesses increase infrastructure productivity and system usability with features such as automated provisioning, automated infrastructure management, and self-service for users.

**Benefit**

- Self-service for employees — Users can order, return to the shared pool, and reorder virtual machines themselves through a web console.

**Drawbacks**

- High capital costs — Although the operating costs of a private cloud are moderate, this infrastructure type is still expensive. An organization must not only purchase it but also hire extra personnel for its maintenance.
- Under-utilization — Privately owned resources are underused from time to time.

**When to Use Private Clouds**

When an organization needs its own data centers because of regulatory, governmental, or data protection reasons and wants to provide users with the capabilities to manage and optimize infrastructure usage through self-service.

**Public Clouds and Elastic Computing**

Public clouds and elastic computing are the most modern infrastructure option. A public cloud is an infrastructure provided by a vendor for commercial use, for example, for hosting applications. Elastic computing allows organizations to expand or decrease computer processing, memory, and storage resources on demand. Public clouds enable elastic computing for their users and make it global. They enable users to launch applications across the globe by using **points of presence**

functionality.

**Benefits**

- Easy infrastructure management — Because a third party manages public clouds, teams do not need to invest efforts in managing physical infrastructure. The vendor takes care of that.
- Scalability — Teams can easily scale the public cloud upon request.

**Drawbacks**

- Costs — Although a project pays no capital expenses, operating costs in public clouds are high because it’s the only revenue that cloud providers have.
- Lack of customization — Public cloud providers usually offer a standard solution for all users. If a team has a unique need, a public cloud solution might not meet the specific requirements.

**When to Use Public Clouds and Elastic Computing**

When an organization does not have its own servers and wants to avoid capital costs.

  

**Cloud Financial Operations**

A developing discipline and practice that enables organizations to maximize commercial value by helping engineering, finance, and business teams collaborate on cloud financial management.

- **Terminate Resources You Don't Use  
      
    **Terminate resources you no longer need or shut them down temporarily if they are not currently being used. Such resources could be a virtual machine you once spun up to validate your ideas or development environments you're not using while on an extended vacation.
- **Use Discounts  
      
    **Agree with the client to take advantage of cloud provider discounts. For example, a virtual machine that is prepaid for three years may be several times less expensive than a regular instance. Demonstrating a commitment to the cloud provider helps you get the instance at a better price.
- **Use Resources Optimally  
      
    **Before spinning up a resource, think about how much you will use it. For example, using only 10% of a virtual machine's or database's capacity may not be the best choice. The rule of thumb is that the size of a resource should correspond to its load.
- **Upgrade Resources  
      
    **From time to time, cloud providers come up with new types of virtual machines or storages that are more efficient and less expensive than the previous ones—so-called resource generation. Try to keep your resources up to date to maximize efficiency and save costs

**Cloud Service Models**

- **IaaS  
      
    **Enabled by virtualization, Infrastructure-as-a-Service is a foundation of all cloud service models. This layer provides on-demand services, such as virtual machines and software defined networks to form the basic cloud infrastructure and run applications.
    
    **Benefit**
    
    - Autoscaling — projects with continuously changing capacity requirements can scale the size of a fleet up or down. For example, a team can use maximum capacity when a system is loaded critically (e.g., on Black Friday) or minimum when the capacity is not needed. You can configure autoscaling rules through infrastructure management tools.
    
    **Drawback**
    
    - Maintenance — Teams opting to run solely on an IaaS service model must have access to the network or system architects and the system engineering team, who will be deeply involved in designing and maintaining the application's architecture.
    
    **When to Use IaaS**
    
    When teams have the capacity and competency to manage the cloud themselves and avoid spending time and money on purchasing hardware.
    
- **PaaS  
      
    **A Platform-as-a-Service model runs on top of the IaaS layer and provides necessary components, such as an operating system (OS), runtime environment, middleware, and a database. By leveraging these services from providers rather than developing and administrating them in-house, your team can focus on adding functionality that contributes to business value.
    
    **Benefits**
    
    - Costs — PaaS can significantly reduce costs since your team can use an existing platform.
    - Availability and customization — Your team can simplify some challenges that come up if you are rapidly developing or deploying an app because PaaS solutions are highly available (i.e., they are always ready and will always work for end users). As a result, PaaS allows for any customizations necessary for a particular project.
    - Simplicity — PaaS solutions allow simple and cost-effective development and deployment of applications.
    
    **Drawbacks**
    
    - Security — Your application and services data are stored in the vendor-controlled cloud servers, which poses security risks.
    - Vendor lock-in — You may have difficulties in migrating to alternative PaaS solutions in the future.
    
    **When to Use PaaS**
    
    When multiple teams and vendors need to collaborate on a project and the project doesn’t need access to OS-level customization or to installing custom hardware.
    
- **SaaS  
      
    **A Software-as-a-Service model delivers an entire software stack to the users who typically pay for the subscription. The SaaS provider is 100% responsible for managing system updates, monitoring security, and handling any other tasks, so your team can have no worries about application coding or infrastructure hosting. Examples of SaaS products are Microsoft Office 365 or SalesForce.  
      
    **Benefit**
    
    - Outsourcing — SaaS is a wise decision when a full-featured, well-maintained software that meets the needs of your project is available.
    
    **Drawbacks**
    
    - Vendor lock-in—You may have difficulties in migrating to alternative SaaS solutions in future.
    - Lack of customization—Because SaaS solutions cannot be customized, they cannot cover all needs of big projects. However, you can use them for certain parts of a project as long as you ensure that:
        - an offering provides the required functionality
        - all users and other applications can access software securely
        - there are no objections to having an outside organization owning the cloud features and enhancements
    
    **When to Use SaaS**
    
    When teams need to launch a standardized application quickly and don’t want to waste time creating software with the functionality they can obtain via subscription.
    

|   |   |   |
|---|---|---|
|**IaaS**|**PaaS**|**SaaS**|
|◦ Virtualization  <br>◦  <br>Servers  <br>◦  <br>Storage  <br>◦  <br>Networking|◦ Runtime  <br>◦  <br>Middleware  <br>◦  <br>Operating System  <br>◦  <br>Virtualization  <br>◦  <br>Servers  <br>◦  <br>Storage  <br>◦  <br>Networking|◦ Applications  <br>◦  <br>Data  <br>◦  <br>Runtime  <br>◦  <br>Middleware  <br>◦  <br>Operating System  <br>◦  <br>Virtualization  <br>◦  <br>Servers  <br>◦  <br>Storage  <br>◦  <br>Networking|

**Approaches to Infrastructure Management**

- **Manual  
      
    **With a manual approach, engineers configure infrastructure through a web UI, such as:
    
    - AWS Management Console
    - Google Cloud Console
    - Microsoft Azure Portal
    
    It requires a lot of time and special skills since each web console has a vast number of unique functions.
    
- **Automatic  
      
    **An automatic approach for infrastructure management is called **Infrastructure as Code** (IaC). With an automatic approach, an infrastructure management system configures the infrastructure itself. There are two types of automatic approaches: imperative and declarative.
    
    With the **imperative approach**, system engineers create a sequence of specific commands, focusing on how they should change the infrastructure.
    
    With the **declarative approach**, system engineers focus on what the eventual target configuration should be, and the system itself makes changes required to achieve that desired state. Compared to the imperative approach, the declarative approach requires less time and effort to configure the infrastructure. Therefore, teams that use IaC usually opt for the declarative approach.
    

**Infrastructure as Code**

Managing and configuring computer data centers through machine-readable definition files instead of configuring them manually or using interactive configuration tools.

![Untitled 1 23.png](../Software_Architecture/_img/Untitled%201%2023.png)

- **Infrastructure Code  
      
    **The infrastructure code consists of files with code that explain how the infrastructure should be set up.
- **Version Control System (VCS)  
      
    **The Version Control System stores both source code (committed by developers) and infrastructure code (committed by system engineers) in the same place.
- **CI Server  
      
    **The CI server works with both source code and infrastructure code. It takes source code from the VCS and compiles it to create an application. In parallel, the CI server takes infrastructure code from the VCS and runs an infrastructure management tool for configuring environments.
- **Infrastructure Management Tool  
      
    **The infrastructure management tool uses infrastructure code for configuring environments.

**Benefits of IaC**

- **Configuration drift detection**
    
    An infrastructure management tool checks whether the current system infrastructure is the same as the IT infrastructure model. If someone makes changes manually instead of using the IaC approach, the system will detect the configuration drift.
    
- **Predictability of changes**
    
    You can review planned changes in the infrastructure before the system implements them.
    
- **Documented inventory**
    
    Your team can examine all existing assets in one place instead of reviewing several sections in the web console. The infrastructure code itself also explains the infrastructure management process, so you don’t need to write additional documentation.
    
- **Modularity**
    
    You can partition system infrastructure and manage pieces separately.
    
- **Strong community support**
    
    Widely used open-source tools have numerous modules and parts of code already written by other community members. As a result, you can significantly reduce time on configuring the infrastructure.
    

**Drawbacks of IaC**

- **Difficult migration**
    
    You can easily manage virtual machines if you use IaC from the beginning of a project. However, you are likely to face challenges in transitioning to IaC for existing legacy resources.
    
- **Barriers to entry**
    
    Your team will need time and expertise in framework and code development to initially create and configure a system using IaC.
    
- **No edge use cases support**
    
    Even with IaC, you will need to use the web console from time to time for occasional but specific tasks.
    

  

**Infrastructure Management Tools**

|   |   |
|---|---|
|**Single-Cloud**|**Multi-Cloud**|
|Single-cloud tools work with a particular cloud. Although all single-cloud management tools use .json/.yaml file format, they are not interoperable. An engineer needs to know each cloud domain’s specific language.  <br>  <br>Examples:  <br>◦  <br>AWS (Amazon Web Services) CloudFormation  <br>◦  <br>GCP (Google Cloud Platform) Deployment Management  <br>◦  <br>Azure Resource Manager|By contrast, multi-cloud tools enable configuring any cloud using a single framework language. These open-source tools allow teams to manage all multi-cloud infrastructure in one place.  <br>  <br>Examples:  <br>◦  <br>Terraform  <br>◦  <br>Pulumi  <br>◦  <br>Puppet  <br>◦  <br>Chef|

  

**Terraform**

An open-source tool for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing, popular service providers as well as custom in-house solutions.

- **Multifunctional  
      
    **Terraform can manage both low-level and high-level infrastructure components. Your team can configure instances, storage, and networking, as well as IaaS and PaaS features.
- **Universal  
      
    **Terraform utilizes a universal programming language that allows your team to work with any cloud you need. Instead of learning multiple vendor-specific languages, you can study a universal programming language and use it for all clouds.
- **Predictable  
      
    **Terraform enables you to change infrastructure safely and predictably because you can review the suggested changes before applying them. The tool also creates incremental execution plans, which can be applied after you make the initial changes.
- **Collaborative  
      
    **Terraform files can be shared among team members and treated as code, so other engineers can review and make changes to them.