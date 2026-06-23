# 3. Unit Testing

> From the Alibaba Java Development Manual (Huangshan Edition). Severity levels: [Mandatory] must follow · [Recommended] should follow · [Reference] good practice for reference.

 1. **[Mandatory]** A good unit test must follow the AIR principle.
Note: When running in production, unit tests are as imperceptible as air (AIR), yet they are critical for safeguarding quality. At a macro level, a good unit test is automatic, independent, and repeatable.
      - A: Automatic
      - I: Independent
      - R: Repeatable

 2. **[Mandatory]** Unit tests should be fully automated and non-interactive. Test cases are usually executed on a regular basis, and the execution process must be fully automated to be meaningful. A test whose output requires manual inspection is not a good unit test. Using System.out for manual verification is not allowed; unit tests must use assert to verify.

 3. **[Mandatory]** Keep unit tests independent. To ensure unit tests are stable, reliable, and easy to maintain, unit test cases must never call one another, nor may they depend on the order of execution.
Counter-example: method2 depends on the execution of method1, taking its result as the input to method2.

 4. **[Mandatory]** Unit tests can be executed repeatedly and must not be affected by the external environment.
Note: Unit tests are usually placed in continuous integration, and are executed every time code is pushed. If unit tests depend on the external environment (network, services, middleware, etc.), it can easily render the continuous integration mechanism unusable.
Positive example: To avoid being affected by the external environment, when designing the code, change the dependencies of the SUT (Software under test) to be injected, and at test time use a DI framework such as Spring to inject a local (in-memory) implementation or a Mock implementation.

 5. **[Mandatory]** For unit tests, ensure the test granularity is small enough to help pinpoint problems precisely. Unit test granularity is at most the class level, and generally the method level.
Note: Only with small granularity can you quickly locate the source of an error when one occurs. Unit tests are not responsible for checking interaction logic across classes or systems; that is the domain of integration tests.

 6. **[Mandatory]** Ensure that the incremental code of core business, core applications, and core modules passes unit tests.
Note: Promptly add unit tests for newly added code. If newly added code affects existing unit tests, fix them promptly.

 7. **[Mandatory]** Unit test code must be written in the following project directory: src/test/java; it is not allowed to be written in the business code directory.
Note: This directory is skipped during source compilation, while the unit test framework scans this directory by default.

 8. **[Recommended]** Basic goals for unit tests: statement coverage reaches 70%; for core modules, both statement coverage and branch coverage must reach 100%.
Note: The DAO layer, Manager layer, and highly reusable Services mentioned in the application layering of the engineering conventions should all be unit tested.

 9. **[Recommended]** Follow the BCDE principle when writing unit test code, to ensure the delivery quality of the module under test.
 - B: Border, boundary value testing, including loop boundaries, special values, special points in time, data ordering, etc.
 - C: Correct, correct input that yields the expected result.
 - D: Design, write unit tests in conjunction with the design documents.
 - E: Error, force error information input (such as: illegal data, exceptional flows, beyond business allowances, etc.) and obtain the expected result.

 10. **[Recommended]** For database-related operations such as query, update, and delete, do not assume that the data in the database exists, or directly operate the database to insert the data; instead, prepare the data by inserting or importing it programmatically.
 Counter-example: For a unit test that deletes a row of data, a row is first manually added directly in the database as the deletion target, but this newly added row does not conform to the business insertion rules, causing abnormal test results.

 11. **[Recommended]** For database-related unit tests, you can set up an automatic rollback mechanism to avoid leaving dirty data in the database. Alternatively, give the data produced by unit tests clear prefix or suffix markers.
 Positive example: In the internal unit tests of the Foundation Technology Department, the prefix FOUNDATION_UNIT_TEST_ is used to mark unit-test-related code.

12. **[Recommended]** Perform necessary refactoring of untestable code at appropriate times to make the code testable, avoiding writing non-standard test code just to meet testing requirements.

13. **[Recommended]** During the design review phase, developers need to work with testers to determine the scope of unit testing. Unit tests should ideally cover all use cases (UC).

14. **[Recommended]** As a means of quality assurance, complete unit testing before submitting the project for testing. It is not recommended to add unit test cases after the project is released.

15. **[Reference]** To make unit testing more convenient, business code should avoid the following situations:
 - Doing too much in the constructor.
 - Having too many global variables and static methods.
 - Having too many external dependencies.
 - Having too many conditional statements.
Note: For multi-level conditional statements, it is recommended to refactor using guard clauses, the strategy pattern, the state pattern, etc.

16. **[Reference]** Do not hold the following misconceptions about unit testing:
 - "That's the testers' job." This is a development manual, and everything in it is strongly relevant to developers.
 - "Unit test code is redundant." The overall functionality of a system is strongly correlated with whether each unit component tests correctly.
 - "Unit test code does not need maintenance." After a year or so, the unit tests will be all but abandoned.
 - "Unit tests have no dialectical relationship with production incidents." Good unit tests can avoid production incidents to the greatest extent possible.
