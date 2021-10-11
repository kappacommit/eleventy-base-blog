---
title: Testing Salesforce
description: Automated Tests with Salesforce
date: 2021-09-28
tags:
  - DevOps
  - Salesforce
layout: layouts/post.njk
---
## Automated Testing
This post is about automating unit testing, and getting them to run in a bit of a smart way. 

I highly recommend reading my post on [deployments](../deployments) first. I will also recommend reading [lead time](../lead-time) for justification on why to bother with this at all (but that one is less necessary.)

## Unit Tests 
I'm not going to justify the "why" for unit tests in this post. Please read my post on [Quality Gates](../quality-gates) for the "why." This post will instead focus on the "how." 

## Testing Pains
Anybody in a sufficiently large org has had issues with unit tests at one point. Probably at many points. Some issues you may see:

1. A test starts failing seemingly at random. Maybe even without a recent deployment.
2. Running all tests is slow.
3. Running all tests in parallel is much faster, but results in random failures.

This post will focus on 2, and will provide a bit of an aside on 3 as well. 

## Partial Testing
If you've read my post on [Partial Deployments](../posts/deployments) then you know we have a way in which we can deploy *only the metadata that has changed since the last deployment.*

The question is, can we run only the subset of unit tests that cover the subset of metadata that has changed since the last deployment?

The answer to that is categorically **no**. A change to field A might cause test C to fail, even if C has no relation to field A in any way. Any single metadata change could theoretically cause any single test to fail.

But we can get pretty close. When we calculated our list of files for a partial deployment, we have inherently also made a list of all Apex classes that have changed. We just need some way to know "Apex Class A has changed -> Run Test Class A"

You can do this by following a specific naming convention:
- Class name: `ApexClass1`
- Test name: `ApexClass1_Test`

We now know that whenever ApexClass1 has changed, we can run ApexClass1_Test. This test class should:
- Confirm that the functionality of the class did not break.
- Generate at least 75% code coverage for this class.

You can then deploy by adding the -r (RunTests) parameter to the deployment job:

```
sfdx force:source:deploy -p $(git diff --name-only <tag-name> | tr '\n' ',') -r <your list of tests>
```

## End Result
The end result is that we can implement both partial deployments and partial tests. Which significantly reduces lead time.

But, things aren't perfect. Like I mentioned before, Class A might break Class C, even if they're unrelated. That means this method of "partial testing" leaves some gaps. How do we deal with those? A couple options:

- Use your CI/CD tool to schedule a regular execution of ALL apex tests (we schedule it once nightly at 3am). This will ensure that unrelated test failures are still surfaced.
  
- Require developers to execute all apex tests in their sandbox prior to merging. (See [Quality Gates](../posts/quality-gates) on how to automate this.)

