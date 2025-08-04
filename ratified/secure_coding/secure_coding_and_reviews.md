---
seo:
  title: HPE GreenLake Development Standard for Secure Coding | HPE GreenLake Cloud Platform
toc:
  enable: true
---

# HPE GreenLake Development Standard for Secure Coding

Following secure coding standards can help to protect against vulnerabilities, loss of data, or attacks. There are multiple factors that can help secure code weaknesses from becoming an actual exploitable vulnerability. Such factors could include firewalls, access controls, surrounding code context, and motivation.

This document describes the standards that cover many common cases that will be encountered by engineers working on the HPE GreenLake Cloud Platform. References to HPE documents that cover additional cases are included at the end. Developers MUST follow the requirements for secure coding standards described in this document.

This document serves as both a set of implementation requirements for developers and a guide for code reviewers. Each major section has a *Requirements* subsection, which outlines the explicit policies that must be followed, this may be followed by a *Considerations* subsection, which provides additional useful context and guidance around the topic for developers and code reviewers.

While examples in this document may be in a particular programming language such as Go or Python, except where explicitly noted otherwise, all of the principles and policies in this document apply regardless of the programming language in use. The remainder of this introduction provides an overview of the requirements. The details are provided in the following sections.

## Requirements

### Requirements for Implementation Language

Where possible, new software components should be implemented in [a memory safe language](https://www.cisa.gov/sites/default/files/2023-12/The-Case-for-Memory-Safe-Roadmaps-508c.pdf). The following languages are examples of languages which are memory safe: Go, Java, Python, Rust, C#, Swift, Scala and Kotlin. The following are examples of languages known not to be memory safe, avoid using them where possible: C, C++ and Objective C.

This does not apply to legacy components or open source components implemented in non-memory safe languages where any updates or changes to code will likely need to be in the implementation language already in use. It also may not be possible to use a memory-safe language if you are working on code intended for browser-only execution.

When using memory safe languages do not use constructions that break the memory-safe guarantees, such as the unsafe keyword in C# and Rust.

### Requirements for Software Development Lifecycle

If a code change includes any code not developed by HPE (such as open-source software (OSS) libraries) then that non-HPE code MUST follow the requirements described in the [Third Party and Open-Source Code](#third-party-and-open-source-components) section of this document.

Before or during the process of building a software artifact, the source code used to build that artifact MUST be scanned for potential violations of this secure coding standard, following the requirements described in the [Software Development Lifecycle](#software-development-lifecycle) section of this document.

### Requirements for Input Validation

All HPE GreenLake Source Code accepting user input MUST ensure that any input data has been appropriately validated prior to being processed. This includes:

- Implementing, as appropriate, the requirements identified in the [Data Validation](#data-validation) section of this document.
- For code that performs SQL database queries, implementing, as appropriate, the requirements identified in the [SQL Injection](#sql-injection) section of this document.
- For code taking a file path as an input, implementing, as appropriate, the requirements identified in the [Directory Traversal](#directory-traversal) section of this document.
- For code taking input data in XML format and parsing that data, implementing, as appropriate, the requirements identified in [XML External Entities](#xml-external-entities-xxe) section of the this document.
- For code that is accepting user input as part of the command used to start the process, implementing as appropriate the requirements identified in the [Data Validation](#data-validation) section of this document.

When looping, boundary checks, input validation MUST be in place. For detailed guidance requirements refer to the [Memory Management](#memory-management) section of this document.

### Requirements for Authentication and Authorization

All HPE GreenLake Source Code interacting with a user or process MUST implement, as appropriate, the best practices detailed in the [Authentication](#authentication) section of this document.

HPE GreenLake Source Code that is performing an action on behalf of a user or process MUST verify that the user or process is authorized to perform the appropriate action by implementing, as appropriate, the best practices described in the [Missing Function Level Access Control](#missing-function-level-access-control) and [Insecure Direct Object Reference](#insecure-direct-object-reference) sections of this document.

If an API is developed that is exposed in any way to a public network then it MUST adhere to all requirements described in the [Secure APIs](#secure-apis) section of this document.

### Requirements for Handling Sensitive Data

If HPE GreenLake Source Code is responsible for capturing, processing, or storing Sensitive Data in any way, it MUST implement, as appropriate, the best practices described in the [Sensitive Data Exposure](#sensitive-data-exposure) section of this document.

In addition, if code specifically handles authentication credentials or private keys in any way, it MUST implement, as appropriate, the requirements identified in the [Access Tokens and Keys](#access-tokens-and-keys) section of this document.

### Requirements for Encryption and Cryptography

If HPE GreenLake Source Code performs encryption of data in any form (including, but not limited to encryption of data at rest and encryption of data in transit using TLS) then it MUST implement the requirements identified in the [Cryptographic Algorithms](#cryptographic-algorithms) section of this document.

If HPE GreenLake Source Code transmits data over a network interface in an encrypted form (in-transit encryption), the code MUST follow, as appropriate, the best practices described in the [Secure Transport Protocols](#secure-transport-protocols) section of the Secure Coding specification.

### Requirements for Web Security

If HPE GreenLake Source Code uses sessions to track requests across users, the code MUST implement, as appropriate, the best practices detailed in the [Session Management](#session-management) section this document.

If HPE GreenLake Source Code uses HTTP cookies specifically, the code MUST implement, as appropriate, the best practices detailed in the [Cookies](#cookies) and [Cross-Site Request Forgery](#cross-site-request-forgery-csrf) sections of the Secure Coding specification.

If HPE GreenLake Source Code is rendering HTML before or after it is loaded into a browser, then the code MUST ensure that all code generated is properly escaped to prevent cross-site scripting attacks and implement as appropriate the best practices in the [Cross-Site Scripting](#cross-site-scripting-xss) section of this document.

### Requirements for Logging and Error Handling

If HPE GreenLake Source Code performs any action that has a reasonable likelihood of a subsequent audit, such as activities that may affect a customer’s infrastructure configuration or incur a cost to the customer, then it SHOULD be logged in accordance with the best practices around [Secure logging and error handling](#secure-logging-and-error-handling) section of this document.

If HPE GreenLake Source Code reports an error message to a user who is not a HPE staff member, directly or indirectly, the reporting of that error MUST implement, as appropriate, the best practices described in the [Secure logging and error handling](#secure-logging-and-error-handling) section of this document.

### Requirements for Secure Coding Practices

If HPE GreenLake Source Code relies on the generation of random numbers, for example, for cryptography, key generation or developing session identifiers, then the code MUST implement, as appropriate, the best practices identified in the [Secure Random Number and Key generation](#secure-random-number-and-key-generation) section of the [HPE PSO Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/infrastructure-security/Shared%20Documents/Forms/AllItems.aspx?id=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding%2FHPE%5FCommon%20Secure%20Design%20and%20Coding%20Ver%201%2E0%20Final%20Dec%206%202022%2Epdf&parent=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding).

## Secure APIs

All APIs (internal and external) must be secure as they could expose sensitive data and/or application logic.

### Requirements

Secure APIs by implementing the key items as follows:

- All APIs must require an authenticated user.
- Strong authentication must be provided in the form of an authentication token issued by the relevant authentication service.
- All APIs must accept the authentication token provided in an Authorization request header.
- APIs that do not receive an authentication token, or whose token is not valid, must fail with an HTTP [401 Unauthorized](https://www.rfc-editor.org/rfc/rfc9110.html#name-401-unauthorized) status.
- APIs that receive a valid token, but whose user lacks privileges needed for the requested operation, must fail with an HTTP [403 Forbidden](https://www.rfc-editor.org/rfc/rfc9110.html#name-403-forbidden) status.
- Requests that are not allowed for any user, such as changing a read-only field, must fail with an HTTP [403 Forbidden](https://www.rfc-editor.org/rfc/rfc9110.html#name-403-forbidden) status.
- Define an appropriate request size limit and reject requests exceeding the limit with HTTP response [413 Request Entity Too Large](https://www.rfc-editor.org/rfc/rfc9110.html#name-413-content-too-large) status. Recommended payload size is 10 Mb.
- Enforce rate limiting to improve the availability of APIs and reject requests exceeding the limit with HTTP response [429 Too Many Requests](https://www.rfc-editor.org/rfc/rfc6585#section-4) status.

## Injection Attacks

Injection attacks allow a malicious user to add or inject content and commands into an application in order to modify its behavior. Always treat content (e.g. request parameters, network data, files uploaded) that comes from the user or other devices as untrusted.

### SQL Injection

#### Requirements

- Make sure any database queries are done via prepared statements or applicable other means to avoid injection attacks.

#### Considerations

- Parameterize all SQL queries or, if dynamic statements must be used, sanitize user input (see Data Validation) and escape special characters before use as part of the statement. In Go, this can be accomplished using the HTMLEscapeString function within the HTML template package. Use SQL methods provided by the programming language or framework that parameterize the statements, so that the SQL parser can distinguish between code and data.
- Use database controls like LIMIT to prevent mass disclosure in case of a successful injection attack.
- Do not use string concatenation to build queries, i.e. *Don't* do this:

    ```java
     String firstName = request.getParameter("firstname");
     String lastName = request.getParameter("lastname");
     String query = “SELECT id, fname, lname FROM authors WHERE fname = " + firstName + " AND lname = " + lastName;

     Statement stmt = conn.createStatement();
     stmt.executeQuery(query);
     ```

- Instead, use parameterized queries:

   ```java
   String firstName = request.getParameter("firstname");
   String lastName = request.getParameter("lastname");
   String query = “SELECT id, fname, lname FROM authors WHERE fname = ? and lname = ?”;
   PreparedStatement pstmt = connection.prepareStatement(query);
   pstmt.setString(1, firstName);
   pstmt.setString(2, lastName);
   ```

- Where possible, utilize methods for automating this process such as OData notation, see [API Standard](/docs/greenlake/standards/ratified/api/query_parameters.md) in which filter parameters can be used to build queries. This filter data can be used with a parser such as [OData-filter-go](https://github.com/glcp/odata-filter-go) for go usage. The parser will utilize a backend Abstract Syntax Tree (AST) structure to translate into conditional logic to safely build SQL WHERE clauses, etc.

### Command Injection

Command injection can allow an attacker to execute arbitrary commands by exploiting the vulnerable application. This can occur if user input is fed directly into system commands without proper validation and sanitization. Commands entered by the attacker will be executed with the same privileges as the application uses to run.

#### Requirements

- Avoid adding user supplied input directly into system commands. If user input is required, use safer alternatives to pass arguments in as elements rather than direct concatenation.(i.e. Instead of os.system(), use subprocess.run())
- Always apply proper input data validation. Data should be validated against a list of allowed values, rather than against a list of unacceptable values (such as special characters that can be interpreted as part of a command).
- Applications should run with the least privilege necessary to prevent an attacker from gaining administrator access.

#### Considerations

In the example below, *command_injection_example* takes cmd which is constructed of user input. It is then appended to the system command with no validation. An attacker could inject malicious data into this string without proper white list validation of expected values. It would be possible to enter a semi-colon to separate commands and inject additional unintended commands.

   ```python
   def command_injection_example(cmd):
     #Line below concatenates user input
     os.system("echo " + cmd)

   user_input = input("Enter Data: ")
   command_injection_example(user_input)
```

To avoid directly concatenating user supplied values, use subprocess.run() to pass arguments in as separate elements.

   ```python
   def command_injection_example(cmd):
     #Line below now passes argument in rather than concatenates
     subprocess.run(["echo", command])

   user_input= input("Enter Data: ")
   command_injection_example(user_input)
```

## Directory Traversal

Any path that takes user input as an argument can be susceptible to a directory or path traversal. This is where a malicious user would attempt to access files outside of the directory (stored on the file system) by manipulating variables using “../”, a series of “../”, or an absolute file path.

### Requirements

- Make sure any input is sanitized and validated
- Use absolute and canonical paths where possible.
- Check programs launched and make sure arguments are properly escaped.
- Output sent to the customers is validated and sanitized.

### Considerations

- Use canonical names when validating file paths. A recent vulnerability (ZipSlip) exposed a lot of applications vulnerable to this. For example:

   ```python
   # assume 'reportinfo' is modified by attacker as '../../etc/passwd'

   file = os.sep.join([paths.REPORTS_DIR, request.args['reportinfo']])
   if os.path.exists(file):
      # zip and return report
      zip = ZipFile(file)
      return zip
   ```

- Instead, use canonical paths to validate input paths:

   ```python
   file = os.sep.join([paths.REPORTS_DIR, request.args['reportinfo']])

   # ensure 'reportinfo' is under REPORTS_DIR
   validate_file_name(file, paths.REPORTS_DIR)
   ```

- where,

   ```python
   def validate_file_name(fileName, dir):
       canonicalPathFile = os.path.realpath(fileName)
       canonicalPathDir = os.path.realpath(dir)
       if str(canonicalPathFile).startswith(str(canonicalPathDir) + os.sep):
           return canonicalPathFile
       else:
           raise ValueError("Invalid input file, outside target directory.")
   ```

- Use absolute paths when launching programs to avoid the wrong binary being launched, especially those under elevated privileges.
For example, if an attacker can modify their $PATH variable to point to a malicious binary, the following may result in the wrong binary being executed:

   ```python
   system("cd /var/yp && make &> /dev/null");
   ```

- Be careful when accepting user input (unless sufficiently validated, a filename may come in as: "`cat /etc/password`") and subsequently feeding it to an external program through the shell. In this example, this would cause the password file to be read and possibly returned (instead of the intended file content). If using the shell, be sure to single quote all arguments or use another means to launch the program that does not use the shell (in Python, recommend using subprocess family of APIs).

## Data Validation

Lack of validation can lead to possible exploits such as Command Injection, SQL Injection, Path Manipulation and Cross-Site Scripting (XSS). Controls should be in place to ensure that data values coming into or going out of the application satisfy any API or UI based assumptions. Consider user input insecure by default. All external input, no matter what it is, must be examined and validated.

### Requirements

- When data is untrusted (i.e. User Input), use Input Validation + Output Encoding to provide protections against various injection attacks. These protections can include HTTP headers, input fields, hidden fields, drop down lists, and other web components.
- Ensure that all fields, cookies, http headers/bodies, and form fields are validated against a white list of known “good values”. Content validation can be accomplished with a regexp in many programming languages. Input validation should include length checking, type, and range. Preferably use native packages, (e.g. in Go, define an input struct) or regexp can also accomplish this. There are also third-party validators that can be used.
- Ensure that the data validation occurs on the server side.
- Examine where data validation occurs and if a centralized model or decentralized model is used.
- Ensure there are no backdoors in the data validation model.
- Use output encoding before displaying untrusted input. Output encoding replaces HTML characters with an encoded version so that the browser doesn’t execute the potentially malicious HTML (e.g. \<script>alert(‘Hacked!’>;\</script>).  In Go, this can be accomplished using the html/template package, <https://pkg.go.dev/html/template>) or by using third-party libraries
- Avoid feeding user input directly in commands (such as Eval(), Exec(), OS commands, etc.) wherever possible to prevent Command Injection
- Avoid working with user input in file system calls, do not let the user supply all parts of the path, validate user input as mentioned above and normalize the input before using to prevent path traversal attacks.

## Authentication

Authentication is used to verify the identity of a user or process. Authentication controls are essential to the prevention of confidential files, data, or web pages from being accessed by hackers or users who do not have the necessary access control level. Broken authentication can occur when identity and access controls are not robust, are not implemented correctly, and/or can be bypassed. See also [Session Management](#session-management).

### Requirements

- Support strong authentication mechanisms and password policies. A secure authentication process should consider the following best practices (see Considerations).

### Considerations

Strong Authentication -

- Always use standardized frameworks for authentication, with multifactor, specifically an HPE approved authentication such as IAM. [See here for Global Security Policy – User Access Control.](https://hpe.sharepoint.com/sites/WW/globalsecurity/Pages/global-security-policy.aspx?ID=2897)
- Do not write/use a custom authentication mechanism.
- Use multi-factor – Implementing Multi-Factor Authentication (MFA) is the best defense against password related attacks.  [Global Security Policy - MFA Specifications can be found here.](https://hpe.sharepoint.com/sites/WW/globalsecurity/Pages/global-security-policy.aspx?ID=2904)
- Use strong token based authentication over weaker methods like basic authentication - Basic authentication is not a secure or recommended method of authentication, when sent over an unencrypted connection, the credentials are left open to credential disclosure, and theft.
  - JSON Web Tokens (JWT) is a stateless token that is signed and encrypted. There is no need to store the password in the token. When using JWT’s, choose a strong algorithm with an appropriate key size and utilize the expiration time.
  - If using basic authentication, at minimum implement an [encrypted connection](#secure-transport-protocols).

Username/Password Security -

- Enforce password complexity policies to prevent, or at least slow, the possibility of brute force attacks.
- Passwords should be a passphrase of at least 14 characters. A passphrase is a type of password that strings together words which may form a full or partial sentence, or may be randomly selected. [See Global Security Policy – Passwords for more information.](https://hpe.sharepoint.com/sites/WW/globalsecurity/Pages/global-security-policy.aspx?ID=2897)
- Make sure passwords are never logged.
- Ensure failure messages are generic in nature, do not state whether the username or password failed, only that the login failed.
- Do not use shared credentials, this is most impactful to accountability
  - For more information on shared accounts, see the [Secure Architecture Design](../security/secure_design_and_architecture.md) standard.
- Default or well-known passwords MUST NOT be used.
  - Software products cannot be shipped with unique passwords. The recommended solution is to require the user to set a unique password that meets defined complexity requirements as part of the installation process. This should be as early in the installation process as possible.
- Change all default passwords and user id’s such as vendor supplied accounts.
- Make sure that every character the user types in is actually included in the password.
- Make sure usernames/user-ids are case insensitive. Many sites use email addresses for usernames and email addresses are already case insensitive.
- Review and update password standards periodically as thresholds may change over time.

Other Authentication Protections -

- Use TLS throughout to prevent man-in-the-middle (MiTM) attacks [see here](#secure-transport-protocols).
- Log all failed login attempts and actively monitor for brute force and other attacks.
- Ensure that Session IDs are invalidated (time-out after inactivity).
- Do not expose Session IDs in URLs. See also [Session Management](#session-management).
- Re-authenticate prior to performing those operations deemed “critical”.
- Ensure the login page is only available over TLS.

## Session Management

A session ID or token binds the user authentication credentials (in the form of a user session) to the user HTTP traffic and the appropriate access controls enforced by the web application. The disclosure, capture, prediction, brute force, or fixation of the session ID will lead to session hijacking attacks, where an attacker is able to fully impersonate a victim user in the web application. See also [Authentication](#authentication)

### Requirements

Appropriate timeouts MUST exist on sessions.

Sensitive Data MUST NOT be stored in session cookies.

The generation of secure session IDs MUST include the following properties:

- The name used by the session ID should not be extremely descriptive nor infer unnecessary details about the purpose and meaning of the ID. They should not contain Personally Identifiable data.
- It is recommended to change the default session ID name of the web development framework to a generic name, such as “id”.
- Session tokens must contain at least 128 bits of randomness generated by a cryptographically sound random number generator (see requirements for random number generation below).
- The session ID content (or value) must be meaningless to prevent information disclosure attacks, where an attacker is able to decode the contents of the ID and extract details of the user, the session, or the inner workings of the web application.
- Sessions must timeout after 30 minutes of inactivity, or 15 minutes of inactivity in the case of a session with elevated privilege.
- Sessions must have a maximum life not to exceed 8 hours.
- Session IDs must not be generated on the client.
- Session IDs must not appear in error or log messages.
- Session IDs must only be transmitted over TLS to prevent capture.
- When storing Session IDs in cookies:
  - Use the cookie attribute: 'HttpOnly' option so that the cookie is not accessible via scripts. This mitigates against stealing of cookie values via script attacks like Cross-Site Scripting (XSS).
  - Use the cookie attribute "SameSite" to mitigate Cross-Site Request Forgery (CSRF).
  - Use the cookie attribute: 'secure' option so that the cookie can be sent by the browser only over a secure channel (e.g. https).

Further guidance on session IDs is available in [HPE PSO Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/infrastructure-security/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding&FolderCTID=0x012000562BC6522769AF42950D43BD818AE880) and [Global Security's Session Management Specification](https://hpe.sharepoint.com/sites/WW/globalsecurity/Pages/global-security-policy.aspx?ID=2873).

Require cookies when your application includes authentication and use the secure attribute [see OWASP documentation](https://owasp.org/www-community/controls/SecureCookieAttribute).

## Cookies

Cookies should be secured to protect them from unauthorized access and/or tampering. Cookies can be either persistent (stored on disk by browser until expiration time) or non-persistent (session cookies).

### Requirements

- Implement appropriate expiration, only use secure transmission, preferably no sensitive information should be included
- If sensitive information is needed, then it should be encrypted for an extra layer of security in the event that a cookie is accessed.

### Considerations

- Do not store any critical information in cookies. For example, do not store a user’s password in a cookie. As a rule, do not keep anything in a cookie that can compromise your application.
- Consider encrypting information in cookies.
- Set expiration dates on cookies to the shortest practical time.
  - For session cookies, they should expire when the browser is closed, however a timeout should be set for inactivity (see [Session Management](#session-management)).
  - Persistent cookies should set an expiration based on sensitivity and/or regulatory compliance requirements. Many times, these are used for user preferences and convenience.
- Avoid using permanent cookies.
- Use 'Secure' and 'HttpOnly' flags.
- Use 'SameSite' attribute.

## Cross-Site Scripting (XSS)

XSS refers to a type of attack where an attacker can inject arbitrary HTML onto another page. This code (e.g. a `<script>` tag) can then do potentially bad things, such as scrape passwords from forms, pop up alerts, redirect the user to other sites (malware), or anything else that is possible to code up via script.

### Requirements

- Untrusted data MUST be properly validated, encoded and escaped before use. Use built-in functions for encoding and escaping wherever available. If custom code is being written, verify your encoding and escaping logic.
- Don't rely entirely on selected web frameworks to protect from XSS attack payloads. Ensure you understand the limitations of web frameworks used in the development of your application and always verify untrusted data and sanitize before use.
- If modifying the DOM, only use safe/authorized functions (such as Node.textContent, document.createTextNode)

### Considerations

The key principle behind preventing XSS attacks is very simple:

*Always treat content that comes from the user as untrusted.*

The strategies that flow from this principle are:

- Untrusted content MUST be treated consistently: it should ALWAYS be escaped (so that < becomes &lt;, etc.) to prevent insertion of random HTML tags.
- Application content, such as error messages, that are generated from untrusted content must also be treated as untrusted.

When data is transmitted from the server to the client, untrusted data must be properly encoded. Do not assume data from the server is safe. *In Go, this can be accomplished using the html/template package.* Best practice is to always check data.

Be consistent with escaping: the best strategy seems to be to always escape variables at the point of use. That way all escaping goes in one place and we don't need to worry about sprinkling lots of escaping throughout the code.

## Insecure Direct Object Reference

This is a commonplace vulnerability with web applications that provide varying levels of access or expose an internal object, thus bypassing web security controls by simply manipulating URLs.

### Requirements

- Ensure modification of the input used to reference objects cannot result in the retrieval of objects the user is not authorized to view using strong authorization mechanisms.
- If untrusted input is used to access objects on the server side, then proper authorization checks (on the server side) are employed to ensure data cannot be leaked.
- [Input validation](#data-validation) is used on untrusted input.

### Considerations

Consider the following:

```python
POST cmd=delete_item&crumb=SbWqLz.LDP0&fd=xxxxxxxx
```

Where the attacker may have manipulated 'fd' to delete any random item.

Also, the attackers may find out the internal naming conventions and infer the method names for operation functionality based on IDs returned in responses. Internal IDs should be sufficiently obfuscated to avoid 'guessing'.

In any case, the server must perform authorization checks on the server side before carrying out any actions.

## Sensitive Data Exposure

Sensitive user data and system information should be protected so that they are not accidentally exposed to potential attackers. Many web applications do not properly protect sensitive data, such as credit cards, SSNs, network topologies and authentication credentials. All sensitive data that the application handles should be identified and encryption both at rest and in transit should be enforced. Additionally, sensitive information should never be hard coded in source code and checked into GitHub as they increase the risk of disclosure and theft. Sensitive information should also never be written to/stored in a log file. If an attacker could gain access to the log file, sensitive data could be disclosed and/or used maliciously. Additionally, a valid user with access to the audit log may be able to expand their privileges based upon sensitive information in the log file.

### Requirements

- Take steps to ensure sensitive data is not exposed at rest or in transit (see Considerations)

### Considerations

For data at-rest, ensure:

- Sensitive information is never hard-coded in source code.

  - Potential Sensitive data include but are not limited to passwords and passphrases, encryption keys, private Keys, oAuth tokens, API Keys, cloud Access keys, and cloud IAM credentials.
  - Storing credentials in source code is a poor practice for numerous reasons. The code is checked into a version control system (ex. GitHub) where the credentials could be exposed to users of different access levels and could potentially be exposed to the public. Hard coding credentials in source code also violates the “need to know” principle as anyone who has access to the source code has access to the credentials.
  - Store in a separate file and protect with encryption and access control mechanisms.

  - For database credentials or service account credentials, store them in a configuration file that is not in the code repository and protect them with encryption and access control mechanisms. Alternatively, use a cloud provider compliant solution such as SOPS (Secrets OPerationS) encryption to secure your secrets. In Go, the secrets can be loaded from the secret manager and read using os.Getenv, such as:

     ```Go
      db_pass := os.Getenv("DB_PASS").
       ```

- Sensitive information must not be stored in databases in plain text.
  - Use secure vaults to store all your security sensitive secrets like passwords.
  - If passwords must be stored locally, store its hash instead of plain text. Use the hash for comparisons and other purposes and then erase it from disk/memory (zeroization).
  - Add a 128-bit salt to the password prior to hashing to help prevent [brute force attacks](https://en.wikipedia.org/wiki/Rainbow_table).
  - A salt is a random number which may be stored with the hash as plain text - see [HPE PSO Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/infrastructure-security/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding&FolderCTID=0x012000562BC6522769AF42950D43BD818AE880) for further guidance.
- If you must store sensitive information other than passwords, encryption must be used to protect the confidentiality of data. Keep in mind the following requirements/best practices when implementing encryption: [Cryptographic Algorithms](#cryptographic-algorithms).
- Do not hard code encryption keys in source code.
- Deprecate all uses of weak cryptographic algorithms. Please see [GreenLake Cryptographic Requirements](../security/GLS_Crypto_Requirements.md) for more information.
- Prevent the logging and caching of sensitive data.
  - Take time to determine the sensitivity of the data to be logged.
  - Operationally, protect log files from unauthorized viewing/modifications.
- No stack traces or other security relevant information is included in any user-facing error message.
- Password entry must be obscured on the user’s screen.
- Disable remember me functionality.
- Private keys must be sufficiently protected using key vaults or other means.

For data in-transit, ensure:

- Use secure and approved TLS protocols for login pages and any authenticated pages. See [Cryptographic Algorithms](#cryptographic-algorithms) for information on in-transit encryption.
- Prefer all interfaces (or pages) being accessible only over HTTPS.
- Use the “secure” and “http-only” cookie flags for authentication cookies.
- Do not put sensitive data in the URL.
- Use proper certificate validation. This should include a valid CA-issued certificate with proper revocation checking.
- Always provide all certificates in the chain.

## Missing Function Level Access Control

Authorization is as important as authentication. Access to application functionality and access to all data should be authorized. For data access authorization, application logic should check if the data belongs to the authenticated user, or if the user should be able to access that data.

If requests are not verified, attackers will be able to forge requests to gain access to functionality without proper authorization.

### Requirements

- Perform authorization checks at every entry point. Use best practices such as default-deny and least privilege. See Considerations for more information.

### Considerations

- Every entry point should be authorized. Every function should be authorized.
  - Use a default-deny authorization pattern rather than default-allow for security sensitive functionality.
  - Use the principle of least privilege, ensure that a user is only allowed to access resources and perform operations that are necessary.
- Authorization checks should be efficient, and implemented in a central code base such that it can be applied consistently.
- Never base authorization decisions on untrusted data. For example, do not use a header, or hidden field, from the client request to determine the level of authorization a user will have; this can be manipulated by an attacker.
- At a design level, attempt to keep the range of roles simple. Applications with multiple permission levels/roles often increases the possibility of conflicting permission sets resulting in unanticipated privileges.
- Use trusted system objects for access control decisions
  - JWT tokens on server-side
- In browser based applications do not perform or rely on any authorization decisions in JavaScript.
- Set file permissions to the minimum level required
  - System-wide privileges (0777) should not be used. The permission 0777 grants the world group to have READ/WRITE/EXECUTE access to files. Malicious users of the system may access/modify/delete sensitive data contained in the affected files.
- In cases where authorization fails, a HTTP 403 not authorized page should be returned.

## Cross-Site Request Forgery (CSRF)

CSRF is an attack which tricks an end user to perform actions on a web application in which they are currently authenticated. This is usually achieved by luring the authenticated user to click a phishing link which launches a request towards the application that user is logged into. Since the authentication cookie is automatically attached by browser to such request, request successfully executes in application. The impact of the attack depends on the level of permissions that the victim has.

### Requirements

- CSRF protections must be applied to web applications that use cookie-based authentication. Sensitive actions such as updating user data, modifying data, financial transactions, or critical operations should be protected against CSRF attacks. CSRF protections should also be in place for hybrid applications (both web pages and APIs which are served from the same domain). Most REST APIs do not require CSRF protections but there are scenarios where CSRF protections may still be relevant. This will depend on the implementation and the sensitivity of the actions (e.g. Is the API accessed directly by browsers or accessible via HTTP GET requests?). In cases where CSRF protections are necessary, follow the guidance provided below.

### Considerations

- Enable CSRF protections. Note the Go language does not have CSRF protections enabled by default, so make sure they are enabled.
- Utilize built-in CSRF protections by the web application framework - such as synchronizer token defense pattern. There are numerous possible approaches to mitigating against CSRF attacks, best practice is to use components from reputable vendors rather than writing custom solutions whenever possible (e.g. utilize anti-CSRF mechanisms built into the application’s existing framework).
- If no built-in CSRF protections exist, add state changing CSRF tokens to all requests and validate them on the backend.
- Use SameSite cookie attribute for session cookies
- The `POST`, `PUT`, `PATCH`, and `DELETE` methods, being state changing verbs, should have a CSRF token attached to the request.
- Do not use GET requests for state changing operations.
- None of the above approaches provide defense from CSRF when HTTP requests are initiated by XHR (AJAX) calls. For AJAX calls, the browser includes an Origin header indicating where request has originated from, e.g., if user clicked on phishing link that triggered the request, the Origin value will be of phishing site. Make sure to check the Origin header in request when included by the browser and validate that it has come from domain you trust. The Origin header can be in actual request or pre-flight cross-origin notification request sent by browser.

## XML External Entities (XXE)

XXE is a vulnerability in XML parsers in web applications that might process and execute some payload included as an external reference in the XML document.

If an attacker adds or modifies these entities in an XML file and points them to a malicious source, they can cause a denial of service (DoS) attack or a server-side request forgery (SSRF) attack. They can also scan internal systems, run port scans, extract data, etc.

Here’s an attack scenario from OWASP that involves an attacker attempting to extract data from the server:

```java
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<foo>&xxe;</foo>
```

*Considerations for Go programmers: Note that Golang’s XML decoder doesn’t process external entities by default and are therefore resilient to XXE attacks.*

### Requirements

When enabling parsing of external entities, steps must be taken to avoid processing untrusted XML. Specifically:

- Implement server-side input validation, sanitization checks, etc. to prevent hostile data within XML documents.
- Disable XML external entity and DTD processing.

   ```java
   DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
   dbf.setAttribute(XMLConstants.ACCESS_EXTERNAL_DTD, "");
   dbf.setAttribute(XMLConstants.ACCESS_EXTERNAL_SCHEMA, "");
   ...
   ```

- Where possible, use less complicated formats like JSON

## Third party and Open Source components

Components, such as libraries, frameworks, and other software modules, almost always run with full privileges. If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover. Applications using components with known vulnerabilities may undermine application defenses and enable a range of possible attacks and impacts.

### Requirements

If a code change includes any code not developed by HPE (such as open-source software (OSS) libraries) then that non-HPE code MUST:

- Be identified along with its license and approved by the HPE Open Source Review Board. *This is also a mandatory HPE corporate requirement*.
- NOT have any have any identified Critical or High vulnerabilities, as defined by the Common Vulnerability Scoring System, present in the code when it is added.
- NOT have been declared end-of-life or unsupported by its maintainer, unless the development team explicitly takes on ownership of this software and becomes responsible for security patching at a source code level.
- Be registered with [HPE Vulnerability Tracking and Notification (VTN)](https://hpe.sharepoint.com/sites/Product-Security-Office/SitePages/VTN.aspx) service. *This is also a mandatory HPE corporate requirement*

*Note: Your engineering team may require you to use a CI/CD pipeline that will automatically register libraries with VTN. Check with the owner of your CICD pipeline. If your team does not use a CI/CD pipeline that does this, you may have to [register your components manually](https://hpe.sharepoint.com/sites/Product-Security-Office/SitePages/VTN.aspx)*

## Secure Transport Protocols

All transport protocols should be secure and only standard protocols shall be used such as TLS 1.2 and TLS 1.3 or SSH-2. A secure protocol is one which provides authenticated encryption of its content. Designing secure transport protocols is hard even if you use only standard cryptographic algorithms. For example, TLS 1.3 uses only standard cryptographic algorithms. It was analyzed by approximately 100 cryptographers and protocol experts and many flaws were found prior to approval by the IETF.

### Requirements

- Only use secure and standard transport protocols with no down-select to vulnerable versions, e.g. to TLS 1.1.
- Invalid certificates must be rejected. Be sure to set InsecureSkipVerify to false:
  - config := &tls.Config{InsecureSkipVerify: false}

## Access Tokens and Keys

Access tokens and API keys can be used for requests requiring authentication. They are confidential and must be protected in transit and in storage. Likewise, keys are also confidential and must be protected. Proper key management should be used, this includes life-cycle management, storage, and revocation practices.

### Requirements

- Never hardcode tokens and API keys in application source code.

- For onboarded GLCP services, all access tokens and keys must be managed by the GLCP Access Token Management services, when it is available.

Access Tokens:

- Access tokens should have a short lifespan (15 minutes) to prevent against attacks such as tampering.
- Access tokens must only be sent over encrypted connections.
- Do not hardcode access tokens and API keys in source code.

Key Management:

- Private keys must be sufficiently protected using key vaults or other means. * Generation of keys must use industry-accepted and approved cryptographic libraries, see [Secure random number and key generation](#secure-random-number-and-key-generation).
- Cryptographic keys must be rotated regularly (at least annually)
- Upon compromise, cryptographic keys must be revoked
- For services onboarded to GLCP, access tokens and keys are managed by the GLCP Access Token Management service.

## Cryptographic Algorithms

### Requirements

Follow the [GreenLake Cryptographic Requirements](../security/GLS_Crypto_Requirements.md) standard.

### Considerations

- Developers MUST use cryptographic algorithms approved in the [HPE GreenLake Cryptographic Requirements](../security/GLS_Crypto_Requirements.md) standard or by NIST.
  - Developers should never attempt to write encryption methods; standard encryption methods are thoroughly tested by cryptographic service providers (CSPs) to ensure that they cannot easily be broken. For instance, for robust implementations to encrypt utilize the go package crypto, but be careful not to use the cryptographically broken algorithms such as DES, MD5 and SHA1.
- All other uses of weak cryptographic algorithms have been deprecated and replaced with approved algorithms.
- For in-transit encryption, use TLS 1.2 or 1.3 by default with no down-select.
  - In addition, applications under FedRamp must use a FIPS 140-2 or [FIPS 140-3](https://csrc.nist.gov/CSRC/media/Projects/cryptographic-module-validation-program/documents/fips%20140-3/FIPS%20140-3%20IG.pdf) validated cryptographic module, 140-3 is preferred if available.

## Secure random number and key generation

Secure random number generation is important for key derivation and other uses such as session IDs. The guidance in the [HPE PSO Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/infrastructure-security/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding&FolderCTID=0x012000562BC6522769AF42950D43BD818AE880), must be followed.

### Requirements

- Ensure use of industry-accepted and approved cryptographic libraries. For GoLang, the crypto/rand package creates cryptographically secure random numbers.
- Ensure that guidance in [HPE PSO Secure Coding Guidelines](https://hpe.sharepoint.com/teams/infrastructure-security/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding&FolderCTID=0x012000562BC6522769AF42950D43BD818AE880) is followed for languages other than GoLang.

## Secure logging and error handling

Logging and error handling are important features used to successfully troubleshoot applications and provide accountability. An absence of logging security relevant events jeopardizes the effectiveness of the auditing capability. In the event of a breach, an audit trail is intended to trace events and actions. Therefore, all security relevant information is required. Error messages are helpful for troubleshooting but should not contain too much information. Take care to avoid verbose error messages. In general, error messages aren’t as verbose in GoLang as other languages so take care not to add too much information when trying to build more robust error handling.

### Requirements

- Log all security relevant information taking into account the Considerations below. Avoid overly verbose error handling.

### Considerations

- All security relevant actions must be logged with username, timestamp and action at a minimum for traceability detail.
  - This should include invalid login attempts, privileged actions and configurations changes.
- Audit records must not include sensitive information. Sensitive information includes Personally Identifiable data, tokens, keys, and passwords.
  - Personally Identifiable data includes details like email addresses and phone numbers
- Apply the same rules for protecting sensitive data to data placed into audit logs.
- Whenever logging data that has been input by a user, use proper input data validation to help mitigate the risk of log injection attacks.
- Error handling, whether displayed on screen or in audit logs, MUST not include information on what exactly went wrong (e.g. password failure reasons). Verbose error messages for critical security operations can be used by attackers to breach the application.
- Do not display stack trace information or file system path. This information would be valuable to an attacker to gain information on the services/technologies used.

   **Note that Go’s built-in errors don’t contain stack traces so this is only pertinent to other languages.

  One possible example of simple error message is seen below as there are many ways to implement error handling in go. This message doesn’t say whether the user name or password was incorrect, only that the login failed:

   ```Go
    if err != nil {
     log.Errorf("User login failed! Error: %v", err)
     return nil, err
    }
   ```

## Memory Management

Any application can contain memory leaks, especially when a reference to an object isn’t managed properly and left assigned. Memory leaks are complex and difficult to analyze and detect.

Special considerations for Go developers: Golang has a powerful tool, pprof, which has a heap allocation profiler to help trace memory allocation and detect leaks.

Because Go is a garbage collected language, there is no need to deallocate memory. Go data types such as String are not NULL terminated and not susceptible to buffer overflows as in the case with languages such as C and C++.

### Requirements

There are still a few points to consider with memory management:

- When looping, boundary checks should still be made to ensure that it hasn’t gone beyond the boundary set where Go will Panic.
- Explicitly release memory that is no longer needed.
- Consistent with other parts of this document, implement input validation and output encoding to ensure that no malicious content is allowed.
- For Golang, utilize built-in profiling tool pprof for identifying memory leaks.

### Considerations

The following example details how a memory leak could occur in Go. The function memory_leak() will allocate 1MB or memory in each iteration of the for loop. The loop is not terminated and memory is not released or garbage collected which will cause the program's memory to grow exponentially.

 ```golang
   func memory_leak() {
     for {
       data := make([]byte, 1024*1024)
       _ = data
       time.Sleep(time.Second)
     }
   }

   func main() {
     memory_leak()
     fmt.Println("Not going to get here")
   }
```

## Software development lifecycle

Code reviews aim to identify security flaws in the application related to its features and design, along with the exact root causes. Code review as part of the Pull Request process are used together with architecture threat analysis, penetration testing, and automated tools for defense-in-depth.

Secure Code Review is an enhancement to the standard code review practice where the structure of the review process places security considerations, such as company security standards, at the forefront of the decision-making.

Code is reviewed against the security items above most of which are included in [OWASP top 10](https://owasp.org/www-project-top-ten/), [SANS top 25](https://www.sans.org/top25-software-errors/) and [HPE PSO Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/infrastructure-security/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding&FolderCTID=0x012000562BC6522769AF42950D43BD818AE880).

### Requirements

Before or during the process of building a software artifact, the source code used to build that artifact MUST be scanned for potential violations of this secure coding standard, including at a minimum:

- Static analysis of the source code for common coding errors and leaked secrets using HPE approved static analysis tools (e.g. Coverity).
- All Web applications and APIs MUST be scanned using HPE approved dynamic security tools (e.g. Armor or HPE approved Web application scanning tools).
- All identified CRITICAL or HIGH vulnerabilities (as defined under the Common Vulnerability Scoring System) MUST be triaged and fixed in the product/service before release

When source code is reviewed, at least one reviewer SHOULD audit to ensure that the change has been verified as passing the criteria in the Secure Coding Checklist. While a human security review for each change is unlikely to be feasible, a qualified security review SHOULD be prioritized in the following cases:

- When a new Service is developed
- When APIs are created or modified
- Any changes to authentication or authorization code

Individual development teams may have stricter requirements around security review requirements.

## Common Coding Mistakes

This section contains some examples of poor coding practices that must be avoided.

### **Hard-coded credential**

Example containing a hard-coded admin credential. Note any credentials found in source code, scripts, IAC, or gitHub must be avoided and considered compromised.

```Go
    adminUser := "user_name";
    var adminPassword = “[Redacted]”; //Hard-coded admin password
```

### **Displaying Passwords**

Example code where credentials are displayed on the end users screen in clear text via the print command.

```Python
     elif args.section == "creds":
        creds = CredentialSets(client=client)
        if args.operation == "list":
            result = creds.list()
        elif args.operation == "get":
            result = creds.get(args.creds_id)
        elif args.operation == "password":
            result = creds.password(args.creds_id)
            print(result) #result contains password
```

### **Database Connection String**

Example containing a hard-coded plain text password for the database connection. This allows anyone with access to the code to have access to those related databases.

```Go
    cfg := mysql.Config{
        User: testUser,
        Passwd: [Redacted], //Hard-coded password (redacted here)
        Net:    "tcp",
        Addr:   "127.0.0.1:3306",
        DBName: "testDB",
    }
```

### **Weak Password Requirements**

Example of weak password requirements. In the following example, passwords can be as short as 8 characters with no other complexity requirements. The mixture of password length plus complexity will make it harder for an attacker to brute force the password, but not impossible.
<https://www.newsweek.com/read-this-chart-find-out-how-fast-someone-can-hack-all-your-accounts-1528937>

```Go
    //passwordOK only checking password length, missing complexity
    func passwordOK(password string) bool {
           return len(password) >= 8
    }
```

### **Generation of Passwords not Cryptographically Strong**

Example of utilizing a module which is not cryptographically secure. In the following example, *random_pass_generator* is creating a custom string to be used for password. It makes use of the python module *random* which is not designed for security. To accomplish a cryptographically sound random string utilize the *secrets* method and use secrets.choice.

```Python
def random_pass_generator(self, length, charset=None, lower=True, upper=True, number=True, symbol=True):

      """ Generate a random string of given length."""
        if not charset:
            if lower+upper+number+symbol >= length:
                raise ValueError("string requires more characters than the length provided")
            charset = ""
            tmpres = []
..…
            tmpres += random.sample(charset,length-1)
            random.shuffle(tmpres)
            res = "".join(random.sample(string.ascii_letters,1)+tmpres)

        else:
            res = "".join(random.sample(string.ascii_letters, 1) + \
                random.sample(charset, length-1))

        return res
```

### **Cert Validity Timeframe Exceeds Industry Standards**

Example of setting a certificate validity to 3650 days, which equates to 10 years. Longer validity periods may make it harder to implement newer stronger algorithms or updates.

```Python
MY_CERT_VALIDITY_DAYS=3650
MY_CERT_CONF_FILE=DIR_PATH+"/mycert-hpe-intermediate-csr.conf"
MY_CERT_FILE=DIR_PATH+"/rename_mycert.sh"
```

### **Default Password**

Example of setting a default password value if the password environment variable is not set. The default is both hard coded and easily guessable.

```Go
    // ServiceUserConfig - structure for service user config
    type ServiceUserConfig struct {
        Username string `env:"EXAMPLE_USERNAME" default:"user1"`
        Password string `env:"EXAMPLE_PASSWORD" default:"password1"` //hard-coded default fall-back value
    }
```

### **Basic authentication over HTTP**

Example using basic authentication with HTTP URL sending credentials over an unencrypted connection.

```Go
    req, respErr := http.NewRequest(http.MethodGet, url, nil)
    if respErr != nil {
        log.Errorf("Unable to construct request for %s", url)
        return err
    }
    req.SetBasicAuth(mob.ServiceConfig, mob.ServiceConfig.servPassword)
```

### **Insecure Connection: -k Disabling Certificate Validation**

Example code utilizing the -k flag which skips certificate verification. Note that this example is passing a basic auth token over this insecure connection.

```Python
    def get_basic_token(self, username, password):
        return base64.b64encode("{}:{}".format(username,password))

    def upload_bundle(self, upgrade_bundle):
        # -k flag below skips certificate validation
        cmd = "curl -k -H 'Authorization: Basic {}' " \
              "https://{}/api/1.0/appliance-management/upgrade/uploadbundle/test " \
              "-F file=@{}".format(self.base_token,self.test_manager_ip,upgrade_bundle)
```

### **Insecure Connection: InsecureSkipVerify Disabling Certificate Validation**

Example code with InsecureSkipVerify set to true, disabling SSL certificate verification.

```Go
 testRoute := router.Group("/ab/test")
 configureAuthMiddleware(testRoute, serviceConfig)
    // InsecureSkipVerify below set to true, disabling certificate verification
 tr := &http.Transport{TLSClientConfig: &tls.Config{InsecureSkipVerify: true}}
 httpClient = &http.Client{Timeout: time.Second * 30, Transport: tr}
```

### **Escalation of Privileges**

Example code containing design patterns that lead to escalation of privileges. If the parameter in the first instance were left undefined, it would allow for full access, this is what is considered a “default-allow.” The second instance gives super users full access, while this indicates it is for testing purposes, a super user in a production environment is dangerous functionality.

```Go
    //If we get here then the user is "authenticated"

    if !is_AuthRequired {
        //"No authorization required" has been explicitly specified.
     userAuth = “FullAccess”;
     return userAuth;
    }
    else if is_SuperUserMode {
        //Indicates a setting used by developers for testing purposes.
        userAuth = “FullAccess”;
     return userAuth;
    }
```

### **World-Writeable Permissions**

Example code writing to a cert file and a key file with world writeable permissions.

```Go
    //0777 seen below sets world-writeable permissions
    err = ioutil.WriteFile(certFile, res.SignedCert, 0777)
    if err != nil {
       fmt.Printf("Unable to write signed cert to: %v\n - %v", certFile, err)
       return
    }

    err = ioutil.WriteFile(keyFile, res.PrivateKey, 0777)
    if err != nil {
       fmt.Printf("Unable to write signed cert to: %v\n - %v", certFile, err)
       return
    }
```

### **Insecure Protocol**

Example code downloading images over FTP. FTP is considered an insecure protocol and utilizes clear text usernames and passwords with no encryption. FTP should be replaced with a secure alternative such as SFTP.

```Python
    echo -e "\033[33m==> Downloading images\033[0m"
    if [[ -n "${FTP_USER}" && -n "${FTP_PASSWORD}" && -n "${FTP}    " ]]; then
        unset ftp_proxy
        wget -nd -r --quiet --no-parent -P /sample/ --user="${FTP_USER}" --password="${FTP_PASSWORD}" "${FTP}"/*
        cd /sample/isos || exit 1
        find ../ -type f -iname "*iso" -exec ln -s {} . \;
    fi
```

## Definitions

For the purposes of this document:

*Production Use* means that the software has access to real customer data, and/or interacts with a customer or a device managed by or on behalf of a customer.

*HPE GreenLake Source Code* or *HPE Developed GreenLake Cloud Software* refers to software that is written for *Production Use* in a hosted solution that forms part of the GreenLake Cloud product experience.

*Sensitive Data* refers to data that might be leveraged by a malicious actor including:

- Any authentication credential such as tokens or private keys, including temporary ones,
- Any information which would allow users to be personally identified such as an e-mail address or government identifier,
- Other information held by HPE on behalf of a customer that if exposed could cause significant damage to that customer, such as network topologies.

*Service* refers to a set of software components that provide a capability to other software or users, and that are expected to be deployed together.

## Useful Resources

- <https://owasp.org/www-project-top-ten/>
- <https://www.sans.org/top25-software-errors/>
- [NIST Risk Management Framework](https://nvd.nist.gov/800-53/Rev4/control/SC-18)
- [NIST Guidelines on Active Content and Mobile Code](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-28ver2.pdf)
- [Code Review Guide](https://owasp.org/www-pdf-archive/OWASP_Code_Review_Guide_v2.pdf), OWASP.
- [HPE Vulnerability Tracking and Notification (VTN) tool](https://hpe.sharepoint.com/sites/Product-Security-Office/SitePages/VTN.aspx)
- [Implementation Guidance for FIPS 140-3 and the Cryptographic Module Validation Program](https://csrc.nist.gov/CSRC/media/Projects/cryptographic-module-validation-program/documents/fips%20140-3/FIPS%20140-3%20IG.pdf)
- [Rainbow Table](https://en.wikipedia.org/wiki/Rainbow_table)
- [HPE PSO Secure Design and Coding Guidelines](https://hpe.sharepoint.com/teams/infrastructure-security/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2Finfrastructure%2Dsecurity%2FShared%20Documents%2FSTAR%5FRepository%2FSDL%2FHPE%20Common%20Secure%20Design%20and%20Coding&FolderCTID=0x012000562BC6522769AF42950D43BD818AE880), HPE Product Security Office.
- [Session management, Developer Software Security Specification](https://hpe.sharepoint.com/sites/WW/globalsecurity/Pages/global-security-policy.aspx?ID=2873), HPE Global Security.
- [Secure Cookie Attribute](https://owasp.org/www-community/controls/SecureCookieAttribute), OWASP.
- [Go Language Guide – Web Application Secure Coding Practices](https://github.com/OWASP/Go-SCP/blob/master/dist/go-webapp-scp.pdf)
- [NIST Computer Security Resource Center (CSRC) Secure Software Development Framework](https://csrc.nist.gov/projects/ssdf)
- [MITRE Common Weakness Enumeration (CWE) View: Software Development](https://cwe.mitre.org/data/definitions/699.html)

Provide feedback at  <gl-sec-stds@hpe.com>

**Original publication date (yyyy-mm-dd):** 2022-01-17

**Revised:** 2024-12-19, 2023-06-08, 2023-01-20, 2022-12-09

**What's Changed** 2024-12-19

- Added statement that default or well-known passwords must not be used together with advice on how to avoid in software products

**What's Changed** 2023-06-08

- Refactored overview section to incorporate policy statements from the policy document
- Added secure software development lifecycle section (to capture statements from the policy document that didn't have an obvious location in this one)
- Added definitions section
- Changed "Action" to "Requirements" and added an explicit "Considerations" section, to more clearly distinguish requirements from guidance.

**What's Changed** 2023-01-20

- Added Memory Management and Common Coding Mistakes sections
- Changed the following sections: Overview, Data Validation, Data Validation, Broken Authentication, Sensitive Data Exposure, Missing Function Level Access Control, Cross-Site Request Forgery (CSRF), Secure Transport Protocols, Access Tokens and Keys, Cryptographic Algorithms, and Secure logging and error handling
- Added accompanying Secure Coding Guidelines Checklist

**What’s Changed** 2022-12-09

- Added checklist for verifying that APIs are secure
- Added checklist for verifying that cookies are secure
- Changed filename from secure\_code\_reviews.md to secure\_coding\_and\_reviews.md
