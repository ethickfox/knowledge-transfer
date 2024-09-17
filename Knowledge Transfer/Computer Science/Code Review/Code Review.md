---
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Planned
Topic:
  - "[[Knowledge Transfer/Computer Science/Development/Clean Code\\|Clean Code]]"
---
Code review results in several benefits

- **Fewer Defects  
      
    **It is often easier for a reviewer with an outside perspective to identify structural errors (e.g. dead code, logic or algorithm bugs, performance or architecture concerns, etc.) and functional errors (when code does not work as expected). Even short and informal code reviews can significantly impact code quality and bug frequency.
- **Knowledge Sharing  
      
    **The valuable knowledge your team shares during the code review process primarily relates to the functionality of a specific application, its domain, and its business logic. It also covers coding best practices, including optimization and refactoring techniques. Knowledge sharing guarantees all team members are on the same page and strengthens positive communication and cooperation.
- **Consistent Standards  
      
    **Code review ensures that your team members follow the agreed upon style guide. Consistency in a codebase makes code easier to read and understand, prevents bugs, and facilitates collaboration between regular and migratory developers. Legible code is more reusable, bug-free, and future-proof. A code author may not be able to judge the legibility of their code fragment as easily as a reviewer can. Following consistent standards makes the cooperation between code authors and reviewers easier.
- **Compliance  
      
    **Code review is a great way to avoid common technical traps. For example, if your application has strict security requirements, your local security specialist should need to check it to ensure it meets compliance requirements. A reviewer can also spot and replace newly introduced external dependencies with inappropriate licenses or known vulnerabilities before they appear in production.

**The Risks of Neglecting Code Review**

- **Lower Structural Code Quality  
      
    **Neglecting code review can impact code’s structural quality, making it unreadable and difficult to maintain.
- **Lower Functional Code Quality  
      
    **Skipping code review can negatively affect the functional quality of the code, too. Poor quality code, in turn, lowers the product quality.
- **Lack of Knowledge Sharing  
      
    **Neglecting code review may cause some members of your team to miss important information. This can lead to a situation where several team members are implementing similar functionality instead of reusing the existing solution. Moreover, improper knowledge sharing can result in missing some reusable business functionality.
- **Possible Rework  
      
    **Lack of transparency and early feedback in your team might require rework at a later stage. For example, while working on two different modules, several team members can incorporate different technical approaches. To make the code base consistent, one of the approaches will have to be refactored. This kind of situation can lead to interpersonal conflicts among your team members, as well as additional work.
- **Possible Technical Issues  
      
    **Without code review, your team has a higher chance of introducing security issues that affect end users. These issues could lead to sensitive data breaches, vulnerability to ransomware attacks, and other negative consequences for your customers and your company’s reputation.

**Code Review Types and Workflow**

- **Peer Review  
      
    **Peer review allows several team members to review code at different times. Its convenience makes it a popular review type. With the version control system’s help, the author makes code available to other team members for review. After that, the author starts working on another task while their peers perform the review. Various tools and branching strategies facilitate the process. Peer review might be internal or external. When your team conducts an internal peer review, it is a great way to improve knowledge sharing. Your team can also opt for the help of an external specialist with specific expertise who is not part of the team.
- **Specialist's Review  
      
    **A specialist’s review is an example of a cross-team code review practice.  
    Sometimes a code fragment might require the review of a specialist who has specific skills and in-depth knowledge in a particular area. Very often, this kind of specialist is not part of a dev team. A specialist’s review could be an architectural, security, or performance review. This type of review can be required periodically or upon request.  
    
- **Instant Code Review  
      
    **Instant code review allows several team members to review code simultaneously. It is usually conducted as pair programming: when two team members write code and review it line by line.  
    This approach might be appropriate for two developers of approximately the same maturity level who are working on a complex business problem together. It is also useful if more senior developers want to help junior developers enhance their technical skills: a senior developer codes and explains what he or she is doing line by line while a junior developer watches and learns. The same principle applies to onboarding, when a newcomer watches and listens to another team member coding and explaining the essential ideas line by line.  
    

### **Code Review Workflow**

No matter what type of code review your team chooses, there is a universal workflow that applies.

![[/Untitled 51.png|Untitled 51.png]]

**Key Areas of Code Review**

Your team’s code review should cover several major areas.

- **Functional Correctness/Business Logic  
      
    **New changes should not break the existing business logic, and the code author should implement all requested behaviors.
- **Structural Correctness/Design  
      
    **When it comes to implementation, the main task of the reviewer is to think whether the code:
    
    - Handles enough edge cases
    - Could be shorter or faster
    - Could be safer
    - Could be replaced with a more effective functional equivalent
    
    In addition, the reviewer should also ensure that:
    
    - Appropriate patterns are used for solving the problem
    - Redundancies are eliminated
    - Dependencies introduced by the code change are needed or worthwhile
    - All other "clean code" principles are followed
- **Readability/Complexity  
      
    **This area covers your reading experience as a reviewer. There are certain questions you should ask:
    - Am I able to grasp the concepts in a reasonable amount of time?
    - Is the flow logical, and are the variables and the method names easy to follow?
    - Am I able to keep track through multiple files or functions?
    - Is there any inconsistent naming that is confusing?
    - Is the code consistent with the project in terms of style, API conventions, etc.?
    - Does this code have TODO comments?
- **Test Correctness  
      
    **As a reviewer, to check maintainability and tests, you should:
    - Read the tests. If there are no tests, but there should be, ask the author to write them. While truly untestable features are rare, untested implementations of features are unfortunately common.
    - Check the tests themselves. Are they covering edge cases? Are they readable? Does the code change lower overall test coverage?
    - Think of ways the code could break and check if they are handled. Does the change request introduce the risk of breaking test code, staging stacks, or integration tests? Specific things to look for are changes in artifact layout/structure and removal of test utilities or path changes in configuration.
    - Verify consistency. Style standards for tests are often different from the core code, but they are still important.
    - Check if there is enough documentation or if the test covers all use cases of business logic.
- **Non-Functional Hidden Implications  
      
    **Identifying hidden implications is an important part of review. You should:
    
    - Verify that the code does not have security vulnerabilities. For example, API endpoints should perform appropriate authorization, and authentication should be consistent with the rest of the codebase.
    - Check for other common weaknesses, like weak configuration, malicious user input, missing log events, etc.
    
    When in doubt, you can refer a change request to an application security expert, if possible, and check **OWASP** recommendations.
    
    Other examples of potential problems are:
    
    - Unobvious/uncounted behaviors of implementation methods
    - Use of standards and features not supported in required systems/browsers
    - Use of libraries with license types that are not permitted on the project

**Code Review Checklist**

A code review checklist ensures that:

- Reviewers check all the necessary steps without skipping something important.
- The quality of each review is approximately equal, despite each reviewer’s background.
- Newcomers to a project remember/follow all the steps of the process.
- It is easier to introduce a new practice/step that everyone should be aware of and follow. Highlighting it in a shared document makes it hard to forget.

  

Every member of a team can contribute to the creation of a code review checklist for a project. To create a checklist, you need to take several steps.

- **Secure Approval to Create the Code Review Checklist  
      
    **To secure approval to create the checklist, you should typically contact either the project or account manager—depending on your client’s business—as some assistance from their side might be required.
- **Describe in Detail the Process Your Team Follows  
      
    **Next, describe the existing code review procedure that your team follows. Even if it is not documented yet, provide as many details as possible.  
    Be sure to answer the following questions:  
    - How extensive can the changes be from code review?
    - Which checks should be passed before a manual code review?
    - Which tools can team members use?
    - How many reviewers should approve the changes, and how are the reviewers chosen?
    - Which steps should your code review process have?
    - What are the time limits for every step?
    - Which changes of the code parts require an in-depth review?
    - How should non-critical review notes be handled?\
- **Provide Improvement Suggestions  
      
    **If you identify parts of the checklist that your team could improve, do not rush to update the existing process.  
    Instead, create a list of possible improvements and discuss it later with everyone involved.  
    

As your team develops a code review checklist, tailor the final document to the goals of code review and keep these tips in mind:

1. Reaching your code review goals can require a different number of reviewers with a range of expertise. If the goal is knowledge sharing, it is better to engage more reviewers, including new or junior team members. However, focusing on quality or security standards requires participation of the most experienced specialists.
2. Peer review should be mandatory for each piece of code your team produces, while a specialist’s review is mandatory for critical parts only. This approach applies to every code review, regardless of its goal.
3. Pay special attention to high-risk code that implements critical business logic, is performance-critical, or handles sensitive data. High-risk changes are the ones that are most likely to hide mistakes: this might be code written by a new member of the team or code that has been affected by large-scale refactoring.
4. Checklists are living documents that evolve, and every team member must be responsible for updating them. Make it a habit to review the checklist every time you face issues or unclear aspects of the code review process and examine the code review process periodically—for example, during retrospective sessions.
5. Checklists must be accessible to everyone on the team through a shared space, like Confluence or Teams. To make a checklist even more accessible, pin it in your team’s communication channel.

Although creating a checklist is time-consuming and maintaining it requires all team members’ commitment, the long-lasting results are worth the effort.

**Developers’ Checklist**

A developers’ checklist should contain a set of affirmatives. Here is a generalized list; remember that you might need to adapt it for your project.

- My code compiles (e.g., linting passed, tests are passed, quality gates are passed).
- My code has been developer-tested and includes unit tests.
- My code is well-documented.
- My code is tidy (I paid attention to indentation and line length, and I avoided commented-out code, spelling mistakes, etc.)
- I have eliminated unused imports and code editor warnings.
- My code follows my team’s established coding standards.
- There are no hardcoded, development-only details still in the code.
- I considered performance and security.
- No code can be replaced with calls to external reusable components or library functions.

If you are a developer, it is best to review your code after completing the whole change request (not just its pieces) as if you are its initial reviewer.

**Reviewers’ Checklist**

The reviewers’ checklist should cover the following areas:

- Functional Correctness/Business Logic
- Structural Correctness/Design
- Readability/Complexity
- Test Correctness
- Non-Functional Hidden Implications

For a reminder of what is included in each of these areas, review the _Key Areas of Code Review_ section from earlier in this lesson.

The reviewers’ checklist is defined during the project initiation, and the whole team should agree on it. It might require changes at a later stage as a project continues and your team evolves.

**Tips for Implementing Code Review**

- **Use Time Optimization Tips**
    
    - **Limit changes per merge request**. Smaller requests are easier to review. Ensuring that each merge request contains changes for only one issue will make the review process more thorough and effective.
    - **Set a time limit.** Code review requires regularity. To facilitate it, allocate enough time in your schedule for this task (e.g., one hour at the beginning of each day). If there is no merge request to review, then you can use this slot for another task.
    
    Whenever possible, try to complete the review of one coding piece in a day. This will make it easier for you to remember the context, and you will not need to re-explore it the next day. When you begin to lose focus, take a break or temporarily switch to another task.
    
    This rule will help you complete the code review in time instead of postponing it until the end of iteration, and your team will be able to integrate code as soon as it is ready.
    
- **Introduce a Main Reviewer Role for Each Iteration (Sprint)  
      
    **Select a person who will be responsible for the initial code review of every request in a given iteration. This main reviewer should have a reduced workload during this period. The main reviewer chooses additional reviewer(s) for each request based on their peers’ workload and knowledge related to changes in a request. The total number of reviewers depends on team size. It is best to take turns in this activity and select another main reviewer for the next iteration (sprint).
- **Run Static Code Analysis Before Code Review  
      
    **Code review is only one part of the process that ensures the delivery of higher quality code to production. To make this process faster and more efficient, you should automate code review as much as possible. Automation not only saves time, but it also helps your team ensure accuracy and higher quality of review.  
    Many different tools, linters, code analyzers, and systems—like SonarQube or Codacy—help automate code review. One of the new trends is using machine learning, and there are some products for that on the market. Your team should choose the most appropriate tool based on your specific project and needs.  
    The greatest advantage of automatic checks is that the reviewer will not need to spend additional time checking the same aspects covered by automation.  
    
- **Run Tests Before the Code Review  
      
    **Your branching strategy and CI/CD process can help you reduce the time your team spends on code review. Only review high-quality code that has passed all possible automated checks and tests.  
    The code author should run tests before code review to ensure the codebase is stable (i.e., there are no tests failing). Such an approach would enable passing the project quality gates. “All tests should pass” is the most common quality gate.  
    Running tests before code review will also save a lot of time. Reviewing code that did not pass tests will require additional work: after a reviewer provides feedback, the code author will incorporate it, fix the issues, and then submit it back for another round of review. If you see high-risk code, you may apply additional automated testing to make the review of changes in the code simpler and quicker.  
    Your team might also try   
    **TDD**,**BDD**, and other techniques that can help you ensure the correctness of new code without a detailed investigation.
- **Use Appropriate Tools  
      
    **Manual code review complements automated checks. For manual code review, you can use the Web UI of GitHub and GitLab, plugins for VSCode and JetBrains products, and other tools. You can highlight changes, mark checked parts, make notes linked to the code, and use other useful features. Again, your team should be empowered to decide how to establish a convenient environment to conduct manual code reviews on your project. Try different tools and consider your project’s unique requirements.

**Conducting Ethical Code Reviews**

- **Comment on the Code, Not the Author  
      
    **It's vital to be courteous and respectful to the developer whose code you're reviewing while at the same time being clear and helpful. You can do this by making sure your comments are always about the code and never about the author. Make sure you follow this practice when you need to point out a potentially contentious issue.  
      
    _A bad example:_ Why did you use threads in this case when there is clearly no performance gain from this concurrency?_  
    A good example:  
    _ In my opinion, the concurrency model here adds complexity to the system without providing any performance gain. Since there is no performance gain, it would be better to keep the code single-threaded instead of using multiple threads.
- **Personalize Your Comment  
      
    **When addressing a confusing block of code, it is good to highlight that this is just your own opinion, not a fact. Try to use “I” statements in such cases.  
      
    _A bad example:_ This line of code is very confusing.  
      
    _A good example:_ I'm confused about why this line of code was added.
- **Provide Reasoning  
      
    **When your team member reads your comment, they may not understand it right away. To avoid confusion, try to be specific and provide a rationale to support your comment.  
      
    _A bad example:_ This code will fail because it's not connected to the back end.  
      
    _A good example:_ I think this code might fail because we're not calling a back-end service method.
- **Avoid Blaming  
      
    **Code review is designed to improve code quality. As in any improvement process, it is always good to promote a culture of discussion among team members. Questions are the key to initiating a conversation between the reviewer and the author.  
      
    _A bad example:_ You are not closing the connection after reading data from the server.  
      
    _A good example:_ Could you please make sure we are closing the connection after data is successfully read from the server?
- **Avoid Judgments  
      
    **Remember that phrases like "It's obvious from the code …," "Why didn't you check…," and "This is a trivial task …" may sound offensive to the author. Code review should be based on mutual respect, so avoid such phrases when commenting on code.  
      
    _A bad example:_ Obviously, function X duplicates the functionality of function Y.  
      
    _A good example:_ It looks like function X provides the same functionality as function Y. Could you please check it?