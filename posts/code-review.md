---
title: Code Review
description: Code Review with Salesforce
date: 2021-09-28
tags:
  - DevOps
  - Salesforce
layout: layouts/post.njk
---
## Code (or no-code) Review
**Code review** is a simple concept: writing code is hard and it is easy to overlook things, so you should have another experienced invidiual look over new code additions prior to releasing that code.

## Code Review In Salesforce
Salesforce is a bit of a snowflake in regards to "code review", such that the phrase "code review" doesn't exactly fit. Eg: Flows are effectively "visual code", and should be reviewed as such.

I will separate the rest of this post into 2 sections: "Code Review" and "No-Code (Declaractive Automation) Review"

## Code Review
This section is about reviewing actual code in Salesforce (Apex, Javascript). Salesforce really isn't all that much different in regards to code review that, for example, a java app. And as such, it makes sense to follow standard code review practices. 

If you have implemented [Automated Quality Gates](../quality-gates) then most of the hard work is done for you. If not, some special considerations for Salesforce:

- Ensure code is not using an undue amount of limits. Eg: SOQL in a loop.
- Ensure appopriate metadata is included alongside the code. Eg: If a new apex class was added, ensure appropriate profiles and permission sets have access to this code.

## No-Code Review
This section is mostly about declarative automation in Salesforce (Flow, Process Builder, Workflow Rules). 

Reviewing declarative automation is hard. Much harder than reviewing code. And as of now, there are no static analysis tools to handle automatically checking anti-patterns in flows. As such, the review needs to make additional effort to review these types of automation.

- Ensure correct metadata is included with the deployment. If deploying a new version of a flow, make sure that the correct version will be active post-deployment.
- Check potential limit issues. No queries in a loop, no DML in a loop, no particularly heavy operations in a loop.
- Generally, the reviewer will need a link to the actual piece of automation, it isn't really possible to review these types of automation entirely based on their XML metadata files.

## General Code Review Tips
Code review can be a high-friction process, but it doesn't need to be. Here are some tips I have learned over time in regards to make code review less of a "big deal."

- Reviews should be **fast**, but **asynchronous**. This requires buy-in from both the reviewer and reviewee. 
  - **Fast**: When the reviewee requests a code review, it should be reviewed by the team within 1 day, ideally in half a day or less. This means the team needs to make it a priority to perform reviews. 
    - If it is taking several days to get somebody to review your work, your process is broken.
  - **Asynchronous**: When the reviewee requests a review, the reviewer should not be expected to immediately drop what they're doing and perform the review. They should be free to perform the review at their leisure, within the 4-8 hour window. 
- Do not block review based on nitpicks or style issues. Feel free to mention them (I prefer the format: "*Nit: I personally prefer doing this like xyz*") but do not block code from being approved over small style issues.
- "Request a review on 10 lines of code, the reviewer will find 10 issues. Request a review on 1000 lines of code, the reviewer will give an 'all good'"
  - I like this quote. The gist is that small changes are easier to review. If you're deploying 6 months of work to production in one big-bang release, nobody is going to give a thorough review. 
  - **Keep changes small**, release fucntionality incremenetally.