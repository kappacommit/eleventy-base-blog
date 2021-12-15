---
title: Automated Quality Gates
description: Write-up on automated quality gates with Salesforce
date: 2018-09-13
tags:
  - DevOps
  - Salesforce
layout: layouts/post.njk
---
## Automated Quality Gates
Quality gates are exactly what they sound like. They're gates that prevent new code (or declarative functionality) from being deployed unless they pass some certain degree of quality. 

Quality gates are considered critical functionality at virtually every notable tech company in existence. Some companies have hundreds, even thousands of rules that must be satisfied before anything can be deployed.

It isn't hard to imagine the value here. Every single piece of functionality, no matter how small, was run against thousands of different gates. 

- If any of those pre-defined checks fail, the code can't go to production. 
- Manual reviewers (code review) no longer need to check these small things, reducing the time impact of code reviews.
- Automated tests never "glance over" or "let it slide" in the same way a human reviewer could.

## Quality Gates in Salesforce
So what sort of quality gates exist in Salesforce? There are generally fewer options available than, for example, a Java application, but there are still options available.

- Unit Tests
  - These are the "big one." Large orgs often have thousands of tests. They're so important, Salesforce mandates some amount of minimum code coverage before you can even deploy. 
- PMD
  - https://pmd.github.io/
  - Salesforce also implements PMD as one of the options in the SFDX Scanner tool https://forcedotcom.github.io/sfdx-scanner/
  - PMD is a static code analysis tool. What this means is that it scans "static" code (basically, it reads the actual code text, it does not execute the code) and locates common anti-patterns. 
    - Eg: If you execute SOQL in a loop, PMD can catch that. 
- Integration / UI Tests
  - This is a more rarely implemented form of quality gate with Salesforce. Often done with a tool like Selenium or Tosca. 
  - Unit tests can't test everything. Example: Lightning Web Components. You can't unit test that clicking into a LWC loads correctly, or that clicking certain buttons modifies the UI correctly.
  - Additionally, unit tests can't test integrations with external systems. If you need to ensure that Salesforce can successfully make an API call to an external system, you can't do that with apex unit tests. 
  - These tools allow you to have a testing layer that is external to Salesforce. It can call the API of the external system outside of a unit test context. They can also "click through" LWC and verify that they render correctly.

## Prod-Like Environments
This section is a bit of an aside. A common devops pillar is that all work should be done in "prod-like" environments. Basically, your development and test environments should be as similar to production as possible.

Salesforce makes this easy for us. You can always refresh a sandbox and you are automatically up to date with production. 

However many orgs have issues regularly refreshing their environments. Especially full copy sandboxes. The longer these sandboxes remain out of date, the less "true" your testing will be.
  - Eg: Imagine you remove read access to field A in production. But your UAT full copy sandbox does not get this change. All quality gates will run with the user still having access to field A, and you might be in for a surprise when that functionality hits production and they no longer have access.