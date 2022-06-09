---
title: Building up your test suite 
description: How today's bug becomes tomorrows automated checks
weight: 1
---

An important piece of continuous delivery is having a dependable test suite. Without this trunk-based development is challenging and continuous integration is impossible. 

A comprehensive testing suite can't be built overnight. If your new to testing, start by writing automated tests to ensure any new functionality works as intended. Your goal is to leave your code and your test suite in a better place whenever you make a change. Automated testing will likely force you to refactor to a cleaner, easier to test architecture. 

A team that's new to automated testing will likely still produce bugs, either found through manual testing or in production. This happens, but it's important to use these bugs to help build up your test suite. When you find a bug the first thing you should do is write a failing automated test for it. This test should confirm the behavior and sets the requirement for what you need to do to fix the bug. Once fixed you have a test that you know provides value because the situation has happened at least once before. 

## Examples

### Smoke testing a software package
I was working on a project that released to the software repository PyPi. We practiced continuous delivery on the project and were confident in our automated test suite. As long as the pipeline was green we felt comfortable delivering. 

Despite our automated tests we did run into an issue that wasn't caught by our pipeline. We changed how our package was delivered to PyPi and unknowingly broke the new release. It was important that we could deliver on demand with confidence, so we wanted to make sure we could get feedback to ensure our releases worked in the future. 

We decided we needed a smoke test to ensure our delivery went well. We took a sample project that installed our package from PyPi and used the package in it's test suite. Then our CI server ran the tests from the sample project to ensure the new version is downloaded correctly and the release was working in our base cases. We used our error we had to improve our release process for the future.  

The project is called approvaltests - check it out [here](https://github.com/approvals/ApprovalTests.Python)

### CI pipeline for CD manifesto 

We recently had a bug on the CD manfiesto site! A page was accidentally set to be hidden that was intended to be published. This resulted in a 404 error when a user clicked on the link. No problem, a opportunity to build up our tests suite! 

We used a tool called [broken link checker](https://www.npmjs.com/package/broken-link-checker) to crawl through the site and ensure the pages don't result in errors. We setup a github action to run our tests on pull requests and pushes to the git remote. Find the action [here](https://github.com/Minimum-CD/cd-manifesto/tree/master/.github/workflows)

We did run into an interesting dilemma. We weren't sure if we wanted to include external links in this check. While it would be useful in some situations it would also produce false positives as many sites block crawlers. Ultimately, after trying it out we decided not to include external links in the tests. We didn't want to add the noise of failing tests if it was out of the control of the content creator. You can hard-code your crawler to skip these but we it would be overhead and anyone in the community who wanted to contribute would may have to know how to use to crawler.

Depending on the project it might have been worth it but for MinimumCD we felt it would be better to only scan internal links. You may run into a decision similar to this. Automated testing is sometimes a balance between false negatives and false positives. 

## Conclusion

Remember one of the tenants to continuous delivery is "The pipeline decides the releasability of changes, its verdict is definitive". When your pipeline is unable to determine if something is ready for release improve your test suite so it can!

This is just one small practice you can use to help build up your test suite. It's important to practice test-driven development, continuously refactor, and test behavior over implementation. We hope this helps your CD journey! 