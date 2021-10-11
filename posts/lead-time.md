---
title: Lead Time with Salesforce
description: Lead time with Salesforce
date: 2021-10-11
tags:
  - Salesforce
layout: layouts/post.njk
---
## The most important metric in software engineering!
Ok, that isn't always true, but it is certainly top 5 in virtually all serious software companies. Of course, I am talking about **Lead Time**. 

What is **Lead Time**? Lead time is the amount of time it takes for a piece of work to go from an "idea in somebody's head" to generating value. 
- If you think of some great new feature today, and next week this feature is live in production generating value, then you lead time was 1 week. 

## Other types of lead time
Often **Lead Time** is too broad of a metric to really be valuable. Especially when a company becomes huge and it takes hundreds of processes to bring an idea to production. 

- Think about Toyota producing a new car. The lead time on this process would be massive. Across hundreds of teams, thousands of processes, regulations, external entities.

To get value out of this metric, it makes sense to make it more specific, measuring smaller subsets of the total process.

Examples:

- **Distribution Lead Time**: Continuing the Toyota example, Distribution Lead Time would be the time between "A car is completed and sitting in the factory" to "The car is on the dealership floor, ready to be sold." 
  
- **Deployment Lead Time**: Back to software, this would be the lead time between "Development on a piece of work is completed" and "Piece of work is live in production." 
  - **In Salesforce**, this would effectively be the time that it takes to run a change set, metadata API deployment, or other deployment to production.

## Where is the value?
So why is lead time a valuable metric? It is pretty simple: when you are able to measure it with a granular enough lense, it reveals bottlenecks.

Examples:
- When deploying to Salesforce, you need 75% code coverage. This means you need to run unit tests. If you're measuring your **deployment lead time** you might see a substantial portion of that time is from unit test execution. 
  
- If you need sign-offs and manual approvals throughout the deployment process, this can easily increase your lead time.

When you have visibility into bottlenecks, you can address them. I have some blog posts on addressing big lead time problem areas, especially with Salesforce. 