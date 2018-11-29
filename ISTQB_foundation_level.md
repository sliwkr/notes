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

