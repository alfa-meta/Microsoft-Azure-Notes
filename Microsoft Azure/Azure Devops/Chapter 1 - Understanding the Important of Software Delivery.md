
DevOps is the intersection of Software Engineering (Development), Quality Assurance (QA), Business Operations (Technology).

DevOps - is the union of people, process, and products to enable continuous delivery of value to our end users.


Continuous Integration (CI):
	1. Make sure the code compiles, without errors.
	2. Make sure all unit tests are passed, with the latest code changes and coverage.
	3. Validation for code security vulnerabilities.

Continuous Integration - a build that is defined to compile each check-in/commit to the code base and then executes all unit tests to validate the code base to ensure stability of the code base.

### Continuous Delivery:
1. Development (auto)
2. Application/unit tests (auto)
3. Integration tests (auto)
4. Acceptance tests (auto)
5. Production deployment (manual)


### Continuous Deployment:
1. Development (auto)
2. Application/unit tests (auto)
3. Integration tests (auto)
4. Acceptance tests (auto)
5. Production deployment (auto)

### Release/Deployment Pipeline

1. Version Control
2. Build
3. Dev Environment. Dev integration Servers, approved by Dev Lead.
4. QA Servers. Approved by QA Devs.
5. UAT/Staging Servers. Approved by Release Managers.
6. Production Servers. Approved by Release Managers.


### Test Automation Integration

A test build pipeline should include:
1. Functional UI tests.
2. API tests
3. Integration Tests.
4. Load and Performance tests.

This is meant to run against a deployed environment rather than a development environment.
