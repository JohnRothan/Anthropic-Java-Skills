# Appendix: Glossary of Terms

> From the Alibaba Java Development Manual (Huangshan Edition).

 1.   POJO (Plain Ordinary Java Object): In this specification, POJO refers specifically to a simple class that has only setter / getter / toString methods, including
    DO / DTO / BO / VO, etc.

 2.   DO (Data Object): At Alibaba, this refers specifically to a POJO class that corresponds one-to-one with a database table. This object maps one-to-one to the database table structure, and transfers data source objects upward through the DAO layer.

 3.   PO (Persistent Object): Also refers to a POJO class that corresponds one-to-one with a database table. This object maps one-to-one to the database table structure, and transfers data source objects upward through the DAO layer.

 4.   DTO (Data Transfer Object): Data transfer object, the object transferred outward by the Service or Manager.

 5.   BO (Business Object): Business object, an object that encapsulates business logic and may be output by the Service layer.

 6.   Query: Data query object, used by each layer to receive query requests from the layer above. Note that for query encapsulation with more than 2 parameters, using the Map class for transfer is prohibited.

 7.   VO (View Object): Display-layer object, typically the object transferred from the Web to the template rendering engine layer.

 8.   CAS (Compare And Swap): A mechanism for resolving the performance overhead caused by using locks under multi-threaded parallel conditions; it is a hardware-implemented atomic operation. The CAS operation involves three operands: the memory location, the expected original value, and the new value. If the value at the memory location matches the expected original value, the processor automatically updates the value at that location to the new value. Otherwise, the processor does nothing.

 9.   GAV (GroupId, ArtifactId, Version): Maven coordinates, used to uniquely identify a jar package.

 10. OOP (Object Oriented Programming): In this document, it broadly refers to the programming approach based on classes and objects.

 11. AQS (AbstractQueuedSynchronizer): A low-level synchronization utility class implemented using a first-in-first-out queue. It is the foundation for many higher-level synchronization implementation classes, such as ReentrantLock, CountDownLatch, Semaphore, etc. These classes implement their template methods by inheriting AQS, and then use the AQS subclass as an inner class of the synchronization component, usually named Sync.

 12. ORM (Object Relation Mapping): Object-relational mapping, the conversion between the object domain model and the underlying data. In this document, it broadly refers to frameworks such as iBATIS and mybatis.

 13. NPE (java.lang.NullPointerException): Null pointer exception.

 14. OOM (Out Of Memory): Derived from java.lang.OutOfMemoryError, a serious condition that occurs when the JVM does not have enough memory to allocate space for an object and the garbage collector is also unable to reclaim space.

 15. GMT (Greenwich Mean Time): Refers to the standard time at the Royal Greenwich Observatory in the suburbs of London, United Kingdom, because the prime meridian is defined as the line of longitude passing through it. The Earth's daily rotation is somewhat irregular and is slowly decelerating; the current standard time is Coordinated Universal Time (UTC), which is provided by atomic clocks.

 16. First-party library: A library (jar package) depended upon by sub-project modules within this project itself.

 17. Second-party library: A library (jar package) published to the central repository internally within the company, which can be depended upon by other applications within the company.

 18. Third-party library: An open-source library (jar package) from outside the company.

Appendix 3: Error Code List

 Error Code               Chinese Description                       Notes

 00000   Everything OK                     Return after correct execution

 A0001   Client-side error                     Level-1 macro error code

 A0100   User registration error                    Level-2 macro error code

 A0101   User has not agreed to the privacy policy

 A0102   Registration restricted by country or region

 A0110   Username validation failed

 A0111   Username already exists

 A0112   Username contains sensitive words

 A0113   Username contains special characters

 A0120   Password validation failed

 A0121   Password length insufficient

 A0122   Password strength insufficient

 A0130   Verification code input error

 A0131   SMS verification code input error

 A0132   Email verification code input error

 A0133   Voice verification code input error

 A0140   User identity document anomaly

 A0141   User identity document type not selected

 A0142   Mainland ID card number validation invalid

 A0143   Passport number validation invalid

 A0144   Military officer ID card number validation invalid

 A0150   User basic information validation failed

 A0151   Phone number format validation failed

 A0152   Address format validation failed

 A0153   Email format validation failed

A0200   User login anomaly                  Level-2 macro error code

A0201   User account does not exist

A0202   User account is frozen

A0203   User account has been voided

A0210   User password incorrect

A0211   Number of incorrect password attempts exceeded

A0220   User identity verification failed

A0221   User fingerprint recognition failed

A0222   User facial recognition failed

A0223   User has not obtained third-party login authorization

A0230   User login has expired

A0240   User verification code incorrect

A0241   Number of user verification code attempts exceeded

A0300   Access permission anomaly                  Level-2 macro error code

A0301   Access not authorized

A0302   Authorization in progress

A0303   User authorization request rejected

A0310   Intercepted due to access object privacy settings

A0311   Authorization has expired

A0312   No permission to use the API

A0320   User access intercepted

A0321   Blacklisted user

A0322   Account is frozen

A0323   Illegal IP address

A0324   Gateway access restricted

A0325   Regional blacklist

A0330   Service is in arrears

A0340   User signature anomaly

A0341   RSA signature error

A0400   User request parameter error                 Level-2 macro error code

A0401   Contains illegal malicious redirect link

A0402   Invalid user input

A0410   Required request parameter is empty

A0411   User order number is empty

A0412   Order quantity is empty

A0413   Missing timestamp parameter

A0414   Illegal timestamp parameter

A0420   Request parameter value out of allowed range

A0421   Parameter format does not match

A0422   Address not within service range

A0423   Time not within service range

A0424   Amount exceeds limit

A0425   Quantity exceeds limit

A0426   Total number of items in batch processing request exceeds limit

A0427   Request JSON parsing failed

A0430   User input content illegal

A0431   Contains prohibited sensitive words

A0432   Image contains prohibited information

A0433   File infringes copyright

A0440   User operation anomaly

A0441   User payment timeout

A0442   Order confirmation timeout

A0443   Order is closed

A0500   User request service anomaly                 Level-2 macro error code

A0501   Number of requests exceeds limit

A0502   Number of concurrent requests exceeds limit

A0503   User operation, please wait

A0504   WebSocket connection anomaly

A0505   WebSocket connection disconnected

A0506   Duplicate user request

A0600   User resource anomaly                    Level-2 macro error code

A0601   Insufficient account balance

A0602   Insufficient user disk space

A0603   Insufficient user memory space

A0604   Insufficient user OSS capacity

A0605   User quota exhausted                   Number of Ant Forest waterings or daily lottery draws

A0700   User file upload anomaly                  Level-2 macro error code

A0701   User uploaded file type does not match

A0702   User uploaded file too large

A0703   User uploaded image too large

A0704   User uploaded video too large

A0705   User uploaded compressed file too large

A0800   User current version anomaly                  Level-2 macro error code

A0801   User installed version does not match the system

A0802   User installed version too low

A0803   User installed version too high

A0804   User installed version has expired

A0805   User API request version does not match

A0806   User API request version too high

A0807   User API request version too low

A0900   User privacy not authorized                   Level-2 macro error code

A0901   User privacy agreement not signed

A0902   User camera not authorized

A0903   User camera not authorized

A0904   User photo library not authorized

A0905   User files not authorized

A0906   User location information not authorized

A0907   User contacts not authorized

A1000   User device anomaly                 Level-2 macro error code

A1001   User camera anomaly

A1002   User microphone anomaly

A1003   User earpiece anomaly

A1004   User speaker anomaly

A1005   User GPS positioning anomaly

B0001   System execution error                 Level-1 macro error code

B0100   System execution timeout                 Level-2 macro error code

B0101   System order processing timeout

B0200   System disaster recovery function triggered              Level-2 macro error code

B0210   System rate limiting

B0220   System feature degradation

B0300   System resource anomaly                 Level-2 macro error code

B0310   System resources exhausted

B0311   System disk space exhausted

B0312   System memory exhausted

B0313   File handles exhausted

B0314   System connection pool exhausted

B0315   System thread pool exhausted

B0320   System resource access anomaly

B0321   System failed to read disk file

C0001   Error calling third-party service

C0100   Middleware service error                 Level-1 macro error code

C0110   RPC service error                Level-2 macro error code

C0111   RPC service not found

C0112   RPC service not registered

C0113   Interface does not exist

C0120   Message service error

C0121   Message delivery error

C0122   Message consumption error

C0123   Message subscription error

C0124   Message group not found

C0130   Cache service error

C0131   key length exceeds limit

C0132   value length exceeds limit

C0133   Storage capacity is full

C0134   Unsupported data format

C0140   Configuration service error

C0150   Network resource service error

C0151   VPN service error

C0152   CDN service error

C0153   Domain name resolution service error

C0154   Gateway service error

C0200   Third-party system execution timeout               Level-2 macro error code

C0210   RPC execution timeout

C0220   Message delivery timeout

C0230   Cache service timeout

C0240   Configuration service timeout

C0250   Database service timeout

C0300   Database service error                Level-2 macro error code

C0311   Table does not exist

C0312   Column does not exist

C0321   Multiple columns with the same name exist in a multi-table join

C0331   Database deadlock

C0341   Primary key conflict

C0400   Third-party disaster recovery system triggered             Level-2 macro error code

C0401   Third-party system rate limiting

C0402   Third-party feature degradation

C0500   Notification service error                 Level-2 macro error code

C0501   SMS reminder service failed

C0502   Voice reminder service failed

C0503   Email reminder service failed
