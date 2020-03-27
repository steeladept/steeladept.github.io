---
title:  "Enterprise Tooling - Development"
published: true
permalink: enttoolsdev.html
summary: "One thing about IT tools is it isn't only about your personal tools. IT is first, and foremost, about enabling others to do their jobs safely, efficiently, and effectively. To do that, requires a host of Enterprise Tools to ensure security, data integrity, and a functional environment for the business applications to function in.This is another tooling post that reviews some of the Enterprise solutions I have used/implemented and why I liked them."
tags: [news, tools]
---

![alt text:  Development Banner][DevBanner]

## Back to the blog ##

It has been some time since I added a blog entry - since before my last contract started, as a matter of fact. Well it was a busy time, so I do hope you forgive me, but now as it is wound down, I am posting this with new revelations in a few new tool categories I have learned a lot more about recently.

## Development Software ##

This is not the individual IDE but the team software used to aggregate code, build code, store code and builds, etc. Basically it is everything outside the individual's development computer that is needed to turn code into useful software. It includes repositories, CI/CD solutions, and even build tooling, if appropriate. The tooling here is quite appropriately the realm of DevOps, and often associated quite specifically with the DevOps approach to release management.

### Favorite Repository Manager ###

I have not really messed with repositories much and those I have messed with have been exclusively GIT based. The world has pretty much chosen GIT as a standard for code, and other repositories are often just rebranded versions of GIT or some off brand that fill niche needs. That said, there are a number of other repository types (such as docker repositories, application repositories, etc.) and that give rise to Repository Managers.

There are a number of Repository Managers that are worth mentioning and considering, but the two biggest kids on the block are Artifactory and Sonatype Nexus. Unfortunately, I have never had the pleasure of using Artifactory, primarily because the repositories I am interested in are only available in the paid version. However, I have used Sonatype Nexus and found it quite easy to setup and use. Moreover it proxied my external repositories and made creating and accessing a variety of repositories simple and secure.

Rumor has it (mostly from the overwhelming number of blogs on the internet stating so), that Artifactory is slightly more versatile and slightly easier to use while also being slightly faster. I can not personally vouch for that, but given the number of corroborating posts, I would say Artifactory wins hands down in the paid version comparison. As is, my favorite is Sonatype Nexus Free Version because it fit all my needs and was VERY easy to use.

### Continuous Integration Solutions ###

This is a tough one because there are SO MANY variations. They all generally work the same, however, and with very little functional difference. Must haves in this category are the ability to check-out code from the repository, test it via a linter (and maybe a few other static code testers), run build scripts against the code, perform unit tests, move the generated code into an environment, and run integration tests against that. They also provide pipelines that show the testing and results as well as the phase it is in and allows for repeated testing (and/or parallel testing) as it moves through various environments.

Many tools fit this bill, but one of the most ubiquitous ones is also one of the easier ones to setup and use - well, use anyway. Jenkins is the gold standard against which other CI tools are usually measured against, and with good reason. It is reasonably easy to setup (for smaller deployments), scales nearly infinitely, is Open-Sourced (for those who prefer that), and has all the core capabilities of any CI solution. Moreover, it has a plugin architecture to allow it to work with future tools or those too niche for the programmers to build in allowing it to work in nearly any environment.

While other tools are known to work considerably better in certain areas than Jenkins; it is my favorite simply because it is easy to learn, easy to scale and works well everywhere. Other tools are worth looking into if you have specific needs that it fills better, and I don't knock them for that, but Jenkins is the well-rounded, "Git'er Done" solution that never fails to meet the need in my experience.

### Continuous Delivery/Deployment Solutions ###

Continuous Delivery and Continuous Deployment are similar concepts that extend from the Continuous Integration concepts. It often uses the same toolchain to deliver/deploy the completed solution to a production environment ready for deployment.

The key focus here (and key differentiator) is who and when a set of code passing all tests gets deployed. In Continuous Delivery, the code passing all tests is delivered to the deployment team that then schedules a production deployment, most typically during a scheduled outage (though that is not necessary). In contrast, Continuous Deployment is automated to deploy the code to the production environment automatically on a scheduled basis - either immediately after testing or at a set time after testing is completed.

Regardless of which solution you choose, there is no solution out there that I have seen that is better than Octopus Deploy. It can do the delivery or deployment on whatever schedule makes the most sense for your business and is as easy to setup as any CI chain (okay, it is actually quite a bit easier than the CI Chain, more like a step in the CI chain). While most CI solutions are quite capable of doing the CD side of the CI/CD pipeline as well, it is almost never as easy to setup as it is with Octopus Deploy. The real question here is: Is it worth the extra money for another tool? For that answer, only you can decide.

### Configuration Management Tool ###

Configuration Management is a hot topic lately. Tools that allow you to deploy and configure servers based on standardized specifications so developers can have a consistent platform to deploy their software on. Typical uses are for database servers, web servers, application servers, and file servers. The key for this is that the same type of server is setup the same way for every application, no matter what; that way, when their is a variation - a change to the settings, for example - that can be captured as part of the application setup and differentiated from any server settings that are defined in the baseline setup. It also helps for continuity and for patching and patch control.

Configuration Management tools cover a wide range of tools and capabilities, from system specific tools such as Powershell DSC to general purpose monolithic tools like Chef. They come in a variety of formats and can work through either a push or a pull configuration. Pull configurations require an agent be installed, but are easier to support a diversified IT department as seen in larger organizations. Push configurations are usually easier to setup and do not usually require an agent (though may work better with it if available), and allow for better overall mapping of application to server resources.

Generally speaking, I tend to prefer using agents as they provide many more features and capabilities - capabilities that are very useful and worth the minor performance penalty they offer. However, one push technology focused option in this field, Ansible, is an exception to that preference. The additional features of a CM agent are often duplicated by other tools, either VMware or Jenkins in my case, depending on the feature you are looking for. The key component that makes Ansible useful between these two tools is a core feature that does not require any agent and makes Ansible my favorite overall CM tool.

While larger, more complicated tools such as SCCM, Puppet, or Chef may make sense in larger organizations, most just need a start, and Ansible has a low barrier to entry there. For more advanced capabilities, Ansible has Ansible-Galaxy where others have already configured playbooks you can download to meet most people's needs. There are a few other areas that other solutions may make better sense, but overall it is difficult to deny Ansible as a solid, if not best, choice for most use cases.

---
[DevBanner]:  ../images/Banners/devtoolchain.png "Development Banner"