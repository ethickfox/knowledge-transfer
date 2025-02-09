---
Interview graded: false
Last edited time: 2023-11-29T20:21
Needs Rework: false
Status: Planned
Topic:
  - "[[DevOps/CI-CD/CI-CD\\|CI-CD]]"
---
- **Continuous integration** means that “members of a team integrate their work frequently” (Fowler) by “collaborating on a shared codebase, merging disparate code changes into a version control system (VCS), and automatically creating and testing builds” (Nemeth).
- **Continuous delivery** means that “the software can be released to production at any time” (Fowler) by “automatically deploying builds to nonproduction environments after the continuous integration process completes” (Nemeth).
- Though the term is sometimes used interchangeably with continuous delivery, **continuous deployment** takes things one step further, meaning every change “automatically gets put into production” (Fowler) “without any operator intervention” (Nemeth).

**Definitions**

- **Continuous Integration  
      
    **Continuous integration requires individual developers working on the same code to continually commit their code branches to a shared repository, where they are merged into the main build. Automated testing verifies that nothing breaks with each merge; when issues arise, they are immediately spotted and resolved. With CI, teams agree on how frequently to commit code—with practices ranging from several times a day to as often as possible—and the status of the current build is made transparent to every team member.
- **Continuous Delivery  
      
    **CD most often refers to continuous delivery: the assurance that every change committed is ready to go to production. Each integrated and validated build is automatically deployed to a staging environment in preparation for release; final deployment to production still requires human approval but then is carried out by tools. Continuous delivery puts the release schedule in the hands of the business instead of IT. With continuous delivery, any build can potentially be released to end users by clicking a button to trigger automated deployment in a matter of seconds or minutes.
- **Continuous Deployment  
      
    **CD can also refer to continuous deployment. With continuous deployment, once the integrated build passes all automated quality tests, it is automatically deployed directly to production. The decision to go live becomes a technical one. The lack of human intervention at the deployment stage means automated testing must be of the highest quality. Continuous deployment is not an essential practice for every project, but it is especially beneficial for large organizations, such as Amazon, that release software daily.

**The CI/CD Pipeline**

![[Untitled 107.png|Untitled 107.png]]

**CI/CD Pipeline as Code**

**A CI/CD pipeline as a script** specifies the stages and the order in which the pipeline needs to run successfully. The following script demonstrates basic pipeline stages, such as build, test, and deploy:

```Clojure
pipeline {
   agent {
       docker{
           image 'maven:3-alpine'
           args '-v /root/.m2:/root/.m2'
       }           
   }
   stages {
       stage('Build') {
           steps { 
               sh ‘mvn -B -DskipTests clean package’ 
            } 
       } 
       stage('Test') { 
           steps { 
               sh ‘mvn test’
           } 
            post { 
                always { 
                    junit 'target/surefire-reports/*.xml'
                } 
             }
         } 
         stage('Deploy') { 
            steps { 
                sh './jenkins/scripts/deploy.sh' 
            } 
         } 
    } 
 }
```

**Basic Rules of CI/CD**

- **Use Version Control  
      
    **Track all code in a version control system. Make sure the VCS is the single source of truth. _Nothing_ can be managed manually or off the record.
- **Integrate Frequently in Small Batches  
      
    **Merge code in small pieces as frequently as possible. Because automated tests are run on every commit, it’s immediately clear which set of changes caused the issue. Developers can easily trace a broken build back to the exact lines of code that caused the problem. The more frequently developers commit, the smaller the change is. The smaller the change is, the faster and easier it is to find and fix issues.  
    Frequent integrations also minimize merge conflicts―the main root cause of integration issues. The more frequently developers merge code, the fewer merge conflicts arise, the easier testing is, and the fewer defects there are to fix.  
    
- **Build Once, Deploy Often  
      
    **Once a specific build (also known as the “artifact”) is created, run all tests against that build to confirm it is ready to go to production. Deploy the same artifact to at least one or two environments that match production as closely as possible.
- **Automate End-to-End  
      
    **Building, testing, and deploying code without manual intervention is the key to reliable and reproducible updates. Even if you’re not planning to deploy code continuously to production, the final production deployment step should run fully unattended after being triggered by a human.
- **Share Responsibility  
      
    **Fix the pipeline when something goes wrong. Do not push any new code until the problem is resolved—think of it like halting the assembly line in a factory. Fixing the build before resuming development work is the entire team’s responsibility.
- **Build Fast, Fix Fast  
      
    **Slow build processes are counterproductive. Strive to eliminate redundant and time-consuming steps. Ensure that your build system has enough **agents**—environments in which builds of the CI/CD pipeline are run—and that the agents have sufficient system resources to build quickly.

**CI Servers**

A **CI server**―aka a **build server**―is a tool that enables continuous integration and delivery. Mature versions of a CI server available on the market support different VCSs and allow developers to choose:

- When and how to trigger a pipeline
- What to do as part of a pipeline (e.g., run tests, deploy to a staging server, apply updates to production)
- How to notify developers of the results of pipelines

**Benefits of Using a CI Server**

- **Reproducibility  
      
    **A CI server provides an isolated, stable, and reproducible environment for builds, eliminating the “works on my machine” syndrome―a common problem when code written by one developer only works in certain environments. Using a CI server ensures that your builds won’t fail on other local machines or package the wrong libraries by mistake.
- **Fast Build Time  
      
    **A build server can take the load of running automated tests from the developer’s local machine and run them in parallel. This is particularly beneficial for large and complex projects that may have a lengthy build time.
- **Automation  
      
    **A build server can also execute any automated processes―such as pushing working code to a staging environment or running automated tests after each deployment―any time the team needs.

**Selecting a CI Server**

- **Servers Integrated into a VCS  
      
    **Modern VCSs contain modules, such as [GitLab](https://docs.gitlab.com/ee/ci/) CI and [Bitbucket CI/CD](https://www.atlassian.com/continuous-delivery/tutorials), that provide CI/CD functionality.
- **Dedicated Servers  
      
    **Dedicated servers, such as [TeamCity](https://www.jetbrains.com/teamcity/) and [Jenkins](https://www.jenkins.io/), support CI/CD and can work with any VCS.

  

Концепция, которая реализуется как конвейер, облегчая слияние только что закомиченного кода в основную кодовую базу. Концепция позволяет запускать различные типы тестов на каждом этапе (выполнение интеграционного аспекта) и завершать его запуском с развертыванием закомиченного кода в фактический продукт, который видят конечные пользователи (выполнение доставки)

## Виды окружений

A CI/CD pipeline automates the process of software delivery. It builds code, runs tests, and helps you to safely deploy a new version of the software. CI/CD pipeline reduces manual errors, provides feedback to developers, and allows fast product iterations.

CI/CD pipeline introduces automation and continuous monitoring throughout the lifecycle of a software product. It involves from the integration and testing phase to delivery and deployment. These connected practices are referred as CI/CD pipeline.

## **Continuous integration**

An automation process for developers. Successful CI means new code changes to an app are regularly built, tested, and merged to a shared repository. It’s a solution to the problem of having too many branches of an app in development at once that might conflict with each other.In modern application development, the goal is to have multiple developers working simultaneously on different features of the same app. However, if an organization is set up to merge all branching source code together on one day (known as “merge day”), the resulting work can be tedious, manual, and time-intensive. That’s because when a developer working in isolation makes a change to an application, there’s a chance it will conflict with different changes being simultaneously made by other developers. This problem can be further compounded if each developer has customized their own local integrated development environment (IDE), rather than the team agreeing on one cloud-based IDE.Continuous integration (CI) helps developers merge their code changes back to a shared branch, or “trunk,” more frequently—sometimes even daily. Once a developer’s changes to an application are merged, those changes are validated by automatically building the application and running different levels of automated testing, typically unit and integration tests, to ensure the changes haven’t broken the app. This means testing everything from classes and function to the different modules that comprise the entire app. If automated testing discovers a conflict between new and existing code, CI makes it easier to fix those bugs quickly and often.

## **Continuous delivery**

Continuous delivery usually means a developer’s changes to an application are automatically bug tested and uploaded to a repository (like GitHub or a container registry), where they can then be deployed to a live production environment by the operations team. It’s an answer to the problem of poor visibility and communication between dev and business teams. To that end, the purpose of continuous delivery is to ensure that it takes minimal effort to deploy new code. Following the automation of builds and unit and integration testing in CI, continuous delivery automates the release of that validated code to a repository. So, in order to have an effective continuous delivery process, it’s important that CI is already built into your development pipeline. The goal of continuous delivery is to have a codebase that is always ready for deployment to a production environment.

In continuous delivery, every stage—from the merger of code changes to the delivery of production-ready builds—involves test automation and code release automation. At the end of that process, the operations team is able to deploy an app to production quickly and easily.

## **Continuous deployment**

Continuous deployment (the other possible "CD") can refer to automatically releasing a developer’s changes from the repository to production, where it is usable by customers. It addresses the problem of overloading operations teams with manual processes that slow down app delivery. It builds on the benefits of continuous delivery by automating the next stage in the pipeline. The final stage of a mature CI/CD pipeline is continuous deployment. As an extension of continuous delivery, which automates the release of a production-ready build to a code repository, continuous deployment automates releasing an app to production. Because there is no manual gate at the stage of the pipeline before production, continuous deployment relies heavily on well-designed test automation.In practice, continuous deployment means that a developer’s change to a cloud application could go live within minutes of writing it (assuming it passes automated testing). This makes it much easier to continuously receive and incorporate user feedback. Taken together, all of these connected CI/CD practices make deployment of an application less risky, whereby it’s easier to release changes to apps in small pieces, rather than all at once. There’s also a lot of upfront investment, though, since automated tests will need to be written to accommodate a variety of testing and release stages in the CI/CD pipeline.

[![](https://lh4.googleusercontent.com/67EnoPTMBIiWMOUpfARxeciiCdLXXVc9RwN6fvHd_3PJvBix8wUg-5vifvVGm_ycOl-gup69MpiHmrdSQTn3ArJc1HTUlvMUJyA71MU710APiVT6hEDWZqTMCg-ZdheyNbU6s0GLKSIQ4yBLy1rAE6FCBwtbuecvTqqOpYbttkbeSp4ZQ9Xt-kxuzOrg)](https://lh4.googleusercontent.com/67EnoPTMBIiWMOUpfARxeciiCdLXXVc9RwN6fvHd_3PJvBix8wUg-5vifvVGm_ycOl-gup69MpiHmrdSQTn3ArJc1HTUlvMUJyA71MU710APiVT6hEDWZqTMCg-ZdheyNbU6s0GLKSIQ4yBLy1rAE6FCBwtbuecvTqqOpYbttkbeSp4ZQ9Xt-kxuzOrg)

### **Could you describe the CI/CD pipelines on the project?**

- **Source Stage** - In the source stage, CI/CD pipeline is triggered by a code repository. Any change in the program triggers a notification to the CI/CD tool that runs an equivalent pipeline. Other common triggers include user-initiated workflows, automated schedules, and the results of other pipelines.
- **Build Stage** - Programs that are written in languages like C++, Java, C, or Go language should be compiled. On the other hand, JavaScript, Python, and Ruby programs can work without the build stage. Failure to pass the build stage means there is a fundamental project misconfiguration, so it is better that you address such issues immediately.
- **Test Stage**includes the execution of automated tests to validate the correctness of code and the behavior of the software. This stage prevents easily reproducible bugs from reaching the clients. It is the responsibility of developers to write automated tests.
- **Deploy Stage** - This is the last stage where your product goes live. Once the build has successfully passed through all the required test scenarios, it is ready to deploy to the live server.

### **Code Pipelines**

Code pipelines are the primary technical artifacts of continuous delivery. Modern-day pipelines transform application and infrastructure source code into versioned packages deployable to any environment. By automating all the mundane tasks to build and deploy systems, teams are free to focus on value-added capabilities.

[![](https://lh4.googleusercontent.com/--ey7GiogBWviUjM6gk6BHggPxj_LK9XgY_YsloVN3yIV8WjShMl43TPBh0jph-7HnQoUsK7SRsgeCDAnAi0QoVQ7Dtbn0tuBTSPPf_CeR1aQ5DksAU092tyV9x0TAD_H75aoqrnWYp9mQmp4TasVYC1ygur_oQAncZ3i4-r8UI0pmd1zpFitavOKhfu)](https://lh4.googleusercontent.com/--ey7GiogBWviUjM6gk6BHggPxj_LK9XgY_YsloVN3yIV8WjShMl43TPBh0jph-7HnQoUsK7SRsgeCDAnAi0QoVQ7Dtbn0tuBTSPPf_CeR1aQ5DksAU092tyV9x0TAD_H75aoqrnWYp9mQmp4TasVYC1ygur_oQAncZ3i4-r8UI0pmd1zpFitavOKhfu)

### **Pipeline Design Patterns**

- **Pipelines as Code** **- Pipeline logic is codified, stored alongside application or infrastructure code and utilizes containerized runners.**
- **Externalize Logic into Reusable Libraries** - Reusable libraries contain common pipeline logic that is referenceable from pipeline code and independently developed and tested.
- **Separate Build and Deploy Pipelines** Build and deploy pipelines should be logically separated, independently runnable and triggered by automated or manual events
    - Build once, deploy many.
    - Be environmentally agnostic.
    - Package it all together
- **Trigger the Right Pipeline** **- Branch commits, pull requests, and merges to the mainline can all trigger different pipeline behavior, optimized to the team’s way of working.**
    - Pushing a commit to an open pull request builds an Ephemeral Environment for testing.
    - Merges to the mainline are deployed to a non-production or demo environment displaying the latest integrated code
    - Pushing a new tag stages a production release.
- **Fast Team Feedback** - Every commit automatically triggers the right pipeline, with build pipelines especially optimized for speed and quick reporting of any issues.
    - Build pipelines use parallelization for non-interdependent jobs to increase speed.
    - Fast build pipelines - only run the jobs that are necessary in a few minutes.
    - Each successful run produces a versioned package and static analysis results.
    - With omni-channel notifications, you can enable team notifications on pull request status in dashboards, chat channels, email, and other mediums.
- **Stable Internal Releases** **Only versioned packages produced by the build pipeline are deployed and these deployments are triggered by humans or automated events.**
    - Each code branch gets a complete ephemeral environment named for the branch that can easily be created or destroyed.
    - Each engineer can stand up and delete ephemeral environments at any time.
    - CI runners use cloud-native IAM capabilities with temporary permissions so they can assume roles and acquire the right permissions to complete their work.
- **Buttoned Up Product Releases -** **Deploy tagged releases to production and automate the paperwork but leave a paper trail.**
    - Codified release gates and standardized release processes enables teams to release on demand.
    - Automated releases leave a transparent paper trail that’s auditable for governance and quality
    - Release gates can invoke external API’s and use the responses to decide whether to proceed with the release or halt.

### **How would you improve your team's pipeline?**

First of all it would be great to add pipelines for our etl services, or at least the deployment part. Because it’s uncomfortable to build it locally, send via ftp and update the current working version manually. For the neli we should add a trigger that will start build and testing process after each push

### **What is the main purpose of CI/CD?**

To automate testing and deployment of your product. To increase development speed. A CI/CD pipeline automates the process of software delivery.

CI/CD pipeline introduces automation and continuous monitoring throughout the lifecycle of a software product.

- CI/CD pipeline can improve reliability.

### **If you are asked to implement a pipeline yourself what steps would it include?**

- Checkout branch
- Build
- Test
- Deploy

Example of pipeline

- Source Code Control.
- Continuous integration - When the changes notify, this tool will pull the code available in GitHub and process to build and run the test.
- Deploy code to UAT
- Deploy to production