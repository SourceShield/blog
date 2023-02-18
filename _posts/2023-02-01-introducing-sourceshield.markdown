---
layout: post
title:  "Introducing SourceShield"
date:   2023-02-01 10:00:00 -0500
categories: update
---

Today, I am releasing a limited preview of a new software supply chain security platform I've been working on called [SourceShield](https://sourceshield.io).

Before I dive into the features that I think distinguish SourceShield, I wanted to share a bit about _why_ I decided to work on an application in this growing, and admittedly-crowded, problem space.

Software supply chain security certainly isn't a new concept, but a string of high-profile incidents over the past few years (e.g., Solarwinds, Log4Shell) have brought it to the forefront of security concerns. As security vendors have rushed to respond, the focus has primarily been on dependency security. While many of these incidents were indeed caused by compromises in upstream dependencies, I don't believe this tells the whole story...

**Supply chain security is more than just software dependencies and CVE scanning.**

If you squint at the path that most software takes as it gets from a developer's laptop into production, the true scale of the supply chain security problem comes into focus. It's not _just_ dependencies that are putting this software at risk, but the hundreds of other moving pieces and trust relationships that are being defined in SCM tools, pipelines, build systems, cloud environments, and more.

A few months ago, I chatted with a company that had adopted a pretty rigorous stance on third-party software dependencies. Dependencies were locked to known versions, mirrored to an internal package management system, and scanned regularly for CVEs. These are all really great things to do! But as we dug into the architecture of their build system, it became apparent that the same rigor was not applied there. Some teams were leveraging GitHub Actions that used build environments from questionable sources. They did not have a process in place for systematically auditing GitHub users who had write access to those build environments. Built images destined for production were pushed into an ECR repository in a non-production AWS account that had more lax security standards.

Across the industry, security practices have not kept pace with the proliferation of SCM features and configurations, build systems, pipeline tooling, and other means of getting software into production. And to be fair, that is a tall ask! GitHub alone has close to fifty different settings and features that can be used to configure a repository, and that number is only increasing as they expand their product offerings.

I'm building SourceShield to address these problems. In many cases, I think organizations are simply not aware of the supply chain risks that exist beyond dependency monitoring. In others, they are overwhelmed by the myriad of complex configurations and hidden risks across the supply chain.

**A different kind of security application.**

SourceShield addresses these risks by surfacing them where developers are already operating: in code, SCM, pipelines, and build systems. Rather than using dashboards, charts and graphs, pages full of CVEs, and proprietary SQL-like languages, SourceShield integrates directly into GitHub, is fully configured via code, and its interface is familiar to developers: pull request checks and repository issues.

Initially, SourceShield will prioritize three areas:

1. **Surfacing contextual security signals.** SourceShield runs on every pull request, gathering context about the proposed change. Who is making the change? Does the author have experience in this part of the codebase? What files are being modified? If dependencies are being added, what are their reputations? Is a dependency being upgraded (likely good) or downgraded (maybe not)? Are there any anomalies or suspicious patterns in the changes? Are commits properly signed? Is a pipeline configuration file being modified? There are hundreds of such signals, many of which are not immediately available to the pull request reviewer, that SourceShield can discover.

2. **Enforcing SCM, pipeline, and build best practices.** As the complexity of the supply chain continues to increase, it becomes more challenging to adhere to best practices. Are GitHub Action steps locked to SHAs and not tags? Are they at risk of parameter injection? Are third-party applications granted write access to a repository? Is branch protection properly configured? Are image registries configured to prevent overwriting existing image tags? SourceShield can detect these risks and suggest changes inline.

3. **Guiding organizations to adopt a more secure supply chain posture.** SourceShield adopts a flexible approach, allowing organizations to define how these signals should be interpreted and best practices enforced. Perhaps pull requests that downgrade dependencies should simply require an additional review from a member of the security team, while pull requests that originate from untrusted bots are blocked. Organizations can selectively enable the policies, and enforcement configuration, that makes sense for them.

**Try out SourceShield!**

You can [try out a limited preview of SourceShield today](https://docs.sourceshield.io/getting_started.html), free for public repositories (no need to contact a sales team!).

If you'd be open to chatting about how SourceShield can help your organization, please let me know [here](https://docs.google.com/forms/d/e/1FAIpQLSeHOxckS_aCSu5rzsYHVTrEEjInfNcTAngzZF2BwDAozb7RpQ/viewform). You can also reach me at `matt@sourceshield.io`.