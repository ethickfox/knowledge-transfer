---
Interview graded: true
Last edited time: 2023-11-25T18:36
Last recall: 2023-11-25
Needs Rework: false
Status: Planned
Topic:
  - "[[Knowledge Transfer/Testing/Testing Basics\\|Testing Basics]]"
---
Activities focused on providing confidence that quality requirements will be fulfilled

**Metrics**

- **Functionality  
      
    **This attribute determines how well the application is working according to its specifications.
- **Availability  
      
    **This attribute records the amount of time the application software is available to users, fully functional.
- **Performance  
      
    **This attribute indicates how quickly and accurately the application completes required functions.
- **Testability  
      
    **This attribute determines how easily and successfully developers and quality assurance specialists can perform routine tests on the codebase.
- **Security  
      
    **This attribute applies to the security features and requirements necessary to protect the product code—as well as company and user data—from possible breaches or catastrophic losses.
- **Usability  
      
    **This attribute assesses how easy user interfaces are to use.
- **Reliability  
      
    **This attribute determines a system’s capacity to continue operating under specified conditions for a certain amount of time.

**Components of Quality**

- **Quality Assurance  
    Quality assurance  
    ** is a process that monitors each phase of a software product’s life cycle, from development to delivery. The goal of quality assurance is to ensure the product works according to client specifications. QA focuses on the processes related to planning and maintaining the requirements of a software application, client specifications, and user experience.
- **Quality Control  
      
    **Quality control is part of quality assurance. While quality assurance focuses on the holistic success of the development process, **quality control** ensures the quality of the software application itself. The purpose of quality control is to resolve any problems your team identifies through testing or other means.
- **Testing  
      
    **Software **testing** is an important aspect of quality control. The purpose of testing is to ensure that the product is functioning according to technical specifications and requirements, such as functionality or client security standards, and is free of defects.

|   |   |
|---|---|
|**Manual Testing**|**Automated Testing**|
|Each developer and test engineer on your team must be able to perform basic levels of manual testing during the development process.  <br>  <br>This testing type requires the developer or test engineer to create test environments to detect any bugs or errors before a release. This process is done without the use of any tools or automation, so expertise is needed.  <br>  <br>Disadvantages of manual testing include its tendencies to be difficult and time-consuming, repetitive and boring, and less accurate than automated testing.  <br>  <br>The benefits of manual testing include lower costs in the short term, flexibility, execution randomness, and adaptability. Manual testing can be replaced by automated testing, but only partially, because manual testing allows teams to identify defects that automation tools may miss.|This testing uses a specific set of automated tools to complete testing tasks. Developers or specialists on your team may choose automated testing for repetitive tests or for large-scale tests that would be difficult for manual testers.  <br>  <br>Automation has limitations and cannot (or should not) be applied universally to every aspect of the testing process. Automated testing can be an expensive investment for any organization, but the benefits of automation can outweigh the costs.  <br>  <br>Disadvantages of automated testing include its heavy reliance on tools, as well as the costs and limitations of those tools. Automated testing requires expertise and is not suitable for every testing type.  <br>  <br>Benefits of automated testing include long-term cost savings and the ability to execute tests in parallel. Automated testing is repeatable, quick, and creative.|

|   |   |
|---|---|
|**Functional Testing**|**Non-Functional Testing**|
|Every application has a set of functional requirements your team’s code must meet to determine if the application meets the client’s requirements.  <br>  <br>Functional testing focuses only on whether or not the system is working as intended. Functional testing can be both automated and manual.|A **non-functional requirement** describes operational qualities rather than behavioral qualities—not _what_ the system will do but _how_ the system will do it. **Non-functional testing** verifies how well the system meets these requirements by measuring its performance, usability, reliability, etc. For example, a non-functional test would check how many people could simultaneously place an order in an e-shop.  <br>  <br>Non-functional testing works best when using automation.|

**Testing Methods**

- **White Box Testing  
      
    **Testing based on an analysis of the internal structure of the component or system.  
    Also known as structural testing,   
    **white box testing** is unit-level testing that ensures that the software application’s internal performance aligns with the product’s specifications. Test engineers utilize white box testing to verify the application’s data domains and internal boundaries.
- **Black Box Testing  
    Black box testing** is used to test the functionality of the application. This testing is focused only on the interface and does not test the internal structure of the software.
- **Grey Box Testing  
    Grey box testing  
    ** is a hybrid of white box and black box testing that requires engineers to have access to the database and design documentation. Grey box testing provides a more user experience–centered test.

  

**Issues**

- **Errors  
      
    **Errors are mistakes made by a developer while writing the software code, including any possible misunderstandings of the requirements or intended functionality.
- **Failures  
      
    **A failure means that the application code is not functioning or is unable to function as the developers or clients intended. In this case, the developer may not have made a mistake, but the code must be reconfigured to meet the requirements of the product.
- **Defects  
      
    **A defect, also known as a bug or fault, happens when the code does not produce an expected result. A defect could result in an interruption of the user experience.

  

**Software Testing Life Cycle (STLC)**

- **Requirement Analysis  
      
    **Before testing can begin, your team must determine which requirements of the software application must be tested. This step is vital because testing without planning can yield useless or overwhelming amounts of data. Involve clients in this discussion to be sure you understand their expectations as well.
- **Test Planning  
      
    **During this stage, the QA team must establish the testing plan, including priorities, goals, strategy, and resources.
- **Test Case Design/Development  
      
    **The QA team must design each test case or test script before testing begins. You should carefully refer to the results from the Requirement Analysis to ensure the test cases align with the requirements of the testing process and cover all requirements comprehensively.
- **Test Environment Setup  
      
    **Either test engineers or the developers on your team must create a testing environment that simulates an actual user experience.
- **Test Execution  
      
    **At this stage, test engineers run through each step of the established test plan and identify any errors, defects, or failures.
- **Test Closure  
      
    **Once the test execution is completed, the test is closed, and any errors, defects, or failures are reported. The cycle can then start again.

**Testing Documentation**

- **Test Plan  
      
    **Part of your team’s testing documentation is a **test plan**: a complete planning document containing the scope, approach, schedule, and resources of the testing activities.  
    Test plans should include:  
    
    - A project-specific impact on testing
    - The scope of testing
    - Quality and acceptance criteria
    - Risk management
    - The test team, test schedule, and test deliverables
    
    Taking the time to document your test plan properly will ensure that your team completes testing successfully.
    
    While the test plan focuses on the “what” and the “when” of testing, the test strategy addresses the “how.”
    
- **Test Strategy  
      
    **Another crucial element of testing documentation is creating and adhering to a **test strategy**, which describes how the target-of-test will be tested.  
    The test strategy can include:  
    - How the team will organize testing
    - Which testing types will be in scope to address product risks
    - Which test design and execution approaches the team will use for a particular testing type

Your test strategy may cover a general introduction, the test strategy, the test strategy outline, and testing types. Ensure your team takes the time to include a test strategy as part of your testing documentation.