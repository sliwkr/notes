# Chapter 0: Introduction
* K1 : Remember
* K2 : Understand
* K3 : Apply
* K4 : Analyze
# Chapter 1 Fundaments of testing
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
# Chapter 2 Testing throughout the software life cycle

