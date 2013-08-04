---
layout: post
title: "Call for Survey Participants"
alias: /2008/01/call-for-survey-participants.html
categories:
---
I'm looking to do a survey on software quality using source code duplication, cyclmatic complexity and npath complexity as the measure.

The general idea is to take open and closed source projects and measure the "quality". The results will then be analysed to determine:

* How open/closed source compare;
* How individual projects compare to the "norm"; and
* What, if any, discernible factors have influenced the quality of the code.

Once the data has been compiled and analysed, I will make the results available to the participants and to the general public. The identity of participants, projects, resources and people involved will be completely confidential -- indeed even I don't actually need to know the names of projects nor will I have access to or need access to the source code -- and will not be disclosed to third-parties.

At the request of the study participants, I will be more than happy to present the findings -- in person if possible -- and make any recommendations that may arise. I will also make myself available to perform a repeat survey in 12-18 months time should this be so desired.

For largely pragmatic reasons, there are several requirements for participation:

* The project MUST be in a subversion repository;
* The source code MUST be Java;
* The project SHOULD NOT be using Simain or CPD (or for that matter any other duplicate code checking tool);
* The project SHOULD NOT be using Complexian (or for that matter any other complexity checking tool); and
* An environment that can execute Java and Subversion command-line tools.

Participation couldn't be simpler. After you [contact me](mailto:haruki_zaemon@mac.com), I'll send you a package containing some processing scripts. To run, simply checkout the HEAD of your project's source tree into a directory using subversion and run one of the batch scripts contained in the package. For Windows use `bin\process.bat src_dir cvs_file` For *nix, use `bin/process src_dir cvs_file`. The scripts will process each revision starting from the currently checked out revision to the first revision.

This processing may take considerable time so if you need to interrupt the process at any point you may do so in the usual manner -- eg. `ctrl+c`. You can then re-start the processing at a later date and it will pick-up where it left off.

Once processing has finished, you can [email me](mailto:haruki_zaemon@mac.com) the CSV file.

If you think you have a project that fits the profile and would be willing to share some processing time, your participation would be greatly appreciated.
