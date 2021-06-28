# Network security
## External encryption in transit

Encryption in transit protects your data if communications are intercepted while data moves between applications hosted on Assembler and the outside world. This protection is achieved by encrypting the data before transmission; authenticating the endpoints; and decrypting and verifying the data on arrival.

By default, only one ingress is configured per application, allowing  TLS application tier connections. 

SSL certificates are issued and managed automatically via LetsEncrypt (although custom certificates can also be manually uploaded).

### Protocols:

| **Name** | **Allowed** |
|----------|-------------|
| TLS 1.3  | Yes         |
| TLS 1.2  | Yes         |
| TLS 1.1  | No          |
| TLS 1.0  | No          |
| SSL 3    | No          |
| SSL 2    | No          |

### Allowed Cipher Suites

TLS 1.3:

| **Cipher**                    |
|-------------------------------|
| TLS_AES_256_GCM_SHA384        |
| TLS_CHACHA20_POLY1305_SHA256  |
| TLS_AES_128_GCM_SHA256        |

TLS 1.2:

| TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256        |
|----------------------------------------------|
| TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384        |
| TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256  |

Applications have been tested against all known SSL vulnerabilities by the SSL Labs scan.

## Internal encryption in transit

Assembler deploys an [Istio](https://istio.io/latest/) service mesh for all internal networking throughout the cluster. This provides internal, end to end TSL encryption for all data within Assembler.

Client certificate authentication is also enabled for component communication within an Assembler Namespace for a specific application environment.

## Web Application Firewall

All production applications are protected at the ingress level with a Web Application Firewall. 

[SpiderLabs/ModSecurity](https://github.com/SpiderLabs/ModSecurity) is used to provide this protection:

> Libmodsecurity is one component of the ModSecurity v3 project. The library codebase serves as an interface to ModSecurity Connectors taking in web traffic and applying traditional ModSecurity processing. In general, it provides the capability to load/interpret rules written in the ModSecurity SecRules format and apply them to HTTP content provided by your application via Connectors.

The following ModSecurity rules are applied by the Web Application Firewall:

| **Rule name**                       | **Description**                                                                                                                                                                                                                                                                                                                                                                      |
|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| COMMON-EXCEPTIONS                   | Some other rules are quite prone to causing false positives in well established software, such as Apache callbacks or Google Analytics tracking cookie. This file offers rules that will allow the transactions to avoid triggering these false positives.                                                                                                                           |
| IP-REPUTATION                       | These rules deal with detecting traffic from IPs that have previously been involved with malicious activity, either on our local site or globally.                                                                                                                                                                                                                                   |
| DOS-PROTECTION                      | The rules in this file will attempt to detect some level 7 DoS (Denial of Service) attacks against your server.                                                                                                                                                                                                                                                                      |
| SCANNER-DETECTION                   | These rules are concentrated around detecting security tools and scanners.                                                                                                                                                                                                                                                                                                           |
| PROTOCOL-ENFORCEMENT                | The rules in this file center around detecting requests that either violate HTTP or represent a request that no modern browser would generate, for instance missing a user-agent.                                                                                                                                                                                                    |
| PROTOCOL-ATTACK                     | The rules in this file focus on specific attacks against the HTTP protocol itself such as HTTP Request Smuggling and Response Splitting.                                                                                                                                                                                                                                             |
| APPLICATION-ATTACK-LFI              | These rules attempt to detect when a user is trying to include a file that would be local to the webserver that they should not have access to. Exploiting this type of attack can lead to the web application or server being compromised.                                                                                                                                          |
| APPLICATION-ATTACK-RFI              | These rules attempt to detect when a user is trying to include a remote resource into the web application that will be executed. Exploiting this type of attack can lead to the web application or server being compromised.                                                                                                                                                         |
| APPLICATION-ATTACK-SQLI             | Within this configuration file we provide rules that protect against SQL injection attacks. SQLi attackers occur when an attacker passes crafted control characters to parameters to an area of the application that is expecting only data. The application will then pass the control characters to the database. This will end up changing the meaning of the expected SQL query. |
| APPLICATION-ATTACK-SESSION-FIXATION | These rules focus around providing protection against Session Fixation attacks.                                                                                                                                                                                                                                                                                                      |
| BLOCKING-EVALUATION                 | These rules provide the anomaly based blocking for a given request. If you are in anomaly detection mode this file must not be deleted.                                                                                                                                                                                                                                              |
| DATA-LEAKAGES                       | These rules provide protection against data leakages that may occur generically                                                                                                                                                                                                                                                                                                      |
| DATA-LEAKAGES-SQL                   | These rules provide protection against data leakages that may occur from backend SQL servers. Often these are indicative of SQL injection issues being present.                                                                                                                                                                                                                      |