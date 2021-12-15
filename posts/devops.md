---
title: DevOps With Salesforce
description: Brain dump on DevOps + Salesforce
date: 2021-09-28
tags:
  - DevOps
  - Salesforce
layout: layouts/post.njk
---
## Extremely Brief DevOps Crash Course
What is DevOps? There isn't really a good one-line definition for DevOps, and I don't want to write out a whole thing on all the nuances, so I will describe it like this:

- DevOps is about reducing friction in the software development process. Some causes of friction:
  - Deploying new work is frustrating or slow
  - New work is deployed broken and requires fire-fighting
  - It takes a long time to do *x*
  - Multiple approvals or sign-offs needed to do certain things

## Automated Quality Gates
One method of reducing friction in your development process is to automate testing. You can have a guarantee that no existing functionality broke when new functionality is added, and no human is involved in repeatedly testing older existing functionality.

See: [Automated Quality Gates](../quality-gates)
See: [Automated Testing](../testing)

## Lead Time
- Lead Time is the amount of time it takes for something to go from Point X in a process to Point Y. Eg: "Time between somebody having an idea, and that idea being deployed in production." A pillar of DevOps is around reducing lead time, mostly by reducing other forms of friction.

See: [Lead Time](../lead-time)

## Automated Deployments
Deploying in Salesforce can be an arduous process. When deploying is difficult, your developers and admins will be incentivized to deploy fewer times. This means larger changes. Larger changes are almost universally worse than frequent, smaller changes.

See: [Automated Deployments](../deployments)

## Production-Like Environments
A pillar of DevOps is to ensure all of your development process is occurring in environments that are as close to production as possible. After all, what is the point of testing a new feature in a full-copy UAT Sandbox that was last refreshed 1 year ago? 

See: [Automated Quality Gates](../quality-gates)

## High Trust Environments
- Maybe something about this
  
## Upskilling
Knowledge sharing is a critical piece of the software development process. Having 1 person who knows something is always a risk to every project.

See: [Upskilling](../upskilling)

## Code Review
One cause of friction in the development process is when broken functionality is deployed to production. A process that all notable tech companies to help avoid this is "code review" (or in Salesforce's case, sometimes "No-Code Review"). 

See: [Code Review](../code-review)