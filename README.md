# QA-Learning-Portfolio
Personal QA learning portfolio with testing theory, examples, and practice exercises.

# 01_role_of_testing.md

## Why testing is important

Software testing is the process of checking whether a system behaves as expected and meets user and business requirements. It helps us find defects before the software reaches real users, which reduces project risk and improves product quality.

Testing is also important for cost and reputation. By running different kinds of tests, we can see how the system behaves with valid and invalid inputs, under high load, and in real‑world scenarios. When defects are found early, they are cheaper to fix and do not affect customers, which protects the company’s reputation.

## Scenario: missing testing causing a serious issue

Imagine an e‑commerce site where 1,000 customers try to buy a product at the same time during a big sale. If performance and load testing were not done, the checkout page might hang or crash under this traffic. Customers would be frustrated, some orders might fail, and the company could lose sales, discounts, and customer trust because the system was not tested for this situation.
text
# 02_sdlc_phases.md

## SDLC phases and tester responsibilities

The Software Development Life Cycle (SDLC) describes the phases used to deliver software from start to finish: Planning, Defining, Building, Testing, Deploying, and Maintaining.


In Planning phase 

Description:Decide what to build and why.
Tester Responsibilities:
Collaborate with stakeholders, help define the test strategy, estimate testing effort, and participate in risk analysis.

In Defining phase

Description:Refine and document requirements and design
Tester Responsibilities:
Review requirements, highlight ambiguities, help make requirements testable, and update risk analysis.

In Building phase

Description:Developers implement the solution.
Tester Responsibilities:
Prepare test cases, test data, and test environments; support developers with unit‑test ideas when needed.

In Testing phase

Description:Verify that the system works as expected.
Tester Responsibilities:
Execute test cases, log defects, retest fixes, run regression tests, and report test results.

In Deploying phase

Description:Release the software to users.
Tester Responsibilities:
Run smoke or sanity tests on the deployed build and support user acceptance testing where needed.

In Maintaining phase

Description:Improve and support the live system.
Tester Responsibilities:
Test bug fixes and enhancements, perform regression testing, and help monitor quality in production.

## Why early involvement reduces cost

When testers are involved early in the SDLC, they can find problems in requirements or design before code is written. Fixing an issue at this stage usually means only updating documents, which is much cheaper than changing code, retesting, and repairing data later.

If the same defect is found after release, the team may need emergency code changes, extra testing, possible data correction, and customer support, so the total cost is much higher.

### Simple cost comparison (illustrative)

- Requirements / Planning phase: **1 unit** of cost  
- Design / Static testing phase: **5 units**  
- System / Acceptance testing phase: **10–15 units**  
- Production (after release): **30 units or more**  

The idea is that the later a defect is found, the more people, time, and rework are involved, so the cost grows significantly.
text
# 03_srs_requirements.md

## Definition and importance of SRS

A Software Requirements Specification (SRS) is a document that describes what a software system should do and how it should behave. It includes functional requirements (features and behaviours) and non‑functional requirements (performance, security, usability, etc.), and may also describe user interactions and constraints.

For testers, the SRS is critical because it is the main source for designing test cases and defining what “correct behaviour” means. If the SRS is unclear or wrong, testing and development will also go in the wrong direction.

## Good vs ambiguous requirement

**Good requirement example**

- “The system shall lock the user account after 5 consecutive failed login attempts and display the message: ‘Your account is locked. Please contact support.’”

This is good because it is clear, specific, and testable. It tells us **when** the lock happens, **what** the system must do, and **what message** to show.

**Ambiguous requirement example**

- “The system should lock the account after several failed logins.”

This is ambiguous because “several” is not defined. Testers and developers might interpret it differently (3, 5, or 10 attempts). Without clarification, it is hard to design correct tests.

## Cost impact of catching issues early vs late

If a requirement problem (for example, an unclear rule about account locking) is found during the SRS review, updating the document is quick and cheap. Developers and testers then work with the corrected version.

If the same issue is discovered after release, the team may need to change code, update tests, modify existing data, and possibly communicate with affected users. This makes the total cost much higher than fixing it during the requirements phase.

## Template for clarifying unclear requirements

When a requirement is ambiguous, a tester can use a simple template to ask for clarification:

- **Requirement ID / text:**  
  Copy the exact line from the SRS.

- **Issue:**  
  Explain what is unclear (for example: “The term ‘several attempts’ is not defined.”).

- **Questions to stakeholders:**  
  - What is the exact number of attempts?  
  - Should the lock be temporary or permanent?  
  - What message should the user see?

- **Proposed clarified requirement (optional):**  
  Write a suggested clear version, such as:  
  “The system shall lock the user account after 5 consecutive failed login attempts and display the message ‘Your account is locked. Please contact support.’”

- **Decision / final wording:**  
  Capture the agreed final text so everyone can use the same requirement going 
