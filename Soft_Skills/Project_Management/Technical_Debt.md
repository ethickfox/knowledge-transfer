---
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Planned
Topic:
  - "[Clean Code](Computer%20Science/Development/Clean%20Code%5C%5C)"
---
  

Technical debt is a concept that illustrates the implicit cost of further rework caused by implementation of an immediate simple solution over a better approach that could take longer.

**Benefits of Intentional Technical Debt**

- **Meeting time-to-market deadline  
      
    **The time-to-market period covers the time to generate the idea of a product and then design, develop, and release it. Often a customer or client has strict release deadlines due to marketing events or important demos.  
    In this type of situation, your team has to work quickly and may need to allow for minor code flaws (that don’t disrupt a successful release) to meet the deadline. These issues then become technical debt that your team addresses after the release.  
    
- **Creating POCs and MVPs  
      
    **If your team prepares a Proof of Concept (POC) or a Minimum Viable Product (MVP) for a customer, you need to present the overall idea of how you will implement the solution. In this case, it’s okay if the code quality isn’t perfect.  
    Here, going into technical debt will save time, and there is no risk of harming the business  
    

**Technical Debt Indicators**

**Delivery Indicators**

Delivery managers can recognize technical debt through indicators related to solution quality, ease of introducing changes to the existing system, ability to experiment with new concepts, and human factors.

- **Quality Degradation  
      
    **As technical debt accumulates, a system experiences an increase in the number of production, regression, and known issues:
    - **Production issues** are defects and production bugs.
    - **Regression issues** appear because of side effects provoked by a defect in the old functionality. A team conducts regression testing before releasing a new build to production, and the existing technical debt influences the number of bugs they find during the testing. When a team makes changes in one part of the system, quality degradation leads to a situation when a defect or unexpected behavior might suddenly appear in another part of the system.
    - **Known issues** are bugs that a team is aware of but that are considered non-critical and suitable for release. A list of known issues is often a sign that a team has technical debt they need to manage.
- **High Cost of System Change  
      
    **If the amount of effort and time it takes to make a new change increases, it can indicate technical debt. If you have introduced unintended technical debt, understanding the fix, bypassing the restrictions existing in the system, and testing and documenting a new change will require an excessive level of development efforts. In addition, the existing code becomes hard to reuse, which also increases the workload.
- **Inability to Experiment Quickly  
      
    **A typical software development process includes investigating technical possibilities and preparing **spikes** and POCs. These activities often have a limited timeline, which is usually enough for a team to produce a desirable outcome. However, if you realize that your work on spikes or POCs is accompanied by many side effects, deployment issues, and a lack of transparency and readability, this could indicate technical debt.
- **Increased Barriers to Entry  
      
    **If you find that navigating through your code requires expert knowledge, it could indicate technical debt. If only one or two teammates can implement the functionality (bus factor alert!) and the learning curve for a new person is very high, be on the lookout for technical debt.

**Architecture Indicators**

- **Hard to Integrate  
      
    **When your team finds it difficult to integrate new features into the system or perform system-to-system integration, it might be a sign of technical debt. Technical debt makes it impossible for your team not only to implement your plan, but even to imagine a way to conduct that integration.
- **Hard to Reuse  
      
    **A high-quality design has low coupling and high cohesion. In contrast, high coupling and low cohesion are serious system flaws that make it hard to reuse existing components in the system. If your design is difficult to use due to these factors, there is a good chance that technical debt has accumulated.
- **Hard to Grow  
      
    **When your team sees that a system doesn’t support scaling, or if it is hard to increase performance, this might be an indicator of technical debt.
- **Hard to Support  
      
    **Technical debt can also cause low system maintainability and documentation quality. When documentation quality only covers some areas of a solution, it is an alarming sign. Teams can find it difficult or even impossible to update documentation amongst the chaos caused by technical debt.

**Team Indicators**

- **Delays  
      
    **Delays caused by project intricacies are a common indicator of technical debt. These issues can manifest as difficulty with onboarding new project team members or necessary overtime during the regression phase.
- **Low Level of Confidence in Scope Estimation  
      
    **If you see that your team either underestimates or overestimates project scope, that might indicate technical debt. It usually makes it hard for a team to make reliable estimations for the time they need to complete the tasks due to the unpredictable issues technical debt might cause.
- **Demotivation  
      
    **The impacts of technical debt can lead to team demotivation. It occurs when teams feel that project circumstances are forcing them to code quickly without a chance to follow best practices and generally accepted standards. Sometimes the effects of demotivation are so strong that team members may request to be removed from the project.

**Tracking Technical Debt**

- **Keep the Technical Debt Registry Up-to-Date  
      
    **A technical debt registry is a free form shared document where a team puts notes about the technical debt they identify. The goal of this document is to have all the notes in one place so that the team can address them in the future.  
    Your team needs to keep the technical debt registry up to date to realistically assess the current situation and sort your debt repayment plan into tasks that make up your backlog.  
    
- **Use a Technical Debt Backlog  
      
    **A technical debt backlog is a list of tasks a team makes by regularly cleaning up and sorting the registry. After that, the team puts a new list of tasks into the tracking system, then describes and prioritizes the tasks.  
    The technical debt backlog helps a team make informed decisions about how much time and capacity they need to minimize their technical debt and track their progress on repaying it.  
    
- **Use Technical Debt Management Tools  
      
    **Your team can use technical debt management tools—including quality gate tools—to track issues and build a debt minimization strategy. Unlike the technical debt registry and backlog, where your team has to log the debt themselves, technical debt management tools identify the technical debt automatically.  
    Quality gate tools—like SonarQube—can help your team visualize and track:  
    - The percent of test coverage
    - The code complexity index
    - The number of rule violations
    - Other quantitative indicators

**Managing Technical Debt**

To ensure that project implementation runs smoothly, leaders and managers regularly keep track of each project’s status. Often, they use red, amber, and green (RAG) colors in reporting systems to summarize the current status:

- **Red** means there are issues your team needs to address for the project to be delivered successfully.
- **Amber** signals the project has some potential issues that your team may need to revisit in the future.
- **Green** indicates healthy project performance.

**The Risks of Unmanaged Technical Debt**

- **Substantial Damage to Quality  
      
    **It’s difficult to add new features and functionality on top of existing technical debt. At some point, it might become impossible to produce high-quality code because it will not comply with the existing codebase.  
    As a result, the quality of the final product deteriorates, and the entire solution may need to be reworked.  This often leads to increased costs and disrupted timelines, which ultimately result in customer dissatisfaction.  
    
- **Scalability and Performance Issues  
      
    **Neglected technical debt might make it difficult or impossible to scale a solution and/or improve its performance. To ensure that the solution works for a larger number of end users, requests, or business processes, your team must first fix the initial technical trade-offs. Otherwise, they will lead to more complex issues in the future.  
    For example, if a team reaches a scalability limit with a solution’s current design, further scaling becomes too expensive.  
    
- **Team Issues  
      
    **When there’s significant technical debt on a project, it’s difficult to move forward because that debt needs to be minimized first. It requires a lot of time and effort to balance fixing those initial flaws with moving forward in development.  
    As a result, team members might feel demotivated and burnt out, and their work product suffers.  
    
- **Tense Communication  
      
    **Unmanaged technical debt and the resulting complication can be a source of tension with a client or customer.  
    For example, if the existence or amount of technical debt is not transparent to a client, a software development team might damage its reputation.  
    

**Recommendations for Leaders**

- **Reserve Time for Technical Debt Tasks.** Make sure your team has time to resolve the technical debt. Plan a buffer in one of your sprints or agree on a stabilization sprint to ensure that your team has enough time to complete technical debt tasks.
- **Motivate Your Team.** Encourage technical debt mitigation and highlight team members' achievements to motivate them. This approach will encourage your team to prioritize resolving technical debt.
- **Report Technical Debt Statistics.** Update your team by including technical debt statistics at the iteration, release, and milestone levels. The more visibility your team has, the better.
- **Conduct Regular Review Meetings with Client Stakeholders.** Keep your customer updated about technical debt. When you meet with the stakeholders, review the relevant statistics and technical debt mitigation roadmap together. You can also involve your product owner in technical debt backlog prioritization. These suggestions will help you avoid miscommunication and build informed consensus around project timelines.

**Recommendations for Developers**

- **Avoid Code Smells.** As a developer, the most significant contribution you can make to technical debt management is to avoid code smells by following clean code principles, such as Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion (SOLID) and Don't Repeat Yourself (DRY).
- **Conduct Code Reviews.** Code review can help your team identify code discrepancies and signal if your team follows the same standards and guidelines. The earlier you can identify an issue, the faster you can ensure it doesn’t lead to any issues with your solution.
- **Share Knowledge.** Effective knowledge sharing ensures everyone on your team is on the same page. The more knowledge keepers you have on your project, the more stable it is.
- **Use Code Analysis Tools.** Various code analysis tools can help your team check for reliability, maintainability, and other code characteristics. Checking code manually instead of using appropriate tools is not an efficient use of time and does not guarantee you will identify all flaws.
- **Use Automated Testing.** Automated testing can help your team identify issues that arise from neglecting software design principles. Automating unit, Application Programming Interface (API), and end-to-end (E2E) testing will provide your team with fast feedback that will enable you to introduce safe changes into your solution.