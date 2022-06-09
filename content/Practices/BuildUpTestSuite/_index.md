---
title: Build up your test suite 
description: How your current processes can inform your team on important tests to create
weight: 1
---

One of the important parts of continuous delivery is having a dependable test suite. Without this trunk-based development will become challenging and continuous integration will be impossible. 

A good testing suite can't be built overnight. Begin writing automated tests  to ensure any new functionality works as intended. Your goal should be to always leave your code and your test suite in a better place whenever you make a change. Automated testing will likely force you to refactor to a cleaner, easier to test architecture. 

A team that's new to automated testing will likely still produce bugs, either found through manual testing or in production! This is okay, but it's important to use these bugs to help build up your test suite as you move towards continuous integration. When your team finds a bug the first thing they should do is write a failing automated for it. This confirms the behavior and sets the requirement for what you need to do to fix the bug. Once fixed you have a test that you know provides value because the situation has happened at least once before. 

## Examples

### Smoke tests with package manager
I was working on a project that released to the software repoistory PyPi. We practiced continuous delivery on the project and were confident in our automated tests suite. As long as the pipeline was green we felt comfortable delivering. 

Despite our automated tests we did run into an issue that wasn't caught by our pipeline. We changed how our package was delivered to PyPi and unknowingly broke the new release. It was important that we could deliver on demand with confidence, so we wanted to make sure we could get feedback to ensure our releases worked in the future. 

We decided we needed a smoke test to ensure our delivery went well. We took a sample project that installed our package from PyPi and used the package in it's test sutie. Then our CI server ran the tests from thesample project to ensure the new version is downloads correctly and the release was working in our base cases. We used our error we had to improve our release process for the future.  

The project is called approvaltests - check it out [here](https://github.com/approvals/ApprovalTests.Python)

### CI pipeline for CD manifesto 

We recently had a bug on the CD manfiesto site! A page was accidentally set to draft status that was intended to be published. This resulted in a 404 error when the user clicked on the link. No problem, another opportunity to build up our tests suite! 

We used a tool called [broken link checker](https://www.npmjs.com/package/broken-link-checker) to crawl through the site and ensure the pages don't result in errors. We also setup a github action to run our tests on pull requests and pushes to the remote.

We did run into an interesting dilemma. We weren't sure if we wanted to include extra links in this check. It may be useful to include external links if a link on the page goes bad, or if someone mistyped a link they meant to put in. Ultimately, after trying it out we decided not to include external links in the tests. We didn't want to add the noise of failing tests if it was out of the control of the content 
creator as there's many sites that block crawlers. You can hardcode your crawler to skip these but we didn't want the project to have to maintain the overhead of keeping a list of those sites. Depending on the project it might have been worth it but for Minimum-CD we felt it would be better to only scan internal links. You will likely run into a decision similar to this. Automated testing is sometimes a balance to avoid noise (false positives) but still ensuring tests are able to find breakages.


Remember one of the tenants to continuous delivery is "The pipeline decides the releasability of changes, its verdict is definitive" when you reach a situation when the pipeline didn't properly determine if something was ready for release improve your test suite so it does!