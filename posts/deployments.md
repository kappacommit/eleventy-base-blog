---
title: Automated Deployments
description: Automated Deployments with Salesforce
date: 2018-10-11
tags:
  - DevOps
  - Salesforce
layout: layouts/post.njk
---
## Automated Deployments
Question for you, when given the following list of tasks, which gives you the most headache?
1. You are assigned some new work
2. You complete the work
3. You retrieve your work from your environment (sandbox/scratch org)
4. You open a pull request for review
5. You deploy your work to production

If you answered 5, this blog post will hopefully help you out a bit. If you answered 3 or 4, I might be able to help you out later. 

Also, I suggest you check out my other post on [Lead Time](../lead-time) if you're wondering where the value is here.

## When deployments are hard
What makes deployments difficult with Salesforce?
- When your org become large, deployments take a long time.
- Sometimes they fail for seemingly no reason.
- The tools aren't always great, or they require manual processes.

## Making deployments easy
You can solve most of the issues with deployments by automating them, and by configuring them to be a bit smarter. 

For automation, you will need a CI/CD tool. We use [Concourse CI](https://concourse-ci.org/), some people use [Github Actions](https://github.com/features/actions), and another popular tool is [Jenkins](https://www.jenkins.io/).

CI/CD (**Continuous Improvement / Continuous Delivery**) tools are effectively "thing doers", they allow you to automate actions in a predictable way. 

## Automated Full Deployments
Note: I am assuming your metadata is stored in source control. If it is not, this post (and most of this blog) won't really apply.

**Example**:
- When a developer (or admin) commits some change to your source control (GitHub, Bitbucket, GitLab), your CI/CD tool will automatically take this change and deploy it to your specified Salesforce environment.

The easiest way to configure this is such that when any change is comitted to your source control, you take the entire repository and deploy it to a specified environment.
 - (See my post on source control strategies for the nuances of the term 'specified environment'.)

What does this mean? It means your source control repository is truly your **source of truth**. Any change made there is immediately deployed and replicated to Salesforce. 
- If somebody edits a field description in your source control, it is the exact same as editing that field directly in Salesforce.

## When Automated Full Deployments Don't Work
Full deployments work well, until they don't. After your org reaches a certain size, you begin to notice these things:

- It takes a **LONG** time to deploy a small change (because a small change still results in your **WHOLE** org deploying.)
  
- Since you're deploying all of your apex code, you also need to run enough unit tests to get 75% global code coverage. This takes a long time.
  
- Deploying *everything* means there is a higher chance you hit a piece of metadata that causes the deployment to fail for one reason or another.

All of these issues can be fixed with the magic of a **partial deployment**.

## Automated Partial Deployments
Before I explain what a partial deployment is, I will first explain what a **tag** is in Git.

## What is a Tag in Git
Source control stores effectively the entire history of a software project. But like history in real life, there are points in time that we care about more than others. 

Example:
- If you release your code to production 1 time per month, the state of the repo at the time of release is notable.

You can mark these "notable" states in Git by using a **tag**. Git will allow you to easily revert to, or compare against a tag. So if you wanted to revert back to your previous production release, it is as simple as reverting back to that tag.

## Automated Partial Deployments (Cont)
So what is a "partial deployment"? It is pretty simple:

- **Full Deployment:** Deploy the *entire* Salesforce metadata without any nuance.
- **Partial Deployment:** Deploy *only* the metadata that has changed since the last deployment.

Sounds "easier said than done", right? It actually isn't too difficult, especially when you understand the concept of a tag.

When you deploy work to production, tag it in Git. You've now got a dedicated marker indicating exactly what code exists in production. 

You can list the files that have changed since a specified tag with the following git command:

```
git diff --name-only <tag-name>
```
Then turn it into a command delimited list by adding the following:

```
git diff --name-only <tag-name> | tr '\n' ','
```

You can now feed this list of files into the sfdx deploy command:

```
sfdx force:source:deploy -p $(git diff --name-only <tag-name> | tr '\n' ',')
```

[Credit](https://salesforce.stackexchange.com/a/317817)

This will deploy ONLY that metadata that has changed since the tag that you specify in the command. (Note, this uses SFDX, but the same can be done with the metadata API.)

This has some notable benefits:
- Substantially faster
- Less chance of touching "bad" metadata that could fail the deployment

Note, we didn't address the issue with unit tests. With this approach as-is, you still need to run every unit test in order to guarantee sufficient coverage. Luckily, I have a solution to that to (hint: partial testing), see [testing](../testing).