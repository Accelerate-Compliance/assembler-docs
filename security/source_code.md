# Source code security
## Approach

Assembler follows a '[convention over configuration](https://en.wikipedia.org/wiki/Convention_over_configuration)' approach when writing software. We utilise as much pre-existing technology as possible in order to ensure our software is highly secure. 

Our automated build process includes a variety of static analysis tools (such as [Brakeman](https://brakemanscanner.org/)) in order to catch potential software vulnerabilities early in the development cycle. A variety of best-practice safeguards are enforced for all platform features, such as [enforced endpoint policies](https://github.com/varvet/pundit#ensuring-policies-and-scopes-are-used).

## Dependency security

Integrated in to our build process are the following tools which address vulnerabilities in dependencies of the platform. When discovered, vulnerabilities are assessed and new Assembler versions are released (or automatically applied). For critical vulnerabilities, the period from notification to released patches is typically less than 6 hours.

| **Tool**                                                                              | **Description**                                                             |
|---------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
| [rubysec/bundler-audit](https://github.com/rubysec/bundler-audit)                     | Dependency vulnerability management for ruby applications.                  |
| [djfdyuruiry/improved-yarn-audit](https://github.com/djfdyuruiry/improved-yarn-audit) | Dependency vulnerability management for client-side application Javascript. |
| [jeemok/better-npm-audit](https://github.com/jeemok/better-npm-audit)                 | Dependency vulnerability management for NodeJS libraries.                   |

## Penetration testing

The Assembler platform is regularly tested using modern penetration testing tooling. Most notably the [Metasploit Project](https://en.wikipedia.org/wiki/Metasploit_Project). For an up to date report, please contact the Assembler team. 

Completed Assembler applications can also have penetration tests completed prior to going live.