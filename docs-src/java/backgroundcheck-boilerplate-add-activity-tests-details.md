---
id: backgroundcheck-boilerplate-add-activity-tests-details
title: Add Activity method tests
sidebar_label: Test Activity code
description: How to test Activity code
tags:
- testing
- developer guide
- java sdk
---

<!-- DO NOT EDIT THIS FILE DIRECTLY.
THIS FILE IS GENERATED from https://github.com/temporalio/documentation-samples-java/blob/main/backgroundcheck/src/test/java/backgroundcheckboilerplate/BackgroundCheckBoilerplateActivitiesTest.java. -->

Temporal provides the `TestWorkflowExtension` class to simplify the creation of
the test environment. Using this extension you provide your Activity
to register with a Worker created by the testing framework to be used during testing.
The extension provides a `TestWorkflowEnvironment`, `Worker`, and a stubbed Activity
object to each test as well as manage the lifecycle of the test environment.
If you require more granular control of the test environments, you can manually create
and destroy all these parts in methods annotated with `@BeforeEach` and `@AfterEach`
as defined by JUnit.

You annotate the method with @Test and test the results of the Activity via assertions.