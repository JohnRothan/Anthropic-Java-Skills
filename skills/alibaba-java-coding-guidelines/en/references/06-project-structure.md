# 6. Project Structure

> From the Alibaba Java Development Manual (Huangshan Edition). Severity levels: [Mandatory] must follow · [Recommended] should follow · [Reference] good practice for reference.

(1) Application Layering

1. **[Recommended]** Based on business architecture practices, combined with an analysis of industry layering standards and popular technical frameworks, the recommended layered structure is shown in the figure. By default, upper layers depend on lower layers, and the arrows indicate a direct dependency relationship. For example, the Open API layer may depend on the Web layer (Controller layer), and may also depend directly on the Service layer, and so on:

     - Open API layer: Can directly encapsulate Service interfaces and expose them as RPC interfaces; encapsulate them as HTTP interfaces via the Web layer; gateway control layer, etc.
     - Terminal display layer: The layer where templates for each terminal are rendered and displayed. Currently this mainly includes velocity rendering, JS rendering, JSP rendering, mobile display, etc.
     - Web layer: Mainly handles forwarding of access control, validation of various basic parameters, or simple processing of non-reusable business logic, etc.
     - Service layer: The relatively concrete business logic service layer.
     - Manager layer: The general business processing layer, which has the following characteristics:
             1) The layer that encapsulates third-party platforms, preprocessing the returned results, converting exception information, and adapting to upper-layer interfaces.
             2) Sinking of the Service layer's general capabilities, such as caching solutions and middleware general processing.
             3) Interacts with the DAO layer, combining and reusing multiple DAOs.
     - DAO layer: The data access layer, which interacts with underlying data stores such as MySQL, Oracle, Hbase, OceanBase, etc.
     - Third-party services: Including RPC service interfaces from other departments, foundational platforms, and HTTP interfaces from other companies, such as the Taobao Open Platform, Alipay payment service, AutoNavi (Amap) map service, etc.
     - External data interface: Interfaces provided by external (application) data storage services, commonly seen in data migration scenarios.

2. **[Reference]** (Layered exception handling convention) In the DAO layer, there are many types of exceptions that arise, which cannot be caught with fine-grained exceptions, so use the catch(Exception e) approach and throw new DAOException(e). There is no need to print logs here, because the logs must be caught and printed to the log file in the Manager or Service layer; logging again on the same server wastes performance and storage. When an exception occurs in the Service layer, the error log must be recorded to disk, with parameters and context information included whenever possible, which is equivalent to preserving the crime scene. The Manager layer is deployed on the same machine as the Service, so its logging approach is consistent with the DAO layer; if it is deployed separately, then it adopts the same handling approach as the Service. The Web layer should never continue to throw exceptions upward, because it is already at the top layer. If it realizes that an exception will cause the page to fail to render properly, then it should redirect directly to a friendly error page, adding a friendly error message whenever possible. The Open API layer must process exceptions into error codes and error messages to return.

3. **[Reference]** Layered domain model conventions:
 - DO (Data Object): This object corresponds one-to-one with the database table structure, and transmits data source objects upward through the DAO layer.
 - DTO (Data Transfer Object): The data transfer object, the object transmitted outward by the Service or Manager.

 - BO (Business Object): The business object, an object encapsulating business logic that can be output by the Service layer.
 - Query: The data query object, used by each layer to receive query requests from the upper layer. Note: for queries with more than 2 parameters, encapsulation is required; using the Map class for transmission is prohibited.
 - VO (View Object): The display layer object, usually the object transmitted from the Web layer to the template rendering engine layer.

(2) Second-party Library Dependencies

 1. **[Mandatory]** When defining GAV, follow the rules below:
 1) GroupId format: com.{company/BU}.business-line.[sub-business-line], up to 4 levels.
        Note: {company/BU} examples: BU-level identifiers such as alibaba / taobao / tmall / kaikeba; the sub-business-line is optional.
        Positive example: com.taobao.jstorm or com.alibaba.dubbo.register
 2) ArtifactId format: product-line-name-module-name. The semantics should be neither duplicated nor omitted; first check the central repository to verify.
        Positive example: dubbo-client / fastjson-api / jstorm-tool
 3) Version: See the detailed specifications below.

 2. **[Mandatory]** The naming convention for second-party library version numbers: major-version.minor-version.revision-number
 1) Major version number: A change in product direction, or large-scale API incompatibility, or an architecture-incompatible upgrade.
 2) Minor version number: Maintains relative compatibility, adds major feature characteristics, and includes API-incompatible modifications with extremely small impact scope.
 3) Revision number: Maintains full compatibility, fixes BUGs, adds minor feature characteristics, etc.
 Note: Note that the starting version number must be 1.0.0, not 0.0.1.
 Counter-example: A second-party library in the repository started with version number 1.0.0.0 and silently "upgraded" all the way to 1.0.0.64, completely losing the semantic information of the version.

 3. **[Mandatory]** Online applications should not depend on SNAPSHOT versions (except for security packages); officially released libraries must first be verified against the central repository, so that the RELEASE version number has continuity, and the version number is not allowed to be overwritten during an upgrade.
 Note: Not depending on SNAPSHOT versions ensures the idempotency of application releases. In addition, it can also speed up the packaging and build during compilation.

 4. **[Mandatory]** When adding or upgrading a second-party library, keep the arbitration results of other jar packages unchanged except for the functional points. If there is a change, it must be explicitly evaluated and verified.
 Note: When upgrading, compare the information before and after running dependency:resolve. If the arbitration results are completely inconsistent, then use the dependency:tree command to find the differences and use <exclude> to exclude the jar package.

 5. **[Mandatory]** Enum types may be defined in second-party libraries, and parameters may use enum types, but interface return values are not allowed to use enum types or POJO objects containing enum types.

 6. **[Mandatory]** The naming convention for second-party library customized packages: after the specified version number, add "-English-description[sequence-number]". The English description can be a department abbreviation or business name, and the sequence number directly follows the English description, indicating the sequence number of this customized package.
 Note: The version number of fastjson customized for SCM: 1.0.0-SCM1. Note: Please try to resolve class conflicts and loading issues on the application side, and avoid arbitrarily releasing such customized packages.

 7. **[Mandatory]** When depending on a group of second-party libraries, a unified version variable must be defined to avoid inconsistent version numbers.
 Note: When depending on springframework-core, -context, and -beans, which are all the same version, you can define a variable to hold the version: ${spring.version}, and reference this version when defining the dependency.

 8. **[Mandatory]** It is prohibited for the pom dependencies of subprojects to contain the same GroupId, the same ArtifactId, but different Versions.
 Note: During local debugging, the version number specified by each subproject is used, but when merged into a single war, only one version number can appear in the final lib directory. There have been past cases where offline debugging was correct, but a failure occurred after release to production.

 9. **[Recommended]** Be cautious about introducing third-party implementations into low-level foundational technical frameworks, core data management platforms, or systems close to the hardware end.

 10. **[Recommended]** Place all dependency declarations in pom files inside the <dependencies> block, and place all version arbitration inside the <dependencyManagement> block.

  Note: <dependencyManagement> only declares versions and does not actually introduce them, so subprojects need to explicitly declare the dependency, and the version and scope are read from the parent pom. Whereas for <dependencies>, all dependencies declared in the main pom's <dependencies> are automatically introduced and are inherited by all subprojects by default.

11. **[Recommended]** Second-party libraries should not have configuration items; at the very least, do not add more configuration items.

12. **[Recommended]** Do not use unstable toolkits or Utils classes.
  Note: "Unstable" means the provider cannot maintain backward compatibility, working normally at the compilation stage but producing exceptions at runtime. Therefore, try to use industry-stable second-party toolkits.

13. **[Reference]** To avoid dependency conflict problems when applications use second-party libraries, second-party library publishers should follow these principles:
  1) Streamlined. Remove all unnecessary APIs and dependencies, including only the Service API, necessary domain model objects, Utils classes, constants, enums, etc. If it depends on other second-party libraries, introduce them as provided whenever possible, and let the users of the second-party library depend on the specific version number; have no specific log implementation, only depend on the logging framework.
  2) Stable. Each version change should be recorded; who maintains the second-party library and where the source code is should be easy to find. Unless the user actively upgrades the version, the behavior of a public second-party library should not change.

(3) Server

1. **[Mandatory]** Remote operation calls must have a timeout setting.
 Note: Timeout settings similar to those of HttpClient need to be explicitly set by yourself. Experience has shown that countless failures are caused by not setting a timeout.

2. **[Recommended]** The client sets the specific timeout (in ms) for remote interface methods. The order in which timeout settings take effect is generally: 1) client Special Method; 2) client interface level; 3) server Special Method; 4) server interface level.

3. **[Recommended]** For high-concurrency servers, it is recommended to reduce the time_wait timeout of the TCP protocol.
 Note: By default, the operating system only closes connections in the time_wait state after 240 seconds. Under high-concurrency access, the server side may be unable to establish new connections because too many connections are in the time_wait state, so this wait value needs to be reduced on the server.
 Positive example: On a linux server, please change this default value (in seconds) by modifying the /etc/sysctl.conf file: net.ipv4.tcp_fin_timeout=30

4. **[Recommended]** Increase the maximum number of file handles (File Descriptor, abbreviated as fd) supported by the server.
 Note: Mainstream operating systems are designed to manage TCP / UDP connections in the same way as files, that is, one connection corresponds to one fd. Mainstream linux servers support a maximum of 1024 fds by default. When the number of concurrent connections is very large, it is easy to encounter an "open too many files" error due to insufficient fds, causing new connections to fail to be established. It is recommended to increase the maximum number of handles supported by the linux server by several times (related to the amount of memory of the server).

5. **[Recommended]** Set the -XX:+HeapDumpOnOutOfMemoryError parameter in the JVM environment parameters, so that the JVM outputs dump information when it encounters an OOM scenario.
 Note: OOM occurs with a certain probability, sometimes only once every few months. The heap information at the time of the error is very helpful for solving the problem.

6. **[Recommended]** In the online production environment, set the JVM's Xms and Xmx to the same memory capacity to avoid the pressure of adjusting the heap size after GC.

7. **[Recommended]** Understand the approximate average time consumption of each service. By configuring an independent thread pool, you can isolate slower services from the main thread pool, to prevent threads of different services from perishing together.

8. **[Reference]** Internal server redirects must use forward; external redirect addresses must be generated using the URL Broker, otherwise the browser will prompt "not secure" because HTTPS is used online. In addition, it will also bring about the problem of inconsistent URL maintenance.
