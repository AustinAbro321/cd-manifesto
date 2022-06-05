---
title: Automate away 
description: How your current deployments can help inform what to automate/
weight: 1
---

One of the hardest and most important part of continuous delivery is having a comprehsive and
valuable test suite. Without this trunk based development will become much more difficult 
and continuous integration will be impossible. 

A good testing suite this can't be built overnight. When your team is new to automated 
tests start writing tests to ensure the new functionality works as intended. This will help
slowly build up tests overtime. However, as the team is new to testing they will likely
still have bugs that occur, either found through manual testing or in production! 
It's important to use these bugs to build your test suite as you move towards continuous integration. 



# Examples

## Smoke tests with package manager
I was working on a project that released to the package manager pip. We practiced 
continuous delivery on the project and were confident in our automated tests suite. 
As long as the pipeline was green we felt comfortable delivering. 

Despite our automated tests we did run into an issue during one of our deliveries. The issue 
was that we edited some of the configurations of pip and accidentally broke the delivery of our new 
release. It was important to us that we could deliver on demand to our environments without having to do 
manual checks. We decided we needed a smoke test to ensure our delivery went well. We took a sample project 
for our product that installed the package from pip and used it. After our release the
smoke tests run the sample project confirming the new version is downloaded. We used the mistake 
we had in pip to improve our release.  

The project is called approvaltests - check it out [here](https://github.com/approvals/ApprovalTests.Python)

## CI pipeline for CD manifesto 

We recently had a bug on the CD manfiesto site! A page was accidentally set to draft status
that was intended to be published. This resulted in a 404 error
when the user clicked on the link. No problem, another opportunity to build up our tests suite! 

I used a tool called [broken link checker](https://www.npmjs.com/package/broken-link-checker) to crawl 
through the site and ensure the pages don't result in errors. I also setup a github action to run 
our tests on pull requests and pushes to the remote. It was super simple to setup! Check it 

Remember to reach continuous delivery we need to be able to wholly trust the pipeline 
on if we are able to deliver or not.  
