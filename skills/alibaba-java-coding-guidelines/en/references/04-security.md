# 4. Security Specification

> From the Alibaba Java Development Manual (Huangshan Edition). Severity levels: [Mandatory] must follow · [Recommended] should follow · [Reference] good practice for reference.

 1. **[Mandatory]** Pages or features belonging to an individual user must enforce access-control validation.
Note: This prevents accessing, modifying, or deleting other people's data at will without performing horizontal authorization checks, such as viewing another person's private message content.

 2. **[Mandatory]** Sensitive user data must not be displayed directly; the displayed data must be masked.
Positive example: A personal mobile phone number in Mainland China is displayed as 139****1219, hiding the middle 4 digits to prevent privacy leakage.

 3. **[Mandatory]** SQL parameters passed in by users must strictly use parameter binding or METADATA field-value restriction to prevent SQL injection; concatenating strings to access the database via SQL is prohibited.
Counter-example: A large number of signatures in a certain system were maliciously modified, precisely because the dangerous characters #-- were not escaped, causing the information after the where clause to be commented out during a database update, resulting in the entire database being updated.

 4. **[Mandatory]** Any parameter passed in by a user request must be validated for validity.
Note: Ignoring parameter validation may lead to:
   - An excessively large page size causing memory overflow
   - A malicious order by causing slow database queries
   - Cache breakdown
   - SSRF
   - Arbitrary redirection
   - SQL injection, Shell injection, deserialization injection
   - Regular-expression input source-string denial of service (ReDoS)
        Extension: Java code uses regular expressions to validate client input. Some regular-expression patterns work fine when validating ordinary user input, but if an attacker uses a specially crafted string for validation, it may result in an infinite loop.

 5. **[Mandatory]** Outputting user data that has not been security-filtered or properly escaped to an HTML page is prohibited.
Note: XSS (cross-site scripting) attack. It refers to a malicious attacker inserting malicious html code into a Web page; when a user browses it, the html code embedded in the Web page is executed, causing harm such as obtaining the user's cookie, phishing, obtaining the user's page data, worms, and malware injection.

 6. **[Mandatory]** Form and AJAX submissions must perform CSRF security validation.
Note: CSRF (Cross-site request forgery) is a common class of programming vulnerability. For an application/website with a CSRF vulnerability, an attacker can construct a URL in advance; as soon as the victim user visits it, the backend will modify the user's parameters in the database accordingly without the user's knowledge.

 7. **[Mandatory]** The target address passed in for an external URL redirect must undergo whitelist filtering.
Note: By maliciously constructing a redirect link, an attacker can launch a phishing attack against the victim.

 8. **[Mandatory]** When using platform resources such as SMS, email, phone calls, placing orders, and payments, a correct anti-replay mechanism must be implemented, such as quantity limits, fatigue control, and verification-code checking, to avoid abuse that causes financial loss.
Note: For example, when sending a verification code to a phone during registration, if the number and frequency are not limited, this feature can be exploited to harass other users and waste the SMS platform's resources.

 9. **[Mandatory]** For the file upload feature, the file size and type must be strictly checked and controlled.
Note: An attacker can exploit an upload vulnerability to upload a malicious file to the server and execute it remotely, achieving the goal of controlling the website server.

 10. **[Mandatory]** Passwords in configuration files need to be encrypted.

 11. **[Recommended]** For scenarios requiring user-input content such as posting, commenting, and sending instant messages, risk-control strategies such as anti-flooding and prohibited-word content filtering must be implemented.
