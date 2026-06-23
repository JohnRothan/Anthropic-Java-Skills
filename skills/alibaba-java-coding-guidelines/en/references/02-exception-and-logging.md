# 2. Exception and Logging

> From the Alibaba Java Development Manual (Huangshan Edition). Severity levels: [Mandatory] must follow · [Recommended] should follow · [Reference] good practice for reference.

(1) Error Codes

1. **[Mandatory]** Principles for designing error codes: enable fast root-cause tracing and standardized communication.
 Note: An error code designed to be overly perfect and complex is like an obscure character in the Kangxi Dictionary: the wording seems precise, but the dictionary is hard to carry around and not easy to understand.
 Positive example: The questions an error code answers are: Whose fault is it? Where is the error?
  1) An error code must allow the source of the error to be identified quickly, so that it can be quickly determined whose problem it is.
  2) An error code must allow clear comparison (easy to `equals` in code).
  3) An error code helps the team quickly reach a consistent understanding of the cause of the error.

2. **[Mandatory]** Error codes must not embed version number or error-level information.
 Note: Error codes are kept compatible by continually appending new ones. The error level is determined by the log and by the meaning of the error code itself.

3. **[Mandatory]** When everything is normal but an error code must be supplied, return five zeros: 00000.

4. **[Mandatory]** An error code is of string type, consisting of 5 characters in two parts: the source of the error + a four-digit number.
 Note: The source of the error is divided into A/B/C. A indicates the error originates from the user, such as parameter errors, the user having installed too low a version, payment timeouts, etc.;
 B indicates the error originates from the current system, often due to business logic errors or poor program robustness; C indicates the error originates from a third-party service, such as a CDN
 service error, message delivery timeout, etc. The four-digit number ranges from 0001 to 9999, with a reserved step interval of 100 between major categories. Refer to Appendix Table 3 at the end of the document.

5. **[Mandatory]** Numbering is not tied to the company's business architecture, and even less to the organizational structure. It is assigned on a first-come, first-served basis on a unified platform; once approved and effective, the number is permanently fixed.

6. **[Mandatory]** Users of error codes should avoid arbitrarily defining new error codes.
 Note: As much as possible, find an error code with the same or similar semantics in the existing error code appendix and use it directly in the code.

7. **[Mandatory]** Error codes must not be output directly to users as prompt messages.
 Note: The stack trace (stack_trace), error message (error_message), error code (error_code), and user prompt (user_tip) form an effectively related and mutually convertible harmonious whole, but none of them should take over the role of another.

8. **[Recommended]** Business information beyond the error code is carried by error_message, rather than letting the error code itself cover too many specific business attributes.

9. **[Recommended]** When obtaining an error code from a third-party service, when throwing it upward, the current system may convert it, changing C to B, and the original third-party error code should be included in the error message.

10. **[Reference]** Error codes are divided into first-level macro error codes, second-level macro error codes, and third-level macro error codes.
  Note: In error scenarios that cannot be more specifically determined, the first-level macro error codes can be used directly, namely: A0001 (client-side error), B0001 (system execution error), C0001 (error calling a third-party service).
  Positive example: An error calling a third-party service is first-level, a middleware error is second-level, and a message service error is third-level.

11. **[Reference]** The last three digits of an error code have no relationship whatsoever to HTTP status codes.

12. **[Reference]** Error codes facilitate communication and code collaboration among developers from different cultural backgrounds.
  Note: Error codes in the form of English words are not conducive to collaboration among developers from non-English-speaking countries (such as Arabic, Hebrew, Russian, etc.).

13. **[Reference]** Error codes reflect human nature: perceptual cognition plus word of mouth. Using pure numbers to arrange error codes is not conducive to perceptual memory and classification.
  Note: A number is a whole; each digit has the same status and meaning.
  Counter-example: For a five-digit number 12345, the 1st digit is the error level, the 2nd digit is the error source, and 345 is the number. The human brain will not proactively split and distinguish the different meanings of each digit.

(2) Exception Handling

1. **[Mandatory]** RuntimeExceptions defined in the Java class library that can be avoided through pre-checking should not be handled through `catch`, such as: NullPointerException, IndexOutOfBoundsException, etc.
  Note: This excludes exceptions that cannot be avoided through pre-checking. For example, when parsing numbers in string form, there may be a number format error, which must be handled by catching NumberFormatException.
  Positive example: if (obj != null) {...}
  Counter-example: try { obj.method(); } catch (NullPointerException e) {…}

2. **[Mandatory]** Do not use caught exceptions for flow control or condition control.
  Note: Exceptions were designed to handle various unexpected situations during program execution, and exception handling is far less efficient than condition checking.

3. **[Mandatory]** When using `catch`, distinguish between stable code and unstable code. Stable code refers to code that will not go wrong under any circumstances. For the `catch` of unstable code, distinguish the exception types as much as possible, then perform the corresponding exception handling.
  Note: Performing a try-catch over a large block of code prevents the program from responding correctly to different exceptions, and is also not conducive to locating problems. This is a sign of irresponsibility.
  Positive example: In a user registration scenario, if the user enters illegal characters, or the username already exists, or the password the user enters is too simple, make case-by-case judgments programmatically and prompt the user accordingly.

4. **[Mandatory]** Exceptions are caught in order to handle them. Do not catch an exception and then do nothing with it and discard it. If you do not want to handle it, throw the exception to its caller. The outermost business user must handle the exception and convert it into content that users can understand.

5. **[Mandatory]** In transaction scenarios, after an exception is thrown and caught, if a rollback is needed, be sure to remember to manually roll back the transaction.

6. **[Mandatory]** The finally block must close resource objects and stream objects; wrap them in try-catch if an exception may occur.
  Note: For JDK7 and above, the try-with-resources approach can be used.

7. **[Mandatory]** Do not use `return` in a finally block.
  Note: After the return statement in the try block executes successfully, it does not return immediately but continues to execute the statements in the finally block. If a return statement exists there, it will return directly at that point, mercilessly discarding the return point in the try block.
  Counter-example:
     private int x = 0;
     public int checkReturn() {
         try {
                 // x equals 1, this does not return here
                 return ++x;
         } finally {
                 // the returned result is 2
                 return ++x;
         }
     }

8. **[Mandatory]** The caught exception and the thrown exception must match exactly, or the caught exception must be a parent class of the thrown exception.
  Note: If you expect the other party to throw an embroidered ball but actually receive a lead ball, unexpected situations will arise.

9. **[Mandatory]** When calling RPC, second-party packages, or methods of dynamically generated classes, catch exceptions using the Throwable class.
  Note: When calling a method through the reflection mechanism, if the method cannot be found, a NoSuchMethodException is thrown. Under what circumstances is a NoSuchMethodError thrown? When second-party packages conflict, the arbitration mechanism may result in introducing an unexpected version that causes class method signatures to mismatch, or when a bytecode modification framework (such as ASM) dynamically creates or modifies a class and modifies the corresponding method signature. In these cases, even if the code is correct at compile time, a NoSuchMethodError will be thrown at runtime.
  Counter-example: The footprint service introduced a higher version of spring, causing a NoSuchMethodError to be thrown when running a certain core logic. The class used in the catch was Exception, the stack was thrown upward, affecting the upper-layer business. This is a typical counter-example of a non-core function point affecting a core application.

10. **[Recommended]** A method's return value may be null; returning an empty collection or empty object is not mandatory. However, a comment must be added to fully explain under what circumstances null will be returned.
   Note: This specification makes it clear that preventing NPE is the caller's responsibility. Even if the called method returns an empty collection or empty object, the caller cannot rest easy: scenarios such as remote call failures or runtime exceptions returning null must be considered.

11. **[Recommended]** Preventing NPE is a basic discipline for programmers. Pay attention to the scenarios where NPE arises:
   1) When the return type is a primitive data type but a wrapper-type object is returned, auto-unboxing may produce an NPE.
   Counter-example: public int method() { return an Integer object; }; if it is null, auto-unboxing throws NPE.
   2) Database query results may be null.
   3) Even if a collection isNotEmpty, the data elements taken out may be null.
   4) When a remote call returns an object, a null check is always required to prevent NPE.
   5) For data obtained from a Session, it is recommended to perform an NPE check to avoid a null pointer.
   6) Cascading calls obj.getA().getB().getC(); a chain of calls easily produces NPE.
   Positive example: Use JDK8's Optional class to prevent NPE problems.

12. **[Recommended]** When defining exceptions, distinguish between unchecked / checked exceptions. Avoid directly throwing new RuntimeException(), and it is even less acceptable to throw Exception or Throwable. Use custom exceptions with business meaning. Custom exceptions already defined in the industry are recommended, such as: DAOException / ServiceException, etc.

13. **[Reference]** For http / api open interfaces exposed outside the company, error codes must be used, while throwing exceptions is recommended within the application; for cross-application RPC calls, the Result approach is preferred, encapsulating an isSuccess() method, the error code, and a short error message; throwing exceptions is recommended within the application.
   Note: Reasons for using the Result approach for the return method of RPC methods:
   1) With the exception-throwing return approach, if the caller does not catch it, a runtime error will occur.
   2) If no stack information is added and you only `new` a custom exception and add an error message of your own understanding, it will not be of much help to the caller in solving the problem. If stack information is added, then in cases of frequent call errors, the performance overhead of data serialization and transmission is also a problem.

(3) Logging Specification

1. **[Mandatory]** Applications must not directly use the API of the logging system (Log4j, Logback), but should depend on the API of the logging framework (SLF4J, JCL—Jakarta Commons Logging). Using a facade-pattern logging framework facilitates maintenance and unifies the log handling approach across classes.
  Note: How to use the logging framework (SLF4J, JCL—Jakarta Commons Logging) (SLF4J is recommended):
     Using SLF4J:
     import org.slf4j.Logger;
     import org.slf4j.LoggerFactory;
     private static final Logger logger = LoggerFactory.getLogger(Test.class);
     Using JCL:
     import org.apache.commons.logging.Log;
     import org.apache.commons.logging.LogFactory;
     private static final Log log = LogFactory.getLog(Test.class);

2. **[Mandatory]** Log files must be kept for at least 15 days, because some exceptions occur with a "weekly" frequency. The current day's log is saved as "appName.log", stored in the /{unified directory}/{appName}/logs/ directory. Past logs use the format: {logname}.log.{retention date}, with date format: yyyy-MM-dd.
  Positive example: Taking the mppserver application as an example, the log is saved at /home/admin/mppserver/logs/mppserver.log, and the historical log name is mppserver.log.2021-11-28.

3. **[Mandatory]** According to national law, records related to network operation status, network security incidents, sensitive personal information operations, etc., must be retained as logs for no less than six months, and backed up across multiple machines on the network.

4. **[Mandatory]** Naming convention for extension logs in an application (such as tracking points, temporary monitoring, access logs, etc.): appName_logType_logName.log. logType: log type, such as stats / monitor / access, etc.; logName: log description. The benefit of this naming: from the file name you can know which application the log file belongs to, what type it is, and what its purpose is, which also facilitates categorized lookup.
Note: It is recommended to classify logs, putting error logs and business logs separately, which is convenient for developers to view and also convenient for timely monitoring of the system through logs.
Positive example: In the mppserver application, time zone conversion exceptions are monitored separately, e.g.: mppserver_monitor_timeZoneConvert.log.

5. **[Mandatory]** When outputting logs, use placeholders for concatenation between string variables.
Note: Because String concatenation uses StringBuilder's append() method, there is a certain performance overhead. Using placeholders is merely a substitution action, which can effectively improve performance.
Positive example: logger.debug("Processing trade with id : {} and symbol : {}", id, symbol);

6. **[Mandatory]** For log output at the trace / debug / info levels, a log-level switch check must be performed:
Note: Although within the body of the debug(parameters) method, the first line of code isDisabled(Level.DEBUG_INT) returning true (in common Slf4j implementations Log4j and Logback) leads to an immediate return, the parameters may still undergo string concatenation operations. In addition, if a parameter such as debug(getName()) contains a getName() method call, the method-call overhead is wasted needlessly.
Positive example:
   // If the check is true, then trace and debug level logs can be output
   if (logger.isDebugEnabled()) {
       logger.debug("Current ID is: {} and name is: {}", id, getName());
   }

7. **[Mandatory]** Avoid printing duplicate logs, which wastes disk space. Be sure to set additivity=false in the log configuration file.
Positive example: <logger name="com.taobao.dubbo.config" additivity="false">

8. **[Mandatory]** In the production environment, using System.out or System.err for output, or using e.printStackTrace() to print exception stacks, is prohibited.
Note: The standard log output and standard error output files only roll over each time Jboss restarts. If a large amount of output is sent to these two files, the file size easily exceeds the operating system's size limit.

9. **[Mandatory]** Exception information should include two types of information: the scene information at the time of the incident and the exception stack information. If it is not handled, then throw it upward using the keyword `throws`.
Positive example: logger.error("inputParams: {} and errorMessage: {}", various parameters or object toString(), e.getMessage(), e);

10. **[Mandatory]** When printing logs, it is prohibited to directly use a JSON tool to convert an object into a String.
 Note: If some get methods in the object are overridden and may throw exceptions, then printing the log may affect the execution of the normal business flow.
 Positive example: When printing logs, only print business-related attribute values or call the object's toString() method.

11. **[Recommended]** Record logs prudently. In the production environment, outputting debug logs is prohibited; output info logs selectively; if you use warn to record business behavior information at the time of a new launch, be sure to pay attention to the log output volume to avoid filling up the server disk, and remember to delete these observation logs in a timely manner.
 Note: Outputting a large amount of invalid logs is not conducive to improving system performance, nor to quickly locating error points. When recording logs, think: Does anyone really look at these logs? What can you do when you see this log? Can it bring benefits to problem troubleshooting?

12. **[Recommended]** The warn log level can be used to record cases where users enter incorrect input parameters, to avoid being at a loss when users complain. Unless necessary, do not output the error level in this scenario, to avoid frequent alarms.
 Note: Pay attention to the level of log output. The error level only records system logic errors, exceptions, or important error information.

13. **[Recommended]** Try to use English to describe log error messages. If the error message in the log cannot be clearly described in English, then use Chinese, otherwise ambiguity easily arises.

Note: For internationalization teams or servers deployed overseas, due to character set issues, use all-English to comment and describe log error messages.

14. **[Recommended]** To protect user privacy, sensitive user information in log files needs to be desensitized.
Note: When troubleshooting problems via logs, it is recommended to query using unique identifiers such as order numbers or UUIDs.
