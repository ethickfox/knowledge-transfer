---
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Planned
Topic:
  - "[Testing Basics](Knowledge%20Transfer/Testing/Testing%20Basics%5C%5C)"
---
**Types of QA Metrics**

- **Result Metrics  
      
    **Result metrics provide an “absolute” measure surrounding something that has already been completed.  
    These metrics are a good gauge of how thorough and efficient your testing process is, but they aren’t usually enough to provide actionable insights for improvement.  
      
    **Common Result Metrics**
    
    |   |   |   |
    |---|---|---|
    |◦ Total number of test cases  <br>◦  <br>Number of test cases passed  <br>◦  <br>Number of test cases failed  <br>◦  <br>Number of test cases blocked|◦ Number of defects found  <br>◦  <br>Number of defects accepted  <br>◦  <br>Number of defects rejected  <br>◦  <br>Number of defects deferred|◦ Number of critical defects  <br>◦  <br>Number of planned test hours  <br>◦  <br>Number of actual test hours  <br>◦  <br>Number of bugs found after shipping|
    
- **Predictive Metrics**
    
    Predictive metrics use and combine absolute metrics to discover early warning signs of things that may hinder the speed or quality of development.
    
    **Common Predictive Metrics**
    
    - **Passed test cases percentage** = (Number of passed tests/Number of tests executed) x 100
    - **Failed test cases percentage** = (Number of failed tests/Number of tests executed) x 100
    - **Fixed defects percentage** = (Number of defects fixed/Number of defects reported) x 100
    - **Test design efficiency** = Number of tests designed/Total time
    - **Defect containment efficiency** = (Number of defects found during QA/Number of defects) x 100
    - **Defect leakage** = (Number of defects reported by users/Number of defects) x 100
    - **Defect reopen ratio** = (Number of reopened defects/Number of defects reported) x 100
    - **Defects rejection rate** = (Number of invalid defects/Number of defects reported) x 100

  

**QA Metrics Life Cycle**

- **Analyze  
      
    **The Analyze phase is about identifying and defining the right metrics for the team and project. You won’t have time to measure everything, so you must choose the appropriate metrics based on the specific questions your team wants to answer.  
    For example, the question   
    **“What is the quality of the testing on the project?”** calls upon the following metrics:
    
    - Defect containment
    - Defects rejection rate
    - Defect reopen ratio
    - Critical defect ratio
    - Total working time by categories
    
    Planning in advance which metrics are a priority can help your team save time by collecting only the metrics that are meaningful to the success of the project.
    
- **Communicate  
      
    **In this phase, you communicate the need and intention of the identified metrics to your stakeholders and project team. This is also the time for your team to define what data needs to be in place to ensure meaningful metrics.  
    For example, once the team decides to calculate the “defects by priority and status” metric, you should check that each defect is properly submitted with priority and status attributes and you should mark rejected defects to differentiate them from good defects.  
    
- **Evaluate  
      
    **During the evaluation phase, your team collects the data needed for the metrics—ideally via automated processes. Once that data has been validated, the metrics are calculated.
- **Report and Analyze  
      
    **After your team collects the data and calculates the metrics, you create and distribute reports with the final recommendation to the project stakeholders.  
    Remember that metrics aren’t collected to simply populate reports. Each measurement will have certain target goals associated with it. Reporting and analyzing metrics helps your team see where your metrics stand in relation to the specified targets and create an action plan for improvement as needed.  
    

**Report and Analyze Phase**

|   |   |   |
|---|---|---|
|**Metric**|**Measurement**|**Target**|
|Defect Containment Efficiency|Determines how effective quality testing and defects detection are at keeping defects contained|>95%|
|Number of Open Defects|Measures product quality and quality debt|Trend not growing|
|Test Coverage|Determines completeness of test suite and how many requirements are covered by test cases|100%|
|Defects Density|Determines development quality|Trend not growing|
|Invalid Defect Ratio|Determines waste due to invalid defects|<10%|
|Defect Reopen Ratio|Determines waste due to reopened defects|<10%|
|Defects Age (by Priority)|Measures project quality and the health of project processes|Top Priority: <3 dAll Priorities: <30 d|
|Cost of Poor Quality|Measures effort spent on fixing bugs|<15%|

- **Plan  
      
    **This is the goal-setting stage. Your team will pick new processes, practices, and activities that must be introduced or adjusted in the development and QA processes.
- **Do  
      
    **At this stage, you and your team take action and follow the new practices or processes.
- **Check**  
    This is when you check in with the project and ensure that the new practices are actually working and improving the overall process and increasing efficiency. Your team will use metrics as the main tool for checking the “before and after” of implementation.  
    Your development team should also review the goals established in the Plan phase to ensure alignment.  
    
- **Act**  
    During this step, you review flaws or obstacles and create a plan to address them. The goal of this stage is to ensure the process is continually improved until the product is ready to be released.