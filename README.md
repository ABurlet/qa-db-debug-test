Database Test Suite – User Tests (Debugging Assignment)

**Overview**

This project is a debugging-focused database test assignment involving a small user-related test suite. One test was failing while two others were passing. The goal was to reproduce the failure, identify the root cause, and fix the issue without modifying the database schema or constraints.
The final solution updates the failing test so it satisfies existing database validation rules. No database changes were required.


**Tech Stack**
Node.js
PostgreSQL
Docker
Mocha & Chai

**Tools**
Cursor


**Problem Summary**

**Failing test**

should create a user with valid credentials

**Error**
User validation failed: security requirements not met 

**Other tests**
-should reject duplicate usernames
-should reject users younger than 13 years old

These passed successfully.


**Root Cause**

-The database contains a trigger that enforces that every row in the users table must include a non-empty password_hash.
-The two passing tests already included this field in their INSERT statements.
-The failing test attempted to create a user without providing password_hash, which caused the database to reject the insert.
The database behavior was correct. The test data was incomplete.

**Solution**
-Updated the failing test in test_database.js to include the password_hash column.
-Provided a valid hash value with the same format used in the other tests.
-No database changes were made.
This allows the test to satisfy the existing database trigger and pass successfully.


**Build & Environment Fixes**

While reproducing the failing test locally, I encountered a Docker build issue due to the base image using an end of life Node/Debian version. I updated the base image to the supported Node version so the provided test harness builds and runs reliably. I also adjusted PostgreSQL config paths to avoid hardcoded version assumptions and removed an unused environment variable that triggered a security warning. These changes were limited to ensuring the container builds and executes successfully. The database schema and validation logic were not modefied. 

**How to Run**
Build: docker build -t qa-db-test .
Run: docker run --rm qa-db-test

**AI Usage**

This assignment was completed by installing and using Cursor. AI was used as a debugging assistant to analyze error output and compare failing and passing tests. Final code decisions were identified and implemented manually.

**Outcome**
All tests pass
Database unchanged
Test suite accurately reflects existing validation rules
Environment builds successfully

**Summary**

Fixed a failing database test by adding a missing required field (password_hash) so the test matches existing database validation rules, without modifying the database itself.
