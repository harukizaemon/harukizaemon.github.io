---
layout: post
title: "When Corporates Embrace Open Source"
alias: /2004/10/when-corporates-embrace-open-source.html
categories:
---
It is common for organisations to justify the use of popular Open Source Frameworks on the basis that developers with these skills are easy to come by. In addition, because the source code is readily accessible, it's easy to make bug fixes and patches whenever needed. This is clearly justification enough that no analysis need be performed in order to ascertain if said framework _actually_ fits the technical requirements of the application.

The next step always seems to be to download the source code and check it into a local repository. Then, have a core group of developers maintain it internally. This team will be responsible for checking out the source code, building it and distributing it to all the other teams ensuring that changes are controlled and all teams keep up to date with the correct version.

After using the framework for a few months, it becomes obvious that the way the code was originally written is either: broken; wrong; or doesn't quite fit with _The Way We Do Projects Here &trade;_. This then requires massive changes to "simplify" the design and add enhancements wherever "necessary" - Like masking all those pesky exceptions that get thrown and instead returning `null`.

Of course now that so many changes have been made, and coupled with the requirement that all projects be uniform in quality, it becomes necessary to ensure that project teams cannot and will not use the version(s) available from the original project site but instead are forced to use the highly tailored internal version. In fact it's probably a good idea to make the framework a "black box". I mean, why would the non-core developers need or want access to the source code. The core team are providing a service after all and that is all that's important, so access to the internal repository must be on an as-needs basis.

And finally, after 12 months of development and hard-work, it is customary to allow the The Architect who made ALL the proprietary changes (to the supposedly open framework) to go on 4 weeks holiday, just prior to delivery to System Test, leaving the project team to fend for themselves so that when a bug is found, the only solution is to fork the code (again) and check it in to the project repository on the proviso that the changes make their way back into the core ASAP.
