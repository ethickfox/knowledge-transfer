---
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Planned
Topic:
  - "[Clean Code](Computer%20Science/Development/Clean%20Code%5C%5C)"
---
- In a **centralized version control system (CVCS)**, everything is stored in one consolidated repository. Each developer still has their own working copies to edit, but they must upload their edited code to a single repository before other team members can access and use it.
- **Distributed version control system (DVCS)** provides developers on your team with a copy of the entire code, as well as an additional level of repositories, which act as personal repositories for each individual developer.  
    With a DVCS, developers on your team can make edits to their working copies and commit those changes to their own repository before the code is pushed to the primary repository, which the whole team is able to access. Each developer’s workstation also has access to a copy of the entire code and change history.  
      
    **Git** is a free and open source DVCS that works well for teams and projects due to the system's flexibility.

|   |   |   |
|---|---|---|
||**CVCS**|**DVCS**|
|**Focus**|Focuses on synchronizing and backing up files|Focuses on tracking and sharing changes|
|**Learning**|Relatively easy to learn and set up, fewer operations to remember|Requires efforts to understand, practice, and learn operations|
|**Transparency**|Incremental revision numbers are handy to understand and reference|Not always obvious to locate the most recent change across multiple branches|
|**Configurability**|Easier to administer and control the workflow, but all possible branching strategies are rigid and flat|Management is flexible and extensive yet sophisticated, allowing configuration of branching strategies and policies of any kind|
|**Sandboxing**|Local branches are unsupported, unfinished/in-progress work can always be seen|Local branches can be used as sandboxes for experiments, reverts, and integration|
|**Working Offline**|Connection is necessary for uploading changes, updating working copies, and any other work except for local edits to a single downloaded revision|Offline work is possible, including creating local branches, reverting revisions, merging edits, etc.; connection is required only for interaction with the remote server|
|**Main Server**|If the main server crashes or goes down, developers can't save versioned changes; the entire change history could be lost|If the main server crashes or goes down, all repositories and their change history can be restored from the developer's local space|

**Branching**

Branching is a practice of making a duplicate copy of code from the mainline codebase. Developers use a duplicate copy (branch) to make edits to the codebase. Branching involves several aspects, including creating and merging branches.

- **Mainline  
      
    **A **mainline,** also called “main” or “master,” depending on your team’s version control system—is the primary codebase developers pull from to create their working copies.  
    Creating copies from the mainline branch is crucial because several developers may be working on the same fragments of code at the same time. Preserving the mainline by branching ensures that everyone can work on their tasks without interrupting the stable mainline codebase.  
    
- **Branch  
    Branches  
    ** are isolated copies of the mainline codebase that developers create to make edits. Branches protect the mainline from disruption or mistakes, allowing the code to function normally while undergoing updates. They also allow other developers to work on the code concurrently without interfering with each other.  
    Developers keep the mainline healthy by ensuring that any branches are free of errors before integrating the edits to the mainline codebase. Uploading a branch with errors to the mainline compromises the work of other developers on the project, so developers must complete extensive quality control via the execution of defined test cases that proves that the new or updated code works correctly before integrating any branch. Once quality control is completed, developers can integrate the branch through   
    **merging.  
      
    **A branch that is not intended to be merged is usually called a **fork**.

**Merge Conflicts**

Merge conflicts happen. Developers on your team might make different edits on top of each other, or one developer might delete code while another is actively editing it. Merge conflicts are often relatively simple and easy to fix, but they can cost time, money, and—if they happen on a larger scale—major project delays.

**Branching Patterns**

**Long-Lived Branching Pattern**

**Long-lived branching** is a good option for a longer-term, deeply complex project with multiple distributed development teams. A long-lived branch is a copy—or branch—of the mainline code that is used to work on a feature. A long-lived branching pattern works best for a feature isolation development approach.

Because the work happens over an extended period and includes collaboration between multiple developers, there are increased chances for workflow interruptions and errors to the mainline. Creating the feature branch allows your team to work continuously without risking the mainline codebase. A feature branch also stores the complete development history for reference—the mainline code does not need to store the history of the feature.

**Short-Lived Branching Pattern**

This pattern offers a different working approach for developers through **short-lived branches**. Unlike long-lived branching, which can take weeks of development, short-lived branching requires developers to integrate feature updates in days. This style of branching works best for a continuous integration development approach.

Throughout the process, short-lived branches automatically incorporate the codebase with the developer’s feature updates. This automation supports DevOps practices—alignment of development, testing, and deployment practices—by reducing or eliminating the need for alignment between an individual developer, development team, and operations team.

With short-lived branching, a developer can integrate feature updates before they are finalized. This might feel counterintuitive to developers who prefer rolling out whole, finalized functionalities. But, since code integration bureaucracy, communication overhead, and a lack of timely feedback usually disable development transparency and increase time to production, your team members should strive to become comfortable with integrating partially ready features even before they are completed—in small chunks, without impacting the stable mainline codebase.

Before committing to a branching strategy, your team must be aware of the benefits and drawbacks of short-lived and long-lived branches.

|   |   |   |
|---|---|---|
|**Question**|**Long-lived Branching:**|**Short-lived Branching:**|
|How is quality assurance handled?|A feature should be assessed for quality as a unit; thus, a “frozen” mainline branch is often needed for testing.|Requires a commit to healthy branches; encourages automated regression and unit testing with quick feedback.|
|How does the branching pattern maintain clean code?|Forces regular pre-integration code reviews and valuable feedback.|Cultivates continuous refactoring but separating mature and immature code can be tedious.|
|How does the branching pattern impact development performance?|Impaired collaboration, backward integrations, large pull requests, and merge conflicts may result in reduced speed.|Performance could constantly be accelerating with proper automation and less integration friction.|
|What are the integration triggers, and what is the integration frequency?|Only when a feature is complete; low frequency (once per few days or weeks).|A worthwhile number of changes; high frequency (at least once a day) enables continuous delivery.|
|What is the approach to mainline integration?|Merges are larger; rollbacks are harder.|Merges are smaller; rollbacks are easier.|
|How much effort does it take to resolve merge conflicts?|Merge conflicts are unavoidable and can be demanding.|Minimal or no time to resolve merge conflicts.|

**Strategies for DVCS:**

**Gitflow**

**Gitflow** is a branching strategy for distributed version control systems. With Gitflow, which assigns specific roles to different branches, your team’s developers use individual feature branches, as well as branches for preparing and maintaining releases. Gitflow is suited for projects that have a scheduled release cycle.

Developers create a new branch to work on each feature, which other team members can access in a centralized repository. Once all changes to the feature are finalized and tested, they can be integrated with the mainline, usually through a pull request.

Gitflow takes advantage of, and is conceptually founded on, the long-lived feature branching pattern. In addition, Gitflow enforces making use of servicing branches, particularly to isolate releases and hotfixes.

[![](https://elearn.epam.com/assets/courseware/v1/e7733221ef8699a874ba909b7de08e7d/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_gitflow.svg)](https://elearn.epam.com/assets/courseware/v1/e7733221ef8699a874ba909b7de08e7d/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_gitflow.svg)

**Trunk-based Development**

**Trunk-based development** is another DVCS branching strategy that enables your team to use a shared branch—called a trunk—for collaborative projects.

With this strategy, the entire team works from the trunk, which is where all edits and updates are committed to. In some cases, developers can create short-lived branches and only integrate their finalized updates to the trunk.

Trunk-based development is an implementation of short-lived branches, which minimize merge conflicts. Trunk-based development cultivates the habit of working with small commits that are streamed directly into the trunk. Your team may find this strategy especially effective for pre-integration checks, such as code reviews and pull requests.

[![](https://elearn.epam.com/assets/courseware/v1/03e2f48e359d9338a95ebde9f81b03f5/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_trunk-based_development.svg)](https://elearn.epam.com/assets/courseware/v1/03e2f48e359d9338a95ebde9f81b03f5/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_trunk-based_development.svg)

**Github Flow**

Удобнее для среды постоянного развертывания, где нет понятия выпуск. Каждый раз при реализации новой функции они выпускают ее в продакшн. Для работы используются следующие концепции:

Все что в мастере - уже рабочее

После мерджа изменений в мастер, оно должно быть задеплоено

[![](https://lh6.googleusercontent.com/ZGzGzsqyj-dHHg4VxOfq8NUNyY2v-0EmeYUlpQnNnWxK37CMYeKQvmK2ZXarIqsiIHt0UeNaHNvk7rCjGS8dokrr2Ej3u5bwh6GkjltKcVapVMoGNr9OkD3o8slv8M-PnIx6ZUA8FHV6PZfqdzOYA8jTKBeYIsFa4rp0cGEBgfeCA4sjO9f0-m_GdrQi)](https://lh6.googleusercontent.com/ZGzGzsqyj-dHHg4VxOfq8NUNyY2v-0EmeYUlpQnNnWxK37CMYeKQvmK2ZXarIqsiIHt0UeNaHNvk7rCjGS8dokrr2Ej3u5bwh6GkjltKcVapVMoGNr9OkD3o8slv8M-PnIx6ZUA8FHV6PZfqdzOYA8jTKBeYIsFa4rp0cGEBgfeCA4sjO9f0-m_GdrQi)

### **Forking Workflow**

Fork the official repository to create a copy of it on the server. This new copy serves as their personal public repository—no other developers are allowed to push to it, but they can pull changes from it (we’ll see why this is important in a moment). When they're ready to publish a local commit, they push the commit to their own public repository—not the official one. Then, they file a pull request with the main repository, which lets the project maintainer know that an update is ready to be integrated. The pull request also serves as a convenient discussion thread if there are issues with the contributed code. The following is a step-by-step example of this workflow.

1. A developer 'forks' an 'official' server-side repository. This creates their own server-side copy.
2. The new server-side copy is cloned to their local system.
3. A Git remote path for the 'official' repository is added to the local clone.
4. A new local feature branch is created.
5. The developer makes changes on the new branch.
6. New commits are created for the changes.
7. The branch gets pushed to the developer's own server-side copy.
8. The developer opens a pull request from the new branch to the 'official' repository.
9. The pull request gets approved for merge and is merged into the original server-side repository

  

**Strategies for CVCS:**

**Feature Isolation**

[![](https://elearn.epam.com/assets/courseware/v1/16f3110b4d360e9617f1a5251549aa76/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_feature_isolation.svg)](https://elearn.epam.com/assets/courseware/v1/16f3110b4d360e9617f1a5251549aa76/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_feature_isolation.svg)

**Feature isolation** is a branching strategy that allows your team’s developers to work on individual feature branches by creating a separate branch for each. With this strategy, developers can isolate feature development without disrupting the rest of the code. Once an update is completed, it can be integrated backward into the main.

It is worth noting that following this strategy carries the same advantages and risks of long-lived branching. Creating feature branches that require rollbacks from the mainline can end up impacting services and increase costs, so your team should carefully consider whether feature isolation is the right strategy.

**Servicing & Release Isolation**

[![](https://elearn.epam.com/assets/courseware/v1/5cca85633324b9f9a4bd580221b7a8a8/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_servicing___release_isolation.svg)](https://elearn.epam.com/assets/courseware/v1/5cca85633324b9f9a4bd580221b7a8a8/asset-v1:EPAM+EngX_B+0921+type@asset+block/Branching_Strategy_servicing___release_isolation.svg)

**Servicing and release isolation** is an appropriate branching strategy if your team is creating software with servicing packages and specific release parameters.

Introducing servicing branches allows customers to access and upgrade to each new release. This option ensures that updates to future releases can be made.

The release isolation branch is intended to be a finalized, locked software—no future changes to the code will be made (except for critical hot-fixes).

Similar to trunk-based development in the DVCS world, this branching strategy implies that the team chose a branching pattern for continuous integration.