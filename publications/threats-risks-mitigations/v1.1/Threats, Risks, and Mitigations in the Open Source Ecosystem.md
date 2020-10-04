## Threats, Risks, and Mitigations in the Open Source Ecosystem

*[Michael Scovetta](mailto:michael.scovetta@microsoft.com), Microsoft*

*in collaboration with the Open Source Security Coalition*

The purpose of this document is to build a mutual understanding of the high-level threats, security risks, and potential mitigations associated with the open source ecosystem. There is a natural overlap between these threats and risks, and those that affect the more general software development process. The primary intended audience consists of members of the [Open Source Security Coalition](https://securitylab.github.com/) (the “Coalition”, herein) and similar organizations interested in promoting and advancing improvements to the security of the open source ecosystem, but should not be considered as a product roadmap or promised set of features. It should also be noted that this document focuses exclusively on security risk and does not include risks related to intellectual property (i.e. patents, copyright, licensing, contracts).

# Introduction

Open source software is an essential part of modern software development, and of practically all technology solutions. Adoption of open source software has grown over the past two decades, powering everything from tiny Internet of Things devices to the most advanced supercomputers in the world. Over the last decade, the quantity of open source available through package management systems has grown from around 30,000 to well over two million today. This has led to enormous productivity gains, allowing software engineers to focus more on solving business problems and less on creating and re-creating the same building blocks needed in many situations.

Open source itself, however, is primarily created by volunteers, working on their own time on a project they are passionate about. They often receive no monetary compensation for their work other than satisfaction that their creation is useful to others, but their work product is routinely used to power for-profit businesses and other organizations. This can create discord between the producers and consumers.

> *"Open Source & I are going through a labor dispute right now. I really lost a lot of faith in open source when I noticed billion dollar corporations were using my software and not a single one ever bothered to donate even a few dollars to keep it going, but filed tickets." - Jordon Bedwell (@envygeeks)*

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

---

¹ CVEs cover both open source and proprietary software.

---

## Related Work

The following resources contain content that supplements the information to this document.

### Industry Reports

- [The State of Open Source Security Vulnerabilities](https://www.whitesourcesoftware.com/open-source-vulnerability-management-report/) (2020, WhiteSource)

- [The State of Open Source Security ](https://snyk.io/opensourcesecurity-2019/)(2019, Snyk)

- [Open Source Security and Risk Analysis (OSSRA)](https://www.synopsys.com/software-integrity/resources/analyst-reports/2019-open-source-security-risk-analysis.html) (2019, Synopsys)

- [2019 Software Supply Chain Report](https://www.sonatype.com/en-us/software-supply-chain-2019) (2019, Sonatype)

### Guidelines, White Papers, and Standards

- [Fundamental Practices for Secure Software Development](https://safecode.org/wp-content/uploads/2018/03/SAFECode_Fundamental_Practices_for_Secure_Software_Development_March_2018.pdf) (SAFECode)

- [Managing Security Risks Inherent in the Use of Third-party Components](https://safecode.org/wp-content/uploads/2017/05/SAFECode_TPC_Whitepaper.pdf) (SAFECode)

- [Microsoft Security Development Lifecycle](https://www.microsoft.com/en-us/securityengineering/sdl/)

- [NIST SP 800-160](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-160v1.pdf) (Systems Security Engineering)

- [NIST SP 800-37](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-37r2.pdf) (Risk Management Framework for Information Systems and Organizations)

- [OWASP Packman](https://github.com/OWASP/packman) (Documentation/tracking of security controls of popular package management systems)

- [OWASP Software Component Verification Standard](https://owasp.org/www-project-software-component-verification-standard/)

## Acknowledgments

Thank you to everyone who reviewed, commented, and provided content for this document; most especially to Guy Acosta, Bas Alberts, Chris Aniszczyk, Charles Brenner, Jennifer Fernick, John Gossman, Luigi Gubello, Chris Jeuell, Maya Kaczorowski, Radoslaw Karpowicz, Steve Lipner, Jason Keirstead, Dan Lorenc, Elie Saad, Andrew Trompler, and Kay Williams.

## Table of Contents

|                                                   |     |
| ------------------------------------------------- | ---:|
| Introduction                                      | 1   |
| Threats & Risks                                   | 5   |
| Ideation / Concept Phase                          | 5   |
| Local Development Phase                           | 8   |
| External Contributions Phase                      | 17  |
| Central Infrastructure Phase                      | 19  |
| Package Consumption Phase                         | 23  |
| Vulnerability Reporting & Security Response Phase | 34  |
| Cross-Cutting Activities                          | 38  |
| Conclusion                                        | 47  |
| Appendix                                          | 48  |

# Threats & Risks

To better frame what we’re going to be exploring, we’ll start with a simple diagram that describes the major parts of the open source ecosystem and how they often relate to one another.

![Diagram for analyzing threats and possible mitigations](img/Threat&Risks.png)

We will use the diagram above to frame our exploration of threats and possible mitigations, after which we’ll discuss some general, cross-cutting practices and recommendations.

## Ideation / Concept Phase

![Ideation, the first phase of the diagram](img/Ideation.png)

In this phase, there are few explicit threat actors; instead, there is the potential for “business”-level security flaws, biases that have security implications, and other high-level design problems that can have severe consequences if not identified and properly addressed.

These risks can be influenced by various aspects of a system:

- **Attack Surface.** As a system’s attack surface grows or becomes less well-defined, it becomes more susceptible to attack. For example, systems that attackers have physical possession of are often harder to secure against tampering or introspection. Systems that have many dependencies could be attacked using a defect in any of them. 

- **Technology Stack Risks.** Certain technologies are inherently more susceptible to attack than others; for example, most modern programming languages have been built to avoid the memory safety challenges that have [affected](https://www.zdnet.com/article/microsoft-70-percent-of-all-security-bugs-are-memory-safety-issues/) C and other “low-level” languages.

- **Security-Sensitive Functionality.** From a security perspective, certain tasks are simply more important than others. For example, it is more important that a library that performs authentication or authorization do so correctly than it is that a calendar widget properly account for leap years. Operating system kernels, cryptographic libraries, and cloud orchestration systems also fall into this category, as do many other components that depend on the context of the system they are being used with.

- **Unproven Technology.** New technologies often offer advantages, but practically no one “gets it right” on their first try. Whether it’s a new platform, programming language, framework, or library, placing too much reliance on the security of that new technology can be a mistake until it’s been proven out.

In addition, it’s important to realize that not all risks are technical in nature; systems that perform any of the following are more likely to be attacked than others:

- Systems that transfer money or other assets between accounts.

- Systems that provide assurance that certain events have taken place.

- Systems that protect access to highly valuable assets.

- Systems that claim to provide anonymity or pseudonymity.

To mitigate these risks, security practitioners often recommend [threat modeling](https://safecode.org/wp-content/uploads/2017/05/SAFECode_TM_Whitepaper.pdf) and a security-oriented design review (also known as an architectural risk analysis). There are many approaches to performing these, and though they are often as much “art” as “science,” there are some excellent examples that are publicly available:

- [Kubernetes Security Audit and Threat Model](https://github.com/kubernetes/community/tree/master/wg-security-audit)

- [OAuth 2.0 Threat Model and Security Considerations](https://tools.ietf.org/html/rfc6819)

The main challenges around threat modeling include:

- Most software engineers do not perform threat modeling or similar activities when creating open source software. This may be due to limited understanding of the value these tasks provide, or a desire to work on the core (software) parts of the project.

- A project’s core concepts often shift over time, especially at the early in the project’s development, which means this process should be repeated regularly.

- The maintainers may change over time; there may be no one available to describe how a certain part of the project works. 

- These tasks are often difficult or require specialized training or expertise that may not be available.

- There may also be a perception that these activities don’t provide much value to open source components, especially since the developer will not have the context of how the component will be ultimately used.

### Threat Modeling

Formally, threat modeling is a process by which potential threats are identified and rated for severity, and possible mitigations are discussed. Less formally, threat modeling happens when you think about how the system you’re building could be broken, and consider what you can do to prevent that from happening.

It is important to stress that threat modeling is a <u>process</u>, not a tool. While tools can help the process be more efficient (e.g., by providing visualization, tracking changes over time, or identifying changes to software that would be more likely affect its threat model), tools by themselves cannot currently take the place of humans reasoning about how other humans would attack a system.

Threat modeling can be most effective when multiple stakeholders can come together to look at a system from different angles: developers, architects, service engineers, designers, and end users, along with security specialists. The discussion can be as simple as walking through how the system is used, how it is *supposed* to work and comparing that to how it *actually* works. Security specialists will often ask questions to get a better understanding of the security controls in place, and very often, everyone will leave with a better understanding of the risks that affect the system.

There are tools, like [OWASP Threat Dragon](https://owasp.org/www-project-threat-dragon) and [SD Elements](https://www.securitycompass.com/sdelements/), that bring structure to this process, and many good tutorials, including [Threat Modeling in 2019](https://www.youtube.com/watch?v=ZoxHIpzaZ6U) (RSA/Adam Shostack, 2019) and the SAFECode [Tactical Threat Modeling](https://safecode.org/safecodepublications/tactical-threat-modeling/) white paper.

We recommend considering the following projects to advance this area:

- High-quality training materials for conducting threat modeling and a security-oriented design review should be curated or created and targeted at high-risk projects.

- Templated threat models should be created for common (and representative) types of open source components and expanded over time with community involvement.

- Experienced security professionals should collaborate with critical, high-risk open source projects to create security reviews and associated threat models.

## Local Development Phase

<img src="img/LocalDevelopment.png" title="" alt="Local Development phase" width="385">

Local development of open source projects usually takes place on the maintainer’s personal infrastructure (workstations, local network, etc.), which, like any other infrastructure, can be vulnerable to attack. For example, [XcodeGhost](https://en.wikipedia.org/wiki/XcodeGhost) was a malicious distribution of Apple’s Xcode software, targeting the far left of the supply chain—the developer’s IDE and local build environment. Similar attacks have become more common, such as the [eslint-scope malware](https://nodesource.com/blog/a-high-level-post-mortem-of-the-eslint-scope-security-incident/) that attempted to exfiltrate the developer’s NPM access tokens during installation.

Threats that apply to this phase include:

- Attackers compromise a developer’s environment and use that access to compromise the software components themselves (e.g., making false commits, silently pushing content to a source code repository, modifying files, etc.).

- Attackers compromise a developer’s environment in order to exfiltrate commits not made public yet in order to be a step ahead in the competition.

- Attackers compromise a maintainer’s machine, network, or communications tools to intercept researcher-submitted bug reports of zero-day vulnerabilities.

- Developers leave “debug” functionality that bypasses security controls.

- Developers copy/paste source code from Stack Overflow or similar sources without considering whether that code contains security defects.

- Maintainers create software with code-level security defects (vulnerabilities).

- Maintainers accidentally check secrets into source code or publish them in packages, which attackers find and exploit.

### Technical Architecture

The choice of a technical architecture can have a significant impact on the overall security of a system and the investment needed to keep that system secure in the future. A good (secure) technical architecture can mitigate security risk systemically, while poor technical architectures can amplify it. Our goal in choosing a secure technical architecture is to reduce the likelihood that the system will contain exploitable security vulnerabilities in the future.

As an example, consider security vulnerabilities that result from inconsistencies created when a concept is translated into a program’s source code. For example, a developer may assume that a person’s age will never be greater than 120, so they assign an 8-bit field to store it; an attacker submits a record with an age of 100 million, and the program state overflows, resulting in corruption and potential execution of code supplied by the attacker.

Tactically, bugs like this can often be found through static analysis or fuzzing, but one of the best ways to address entire classes of vulnerabilities is to provide software developers programming languages and platforms that make it easier to write secure code. For example, many modern “managed” programming languages avoid manual memory management; in the above example, an attacker supplying a number greater than the allowed size would result in a runtime error rather than memory corruption.

Memory management issues obviously aren’t the only kind of security flaws, however, and many higher-level constructs cannot be feasibly handled at the programming language level, such as a properly-implemented authorization mechanism or the implementation of a new cryptographic protocol. For many of these cases, having well-vetted, commonly used libraries that implement these constructs has advantages over each package author implementing the construct independently. (Indeed, this is one of the primary benefits of using open source software in the first place!)

The [MITRE Common Weakness Enumeration](https://cwe.mitre.org/data/slices/699.html) breakdown is a good resource for understanding the range of issues that can affect a system.
