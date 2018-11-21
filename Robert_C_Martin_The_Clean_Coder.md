# The Clean Coder by Robert C. Martin
## Chapter 1: Professionalism
* Be careful what you ask for
* Non professionals don't have to take responsibility for their job. When a professional makes a mistake, he owns it
* Product is more important than your ego
* Be responsible for your imperfections
* Assume the code is faulty until you check it
* Every time QA finds a problem, be determined to prevent it from happening again
* To know the code works, test it. Then, test it again. And again.
* The code is hard to test if it's been designed to be hard to test
* "If you want your code to be flexible, you have to flex it"
* Merciless refactoring: always make some random acts of kindness to the code whenever you see it
* How do you prove you are not afraid to change the code? You change it all the time.
* It's your responsibility to develop yourself
   * Plan on working 60h/week: 40h for employer, 20h for you. Do the 20h with passion, it should be fun
* Know your domain: discover the abundant wealth of ideas available at the tip of your fingertips. 
* Learn things that are outside of your comfort zone
* The best way to learn is to teach
   * Design patterns
   * Design principles
   * Working Methods: Scrum, Kanban, XP, Structured Analysis, Structured Design
   * Disciplines: TDD, Object Oriented Design, Structured Programming, Continous Integration, Pair Programming
   * Artifacts: 
      * UML
      * DFD
      * Structure Chart
      * Petri Nets
      * Transition Diagrams
      * Flow charts
      * Decision tables
* Identify with your employer - their problems are *your* problems. Understand them and work to develop the solution
* "Programming is an act of supreme arrogance"
## Chapter 2: Saying No
* "Do; or do not. There is no trying." - Yoda
   * When you say you'll try, you're telling you'll use some unused reservoir of energy you've been holding back
* Stop being afraid of confrontation
* Facts > why. 
* The higher the stakes, the more important No becomes
* A team player communicates frequently, keeps an eye for his teammates and does his responsibilities the best he can
* Mind the cost of saying yes. Sometimes the only way to get to the right Yes is to be unafraid to say No
* Don't try to be a hero
## Chapter 3: Saying Yes
* You're not obliged to say Yes to everything, but it's good to work to find ways to make Yes possible
* Use language of Commitment
   * Say what you want. Mean what you say. Do as you mean.
      * Lack of commitment: Need/Should; Hope/wish; Let's
* You can only commit to things you have full control of
* As a professional you're obliged to write code that passes certain standards. Do not cut corners
## Chapter 4: Coding
* Typing blind is about confidence. Being able to sense your errors is really important
* Understand the problem and the way to solve it
* Write the code that solved the problem set by the customer. Make sure the customer defines his problem properly.
* Your code should follow solid engineering principles and be readable by others
* If you don't concentrate doing it, you risk having bugs, wrong structure, opaqueness, convolution and headaches
* If you're tired or distracted, *don't* code
* If you're distracted by some background process in your head, allocate fixed amount of time to it and get back to work
* Avoid the flow zone - you're not hyper productive and certainly not infallible in it
   * It's virtually impossible to enter the flow zone when pair programming
   * You might want to enter it when you're *practicing*
   * Music is the same
* Get enough sleep
* Creative output depends on creative input. Read stuff
* Debugging time is coding time, therefore, do all you can to diminish or avoid it
* Pace yourself. Software development is a marathon, not a sprint
* Creativity and intelligence are fleeting states of mind. Know when to walk away. Learn your patterns of creativity
* Regularely measure your progress and come up with three fact-based dates: best, nominal and worst case
* Hope destroys schedules and ruins reputations
* Overtime might be a good idea if for a short time, you can personally afford it and there's a fallback plan
* Define your "Done" as really completed; do not fall into the trap of false delivery
* Programming is hard. Help others. Accept help gratefully. *Do not protect your turf*. Learn how to ask for help
## Chapter 5: Test Driven Development
* The three laws of TDD
	* You're not allowed to write any production code until you have first written a failing unit test
	* You're not allowed to write more of a unit test than it is sufficient to fail - and noy compiling is failing
	* You're not allowed to write more production code that is sufficient to pass the currently failing unit test
* Benefits
	* Certainty that your code works
	* You lower the defect injection rate
	* You're more confident to *flex* it
	* You document your code by writing unit tests to it
	* It forces you to apply good design patterns
	* The unittests you write after the fact are *defense*. The ones you write before are offense
## Chapter 6: Practicing
* Although you don't have to, it's sometimes wiser to slow down and just think
* In programming and martial arts, speed depends on practice
* Exercises
	* Kata: a precise set of choreographed keystrokes that simulates the solving of some programing problem
		* You're not solving the problem - you're practicing movements and decisions involved in solving the problem
		* A good way to get familiar with IDE's, learn keyboard shortcuts or CI & TDD 
	* Wasa: Two-man kata
	* Randori: Multi-man wasa. Screen projected on the TV, one person writes a unit test, other makes it pass
* Open source is a cool way to broaden your experience in different technologies. 
* Practice when you're not getting paid, so you can get paid and get paid well
* Professionals practice because they care about doing the best job they can
## Chapter 7: Acceptance Testing
* Avoid premature precision. Once customer sees requirement running, he'll have a better idea of what he really want
* Defer precision as long as possible, but watch out for late ambiguities that stem out of wague communication
* Acceptance tests should be created by stakeholders and QA, and reviewed by developers for consistency
	* Business analyst writes a happy path
	* QA imagines what can go wrong and writes unhappy paths
	* Developer executes test for the new feature and see it fail, then make it pass
* As a professional developer, it is your job to negotiate with a QA for a better test. Avoid being passive-agressive
* Writing an automated acceptance test might seem like an extra work, but it's saving your future time
* Single Responsibility Principle: group things that change for the same reason
* A broken CI build should be viewed as emergency
## Chapter 8: Testing Strategies
* The goal of development should be that QA finds nothing wrong
* Unit tests
	* By programmer, for programmers
	* Specify system at the lowest level
* Component tests
	* By QA and Business with developer assistance
	* Test individual components of the system, specify the happy paths rather than unhappy paths
	* Any other component besides the one being tested are be mocked
* Integration tests
	* Assemble group of components and test their behaviour, make sure they work together
	* Any other components besides the tested ones are mocked
* System tests
	* Ultimate integration tests, they contain performance tests
	* Test the correct system construction, not the correct behaviour
* Manual exploratory tests
	* There's no plan
	* Find unexpected system behaviours with humanz 
## Chapter 9: Time Managment
* Meetings 
	* Necessary, but a huge time waster. Send your invites wisely
	* It's okay to decline a meeting invitation and it's ok to leave if it gets boring
	* Meeting should have an agenda with times for each topic and a stated goal
	* Planning is meant to select items from backlog that'll be executed in the next iteration
		* Estimates should be already done
		* No more than 10 minutes on a given item
		* Meeting should take no more than 5% of total iteration time (5% of 1 week = 2h)
	* Retrospective & Demo
		* Discuss what went right and what went wrong
		* Show off
		* Often abused, can steal a lot of time, schedule smartly
* "Any argument that can't be settled in 5 minutes can't be settled by arguing"
* Strong character, yelling or charisma doesn't settle disagreements for long. Data does
* If you agree, you must engage
* Focus resembles manna closely. Find time to recharge it
	* Beware of manna-soaking meetings
	* You propably heard it, but get enough sleep.
	* More caffeine isn't always a good idea
	* Manna can be recharged by de-focussing, that is long walks, mediation, power naps, podcasts and variety of others
	* Physical excersise can help to recharge
* Consider the pomodoro technique
* You might want to avoid something you're scared or plain don't want to do, but it's unprofessional, so don't
* When you realize the path you've chosen is a blind alley, have the courage to back out immediately
## Chapter 10: Estimation
* Business view estimates as commitment wheras developers tend to view them as guesses
* Professionals do not make commitments unless they *know* they can achieve them. Don't make promises you can't keep
* There's no secret skill to estimation
* Estimate is not a number - it's a propability distribution (days * percent)
* PERT's Trivariage analysis: Optimistic, Nominal(most propable) and Pessimistic estimate
	* time it'll take t = (O + 4N + P)/6
		* more tasks: sum all the times 
	* standard deviation d = (P - O)/6
		* more tasks: square root of the sum of the squares of deviations
## Chapter 11: Pressure
* Imagine how surgeons deal with pressure
* The best way to stay calm and decisive under pressure is to minimize situations that cause pressure
* Professionals help business meet their commitments, but they're not obliged to accept their commitments
* Stay clean; quick & dirty fix is an oxymoron - dirty means slow
* You know what you belive in by observing yourself in a crisis. 
	* Choose ways of working you feel comfortable following during a crisis and stick to them all the time
	* Trust your ways of working to get you where you want; do not question or abandon them during a crisis
* Panic won't make it better or faster; slow down. 
	* Think the problem through
	* Plan a course to the best possible outcome
	* Drive towards the outcome in a reasonable and steady pace
* Communicate the trouble with your peers, help peers out of the trouble if they're in some
## Chapter 12: Collaboration
* "We didn't become programmers because we like working with people. As a rule we find interpersonal relationships
messy and unpredictable. We like the clean and predictable behavior of the machines that we program. We are happiest
when we're alone in a room for hours deeply focusing on some really interesting problem."
* Perhaps you didn't get into programming to work with people. Tough luck for you; programming is all about it
* Understanding business and office conventions will help you not get fired
* Don't build a wall of ownership around your code; make the team own the code
* Pairing is simply one of the most efficient tools to solve the problem or share knowledge; use it
* Professionals work together. You can't work together while sitting in the corner wearing headphones
## Chapter 13: Teams and Projects
* There's no such thing as half a person
* It takes time for a team to form; usually a good team consists of around 12 people; good tester ratio is 2:1
* It's harder to form a team than to build a project; think of a team as an engine you feed projects to

