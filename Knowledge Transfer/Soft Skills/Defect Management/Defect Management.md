---
Interview graded: true
Last edited time: 2023-11-25T18:47
Last recall: 2023-11-25
Needs Rework: false
Status: Planned
Topic:
  - "[[Knowledge Transfer/Testing/Testing Basics\\|Testing Basics]]"
---
**Basics of Defect Management**

[[D3 Interview]]

**Error**

When a _developer_ causes a problem or makes a mistake, it’s an error.

Errors are mistakes made by a developer while writing code, including any possible misunderstandings of the intended functionality or requirements of the request.

  

**Defect**

When the _software_ is experiencing a gap between expected and actual results, it’s a defect.

A defect, also known as a bug or fault, happens when the code does not produce an expected result in alignment with product specifications or requirements.

|   |   |
|---|---|
|**Reproducible**|**Irreproducible**|
|Defects are reproducible when the test team or development team can follow the steps identified in the bug report or submission ticket, and successfully recreate the defect in the various test environments. This helps confirm if the problem is, in fact, a defect and allows the tester to elevate the defect for resolution.|Defects are irreproducible when the test engineer who creates the bug report misses some preconditions or cannot see how the described actual behavior follows the steps established by the team for reproducing defects. A defect may also be irreproducible when the defect has an unstable behavior.  <br>  <br>When this happens, the team cannot successfully reproduce the defect and cannot confirm if the problem is a defect or some other type of error.|

  

**Failure**

When the _system_ has a problem, it’s a failure.

A failure means that the application is not functioning or is unable to function as developers or clients intended.

**The Defect Management Process**

- **Discovery  
      
    **During the discovery phase, the Quality Assurance (QA) team works on discovering as many defects in the product as possible. Defects are discovered during testing activities like manual and automated test case execution, exploratory testing, and production usage. Manual and automated testing steps should already be defined before the discovery phase, while exploratory/production testers will need to define the steps to reproduce bugs before submitting.
- **Triage  
      
    **Defect triage helps software developers prioritize their tasks. Setting a bug’s priority enables developers to fix critical defects first. The team lead assigns the defect to a developer who works with the QA engineer to resolve it.  
      
    **Note:** Some bugs are closed during triage and do not advance throughout the rest of the defect management process for various reasons. If, for example, a defect is discovered to be not a bug but a change request, a change request will be opened and the defect closed. Additionally, any duplicate defects found during triage can be closed.
- **Resolution  
      
    **During the Resolution phase, the development team fixes defects in order of priority and then sends a resolution report to the software project managers and testing teams.

**The Defect Life Cycle**

![[/Untitled 46.png|Untitled 46.png]]

**Defect Discovery**

All defects can be categorized according to two different attributes: **priority**  
 and  
 **severity**.

- **Priority  
      
    **Priority, or the order in which bugs should be addressed, typically consists of four categories, including:
    - **Low:** The defect is a burden, but it can be fixed once a more severe defect has been addressed.
    - **Medium:** The defect should be fixed over the course of normal software development activities as it does not hinder application functionality. If the stakeholders approve, such a defect can be fixed before the next release or later.
    - **High:** The defect must be fixed as soon as possible as it significantly hinders system functionality. Such a defect could severely damage a customer's reputation—for example, typos in a logo.
    - **Critical:** A defect shuts down the entire system or functionality, and further testing becomes impossible until the defect is fixed.
- **Severity  
      
    **Severity refers to the amount of damage a defect is inflicting on the system or user experience. It can also imply the level of effort required to mitigate the bug. The categories of severity may vary in name or number, but they usually include:
    - **Trivial:** The defect does not cause any system malfunctions.
    - **Minor (Moderate):** The defect causes certain undesired system behaviors, but the system still functions.
    - **Major:** The defect is significant and causes the system to collapse. However, certain elements of the system still function.
    - **Critical:** The defect causes the system to shut down completely.
    - **Blocker:** The defect is so severe that it causes the system to shut down completely and prevents any further testing activities.

Priority and severity can sometimes conflict. While a defect may not seem severe, it can take priority because it is blocking a critical process or causing a snowball effect somewhere in the software. The test engineer who submits the defect will issue an initial classification, but it can be changed later.

  

**Defect Triage**

**Defect triage** is the stage of the defect management process when the team:

- Reviews and assesses defects.
- Prioritizes and, if needed, reprioritizes defects based on certain criteria (severity, frequency, risk, etc.).
- Assigns resources for defects resolution.

**During defect triage meetings:**

- The testing team leader shares a defect report.
- The team reviews each defect to see whether the right priority and severity are assigned to it.
- The team rearranges priorities as needed.
- A member of the team posts updates in the defect tracking system.
- The testing engineer makes the changes to each defect and discusses them with each attendee.
- A member of the team updates the “Comments” field of the defect report with essential points from the meeting.

**Using Root Cause Analysis to Resolve Defects**

The five steps of root cause analysis include:

1. Forming a team to conduct root cause analysis
2. Defining the problem
3. Determining the root cause of the defect
4. Taking action to address the root cause of the defect
5. Preventing continued occurrences of the root cause of the defect

  

**Root Causes**

- **Human-Introduced Defects  
      
    **Defects can be caused by people, resulting from:
    - A low level of skills
    - Improperly followed instructions
    - An unnecessary execution of an operation
    - Intentional damage
    - User error (a user incorrectly performing a sequence of actions or performing unnecessary operations)
- **Organizational Confusion  
      
    **Defects can result from improper decisions by management teams, such as:
    - Vague instructions that team leaders give to team members
    - Poor task assignments
    - Absence of monitoring tools for quality assessment
    - Poorly constructed testing environments
- **Mechanical Problems  
      
    **Defects can result from the failure of any physical device or system, such as:
    - Computer shutdown (e.g., if a computer shuts down while you’re uploading, turning the computer back on and launching this application can cause a crash)
    - Server turn-off (if the client-server application is down on the client side, you won’t be able to get data)
    - System failure