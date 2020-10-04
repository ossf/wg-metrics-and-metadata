### Threats, Risks, and Mitigations in the Open Source Ecosystem

*[Michael Scovetta](mailto:michael.scovetta@microsoft.com), Microsoft*

*in collaboration with the Open Source Security Coalition*

The purpose of this document is to build a mutual understanding of the high-level threats, security risks, and potential mitigations associated with the open source ecosystem. There is a natural overlap between these threats and risks, and those that affect the more general software development process. The primary intended audience consists of members of the [Open Source Security Coalition](https://securitylab.github.com/) (the “Coalition”, herein) and similar organizations interested in promoting and advancing improvements to the security of the open source ecosystem, but should not be considered as a product roadmap or promised set of features. It should also be noted that this document focuses exclusively on security risk and does not include risks related to intellectual property (i.e. patents, copyright, licensing, contracts).

# Introduction

Open source software is an essential part of modern software development, and of practically all technology solutions. Adoption of open source software has grown over the past two decades, powering everything from tiny Internet of Things devices to the most advanced supercomputers in the world. Over the last decade, the quantity of open source available through package management systems has grown from around 30,000 to well over two million today. This has led to enormous productivity gains, allowing software engineers to focus more on solving business problems and less on creating and re-creating the same building blocks needed in many situations.

Open source itself, however, is primarily created by volunteers, working on their own time on a project they are passionate about. They often receive no monetary compensation for their work other than satisfaction that their creation is useful to others, but their work product is routinely used to power for-profit businesses and other organizations. This can create discord between the producers and consumers.

 

*"Open Source & I are going through a labor dispute right now. I really lost a lot of faith in open source when I noticed billion dollar corporations were using my software and not a single one ever bothered to donate even a few dollars to keep it going, but filed tickets."*

 

Open source software brings great capability, but with it comes some amount of risk. According to the [2019 State of Software Supply Chain](https://www.sonatype.com/en-us/software-supply-chain-2019) report released by Sonatype:

- The number of days between vulnerability disclosure and exploit creation has shrunk from 45 to 3.

- Over half of JavaScript components contain at least one known security vulnerability.

- JavaScript packages are downloaded over 10 billion times per week (via NPM), which averages to more than 53,000 per developer per year.

More generally, security vulnerabilities continue to grow in number, with over 17,000 CVEs¹ published in 2019 and nearly 9,000 published in the first half of 2020. Of those 26,000, over 4,000, or fifteen percent, were rated [critical](https://nvd.nist.gov/vuln-metrics/cvss).

These are scary numbers, but they do not tell the whole story. The purpose of this document is not to promote fear, but to offer solutions and align disparate efforts toward a common goal. To move forward, we must first build a mutual understanding of the threats and risks associated with the open source ecosystem. Where applicable, we offer suggestions on ways to address the threats and mitigate the risks, but we do not presume any of this to be exhaustive. At best, we hope to start a conversation about the best way to proceed.

A summary of recommendations can be found in the [Appendix]().

## Version History

|     |                                   |           |
|:---:|:--------------------------------- |:---------:|
| 0.1 | Initial draft                     | 4/16/2020 |
| 0.2 | Final draft                       | 5/5/2020  |
| 1.0 | Initial release                   | 5/13/2020 |
| 1.1 | Updates based on initial feedback | 6/16/2020 |
