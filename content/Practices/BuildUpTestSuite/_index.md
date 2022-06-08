---
title: Automate away 
description: How your current deployments can help inform what to automate
weight: 1
---

One of the hardest and most important part of continuous delivery is having a comprehsive and
valuable test suite. Without this trunk based development will become much more difficult 
and continuous integration will be impossible. 

A good testing suite this can't be built overnight. When your team is new to automated 
tests start writing tests to ensure the new functionality works as intended. This will help
slowly build up tests overtime. However, a team that is new to automated testing will likely
still produce some bugs, either found through manual testing or in production! 
This is okay, but it's important to use these bugs to build your test suite as you move towards continuous integration. 

When a bug is found on your team the first thing you should do is write a failing automated test which is failing 
because of said bug. This confirms the behavior, and sets the requirement for what you need to do to fix the bug. Perhaps 
More importantly, when done correctly can set a great example on how to catch bugs like this in the future. It 
can also show if you have mistakes currently living in your CI process that aren't able to catch errors. Make
sure you are testing 

## Examples

### Smoke tests with package manager
I was working on a project that released to the package manager pip. We practiced 
continuous delivery on the project and were confident in our automated tests suite. 
As long as the pipeline was green we felt comfortable delivering. 

Despite our automated tests we did run into an issue we didn't initally notice after pushing to production. The issue 
was that we edited some of the configurations of pip and accidentally broke the delivery of our new 
release. It was important to us that we could deliver our product on demand without having to do 
manual checks. 

We decided we needed a smoke test to ensure our delivery went well. We took a sample project 
that installed the package from pip and used it. After our release the
smoke tests run the sample project confirming the new version is downloaded. We used the mistake 
we had in pip to improve our release.  

The project is called approvaltests - check it out [here](https://github.com/approvals/ApprovalTests.Python)

### CI pipeline for CD manifesto 

We recently had a bug on the CD manfiesto site! A page was accidentally set to draft status
that was intended to be published. This resulted in a 404 error
when the user clicked on the link. No problem, another opportunity to build up our tests suite! 

I used a tool called [broken link checker](https://www.npmjs.com/package/broken-link-checker) to crawl 
through the site and ensure the pages don't result in errors. I also setup a github action to run 
our tests on pull requests and pushes to the remote. It was super simple to setup! 

I did run into an interesting dilemma. I wasn't sure if I wanted to include extra links in this check.
It may be useful to include external links if a link on the page goes bad, or if someone mistyped a link
they meant to put in. Ultimately, after trying it out I decided not to include external links in the tests. 
I didn't want to add the noise of failing tests if it was out of the control of the content 
creator as there's many sites that block crawlers. You can hardcode your crawler to skip these but
I didn't want the project to have to maintain the overhead of keeping a list 
of those sites. Depending on the project it might have been worth it but for Minimum-CD I figured it would be better to only scan 
internal links. You will likely run into similar situations. Automated testing is sometimes a balance to avoid 
noise (false positives) and make sure you are checking what you need too.

Remember one of the tenants to continuous delivery is "The pipeline decides the releasability of changes, its verdict is definitive"
when you reach a situation when the pipeline didn't accurately determine if something was ready for release improve 
your test suite so it does!