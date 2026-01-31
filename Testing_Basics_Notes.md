# QA-Learning-Portfolio
Personal QA learning portfolio with testing theory, examples, and practice exercises.

# 01_role_of_testing

## Why testing is important?

Software testing is the process of checking whether a system behaves as expected and meets user and business requirements. It helps us find defects before the software reaches real users, which reduces project risk and improves product quality.

Testing is also important for cost and reputation. By running different kinds of tests, we can see how the system behaves with valid and invalid inputs, under high load, and in real‑world scenarios. When defects are found early, they are cheaper to fix and do not affect customers, which protects the company’s reputation.

## Scenario: missing testing causing a serious issue

Imagine an e‑commerce site where 1,000 customers try to buy a product at the same time during a big sale. If performance and load testing were not done, the checkout page might hang or crash under this traffic. Customers would be frustrated, some orders might fail, and the company could lose sales, discounts, and customer trust because the system was not tested for this situation.
text


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

## Why early involvement reduces cost?

When testers are involved early in the SDLC, they can find problems in requirements or design before code is written. Fixing an issue at this stage usually means only updating documents, which is much cheaper than changing code, retesting, and repairing data later.

If the same defect is found after release, the team may need emergency code changes, extra testing, possible data correction, and customer support, so the total cost is much higher.

### Simple cost comparison (illustrative)

- Requirements / Planning phase: **1 unit** of cost  
- Design / Static testing phase: **5 units**  
- System / Acceptance testing phase: **10–15 units**  
- Production (after release): **30 units or more**  

The idea is that the later a defect is found, the more people, time, and rework are involved, so the cost grows significantly.
text
# 03_srs_requirements

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



  # 4.Black‑box vs White‑box Testing (Login Example)

## Short definitions

**Black‑box testing**  
Black‑box testing is a technique where the tester does not look at the internal code and tests the system from a user point of view, focusing on inputs and outputs.

**White‑box testing**  
White‑box testing is a technique where tests are designed with knowledge of the internal code and logic, and the tester verifies that each important path in the code works as expected.

---

## Quick comparison

| Aspect          | Black‑box Testing                          | White‑box Testing                             |
|-----------------|--------------------------------------------|-----------------------------------------------|
| Code knowledge  | Not required                               | Required                                      |
| Main focus      | User behaviour, inputs and outputs         | Internal logic, branches, and conditions      |
| Typical role    | Tester / QA engineer                       | Developer or technically strong tester        |
| Level           | System / integration / acceptance tests    | Unit / component tests                        |

---

## Login example – Black‑box tests

Requirement: *Login ID must be 6–10 characters.*

From a black‑box point of view, the tester only sees the login screen and tests different inputs:

1. Login ID with **6 characters** → should be accepted and allow login.  
2. Login ID with **5 characters** → should be rejected with a validation message.  
3. Login ID with **10 characters** → should be accepted and allow login.  
4. Login ID with **11 characters** → should be rejected.  
5. Empty Login ID → should be rejected.  
6. Login ID with only special characters → should be rejected.  

Here the tester does not care how the validation is implemented in code; only the behaviour matters.

---

## Login example – White‑box tests

For the same login feature, a white‑box view looks inside the code.  
Examples of what to test:

- Ensure each branch of the length check is executed:
  - `< 6`, `6–10`, and `> 10`.  
- Ensure all conditions for username and password checks are covered:
  - Valid username + valid password.  
  - Valid username + invalid password.  
  - Invalid username + any password.  

These tests are usually written as **unit tests** or similar, and require access to, and understanding of, the login function’s code.

---

## When to use which

- Use **black‑box testing** when you want to verify the feature from the user’s perspective, such as on the full web application.  
- Use **white‑box testing** when you have access to the code and want to make sure every important path and condition is executed, for example at unit or component level.

As a C++ developer, understanding code and control flow helps a lot with white‑box and unit testing, because it is easier to see which branches, loops, and edge cases need additional tests and to communicate these with developers.



# 05_functional_vs_nonfunctional
# Functional vs Non‑functional Testing

## Short definitions

**Functional testing**  
Functional testing checks *what* the system does by verifying that each feature behaves according to the business requirements. It focuses on actions like money transfer, account balance, login, and form submission.

**Non‑functional testing**  
Non‑functional testing checks *how well* the system works under different conditions. It covers performance, usability, reliability, security, and visual aspects such as layout and alignment.

---

## Examples for a banking web application

### Functional test examples

1. Verify that a user can **transfer money** from one account to another and the balances are updated correctly.  
2. Verify that the **account balance** shown on the dashboard is correct after recent transactions.  
3. Verify that a user can **apply for a credit card** and the application is saved in the system.  
4. Verify that a **new bank account** can be created with valid customer details.

### Non‑functional test examples

1. Check how quickly the **dashboard page loads when 100 users** are logged in at the same time (performance / load test).  
2. Verify that **page layout and images** are properly aligned on different screen sizes and browsers (usability / compatibility).  
3. Observe whether the system **crashes or hangs** under high load or after long use (reliability / stability).  

---

## Why non‑functional tests matter for user satisfaction

Even if all features work functionally, users will be unhappy if the system is slow, confusing, or unstable.  
Non‑functional testing ensures that:

- Pages load fast enough.  
- The layout is clear and easy to use.  
- The system does not hang or crash during important actions, such as a big sale or end‑of‑month payments.

This protects both user satisfaction and the company’s reputation.

---

## Positive / Negative non‑functional examples

Examples of **non‑functional problems** that can hurt user experience:

1. The web page **hangs** after the user clicks the Login button.  
2. Submitting a form takes a **very long time**, so users think it failed.  
3. Page **alignment is broken**, with buttons or input fields overlapping.  
4. An **image overlaps text**, making labels or messages hard to read.

A well‑tested system should avoid these issues and provide a fast, stable, and visually clear experience.



# 06_bva_ep_password

## Equivalence Partitioning (EP)

Equivalence Partitioning groups input values into partitions where all values in the same group are expected to behave the same. Instead of testing every possible value, we choose one representative value from each partition.

**Requirement:** “Password must be 8–16 characters.”

We can define these partitions:

1. **Fewer than 8 characters** → Invalid  
2. **Between 8 and 16 characters (inclusive)** → Valid  
3. **More than 16 characters** → Invalid  

**EP example test values:**

- Test 1: 5‑character password → **Invalid**  
- Test 2: 10‑character password → **Valid**  
- Test 3: 20‑character password → **Invalid**

---

## Boundary Value Analysis (BVA)

Boundary Value Analysis focuses on values at the edges of the valid range because many defects appear exactly at these boundaries.

For the same requirement, important boundaries are **8** (minimum) and **16** (maximum).

**BVA test cases:**

1. 7 characters → just below minimum → **Invalid**  
2. 8 characters → minimum → **Valid**  
3. 9 characters → just above minimum → **Valid**  
4. 15 characters → just below maximum → **Valid**  
5. 16 characters → maximum → **Valid**  
6. 17 characters → just above maximum → **Invalid**

Using EP plus BVA gives good coverage with a small number of tests.
text
# 07_decision_table_discount

## What is Decision Table Testing?

Decision Table Testing is a technique used when the outcome depends on combinations of different conditions. It helps ensure that **every combination** of conditions and its expected result is covered by at least one test case.

---

## Example discount rule

Assume this simple business rule:

- Customer types: **Regular** or **Premium**  
- Order amount condition: **amount ≥ 1000**  
- Discount rules:  
  - Regular customers: **no discount**.  
  - Premium customers: **5% discount** if order amount ≥ 1000, otherwise 0%.

---

## Decision table

| Case | Customer type | Order amount ≥ 1000? | Expected discount |
|------|---------------|----------------------|-------------------|
| 1    | Regular       | No                   | 0%                |
| 2    | Regular       | Yes                  | 0%                |
| 3    | Premium       | No                   | 0%                |
| 4    | Premium       | Yes                  | 5%                |

Each row in the table can be turned into one test case to make sure the system applies the correct discount for every combination of customer type and order amount.
