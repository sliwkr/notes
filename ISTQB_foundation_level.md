# Chapter 0 - Introduction
* K1 : Remember
* K2 : Understand
* K3 : Apply
* K4 : Analyze
* COTS: Commercial-Off-The-Shelf
# Chapter 1 - Fundaments of testing
* Why is testing necessary K1
	* Defects occur because humans are fallible
* How much testing is enough?
	* Levels of risk should be taken into account
	* Enough to provide realistic situation to stakeholders
* Objectives of testing
	* Finding defects
	* Gaining confidence about the product
	* Providing information for decision making
	* Preventing defects
* Principles of testing
	* Show presence of defects, not prove that there are none
	* Exhaustive testing is impossible, aside from trivial cases
	* Test as early as possible
	* Defects cluster, one should tests based on the assumed/observed defect density in modules
	* The same suite of tests executed over time will eventually find no defects; one should expand the scope of cases
	* Testing is contex-dependent
	* Fixing defects does not help if the system is buildt incorrectly or is unusable
* Fundamental test process
	* Test planning and Control
		* Planning - defining objectives and specification of test activities in order to meet the mission
		* Control - ongoing activity of comparing actual process against the plan
	* Test analysis and design
		* Reviewing the test basis, risk analysis reports, architecture, design, interface specifications
		* Evalutaing testability of the test basis and test objects
		* Identifying and prioritizing test conditions based on analysis of test items, the specifications and structure
		* Designing and prioritizing high level test cases
		* Identifying the test environment setup and required infrastructure
		* Creating bi-directional traceability between test basis and test cases
	* Test implementation and execution
		* Finalizing, implementing and prioritizing test cases
		* Developing and prioritizing test procedures
		* Creating test suites from the test procedures
		* Verifying that the suites has been setup correctly
		* Verifying and updating bi-directional traceability between test basis and test cases
		* Executing test procedures
		* Logging the test outcome
		* Comparing actual results with expected results
		* Reporting discrepancies as incidents and analyzing them in order to establish their cause
		* Repeating test activities as a result of action taken for each discrepancy
	* Evaluating exit criteria and reporting
		* Checking logs against the exit criteria specified in test planning
		* Assessing if more tests are needed or the criteria should be changed
		* Writing a test summary report for stakeholders
	* Test closure activities
		* Checking which planned deliverables have been delivered
		* Closing incident reports or raising change records for any that remain open
		* Documenting the acceptance of the system
		* Finalizing and archiving testware, the test environment and the infractructure for the later use
		* Handling over the testare to the maintenance organization
		* Analyzing lessons learned to determine changes needed for future releases and projects
		* Using the information gathered to improve tests maturity
* The psychology of testing
	* Proper mindset helps to avoid the author bias
	* People and projects are driven by objectives, therefore it's important to state those for testing
	* Curiosity, professional pessimism, critical eye, attention to detail and good communication are useful tools
	* Bad feelings can be avoided by constructive communication
	* Start with collaboration rather than battles - remind everyone of the common goal of better quality systems
	* Communicate findings in a neutral, fact-focused way without criticizing person who created it
	* Try to understand how other person feels and why they react as they do
	* Confirm that the other person has understood what you have said and vice versa
* Code of ethics
	* PUBLIC - act consistently with the public interest
	* CLIENT AND EMPLOYER - act in a manner that is in the best interest of their client and employer
	* PRODUCT - ensure that the deliverables meet the highest standards possible
	* JUDGMENT - maintain integrity and independence in professional judgment
	* MANAGMENT - subscribe to and promote an ethical approach to testing software
	* PROFESSION - advance the integrity and reputation of the profession consistent with the public interest
	* COLLEAGUES - be fair and supportive to colleagues, promote cooperation with developers
	* SELF - participate in the lifelong learning regarding the testing practice and promote ethical approach to it
# Chapter 2 - Testing throughout the software life cycle
* Software development models
	* V-model
		* Component(unit) testing
		* Integration testing
		* System testing
		* Acceptance testing
		* Verification/validation carried out during development of the software
		* One can join testing types, ex. component integration testing done after component testing
	* Iterative-incremental development model - short cycle of the following activities:
		* Establish requirements
		* Design
		* Build
		* Test
	* Testing within a Life Cycle model
		* For every development activity there's a corresponding testing activity
		* Each test level has it's specific test objectives
		* Testers should be involved in reviewing documents as soon as drafts are available in the development cycle
* Test levels
	* For each level there are:
		* General objectives
		* Test basis (required information, requirements, etc)
		* The test object
		* Typical defects/failures
		* Test tools
		* Specific approaches
	* Component testing (unit testing)
		* Basis:
			* Component requirements
			* Detailed design
			* Code
		* Typical test objects:
			* Components
			* Programs
			* Modules
		* Scope: separately testable modules, objects, classes, etc
		* Defects fixed as soon as found
		* Usually involves programmer who wrote the code
	* Integration testing
		* Basis:
			* Software and system design
			* Architecture
			* Workflows
			* Use cases
		* Typical test objects:
			* Subsystems
			* Database implementation
			* Infrastructure
			* Interfaces
			* System configuration and configuration data
		* Scope: Interfaces between components, interactions w/ different parts of the system
		* Is divided into component and system integration testing
		* The greater the scope of integration, the more difficult to isolate defects to specific component
		* Testers should understand architecture and influence integration planning
	* System testing
		* Basis:
			* Subsystem and software requirement specification
			* Use cases
			* Functional specification
			* Risk analysis reports
		* Typical test objects:
			* System, user and operation manuals
			* System configuration and configuration data
		* Scope: behavior of a whole system/product
		* Test environment should correspond to production environment to minimize environment-specific failures
		* Based on specification, business procedures, use cases, hlvl descriptions, integrations w/ OS, system resources
		* Should cover functional & non-functional requirements & data quality characteristics
		* Starts w/ specification-based techniques (black box), may be completed w/ structure-based techniques (white box) 
		* Often performed by independent test team
	* Acceptance testing
		* Basis:
			* User & system requirements
			* Use cases
			* Business procedures
			* Risk analysis reports
		* Typical test objects:
			* Business procedures on a fully integrated system
			* Operational and maintenance processes
			* User procedures
			* Forms, reports
			* Configuration data
		* Scope: varies, depending on the time in the life cycle it is being used
		* Goal: establish confidence in the system
		* Finding defects is not the main goal
		* Often performed by users
		* May come in various times of the life cycle, not always at the end
		* Is divided into:
			* User acceptance testing - verifies the fitness for use by users
			* Operational(acceptance) testing - backup/restore, disaster recovery, user management, security
			* Contract and regulation acceptance testing - performed to any reulations software must adhere to
			* Alpha and beta testing - feedback, alpha performed inside company; beta, by customers
* Test types
	* Test type is focused on a particular test objective
	* Testing of function (functional testing) (black box)
		* Based on functions and features
		* Considers the external behavior of the software
		* Security test is a type of a functional test related to detection of threats
		* Interoperability testing evaluates the capability of the software to work with other components or systems
	* Testing of non-functional software characteristics (non-functional testing) (black box)
		* Includes but not limited to performance, load, stress, usability, maintainability, reliability, portability testing
		* May be performed at all levels
		* Measures characteristics of systems and software that can be quantified on some scale (ex. response times)
		* Considers the external behavior of the software
	* Testing of software structure/architecture (structural testing) (white box)
		* May be performed at all test levels
		* Best used after specification-based techniques
		* Aims to measure the thoroughness of testing through assessment of coverage of a type of structure
		* Tools are used to measure coverage of elements
		* May be based on the architecture of a system, such as calling hierarchy
		* Structural approach can be applied at system, system integration or acceptance testing levels
	* Testing related to changes: Re-testing and regression testing
		* May be performed at all levels
		* Performed after a defect is fixed to confirm the fixed state and make sure no new defects are introduced
		* The extend of regression testing is based on the risk of not finding defects in software that was working
		* Tests should be repeatable if they're used for confirmation testing and to assist regression testing
		* Good candidate for automation: they evolve slowly and are being run often
	* Maintenance testing
		* Done on an existing system, triggered by modifications
		* May include testing of data migration, regression testing to unchanged parts
		* Scope related to the size of change, risk of change, size of the system
		* May be done on any test level and for any test type
		* May be difficult to perform if specifications are out of date or testers with sufficient knowledge are missing
# Chapter 3 - Static Techniques
* Preface
	* Manual examination (code reviews) and automated analysis (static analysis) can be performed without executing code
	* Any software work product can be reviewed
	* Benefits of reviews:
		* Early detection & correction
		* Improvement of development productivity
		* Reduced development timescales
		* Reduced testing cost & time
		* Improved communication
		* Ability to find omissions, deviations from standards, design defects, incorrect specifications
	* Static analysis rather finds potential/actual cause of defects than defects themselves
* Typical review
	* Planning - Define criteria, find people, allocate roles, define entry & exit criteria, select subject, check entry criteria
	* Kick-off - Distribute documents, explain the objectives & process to participants
	* Individual preparation - review documents, note potential defects and questions
	* Examination/evaluation/recording of results - discuss, note defects, make recommendations, make decisions
	* Rework - Fix found defects, record updated defect status
	* Follow-up - Gather metrics, make sure defects have been adressed, check exit criteria
* Roles and responsibilities
	* Manager - decides on execution of reviews, allocates time for it, determines if objectives have been met
	* Moderator - leads review process(planning, running meeting & follow-up), mediator of different points of view
	* Author - writer or person with chief responsibility for the reviewed code
	* Reviewers - have appropriate technical/business background, identify & describe defects, they should take part in review
	* Scribe - documents issues, problems & open points that were identified during the meeting
* Review types
	* Informal
		* May take the form of pair programming
		* Results may be documented
		* Purpose: inexpensive way to get some benefit
	* Walkthrough
		* Meeting led by author
		* May take the form of scenarios, dry runs, peer group participation
		* Open-ended sessions
		* Optional scribe (not author)
		* May vary in formality
		* Purpose: learning, gaining understanding, finding defects
	* Technical review
		* Documented & defined defect-detection process which involves peers & tech experts
		* May be performed as a peer review
		* Ideally led by trained moderator, not the author
		* Required pre-meeting reviewer preparation
		* Optional checklists
		* Report containing list of findings, verdict whether software meets requirements, reccomendations related to findings
		* Purpose: discuss, make decisions, evaluate alternatives, solve tech problems, check conformance to specifications
	* Inspection
		* Led by moderator (not the author)
		* Can be performed within peer group (peer review)
		* Conducted as peer examination
		* Defined roles
		* Gathering metrics
		* Formal process based on rules and checklists
		* Specified entry & exit criteria for acceptance of software product
		* Required pre-meeting preparation
		* Report containing list of findings
		* Formal follow-up process
		* Purpose: find defects
* Success factors for reviewers
	* Review has clear, predefined objectives
	* Right people are involed
	* Testers are valued reviewers who contribute to review and learn about the product which enables em to prepate tests
	* Defects found are welcomed and expressed objectively
	* People/psychologial issues are dealth with (positive experience for the author)
	* Outcome of review will not be used to evaluate author (atmosphere of trust)
	* Review techniques are suited to the objective, product and reviewers
	* Checklists are used if appropriate to increase effectiveness
	* Training reviewers in more formal review techniques
	* Management supports the good review process
	* Emphasis on learning and process improvement
* Static analysis by tools
	* Purpose: find defects (obviously) that are hard to locate by dynamic (people) testing
	* Typicaly used by developers before and during component testing or when checking-in code
# Chapter 4 - Test design techniques
* Test development process
	* Test analysis - test basis documentation is analyzed to determine test conditions & establish traceability
	* Test design - test cases & data are created & speicified, expected results should be defined
	* Test implementation - test cases are developed, implemented, prioritized, organized & formed into a schedule
* Categories of test design techniques
	* The purpose of a test design technique is to identify test conditions, cases and test data
	* Black box(specification-based) techniques derive test cases based on analysis of documentation
	* White box(structure-based) techniques derive test cases after analysis of structure of the component or system
	* Experience-based techniques are based on the knowlegde of the product by the one who invents them
* Black-box techniques
	* Equivalence partitioning
		* Inputs to the software are divided into groups that exhibit similar behaviour to be processed in a similar way
		* Partitioning is based on valid/invalid data, response times, interface parameters 
	* Boundary value analysis
		* Behaviour at the edge of an equivalence partition is more likely to be invalid than within the partition
		* Maximum and minimum values of a partition are its boundary values
		* Relatively easy to apply, with high defect detection rate
	* Decision table design
		* Good way to capture requirements containing logical conditions (often boolean)
		* Analyze specification, derive expected conditions and actions of the system
		* Coverage standard is to have at least one test case per condidion in the table
		* Main strength: creates combindations of conditions that otherwise would not be very likely to check against
	* State transition testing
		* See software in terms of its states, transision between states and inputs that trigger state changes
		* System may exhibit different behaviour depending on current conditions or previous history(state)
		* The states of the system or object under test are separate, identifiable and finite
		* A state table shows the relationship between states and inputs, and can highlight possible transitions that are not valid
		* Approaches: test typical sequence of states, cover every state, exercise every transition, test invalid transitions
	* Use case testing
		* Use case describes interactions between actors(users or systems) which produce result of value
		* Each use case has preconditions which need to be met for the use case to work
		* Each use case terminates with postconditions(observable results)
		* Useful for designing acceptance tests, helps uncover integration defects caused by integration or interference
* White-box techniques
	* Based on and identified structure of the software
		* component level (code)
		* integration level (diagram of calls)
		* system level (business process)
	* Statement testing and coverage
		* Percentage of executable statements that have been exercised by the test suite
		* Test cases derived to execute specific code statements
		* Coverage determined by the number of executable statements
	* Decision testing and coverage
		* Percentage of decision outcomes that have been exercised by the test suite (ex. true/false IF statements)
		* Test cases derived to execute specific decision outcome
		* Branches originate from different decision points and show transfer of control to different locations in the code
		* 100% decision coverage guarantees 100% statement coverage, but not vice versa
* Experience-based techniques
	* Test cases derived from tester's skill, intuition & experience with similar applications or technologies
	* Yields varying degrees of effectiveness depending on tester's experience
	* Error guessing - enumerate list of possible defects, then design tests to attack these (fault attack)
	* Exploratory testing - concurrent test design, execution, logging and learning based on test objectives
		* Carried out in time boxes
		* Most useful when theres inadequate specifications or one's under time pressure, or to complement formal methods
* Choosing test techniques
	* Generally one should use a combination of methods. The choice should be based on the following factors:
		* Type of system
		* Regulatory stndards
		* Customer/contractual requirements
		* Level & type of risk
		* Test objective
		* Documentation available
		* Knowledge of testers
		* Time & budget
		* Development life cycle
		* Use case models
		* Previous experience with types of defects found
# Chapter 5 - Test Management
* Test Organization and independence
	* Test effeciveness can be improved by using independent testers
	* For large, complex or safety critical projects it's usually best to have multiple levels of testing
	* Lack of development staff objectiveness can limit effectiveness
	* Pros: independent testers are unbiased & can verify assumptions people made during specification/implementation
	* Cons: they're isolated from development team & may be seen as a bottleneck, developers may lose sense of responsibility 
* Tasks of the test leader and tester
	* Test leader tasks
		* Coordinate the strategy, plan with PM and others
		* Write or reviews test strategy for the project and test policy for the organization
		* Contribute the testing perspective to other project activities
		* Plan the tests
		* Initiate the specification, preparation, implementation and execution of tests, monitor results and checks exit criteria
		* Adapt planning based on test results and progress, take actions to correct issues
		* Set up adequate configuration management of testware for traceability
		* Introduce suitable metrics for measuring progress & evaluating quality
		* Decide what should be automated
		* Decide about implementation of test environment
		* Write test summary reports based on the information gathered during testing
	* Tester tasks
		* Review and contribute to test plans
		* Analyze, review and assess user requirements, specifications and models for testability
		* Create test specifications
		* Set up the test environment
		* Prepare and acquire test data
		* Implement tests at all levels, execute and log tests, evaluate results, document deviations
		* Use test administration or management tools and test monitoring tools
		* Automate tests
		* Measure performance of components
		* Review tests developed by others
* Test planning & estimation
	* Planning is a continous activity, performed in all life cycle processes and activites
	* Feedback from test activities is used to recognize chaning risks so planning can be adjusted
	* Test planning activities
		* Determine the scope and risk and identify objectives of testing
		* Define overall approach to testing, including definition of test levels
		* Integrating & coordinating testing activities into the software life cycle activities
		* Making decisions about
			* What to test
			* What roles will perform the test activites
			* How the test activities should be done
			* How the test results will be evaluated
		* Scheduling test analysis and design activities
		* Scheduling test implementation, execution and evaluation
		* Assigning resources for the different activities defined
		* Defining the amount, level of detail, structure and templates for the test documentation
		* Selecting metrics for monitoring and controlling test preparation, execution, defect resolution and risk issues
		* Setting the level of detail for test procedures in order to provide enough information to support tests
	* Entry criteria - decide when to start testing
		* Test environment availability and readiness
		* Test tool readiness in the test environment
		* Testable code availability
		* Test data availability
	* Exit criteria - decide when to stop testing
		* Throughouness measures, such as code coverage, functionality or risk
		* Estimates of defect density or reliability measures
		* Cost
		* Residual risks (ex. defects not fixed, lack of coverage)
		* Schedules such as those based on time to market
	* Test estimation
		* Based on metrics of former or similar projects, or based on typical values
		* Based on expert's or owner's opinion
		* Testing effort may vary depending on:
			* Characteristics of the product
			* Characteristics of the development process
			* The outcome of testing
	* Test strategy, test approach
		* Test approach is the implementation of test strategy, defined in test plans and designs
		* It is the starting point for planning test process, selecting techniques, defining entry & exit criteria
		* Typical approaches:
			* Analytical (ex. risk based testing)
			* Model-based (ex. testing using statistical information about failures or usage)
			* Methodical, such as failure based, experience based, checklist based, quality characteristic based
			* Process/standard compliant - specified by industry standards or various agile methodologies
			* Dynamic & heuristic (ex. exploratory testing) (testing more reactive to events than pre-planned)
			* Consultative (ex. test coverage driven by advice and guidance of technology experts outside of the test team)
			* Regression-averse (automation, standard test suites)
* Test progress monitoring & control
	* Monitoring
		* Provides feedback and visibility about test activities
		* Common metrics:
			* Percentage of test cases prepared
			* Percentage of environment prepared
			* Number of test cases running, passing/failing
			* Defect information (density, found/fixed, failure rate, re-test results)
			* Test coverage of requirements, risks or code
			* Subjective confidence of testers in the product
			* Dates of test milestones
			* Testing costs
	* Reporting
		* Summarize information about testing endeavour
			* What happened during a period of testing
			* Information and metrics to support recommendations and decisions about future actions
		* Information should be collected during and at the end of a test level in order to assess:
			* The adequacy of the test objectives for that test level
			* The adequacy of the test approached taken
			* The effectiveness of the testing with respect to the objectives
	* Test control
		* Describes any actions taken as a result of information and metrics reported
		* Re-prioritizing tests when an identified risk occurs
		* Chaning the test schedule due to availability or unavailability of a test environment
		* Setting an entry criterion requiring fixes to have been confimation-tested before accepting them into a build
* Configuration management 
	* Ensures that:
		* Integrity is maintained through the project & life cycle
		* All items of testware are identified, version controlled, tracked for changes, related to each other
		* All identified documents and software items are referenced unambiguosly in documentation
	* Helps testers uniquely identify & reproduce the tested items, documents and tests
	* During planning, configuration management procedures should be chosen, documented & implemented
* Risk and testing
	* Risk is a chance of an event occuring and resulting in undesirable consequences or a potential problem
	* Risks management provides approach to:
		* Assess (on a regular basis) what can go wrong
		* Determine what risks are the most important
		* Implement actions to deal with those risks
	* Projecct risks
		* Organizational factors
			* Skill/training/staff shortages
			* Personell issues
			* Political issues (communication, team failing to follow information found in testing & reviews, attitude)
		* Technical issues
			* Problems with defining right requirements
			* The extent to which requirements cannot be met given existing constraints
			* Test environment not ready on time
			* Late data conversion, migration planning, development and testing data conversion/migration tools
			* Low quality of design, code, configuration data, test data, tests
		* Supplier issues
			* Third party failure
			* Contractual issues
		* IEE Std 829-1998 "Standard for Software Test Documentation" should be followed when investigating
	* Product risks
		* Potential failure areas in the software or system
			* Failure-prone software delivered
			* The potential that the software could cause harm to an individual or company
			* Poor software characteristics (functionality, reliability, usability and performance)
			* Poor data integrity and quality (migration issues, data conversion problems, violation of data standards)
			* Software not performing it's intended functions
		* Risks are used to investigate where to test more; testing is used to reduce risk or impact of a bug
		* Risk-based approach provies proactive opportunities to reduce level or product risk, starting at the initial stages
		* Identified risks may be used to:
			* Determine test techniques to be employed
			* Determine the extent of testing to be carried out
			* Prioritize testing in an attempt to find critical risk as early as possible
			* Determine whether any non-testing activities can be employed to redcue risk (ex. providing training)
* Incident management
	* Incidents are discrepancies between actual and expected outcome	
	* After investigation, an incident may turn to a defect
	* Incidents can be raised during development, review, testing or use of the software
	* Incident reporting:
		* Provide developers and other parties with feedback about the problem to enable identification, isolation and correction
		* Provide test leaders a means of tracking the quality of the system under test and the progress of testing
		* Provide ideas for test process improvement
	* Issues may include:
		* Date of the issue, issuing organization and author
		* Expected & actual results
		* Identification of the test item and environment
		* Software or system life cycle process in which the incident was observed
		* Description of the incident to enable reproduction and resolution (logs, screenshots)
		* Scope or degree of impact of stakeholder's interests
		* Severity of the impact on the system
		* Urgency/priority to fix
		* Status of the incident (open/deferred/duplicate/waiting to be fixed, fixed awaiting re-test, closed)
		* Conclusions, recommendations, approvals
		* Global issues, such as other areas affected
		* References, including the identity of the test case specification that revealed the problem
# Chapter 6 - Tool support for Testing
* Test tools can be used for many activities to support testing:
	* Test execution tools used direcly in testing, data generation tools, result comparison tools
	* Tools that help managing the testing process, test results, data, requirements, monitoring, reporting test execution
	* Exploration testing tools used in reconnaissance
	* Any other tools that aids in testing (a spreadsheet, a notepad)
* Tool support can have the following purposes:
	* Improve efficiency of test activities by automating repetitive tasks or supporting manual test activities
	* Automate activities that require significant resources when done manually (ex. static testing)
	* Automate activities that cannot be executed manually (ex. large scale performance testing)
	* Increase reliability of testing (ex. automating large data comparisons or simulating behavior)
* Test frameworks - reusable libraries, type of automation design, overall process of execution of testing
* Test tool classification
	* Tools can be classified on several ctiteria (ex. commercial/open source, technology, technical activites supported)
	* Some tools support one activity, some may support more but are associated with the most closely associated
	* Some tools may be intrusive (affect the test outcome). The consequence of them is called the probe effect
	* Some tools offer support for developers (ex. compontent testing tools, linters
	* Tool support for management of testing and tests - apply to testing over the entire software life cycle
		* Test management tools
			* Provide interfaces for executing tests, tracking defects, managing requirements
			* Support quantitative analysis and reporting
			* Support tracing the test object to requirement specifications
		* Requirement management tools
			* Store requirement statements
			* Store attributes for requirements
		* Incident management tools (defect tracking)
			* Store & manage incidents
			* Help managing the life cycle of an incident
		* Configuration management tools
			* Necessary for storage & version management of testware
	* Tool support for static testing
		* Cost-effective way of finding more defect at an earlier sstage in the development process
		* Review tools
		* Static analysis tools - helps enforcing coding standards, providing metrics
		* Modeling tools - used to validate software models by enumeratinig inconsistencies
	* Tool support for test specification
		* Test design tools - generating test inputs, oracles from requirements, design models, code
		* Test data preparation tools - manipulating databases to set up test data to maintain data anonymity
	* Tool support for test execution and logging
		* Test execution tools
			* Enable tests to be executed automatically using stored data inputs & expected outcomes
			* Can be used to record tests
		* Test harness/unit test framework tools
			* Facilitates the component testing by simulating the environment through the provision of mock objects
		* Test comparators
			* Determine differences between files, databases or test results
			* May use a test oracle
		* Coverage measurement tools
			* Through intrusive or non-intrusive means easure percentage of code structures exercised by unit tests
		* Security testing tools
			* Evaluate the ability of the software to protect data confidentiality, integrity, authentication, authorization
			* Mostly focused on particular technology, platform and purpose
	* Tool support for performance and monitoring
		* Dynamic analysis tools
			* Find defects occuring when software is executing, such as time dependencies or memory leaks
			* Usually used during integration testing and when testing middleware
		* Performance testing/load testing/stress testing tools
			* Monitor and report on how a system behaves under variety of simulated usage conditions (ex. concurrent users)
			* Simulated load carried out by load generators
		* Monitoring tools
			* Continous analysis, verification and report on usage of specific system resources
			* Warnings of possible service problems
	* Tool support for specific testing needs
		* Data quality assessment
			* Used when data is at the center of a project (data conversion/migration projects, data warehouses)
* Effective use of tools: Potential benefits and risks
	* Potential benefits of tool support
		* Repetitive work is reduced
		* Greater consistency and repeatability
		* Objective assessment (static measures, coverage)
		* Ease of access to information about tests or testing (statistics, graphs, incident rates, performance)
	* Potential risks of tool support
		* Unrealistic expectations for the tool (functionality, ease of use)
		* Underestimating the time, cost and effort for the initial introduction of a tool
		* Underestimating the time and effort needed to achieve significant and continuing benefits from the tool
		* Underestimating the effort required to maintain the test assets generated by the tool
		* Over-reliance on the tool(replacement for test design or use of automation where manual testing would've been better)
		* Neglecting verison control of test assets within the tool
		* Neglecting relationships and interoperability issues between critical tools
		* Risk of tool vendor going out of business, retiring the tool, selling the tool to a different vendor
		* Poor response from vendor for support/upgrades/defect fixes
		* Risk of suspension of open source/free tool project
		* Unforeseen, such as the inability to support a new platform
	* Special consideration for some types of tools	
		* Test execution tools
			* Often requires significant effort to achieve significant benefits
			* Simply automating the actions of a manual tester tend to work out poorly (poor unexpected events handling)
			* Data-driven testing approach separates out the test inputs and uses a more generic test script to execute tests
			* Keyword-driven testing approach uses action-words to derive test cases from
			* Technical expertise is needed
		* Static analysis tools
			* Can enforce coding standards, but applied to existing code may generate large quantity of messages
			* Gradual implementation of the analysis tool with initial filters to exclude some messages is an effective approach
		* Test management tools
			* Needs to interface with other tools or spreadsheets in order to produce useful information
* Introducing a tool into an organization
	* Main considerations include:
		* Assessment of organizational maturity, strengths, weaknesses, identification of opportunities
		* Evaluation against clear requirements and objective criteria
		* A proof-of-concept by using the tool during the evaluation phase to find out whether it performs effectively
		* Evaluation of the vendor or service support suppliers in case of non-commercial tools
		* Evaluation of training needs
		* Estimation of cost-benefit ratio based on concrete business case
	* Introducing a tool starts with a pilot, which has the following objectives:
		* Learn more detail about the tool
		* Evaluate how the tool fits with existing processes and practices, determine what would need to change
		* Decide on standard ways of using, managing, storing and maintaining the tool and the test assets
		* Assess whether the benefits will be achieved at a reasonable cost
	* Success factors for the deployment of the tool within an organization:
		* Rolling out the tool to the rest of the organization incrementally
		* Adapting and improving processes to fit with the use of the tool
		* Providing training/coaching/mentoring to new users
		* Defining usage guidelines
		* Implementing a way to gather usage information from actual use
		* Monitoring tool use and benefits
		* Providing support for the test team for a given tool
		* Gathering lessons learned from all teams
