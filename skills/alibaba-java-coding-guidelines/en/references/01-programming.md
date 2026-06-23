# 1. Programming Specification

> From the Alibaba Java Development Manual (Huangshan Edition). Severity levels: [Mandatory] must follow · [Recommended] should follow · [Reference] good practice for reference.

(1) Naming Conventions

1. **[Mandatory]** No name related to programming may begin with an underscore or a dollar sign, nor may it end with an underscore or a dollar sign.
  Counter-example: _name / __name / $Object / name_ / name$ / Object$

2. **[Mandatory]** It is strictly forbidden for any programming-related name to mix Pinyin with English, and it is even less acceptable to use Chinese directly.
  Note: Correct English spelling and grammar make code easier for readers to understand and avoid ambiguity. Note that even pure Pinyin naming should be avoided.
  Positive example: ali / alibaba / taobao / kaikeba / aliyun / youku / hangzhou, and other internationally recognized names, may be treated as English.
  Counter-example: DaZhePromotion [discount] / getPingfenByName() [score] / String fw [fuwa] / int variable_name = 3

3. **[Mandatory]** Avoid using any racially discriminatory or insulting words from any human language in both code and comments.
  Positive example: blockList / allowList / secondary
  Counter-example: blackList / whiteList / slave / SB / WTF

4. **[Mandatory]** Class names use the UpperCamelCase style, with the following exceptions: DO / PO / DTO / BO / VO / UID, etc.
  Positive example: ForceCode / UserDO / HtmlDTO / XmlService / TcpUdpDeal / TaPromotion
  Counter-example: forcecode / UserDo / HTMLDto / XMLService / TCPUDPDeal / TAPromotion

5. **[Mandatory]** Method names, parameter names, member variables, and local variables all uniformly use the lowerCamelCase style.
  Positive example: localValue / getHttpMessage() / inputUserId

6. **[Mandatory]** Constant names should be all uppercase, with words separated by underscores, striving to express the meaning completely and clearly. Do not avoid long names.
  Positive example: MAX_STOCK_COUNT / CACHE_EXPIRED_TIME
  Counter-example: MAX_COUNT / EXPIRED_TIME

7. **[Mandatory]** Abstract class names begin with Abstract or Base; exception class names end with Exception; test class names begin with the name of the class they
  test and end with Test.

8. **[Mandatory]** Define arrays by placing the brackets immediately adjacent to the type.
  Positive example: Define an integer array as int[] arrayDemo.
  Counter-example: Using String args[] to define the main parameter.

9. **[Mandatory]** Do not prefix any boolean variable in a POJO class with is, otherwise some frameworks may cause serialization errors during parsing.
  Note: According to the first rule of the table-creation conventions in the MySQL specification of this manual, variables that express yes/no use the is_xxx naming style, so the mapping from is_xxx to xxx must be set in the <resultMap>.
  Counter-example: For a field defined as boolean type Boolean isDeleted, its getter method is also isDeleted(). When some frameworks parse in reverse, they "mistakenly believe" that the corresponding field name is deleted, causing the field to be inaccessible and producing unexpected results or throwing exceptions.

10. **[Mandatory]** Package names uniformly use lowercase, with exactly one English word of natural semantic meaning between dot separators. Package names uniformly use the singular form, but if a class name has a plural meaning, the class name may use the plural form.
   Positive example: The package name for an application utility class is com.alibaba.ei.kunlun.aap.util; the class name is MessageUtils (this rule references the Spring framework structure).

11. **[Mandatory]** Avoid using exactly the same name between member variables of a subclass and superclass, or between local variables of different code blocks, as this reduces comprehensibility.

Note: Member variables with the same name in subclass and superclass can compile even if they are public; local variables with the same name in different code blocks within the same method are also legal, but should be avoided. Parameter names that are not setters/getters should also avoid being the same as member variable names.
Counter-example:
 public class ConfusingName {
     protected int stock;
     protected String alibaba;
     // Parameter names that are not setters/getters must not have the same name as the member variables of this class
     public void access(String alibaba) {
         if (condition) {
              final int money = 666;
             // ...
         }
         for (int i = 0; i < 10; i++) {
             // Within the same method body, must not have the same name as money in other code blocks
              final int money = 15978;
             // ...
         }
     }
 }
 class Son extends ConfusingName {
     // Must not have the same name as the member variable of the superclass
     private int stock;
 }

12. **[Mandatory]** Eliminate completely non-standard English abbreviations to avoid being unable to understand the meaning from the name.
Counter-example: AbstractClass "abbreviated" as AbsClass; condition "abbreviated" as condi; Function "abbreviated" as Fu. Such arbitrary abbreviations severely reduce the readability of the code.

13. **[Recommended]** To achieve the goal of self-documenting code, when naming any custom programming element, use complete word combinations to express it.
Positive example: In the JDK, the class name for atomically updating a volatile field of an object reference is AtomicReferenceFieldUpdater.
Counter-example: The common way of defining a method-internal variable as int a;.

14. **[Recommended]** When naming constants and variables, place the noun indicating the type at the end of the word to improve recognizability.
Positive example: startTime / workQueue / nameList / TERMINATED_THREAD_COUNT
Counter-example: startedAt / QueueOfWork / listName / COUNT_TERMINATED_THREAD

15. **[Recommended]** If a module, interface, class, or method uses a design pattern, reflect the specific pattern in the naming.
Note: Reflecting the design pattern in the name helps readers quickly understand the architectural design philosophy.
Positive example: public class OrderFactory;
      public class LoginProxy;
      public class ResourceObserver;

16. **[Recommended]** Do not add any modifiers (including public) to the methods and attributes in an interface class, to keep the code concise, and add valid Javadoc comments. Try not to define constants in an interface; if you must define one, it is best to ensure that the constant is related to the interface's methods and is a fundamental constant of the entire application.
Positive example: Interface method signature void commit();
     Interface fundamental constant String COMPANY = "alibaba";
Counter-example: Interface method definition public abstract void commit();
Note: In JDK8, interfaces allow default implementations, so this default method is a default implementation that is valuable for all implementing classes.

17. There are two sets of rules for naming interfaces and implementation classes:
 1) **[Mandatory]** For Service and DAO classes, based on the SOA concept, the exposed service must be an interface, and the internal implementation class is distinguished from the interface with the Impl suffix.
 Positive example: CacheServiceImpl implements the CacheService interface.
 2) **[Recommended]** If the interface name describes a capability, take the corresponding adjective as the interface name (usually an adjective ending in –able).
 Positive example: AbstractTranslator implements Translatable.

18. **[Reference]** Enum class names carry the Enum suffix, and enum member names must be all uppercase, with words separated by underscores.
  Note: An enum is essentially a special constant class, and its constructor is by default forcibly private.
  Positive example: For an enum named ProcessStatusEnum, member names: SUCCESS / UNKNOWN_REASON

19. **[Reference]** Naming conventions for each layer:
  A) Method naming conventions for the Service / DAO layer:
    1) Methods that retrieve a single object use get as a prefix.
    2) Methods that retrieve multiple objects use list as a prefix, ending in plural, e.g., listObjects
    3) Methods that retrieve statistical values use count as a prefix.
    4) Insert methods use save / insert as a prefix.
    5) Delete methods use remove / delete as a prefix.
    6) Modify methods use update as a prefix.
  B) Domain model naming conventions:
    1) Data object: xxxDO, where xxx is the name of the data table.
    2) Data transfer object: xxxDTO, where xxx is a name related to the business domain.
    3) Display object: xxxVO, where xxx is generally the name of a web page.
    4) POJO is the general term for DO / DTO / BO / VO; naming it xxxPOJO is forbidden.

(2) Constant Definitions

1. **[Mandatory]** No magic value (i.e., a constant that has not been predefined) is allowed to appear directly in the code.
 Counter-example:
 // Developer A defined the cache key.
 String key = "Id#taobao_" + tradeId;
 cache.put(key, value);
 // Developer B copied it when using the cache but missed the underscore, i.e., key is "Id#taobao" + tradeId, causing a failure.
 String key = "Id#taobao" + tradeId;
 cache.get(key);

2. **[Mandatory]** When assigning a long or Long value, use an uppercase L after the value, not a lowercase l, because lowercase is easily confused with the digit and causes misunderstanding.
 Note: public static final Long NUM = 2l; Is the written value the number 21, or the Long value 2?

3. **[Mandatory]** The numeric suffix for floating-point types is uniformly an uppercase D or F.
 Positive example: public static final double HEIGHT = 175.5D;
       public static final float WEIGHT = 150.3F;

4. **[Recommended]** Do not use a single constant class to maintain all constants; categorize constants by function and maintain them separately.
 Note: A large, all-encompassing constant class is disorganized; you must use the find function to locate the constant to modify, which is unhelpful for understanding and maintenance.
 Positive example: Cache-related constants are placed under the class CacheConsts; system-configuration-related constants are placed under the class SystemConfigConsts.

5. **[Recommended]** There are five levels of constant reuse: cross-application shared constants, intra-application shared constants, sub-project shared constants, intra-package shared constants, and intra-class shared constants.
 1) Cross-application shared constants: placed in a second-party library, usually under the constant directory of the client.jar.
 2) Intra-application shared constants: placed in a first-party library, usually under the constant directory of a submodule.

  Counter-example: Even easily understood constants must be uniformly defined as intra-application shared constants. Two programmers separately defined the constant representing "yes" in two classes:
         In class A: public static final String YES = "yes";
         In class B: public static final String YES = "y";
         A.YES.equals(B.YES) is expected to be true, but it actually returns false, causing an online problem.
  3) Sub-project shared constants: i.e., under the constant directory of the current sub-project.
  4) Intra-package shared constants: i.e., under a separate constant directory in the current package.
  5) Intra-class shared constants: defined directly within the class as private static final.

6. **[Recommended]** If a variable's value varies only within a fixed range, use the enum type to define it.
  Note: If there are extended attributes beyond the name, use the enum type. The number in the positive example below is extended information, indicating which season of the year it is.
  Positive example:
    public enum SeasonEnum {
           SPRING(1), SUMMER(2), AUTUMN(3), WINTER(4);
           private int seq;
           SeasonEnum(int seq) {
               this.seq = seq;
           }
           public int getSeq() {
               return seq;
           }
     }

(3) Code Formatting

1. **[Mandatory]** If the content inside the braces is empty, simply write it concisely as {}, with no need for line breaks or spaces between the braces; for a non-empty code block, then:
  1) No line break before the left brace.
  2) Line break after the left brace.
  3) Line break before the right brace.
  4) No line break if the right brace is followed by code such as else; a line break is required after the right brace that indicates termination.

2. **[Mandatory]** No space is needed between the left parenthesis and the adjacent character to its right; no space is needed between the right parenthesis and the adjacent character to its left; but a space is needed before the left brace. See the positive example below rule 5 for details.
  Counter-example: if(space a == b space)

3. **[Mandatory]** A space must be added between reserved words such as if / for / while / switch / do and the parentheses on either side.

4. **[Mandatory]** A space must be added on both the left and right sides of any binary or ternary operator.
  Note: This includes the assignment operator =, the logical operator &&, the addition/subtraction/multiplication/division symbols, etc.

5. **[Mandatory]** Use 4-space indentation; using the Tab character is forbidden.
  Note: If you use Tab for indentation, you must set 1 Tab to 4 spaces. When setting Tab to 4 spaces in IDEA, please do not check Use tab character; in Eclipse, find tab policy and set it to Spaces only, Tab size: 4, and finally you must check insert spaces for tabs.
  Positive example: (covering points 1-5 above)
    public static void main(String[] args) {
           // Indent 4 spaces
           String say = "hello";
           // There must be a space on the left and right of the operator
           int flag = 0;
           // There must be a space between the keyword if and the parenthesis; no space is needed between f and the left parenthesis, or between 0 and the right parenthesis inside the parentheses
           if (flag == 0) {
               System.out.println(say);

        }
        // Add a space before the left brace without a line break; line break after the left brace
        if (flag == 1) {
               System.out.println("world");
               // Line break before the right brace; since there is else after the right brace, no line break is needed
        } else {
               System.out.println("ok");
               // If it ends directly after the right brace, then a line break is required
        }
   }

6. **[Mandatory]** There is exactly one space between the double slashes of a comment and the comment content.
Positive example:
// This is a sample comment; please note there is a space after the double slashes
String commentString = new String("demo");

7. **[Mandatory]** When performing a type cast, no space is needed between the right parenthesis and the value being cast.
Positive example:
double first = 3.2D;
int second = (int)first + 2;

8. **[Mandatory]** The number of characters in a single line must not exceed 120; when it exceeds, a line break is needed, following these principles when breaking the line:
1) The second line is indented 4 spaces relative to the first line; from the third line on, no further indentation is added. See the example.
2) The operator breaks together with the following content.
3) The dot symbol of a method call breaks together with the following content.
4) When multiple parameters in a method call need a line break, break after the comma.
5) Do not break before a parenthesis; see the counter-example.
Positive example:
   StringBuilder builder = new StringBuilder();
   // When exceeding 120 characters, break and indent 4 spaces, and break with the dot before the method
   builder.append("yang").append("hao")...
        .append("chen")...
        .append("chen")...
        .append("chen");
Counter-example:
   StringBuilder builder = new StringBuilder();
   // When exceeding 120 characters, do not break before the parenthesis
   builder.append("you").append("are")...append
        ("lucky");
   // A method call with many parameters may exceed 120 characters; the line break is only after the comma
   method(args1, args2, args3, ...
            , argsX);

9. **[Mandatory]** When method parameters are defined and passed in, a space must be added after the comma between multiple parameters.
Positive example: In the example below, there must be a space after the comma following the actual argument args1.
       method(args1, args2, args3);

10. **[Mandatory]** Set the IDE's text file encoding to UTF-8; the line break character for files in the IDE uses the Unix format, not the Windows format.

11. **[Recommended]** The total number of lines in a single method should not exceed 80 lines.
 Note: Excluding comments, the total number of lines of the method signature, the left and right braces, the code within the method, blank lines, carriage returns, and any invisible characters should not exceed 80 lines.

   Positive example: The code logic clearly distinguishes the main subject from supporting details, and the individual from the common. Supporting-detail logic is separated into an additional method to make the trunk code clearer; common logic is extracted into a common method for easy reuse and maintenance.

12. **[Recommended]** There is no need to add several spaces to align the assignment equals sign of a variable with the equals sign at the corresponding position on the previous line.
   Positive example:
     int one = 1;
     long two = 2L;
     float three = 3F;
     StringBuilder builder = new StringBuilder();
   Note: After adding the variable builder, if alignment is required, then one, two, and three all need several spaces added. When there are many variables, this is a very cumbersome matter.

13. **[Recommended]** Insert a blank line between code of different logic, different semantics, and different business to separate them and improve readability.
   Note: In no case is it necessary to insert multiple blank lines for separation.

(4) OOP Specification

1. **[Mandatory]** Avoid accessing the static variables or static methods of a class through an object reference of that class, which needlessly increases the compiler's parsing cost; simply use the class name to access them.

2. **[Mandatory]** All overriding methods must be annotated with @Override.
  Note: The problem of getObject() versus get0bject(). One uses the letter O, the other uses the digit 0; adding @Override allows accurate determination of whether the override was successful. Furthermore, if the method signature is modified in the abstract class, the implementing class will immediately report a compilation error.

3. **[Mandatory]** Variable arguments may be used only when the parameter type is the same and the business meaning is the same; avoid defining the parameter type as Object.
  Note: Variable arguments must be placed at the end of the parameter list. (Developers are advised to avoid programming with variable arguments as much as possible.)
  Positive example: public List<User> listUsers(String type, Long... ids) {...}

4. **[Mandatory]** For interfaces being called externally or interfaces depended on by second-party libraries, modifying the method signature is not allowed, to avoid impacting interface callers. When an interface is deprecated, the @Deprecated annotation must be added, and it must clearly explain what the new interface or new service to adopt is.

5. **[Mandatory]** Deprecated classes or methods must not be used.
  Note: The method decode(String encodeStr) in java.net.URLDecoder is deprecated; the two-parameter decode(String source, String encode) should be used. Since the interface provider has clearly marked the interface as deprecated, it has the obligation to provide a new interface at the same time; as a caller, you have the obligation to verify what the new implementation of the deprecated method is.

6. **[Mandatory]** The equals method of Object is prone to throwing a null pointer exception; you should use a constant or an object that definitely has a value to call equals.
  Positive example: "test".equals(param);
  Counter-example: param.equals("test");
  Note: It is recommended to use the utility class java.util.Objects#equals(Object a, Object b) introduced in JDK7.

7. **[Mandatory]** For value comparison between all integer wrapper class objects, use the equals method entirely.
  Note: For Integer var = ? with assignment between -128 and 127, the Integer object is produced in IntegerCache.cache and reuses existing objects. Integer values in this range can be judged directly using ==, but all data outside this range will be produced on the heap and will not reuse existing objects. This is a big pitfall; using the equals method for judgment is recommended.

8. **[Mandatory]** Any monetary amount must be stored in the smallest currency unit and as an integer type.

9. **[Mandatory]** For equality comparison between floating-point numbers, primitive data types must not be compared using ==, and wrapper data types must not be judged using equals.
  Note: Floating-point numbers use a "mantissa + exponent" encoding scheme, similar to the "significant digits + exponent" representation of scientific notation. Binary cannot precisely represent most decimal fractions; for the specific principle, refer to "Code Efficiently."
  Counter-example:

  float a = 1.0F - 0.9F;
  float b = 0.9F - 0.8F;
  if (a == b) {
      // Expected to enter this code block and execute other business logic
      // But in fact the result of a == b is false
  }
  Float x = Float.valueOf(a);
  Float y = Float.valueOf(b);
  if (x.equals(y)) {
      // Expected to enter this code block and execute other business logic
      // But in fact the result of equals is false
  }
Positive example:
(1) Specify an error range; if the difference between two floating-point numbers is within this range, they are considered equal.
  float a = 1.0F - 0.9F;
  float b = 0.9F - 0.8F;
  float diff = 1e-6F;
  if (Math.abs(a - b) < diff) {
       System.out.println("true");
  }
(2) Use BigDecimal to define the values, then perform floating-point arithmetic operations.
  BigDecimal a = new BigDecimal("1.0");
  BigDecimal b = new BigDecimal("0.9");
  BigDecimal c = new BigDecimal("0.8");
  BigDecimal x = a.subtract(b);
  BigDecimal y = b.subtract(c);

  if (x.compareTo(y) == 0) {
       System.out.println("true");
  }

10. **[Mandatory]** Equality comparison of BigDecimal should use the compareTo() method, not the equals() method.
Note: The equals() method compares both value and precision (1.0 and 1.00 return false), whereas compareTo() ignores precision.

11. **[Mandatory]** When defining a data object DO class, the attribute types must match the database field types.
Positive example: The bigint of a database field must correspond to the Long type of the class attribute.
Counter-example: In a business, the id field of a database table is defined as type bigint unsigned, but the actual class object attribute is Integer. As the id grows larger, it exceeds the representation range of Integer and overflows into a negative number. At this point, the database id does not support storing negative numbers and throws an exception, causing an online failure.

12. **[Mandatory]** Using the constructor BigDecimal(double) to convert a double value into a BigDecimal object is forbidden.
Note: BigDecimal(double) carries a risk of precision loss, which may cause business logic exceptions in scenarios of precise calculation or value comparison. For example:
BigDecimal g = new BigDecimal(0.1F); the actual stored value is: 0.100000001490116119384765625
Positive example: Prefer the constructor whose argument is a String, or use the BigDecimal valueOf method. This method internally actually executes Double's toString, and Double's toString truncates the mantissa according to the precision that double can actually represent.
      BigDecimal recommend1 = new BigDecimal("0.1");
      BigDecimal recommend2 = BigDecimal.valueOf(0.1);

13. The usage standards for primitive data types and wrapper data types are as follows:
1) **[Mandatory]** All POJO class attributes must use wrapper data types.
2) **[Mandatory]** The return values and parameters of RPC methods must use wrapper data types.
3) **[Recommended]** All local variables use primitive data types.

Note: POJO class attributes having no initial value is a reminder to the user that they must explicitly assign a value themselves when needed; any NPE problem or database-storage check is guaranteed by the user.
Positive example: The result of a database query may be null. Because of auto-unboxing, receiving with a primitive data type carries an NPE risk.
Counter-example: A business's transaction report displays the rise/fall of the total transaction amount, i.e., plus/minus x%, where x is a primitive data type. When the called RPC service is called unsuccessfully, it returns a default value, and the page displays 0%, which is unreasonable; it should display a dash -. So the null value of a wrapper data type can represent additional information, such as: remote call failed, abnormal exit.

14. **[Mandatory]** When defining POJO classes such as DO / PO / DTO / VO, do not set any attribute default value.
 Counter-example: A business's DO has createTime with a default value of new Date(); but this attribute was not assigned a specific value during data extraction, and when updating other fields, this field was incidentally updated as well, causing the creation time to be modified to the current time.

15. **[Mandatory]** When adding an attribute to a serializable class, please do not modify the serialVersionUID field, to avoid deserialization failure; if it is a completely incompatible upgrade, then modify the serialVersionUID value to avoid deserialization confusion.
 Note: Note that an inconsistent serialVersionUID will throw a serialization runtime exception.

16. **[Mandatory]** Adding any business logic to a constructor is forbidden; if there is initialization logic, please place it in the init method.

17. **[Mandatory]** POJO classes must have a toString method written. When using the IDE tool source > generate toString, if it inherits another POJO class, note that you should prepend super.toString().
 Note: When the method throws an exception during execution, you can directly call the POJO's toString() method to print its attribute values, which facilitates troubleshooting.

18. **[Mandatory]** It is forbidden for both the isXxx() and getXxx() methods of the corresponding attribute xxx to exist simultaneously in a POJO class.
 Note: When the framework calls the extraction method of attribute xxx, it cannot determine which method will definitely be called first; one of the dreaded pitfalls.

19. **[Recommended]** When accessing the array obtained from String's split method by index, you need to check whether there is content after the last separator, otherwise there is a risk of throwing IndexOutOfBoundsException.
 Note:
  String str = "a,b,c,,";
  String[] ary = str.split(",");
  // Expected to be greater than 3, result equals 3
  System.out.println(ary.length);

20. **[Recommended]** When a class has multiple constructors, or multiple methods with the same name, these methods should be placed together in order for easy reading. This rule takes precedence over the next rule.
 Positive example:
  public int method(int param);
  protected double method(int param1, int param2);
  private void method();

21. **[Recommended]** The order of method definitions within a class is, in sequence: public or protected methods > private methods > getter / setter methods.
 Note: Public methods are what the class's callers and maintainers care about most, so it is best to display them on the first screen; protected methods, although only of concern to subclasses, may also be core methods under the "template design pattern"; private methods generally do not require special attention from outside and are a black-box implementation. Because they carry low informational value, all getter / setter methods of Service and DAO are placed at the very end of the class body.

22. **[Recommended]** In a setter method, the parameter name is consistent with the class member variable name, this.memberName = parameterName. In getter / setter methods, do not add business logic, as it increases the difficulty of troubleshooting.
 Counter-example:
  public Integer getData() {
       if (condition) {
           return this.data + 100;
      } else {
           return this.data - 100;

       }
   }

23. **[Recommended]** Within a loop body, use the append method of StringBuilder to concatenate strings.
  Counter-example:
   String str = "start";
   for (int i = 0; i < 100; i++) {
       str = str + "hello";
   }
  Note: The decompiled bytecode file shows that each loop iteration news up a StringBuilder object, then performs the append operation, and finally returns a String object via toString(), causing a waste of memory resources.

 24. **[Recommended]** final can declare classes, member variables, methods, and local variables. Use the final keyword in the following cases:
  1) Classes that are not allowed to be inherited, e.g., the String class.
  2) Domain objects whose reference is not allowed to be modified, e.g., domain variables of POJO classes.
  3) Methods that are not allowed to be overridden, e.g., the setter methods of POJO classes.
  4) Local variables that are not allowed to be reassigned during execution.
  5) Avoid reusing a variable repeatedly in context; using the final keyword can force redefining a variable, facilitating better refactoring.

 25. **[Recommended]** Use Object's clone method to copy objects with caution.
  Note: The object clone method is by default a shallow copy; to achieve a deep copy, you need to override the clone method to implement a deep, traversal-style copy of the domain objects.

 26. **[Recommended]** Keep access control of class members and methods strict:
  1) If external creation of objects directly via new is not allowed, then the constructor must be private.
  2) Utility classes must not have public or default constructors.
  3) A non-static member variable of a class that is shared with subclasses must be protected.
  4) A non-static member variable of a class that is used only within this class must be private.
  5) A static member variable of a class that is used only within this class must be private.
  6) If it is a static member variable, consider whether it should be final.
  7) A class member method that is only for internal use within the class must be private.
  8) A class member method that is only exposed to inheriting classes should be restricted to protected.
  Note: Strictly control the access scope of any class, method, parameter, or variable. An overly broad access scope is unfavorable for module decoupling. Think about it: if it is a private method, you can delete it whenever you want; but if it is a public service member method or member variable, wouldn't deleting it make your palms sweat a little? Variables are like your own children; keep them within your sight as much as possible. If a variable's scope is too large and it runs around everywhere without restriction, then you will worry.

(5) Date and Time

1. **[Mandatory]** When formatting dates, uniformly use lowercase y for the year in the passed-in pattern.
 Note: When formatting dates, yyyy represents the year of the current day, whereas uppercase YYYY represents week in which year (a concept introduced after JDK7), meaning the year to which the week of the current day belongs. A week starts on Sunday and ends on Saturday; as long as this week spans the year boundary, the returned YYYY will be the next year.
 Positive example: The format representing date and time is as follows:
       new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")
 Counter-example: A programmer used YYYY/MM/dd for date formatting, and 2017/12/31 produced the result 2018/12/31, causing an online failure.

2. **[Mandatory]** In date formats, clearly distinguish the meanings represented by uppercase M and lowercase m, and uppercase H and lowercase h respectively.
 Note: The meanings of these two pairs of letters in the date format are as follows:
  1) The month is uppercase M
  2) The minute is lowercase m
  3) The 24-hour clock is uppercase H
  4) The 12-hour clock is lowercase h

3. **[Mandatory]** Get the current milliseconds: System.currentTimeMillis(); not new Date().getTime().
  Note: To get nanosecond-level time, use the System.nanoTime approach. In JDK8, for scenarios such as time statistics, using the Instant class is recommended.

4. **[Mandatory]** It is not allowed to use anywhere in the program: 1) java.sql.Date 2) java.sql.Time 3) java.sql.Timestamp.
  Note: The first does not record time, and getHours() throws an exception; the second does not record the date, and getYear() throws an exception; the third, in the constructor super((time / 1000) * 1000), stores seconds and nanoseconds in the Timestamp attributes fastTime and nanos respectively.
  Counter-example: When using java.util.Date.after(Date) for time comparison, if the argument is a java.sql.Timestamp, it triggers a JDK BUG (fixed in JDK9), possibly leading to unexpected comparison results.

5. **[Mandatory]** It is forbidden to hardcode a year as 365 days in the program, to avoid date conversion errors or program logic errors during a Gregorian leap year.
  Positive example:
    // Get the number of days in this year
    int daysOfThisYear = LocalDate.now().lengthOfYear();
    // Get the number of days in a specified year
     LocalDate.of(2011, 1, 1).lengthOfYear();
  Counter-example:
    // First case: in a leap year with 366 days, an array out-of-bounds exception occurs
    int[] dayArray = new int[365];
    // Second case: a one-year membership, registered on January 26, 2020; hardcoding 365 returns January 25, 2021 instead
    Calendar calendar = Calendar.getInstance();
    calendar.set(2020, 1, 26);
    calendar.add(Calendar.DATE, 365);

6. **[Recommended]** Avoid the Gregorian leap-year February problem. February of a leap year has 29 days, and the same day one year later cannot possibly be February 29.

7. **[Recommended]** Use enum values to refer to months. If you use numbers, note that the month value for date-related classes such as Date and Calendar ranges from 0 to 11.
  Note: Refer to the JDK native comment, Month value is 0-based. e.g., 0 for January.
  Positive example: Use Calendar.JANUARY, Calendar.FEBRUARY, Calendar.MARCH, etc., to refer to the corresponding months for passing parameters or comparison.

(6) Collection Handling

1. **[Mandatory]** For handling hashCode and equals, follow these rules:
  1) Whenever equals is overridden, hashCode must be overridden.
  2) Because a Set stores non-duplicate objects, judged based on hashCode and equals, the objects stored in a Set must override these two methods.
  3) If a custom object is used as a Map key, then hashCode and equals must be overridden.
  Note: Because String overrides the hashCode and equals methods, you can happily use String objects as keys.

2. **[Mandatory]** To judge whether the internal elements of any collection are empty, use the isEmpty() method, not the size() == 0 approach.
  Note: In some collections, the time complexity of the former is O(1), and it is also more readable.
  Positive example:
     Map<String, Object> map = new HashMap<>(16);
    if (map.isEmpty()) {
        System.out.println("no element in this map.");
    }

3. **[Mandatory]** When using the toMap() method of the java.util.stream.Collectors class to convert to a Map collection, you must use the method with a parameter of type BinaryOperator and parameter name mergeFunction, otherwise an IllegalStateException will be thrown when identical keys appear.
  Note: The purpose of the mergeFunction parameter is to customize the handling strategy for the value when a duplicate key appears.
  Positive example:

   List<Pair<String, Double>> pairArrayList = new ArrayList<>(3);
   pairArrayList.add(new Pair<>("version", 12.10));
   pairArrayList.add(new Pair<>("version", 12.19));
   pairArrayList.add(new Pair<>("version", 6.28));

   // The generated map collection has only one key-value pair: {version=6.28}
   Map<String, Double> map = pairArrayList.stream()
         .collect(Collectors.toMap(Pair::getKey, Pair::getValue, (v1, v2) -> v2));
Counter-example:
   String[] departments = new String[]{"RDC", "RDC", "KKB"};
   // Throws an IllegalStateException
   Map<Integer, String> map = Arrays.stream(departments)
         .collect(Collectors.toMap(String::hashCode, str -> str));

4. **[Mandatory]** When using the toMap() method of the java.util.stream.Collectors class to convert to a Map collection, be sure to note that when value is null, an NPE will be thrown.
Note: The merge method in java.util.HashMap performs the following check:
   if (value == null || remappingFunction == null)
       throw new NullPointerException();
Counter-example:
   List<Pair<String, Double>> pairArrayList = new ArrayList<>(2);
   pairArrayList.add(new Pair<>("version1", 8.3));
   pairArrayList.add(new Pair<>("version2", null));

   // Throws a NullPointerException
   Map<String, Double> map = pairArrayList.stream()
         .collect(Collectors.toMap(Pair::getKey, Pair::getValue, (v1, v2) -> v2));

5. **[Mandatory]** The result of ArrayList's subList must not be cast to ArrayList, otherwise it will throw a ClassCastException:
java.util.RandomAccessSubList cannot be cast to java.util.ArrayList.
Note: subList() returns SubList, an inner class of ArrayList, not ArrayList itself, but a view of ArrayList. All operations on the SubList will ultimately be reflected in the original list.

6. **[Mandatory]** When using the Map methods keySet() / values() / entrySet() to return a collection object, you must not perform an add-element operation on it, otherwise it will throw an UnsupportedOperationException.

7. **[Mandatory]** Objects returned by the Collections class, such as emptyList() / singletonList(), are all immutable lists; you must not perform add or remove element operations on them.
Counter-example: If a query yields no results and returns the empty collection object Collections.emptyList(), once the caller performs an add-element operation on the returned collection, it will trigger an UnsupportedOperationException.

8. **[Mandatory]** In subList scenarios, pay close attention: any addition or deletion of elements in the parent collection will cause the traversal, addition, or deletion of the sublist to produce a ConcurrentModificationException.
Note: A spot check shows that 90% of programmers have an incorrect understanding of this knowledge point.

9. **[Mandatory]** To convert a collection to an array, you must use the collection's toArray(T[] array), passing in an empty array of exactly the same type and length 0.
Counter-example: Directly using the no-argument toArray method has a problem; the return value of this method can only be of type Object[], and casting it to another type of array will produce a ClassCastException.
Positive example:
   List<String> list = new ArrayList<>(2);

  list.add("guan");
  list.add("bao");
  String[] array = list.toArray(new String[0]);
Note: When using the toArray method with an argument, regarding the array's space size length:
     1) Equal to 0: dynamically creates an array of the same size as size, with the best performance.
     2) Greater than 0 but less than size: recreates an array of size equal to size, increasing the GC burden.
     3) Equal to size: under high concurrency, after the array is created, if size is growing, the negative effect is the same as 2.
     4) Greater than size: wastes space, and inserts a null value at the size position, posing an NPE risk.

10. **[Mandatory]** When using the addAll() method of any implementation class of the Collection interface, perform an NPE check on the input collection parameter.
 Note: The first line of code in the ArrayList#addAll method is Object[] a = c.toArray(); where c is the input collection parameter; if it is null, it directly throws an exception.

11. **[Mandatory]** When using the utility method Arrays.asList() to convert an array into a collection, you must not use its collection-modifying methods; its add / remove / clear methods will throw an UnsupportedOperationException.
Note: The object returned by asList is an Arrays inner class and does not implement the collection's modification methods. Arrays.asList embodies the adapter pattern; it only converts the interface, while the backing data is still an array.
     String[] str = new String[]{ "yang", "guan", "bao" };
     List list = Arrays.asList(str);
First case: list.add("yangguanbao"); a runtime exception.
Second case: str[0] = "change"; the elements in list will be modified accordingly, and vice versa.

12. **[Mandatory]** The generic wildcard <? extends T> is used to receive returned data; a generic collection written this way cannot use the add method, whereas <? super T> cannot use the get method. Both are prone to errors in scenarios of interface call and assignment.
 Note: Let me elaborate on the PECS (Producer Extends Consumer Super) principle: those that frequently read content outward are suitable for <? extends T>, and those that frequently insert inward are suitable for <? super T>.

13. **[Mandatory]** When assigning a collection defined without generic restrictions to a collection with generic restrictions, when using the collection elements, an instanceof check is needed to avoid throwing a ClassCastException.
 Note: After all, generics only appeared after JDK5; considering forward compatibility, the compiler allows non-generic and generic collections to be assigned to each other.
 Counter-example:
  List<String> generics = null;
  List notGenerics = new ArrayList(10);
  notGenerics.add(new Object());
  notGenerics.add(new Integer(1));
  generics = notGenerics;
  // A ClassCastException is thrown here
  String string = generics.get(0);

14. **[Mandatory]** Do not perform remove / add operations on elements within a foreach loop. To remove an element, please use the iterator approach; if operating concurrently, you need to lock the iterator object.
 Positive example:
  List<String> list = new ArrayList<>();
  list.add("1");
  list.add("2");
  Iterator<String> iterator = list.iterator();
  while (iterator.hasNext()) {
      String item = iterator.next();
      if (condition for deleting the element) {
           iterator.remove();
      }

 }
Counter-example:
 for (String item : list) {
      if ("1".equals(item)) {
            list.remove(item);
      }
 }
Note: The execution result of the counter-example will surely be beyond everyone's expectations; now try replacing "1" with "2" — will the result be the same?

15. **[Mandatory]** In JDK7 and above, a Comparator implementation class must satisfy the following three conditions, otherwise Arrays.sort and Collections.sort will throw an IllegalArgumentException.
Note: The three conditions are as follows
 1) The comparison result of x, y is the opposite of the comparison result of y, x.
2) If x > y and y > z, then x > z.
3) If x = y, then the comparison result of x, z is the same as the comparison result of y, z.
Counter-example: The example below does not handle the equal case; swapping the two objects does not produce mutually inverse judgment results, which does not satisfy the first condition, and may produce exceptions in actual use.
 new Comparator<Student>() {
      @Override
      public int compare(Student o1, Student o2) {
            return o1.getId() > o2.getId() ? 1 : -1;
      }
 };

16. **[Recommended]** When using generic collections, in JDK7 and above, use the diamond syntax or omit entirely.
Note: The diamond generic, i.e., diamond, directly uses <> to refer to the previously specified type.
Positive example:
 // diamond style, i.e., <>
 HashMap<String, String> userCache = new HashMap<>(16);
 // fully omitted style
 ArrayList<User> users = new ArrayList(10);

17. **[Recommended]** When initializing a collection, specify the initial size of the collection.
Note: When initializing a HashMap using the constructor HashMap(int initialCapacity), if the collection size cannot be determined for the time being, then specifying the default value (16) is sufficient.
Positive example: initialCapacity = (number of elements to be stored / load factor) + 1. Note that the load factor (i.e., loaderfactor) defaults to 0.75; if the initial size cannot be determined for the time being, please set it to 16 (i.e., the default value).
Counter-example: A HashMap needs to hold 1024 elements, but because the initial capacity size is not set, it is forced to keep expanding as elements increase. The resize() method is called a total of 8 times, repeatedly rebuilding the hash table and migrating data. When the number of collection elements reaches the tens of millions, it affects program performance.

18. **[Recommended]** Use entrySet to traverse the KV of a Map collection, rather than the keySet approach.
Note: keySet actually traverses twice: once to convert to an Iterator object, and once to retrieve the value corresponding to the key from the hashMap. Whereas entrySet traverses only once and puts both the key and value into the entry, which is more efficient. If it is JDK8, use the Map.forEach method.
Positive example: values() returns the collection of V values, which is a list collection object; keySet() returns the collection of K values, which is a Set collection object; entrySet() returns the Set collection of K-V combinations.

19. **[Recommended]** Pay close attention to whether the K / V of a Map collection can store null values, as shown in the table below:

           Collection Class             Key              Value       Super      Note

          Hashtable           null not allowed     null not allowed     Dictionary   thread-safe

           TreeMap            null not allowed     null allowed         AbstractMap        not thread-safe

   ConcurrentHashMap          null not allowed     null not allowed     AbstractMap   segmented locking technique (JDK8: CAS)

           HashMap            null allowed         null allowed         AbstractMap        not thread-safe

  Counter-example: Due to the interference of HashMap, many people think ConcurrentHashMap can store null values, but in fact, storing a null value will throw an NPE.

20. **[Reference]** Make reasonable use of a collection's sortedness (sort) and stability (order), avoiding the negative effects brought by a collection's unsortedness (unsort) and instability (unorder).
  Note: Sortedness means the traversal result is arranged in sequence according to some comparison rule; stability means the order of elements in each traversal of the collection is fixed. For example: ArrayList is order / unsort; HashMap is unorder / unsort; TreeSet is order / sort.

21. **[Reference]** Leveraging the unique-element property of a Set, you can quickly deduplicate a collection, avoiding using List's contains() for traversal deduplication or containment judgment.

(7) Concurrency Handling

1. **[Mandatory]** Obtaining a singleton object needs to guarantee thread safety, and the methods within it must also guarantee thread safety.
 Note: Resource-driven classes, utility classes, and singleton factory classes all need attention.

2. **[Mandatory]** When creating a thread or thread pool, please specify a meaningful thread name to facilitate tracing when errors occur.
 Positive example: Customize a thread factory and group it according to external characteristics. For example, for calls coming from the same data center, assign the data center number to whatFeatureOfGroup:
   public class UserThreadFactory implements ThreadFactory {
       private final String namePrefix;
       private final AtomicInteger nextId = new AtomicInteger(1);
       // Define the thread group name; very helpful when using jstack to troubleshoot problems
       UserThreadFactory(String whatFeatureOfGroup) {
             namePrefix = "FromUserThreadFactory's" + whatFeatureOfGroup + "-Worker-";
       }
       @Override
       public Thread newThread(Runnable task) {
             String name = namePrefix + nextId.getAndIncrement();
             Thread thread = new Thread(null, task, name, 0, false);
             System.out.println(thread.getName());
             return thread;
       }
   }

 3. **[Mandatory]** Thread resources must be provided through a thread pool; it is not allowed to explicitly create threads on your own within the application.
 Note: The benefit of a thread pool is reducing the time spent on creating and destroying threads as well as the system resource overhead, solving the problem of insufficient resources. If a thread pool is not used, it may cause the system to create a large number of similar threads, leading to exhausting memory or "excessive switching" problems.

4. **[Mandatory]** Thread pools are not allowed to be created using Executors, but rather through the ThreadPoolExecutor approach. This way of handling makes the rules of the thread pool's operation clearer to the writer, avoiding the risk of resource exhaustion.
 Note: The drawbacks of the thread pool objects returned by Executors are as follows:
 1) FixedThreadPool and SingleThreadPool:
 The allowed request queue length is Integer.MAX_VALUE, which may accumulate a large number of requests, thereby leading to OOM.
 2) CachedThreadPool:
 The allowed number of created threads is Integer.MAX_VALUE, which may create a large number of threads, thereby leading to OOM.
 3) ScheduledThreadPool:

 The allowed request queue length is Integer.MAX_VALUE, which may accumulate a large number of requests, thereby leading to OOM.

5. **[Mandatory]** SimpleDateFormat is a thread-unsafe class; generally do not define it as a static variable. If defined as static, you must add a lock, or use the DateUtils utility class.
 Positive example: Pay attention to thread safety; use DateUtils. The following handling is also recommended:
 private static final ThreadLocal<DateFormat> dateStyle = new ThreadLocal<DateFormat>() {
          @Override
          protected DateFormat initialValue() {
                   return new SimpleDateFormat("yyyy-MM-dd");
          }
 };
 Note: If it is a JDK8 application, you can use Instant instead of Date, LocalDateTime instead of Calendar, and DateTimeFormatter instead of SimpleDateFormat. The official explanation given: simple beautiful strong immutable thread-safe.

6. **[Mandatory]** You must reclaim the current-thread value recorded by a custom ThreadLocal variable, especially in thread-pool scenarios, where threads are often reused. If a custom ThreadLocal variable is not cleaned up, it may affect subsequent business logic and cause problems such as memory leaks. Try to use a try-finally block in the code to reclaim it.
 Positive example:
 objectThreadLocal.set(userInfo);
 try {
          // ...
 } finally {
          objectThreadLocal.remove();
 }

7. **[Mandatory]** Under high concurrency, synchronous calls should consider the performance overhead of locks. If a lock-free data structure can be used, do not use a lock; if a block can be locked, do not lock the entire method body; if an object lock can be used, do not use a class lock.
 Note: Make the workload of the locked code block as small as possible, and avoid calling RPC methods within a locked code block.

8. **[Mandatory]** When locking multiple resources, database tables, or objects at the same time, you need to maintain a consistent locking order, otherwise a deadlock may occur.
 Note: If thread one needs to lock tables A, B, and C all in sequence before it can perform an update operation, then thread two's locking order must also be A, B, C, otherwise a deadlock may occur.

9. **[Mandatory]** When using the blocking-wait approach to acquire a lock, the lock acquisition must be outside the try block, and there must be no method calls between the locking method and the try block that could possibly throw an exception, to avoid being unable to unlock in finally after successfully acquiring the lock.
 Note 1: If a method call between the lock method and the try block throws an exception, it cannot be unlocked, causing other threads to be unable to successfully acquire the lock.
 Note 2: If the lock method is inside the try block, an exception thrown by another method may cause unlock in the finally block to unlock an object that has not been locked. It will call the tryRelease method of AQS (depending on the specific implementation class), throwing an IllegalMonitorStateException.
 Note 3: The lock method implementation of a Lock object may throw an unchecked exception, with the same consequences as Note 2.
 Positive example:
 Lock lock = new XxxLock();
 // ...
 lock.lock();
 try {
          doSomething();
          doOthers();
 } finally {
          lock.unlock();
 }
 Counter-example:
 Lock lock = new XxxLock();

 // ...
 try {
          // If an exception is thrown here, the finally block executes directly
          doSomething();
          // Regardless of whether locking succeeds, the finally block will execute
          lock.lock();
          doOthers();
 } finally {
          lock.unlock();
 }

10. **[Mandatory]** When using the try-mechanism approach to acquire a lock, before entering the business code block, you must first judge whether the current thread holds the lock. The lock-release rule is the same as the blocking-wait approach to locking.
Note: When the unlock method of a Lock object executes, it calls the tryRelease method of AQS (depending on the specific implementation class); if the current thread does not hold the lock, it throws an IllegalMonitorStateException.
Positive example:
 Lock lock = new XxxLock();
 // ...
 boolean isLocked = lock.tryLock();
 if (isLocked) {
          try {
                  doSomething();
                  doOthers();
          } finally {
                  lock.unlock();
          }
 }

11. **[Mandatory]** When concurrently modifying the same record, to avoid lost updates, locking is needed. Either lock at the application layer, or lock at the cache, or use an optimistic lock at the database layer, using version as the basis for updating.
Note: If the probability of access conflict each time is less than 20%, an optimistic lock is recommended; otherwise, use a pessimistic lock. The number of retries for an optimistic lock must not be less than 3.

12. **[Mandatory]** When multiple threads process scheduled tasks in parallel, when a Timer runs multiple TimeTasks, as long as one of them does not catch a thrown exception, the other tasks will automatically terminate; using ScheduledExecutorService does not have this problem.

13. **[Recommended]** For funds-related, financially sensitive information, use a pessimistic lock strategy.
Note: An optimistic lock has already completed the update operation while acquiring the lock; the verification logic is prone to loopholes. Additionally, an optimistic lock has rather complex requirements for the conflict resolution strategy; improper handling easily causes system pressure or data anomalies, so optimistic-lock updates are not recommended for funds-related, financially sensitive information.
Positive example: A pessimistic lock follows the principle of first lock, second judge, third update, fourth release.

14. **[Recommended]** When using CountDownLatch to perform an async-to-sync operation, each thread must call the countDown method before exiting, and the thread's executing code should catch exceptions to ensure the countDown method is reached, avoiding the situation where the main thread cannot proceed to the await method and does not return a result until timeout.
Note: Note that the exception stack thrown by a child thread cannot be caught by try-catch in the main thread.

15. **[Recommended]** Avoid a Random instance being used by multiple threads. Although sharing this instance is thread-safe, it will cause performance degradation due to contention over the same seed.
Note: Random instances include instances of java.util.Random or the Math.random() approach.
Positive example: After JDK7, you can directly use the API ThreadLocalRandom; before JDK7, you need to code so that each thread holds a separate Random instance.

16. **[Recommended]** To implement lazy initialization via double-checked locking, you need to declare the target attribute as volatile (for example, modify the helper attribute declaration to private volatile Helper helper = null;).
   Positive example:
    public class LazyInitDemo {
        private volatile Helper helper = null;
        public Helper getHelper() {
            if (helper == null) {
                 synchronized(this) {
                     if (helper == null) {
                         helper = new Helper();
                     }
                 }
            }
            return helper;
        }
        // other methods and fields...
    }

17. **[Reference]** volatile solving the multi-thread memory invisibility problem can solve the variable synchronization problem for one-write-multiple-read, but if there are multiple writes, it likewise cannot solve the thread-safety problem.
   Note: If it is a count++ operation, use the following class to implement it:
   AtomicInteger count = new AtomicInteger();
   count.addAndGet(1);
   If it is JDK8, using the LongAdder object is recommended, which has better performance than AtomicLong (reducing the number of optimistic-lock retries).

18. **[Reference]** When a HashMap resizes due to insufficient capacity, under high concurrency a circular linked list may appear, causing the CPU to spike. Pay attention to avoiding this risk during development.

19. **[Reference]** ThreadLocal objects are modified with static; ThreadLocal cannot solve the update problem of shared objects.
   Note: This variable is shared by all operations within a single thread, so it is set as a static variable. All instances of this class share this static variable, meaning it is loaded the first time the class is used, allocating only one block of storage space, and all objects of this class (as long as they are defined within this thread) can manipulate this variable.

(8) Control Statements

1. **[Mandatory]** Within a switch block, each case must either be terminated via continue / break / return, etc., or have a comment explaining which case the program will continue to execute to; within a switch block, a default statement must be included and placed at the end, even if it has no code at all.
  Note: Note that break exits the switch statement block, whereas return exits the method body.

2. **[Mandatory]** When the type of the variable inside the switch parentheses is String and this variable is an external parameter, a null check must be performed first.
  Counter-example: What is the output of the following code?
    public class SwitchString {
        public static void main(String[] args) {
            method(null);
        }
        public static void method(String param) {
            switch (param) {
                // Definitely does not enter here
                 case "sth":

                       System.out.println("it's sth");
                       break;
                 // Also does not enter here
                 case "null":
                       System.out.println("it's null");
                       break;
                 // Also does not enter here
                 default:
                       System.out.println("default");
             }
       }
   }

3. **[Mandatory]** Braces must be used in if / else / for / while / do statements.
Counter-example: if (condition) statements;
Note: Even if there is only one line of code, use the brace coding style.

4. **[Mandatory]** In the ternary operator condition ? expression 1 : expression 2, pay close attention: when expressions 1 and 2 are type-aligned, an NPE may be thrown due to auto-unboxing.
Note: The following two scenarios trigger the type-alignment unboxing operation:
1) The value of expression 1 or expression 2 is a primitive type.
2) When the value types of expression 1 and expression 2 are inconsistent, they are forcibly unboxed and promoted to the type with the larger representation range.
Counter-example:
   Integer a = 1;
   Integer b = 2;
   Integer c = null;
   Boolean flag = false;
   // The result of a*b is of type int, so c will be forcibly unboxed into type int, throwing an NPE
   Integer result = (flag ? a * b : c);

5. **[Mandatory]** In high-concurrency scenarios, avoid using "equal-to" judgment as a condition for interruption or exit.
Note: If concurrency control is not handled well, an equality judgment is prone to being "broken through"; use a greater-than or less-than range judgment condition instead.
Counter-example: When the remaining prize quantity is judged to equal 0, prize distribution is terminated, but because of an error in concurrent handling, the prize quantity instantly becomes a negative number, in which case the event cannot be terminated.

6. **[Recommended]** When the total number of lines of code in a method exceeds 10 lines, a blank line must be added after the right brace of interruption logic such as return / throw.
Note: Doing so makes the logic clear and facilitates focusing on the key points when reading the code.

7. **[Recommended]** When expressing an exceptional branch, use the if-else approach less; this approach can be rewritten as:
   if (condition) {
       ...
       return obj;
   }
   // Then write the business logic code for else;
Note: If you must use the if()...else if()...else... approach to express the logic, to avoid difficulty in subsequent code maintenance, please do not exceed 3 levels.
Positive example: Logic judgment code with more than 3 levels of if-else can be implemented using guard clauses, the strategy pattern, the state pattern, etc. An example of a guard clause is as follows:
   public void findBoyfriend(Man man) {
       if (man.isUgly()) {
             System.out.println("I am a senior member of the looks-matter club");
             return;
       }

     if (man.isPoor()) {
           System.out.println("A poor couple grieves over everything");
           return;
     }
     if (man.isBadTemper()) {
           System.out.println("As far as the galaxy is, that's how far you should get lost from me");
           return;
     }
     System.out.println("We can date for a while first and see");
 }

8. **[Recommended]** Apart from common methods (such as getXxx / isXxx), do not execute other complex statements in a conditional judgment; assign the result of a complex logical judgment to a meaningfully named boolean variable to improve readability.
 Note: The logical expressions inside many if statements are quite complex, mixing AND, OR, and NOT operations, and even various deep method calls, with a very high cost of comprehension. If you assign it to a very understandable boolean variable name, it is a pleasing matter.
 Positive example:
 // Pseudocode as follows
 final boolean existed = (file.open(fileName, "w") != null) && (...) || (...);
 if (existed) {
     ...
 }
 Counter-example:
 public final void acquire(long arg) {
     if (!tryAcquire(arg) && acquireQueued(addWaiter(Node.EXCLUSIVE), arg)) {
           selfInterrupt();
     }
 }

9. **[Recommended]** Do not insert an assignment statement into another expression (especially a conditional expression).
 Note: Assignment points are like the acupoints of the human body, crucial for understanding the code, so assignment statements need to clearly stand alone on their own line.
 Counter-example:
 public Lock getLock(boolean fair) {
     // An assignment operation appears in an arithmetic expression; it is easy to overlook that the count value has already been changed
     threshold = (count = Integer.MAX_VALUE) - 1;
     // An assignment operation appears in a conditional expression; it is easy to mistake it for sync == fair
     return (sync = fair) ? new FairSync() : new NonfairSync();
 }

10. **[Recommended]** Statements within a loop body should consider performance; try to move the following operations outside the loop body for handling, such as defining objects and variables, obtaining a database connection, and performing unnecessary try-catch operations (can this try-catch be moved outside the loop body?).

11. **[Recommended]** Avoid using the negation logical operator.
Note: Negation logic is unfavorable for quick understanding, and negation-logic writing generally has a corresponding positive-logic writing.
Positive example: Use if(x < 628) to express x being less than 628.
Counter-example: Use if(!(x >= 628)) to express x being less than 628.

12. **[Recommended]** Public interfaces need to perform input-parameter protection, especially interfaces for batch operations.
Counter-example: A business system provides an interface for batch querying users; the API documentation states the maximum number that can be queried, but the interface implementation does no protection at all, so when the caller passed an array of 1000 user ids, after querying the information, memory blew up.

13. **[Reference]** In the following situations, parameter validation needs to be performed:

  1) Methods called with low frequency.
  2) Methods with a very high execution time cost. In this situation, the parameter validation time is almost negligible, but if a parameter error causes a rollback or error midway through execution, it is not worth it.
  3) Methods requiring extremely high stability and availability.
  4) Open interfaces provided externally, whether RPC / API / HTTP interfaces.
  5) Sensitive permission entry points.

14. **[Reference]** In the following situations, parameter validation does not need to be performed:
  1) Methods that are very likely to be called in a loop. But the external parameter check must be noted in the method documentation.
  2) Methods called at the underlying level with relatively high frequency. After all, it is like the last stage of pure-water filtration; a parameter error is unlikely to expose a problem only at the underlying level. Generally, the DAO layer and Service layer are in the same application, deployed on the same server, so the parameter validation of the DAO can be omitted.
  3) Methods declared as private that are only called by their own code; if it can be determined that the code of the calling method has already checked the input parameters or that there will definitely be no problem, then parameter validation may be skipped at this point.

(9) Comment Specification

1. **[Mandatory]** Comments for classes, class attributes, and class methods must use the Javadoc specification, using the /** content */ format; the // xxx approach must not be used.
 Note: In the IDE editing window, the Javadoc style prompts related comments, and generating Javadoc can correctly output the corresponding comments; in the IDE, when a method is called by the project, even without entering the method, hovering prompts the meaning of the method, parameters, and return value, improving reading efficiency.

2. **[Mandatory]** All abstract methods (including methods in interfaces) must use Javadoc comments; in addition to explaining the return value, parameters, and exceptions, they must also point out what the method does and what function it implements.
 Note: Please also explain the implementation requirements for subclasses, or the precautions for calling.

3. **[Mandatory]** All classes must add the creator and creation date.
 Note: When setting the template, note that the @author for IDEA is `${USER}`, whereas the @author for eclipse is `${user}`; there is a difference in case, and the date setting is uniformly in the yyyy/MM/dd format.
 Positive example:
    /**
     *
     * @author yangguanbao
     * @date 2021/11/26
     *
     **/

4. **[Mandatory]** A single-line comment inside a method starts a new line above the statement being commented, using // for the comment. Multi-line comments inside a method use /* */ comments, noting to align with the code.

5. **[Mandatory]** All enum-type fields must have comments explaining the purpose of each data item.

6. **[Recommended]** Rather than commenting with half-baked English, it is better to use Chinese comments to explain clearly. Proper nouns and keywords can be kept in their original English.
 Counter-example: Explaining "TCP connection timeout" as "Transmission Control Protocol connection timeout" instead makes it more brain-taxing to understand.

7. **[Recommended]** When code is modified, the comments must be modified accordingly, especially parameters, return values, exceptions, core logic, etc.
 Note: Code and comments being out of sync in updates is like a road network and navigation software being out of sync in updates; if the navigation software is severely lagging, it loses the meaning of navigation.

8. **[Recommended]** Delete any unused fields, methods, and inner classes in a class; delete unused parameter declarations and internal variables in a method.

9. **[Reference]** Comment out code with caution. Explain in detail above it, rather than simply commenting it out. If it is useless, then delete it.
 Note: Code being commented out has two possibilities: 1) this code logic will be restored later. 2) it will never be used again. For the former, if there is no note, it is hard to know the motivation for commenting. For the latter, it is recommended to delete it directly; if you need to consult historical code, just log in to the code repository.

 10. **[Reference]** Requirements for comments: First, they should accurately reflect the design philosophy and code logic; second, they should describe the business meaning, enabling other programmers to quickly understand the information behind the code. A large block of code with no comments at all is like a heavenly book to the reader. Comments are for yourself to read, so that even after a long time, you can still clearly understand the thinking at the time; comments are also for your successors to read, enabling them to quickly take over your work.

 11. **[Reference]** Good naming and code structure are self-documenting; comments should strive to be concise, accurate, and to the point. Avoid the other extreme of comments: too many and excessive comments. Once the code logic is modified, modifying the comments is also quite a burden.
  Counter-example:
   // put elephant into fridge
   put(elephant, fridge);
  The method name put, plus two meaningful variable names elephant and fridge, already explain what this is doing; semantically clear code does not need extra comments.

 12. **[Reference]** For special comment markers, please note the marker person and marking time. Pay attention to handling these markers promptly; through marker scanning, regularly clean up such markers. Online failures sometimes originate from the code at these markers.
  1) To-do item (TODO): (marker person, marking time, [expected handling time]) indicates a function that needs to be implemented but is not yet implemented. This is actually a Javadoc tag; the current Javadoc has not yet implemented it, but it is already widely used. It can only be applied to classes, interfaces, and methods (because it is a Javadoc tag).
  2) Error, does not work (FIXME): (marker person, marking time, [expected handling time]) using FIXME in a comment to mark that some code is wrong and does not work, and needs to be corrected promptly.

(10) Front-end/Back-end Specification

 1. **[Mandatory]** For the API of front-end/back-end interaction, the protocol, domain name, path, request method, request content, status code, and response body need to be clearly defined.
 Note:
 1) Protocol: the production environment must use HTTPS.
 2) Path: each API needs to correspond to one path, representing the specific request address of the API:
   a) Represents a kind of resource, can only be a noun, plural is recommended, cannot be a verb; the request method already expresses the action meaning.
   b) The URL path cannot use uppercase; if words need to be separated, uniformly use the underscore.
   c) The path is forbidden to carry a suffix indicating the request content type, such as ".json", ".xml"; express it via the accept header.
 3) Request method: the definition of specific operations; common request methods are as follows:
   a) GET: retrieve a resource from the server.
   b) POST: create a new resource on the server.
   c) PUT: update a resource on the server.
   d) DELETE: delete a resource from the server.
 4) Request content: parameters carried by the URL must contain no sensitive information or comply with security requirements; when parameters are carried in the body, Content-Type must be set.
 5) Response body: the response body can hold multiple data types, determined by the Content-Type header.

 2. **[Mandatory]** For front-end/back-end data-list-related interface returns, if empty, return an empty array [] or empty collection {}.
 Note: This convention facilitates more efficient collaboration at the data level and reduces a lot of trivial null checks on the front end.

 3. **[Mandatory]** When a server-side error occurs, the response information returned to the front end must contain four parts: the HTTP status code, errorCode, errorMessage, and a user-facing prompt message.
 Note: The stakeholders of the four parts are respectively the browser, front-end development, error-troubleshooting personnel, and the user. Among them, the prompt message output to the user is required to be: short and clear, friendly, guiding the user to the next operation or explaining the cause of the error; the prompt message can include the cause of the error, contextual environment, recommended operations, etc.
 errorCode: refer to       . errorMessage: briefly describe the cause of the back-end error to facilitate error-troubleshooting personnel quickly locating the problem; note not to include sensitive data information.
 Positive example: Common HTTP status codes are as follows
 1) 200 OK: indicates that the request was successfully completed and the requested resource was sent to the client.

 2) 401 Unauthorized: the request requires authentication, common for cases where login is required but the user is not logged in.
 3) 403 Forbidden: the server refused the request, common for confidential information or for accessing the server by copying another logged-in user's link.
 4) 404 NotFound: the server could not get the requested web page; the requested resource does not exist.
 5) 500 InternalServerError: server internal error.

4. **[Mandatory]** In the JSON-format data of front-end/back-end interaction, all keys must be in the lowerCamelCase style starting with a lowercase letter, conforming to English expression conventions, and fully expressing the meaning.
 Positive example: errorCode / errorMessage / assetStatus / menuList / orderList / configFlag
 Counter-example: ERRORCODE / ERROR_CODE / error_message / error-message / errormessage

5. **[Mandatory]** errorMessage is the embodiment of the front-end/back-end error-tracking mechanism; it can be output on the front end into a type="hidden" text control, or into the user-side logs, helping us quickly locate problems.

6. **[Mandatory]** For scenarios requiring very large integers, the server side uniformly returns the String type; using the Long type is forbidden.
 Note: If the Java server side directly returns Long integer data to the front end, JavaScript will automatically convert it to the Number type (note: this type is a double-precision floating-point number, whose representation principle and value range are equivalent to Double in Java). The maximum value that the Long type can represent is 2^63-1; within the value range, when a value exceeding 2^53 (9007199254740992) is converted to JavaScript's Number, some values will suffer precision loss. As an extended note, within the Long value range, any integer that is a power of 2 will absolutely never have precision loss, so precision loss is a probability problem. If the space of the floating-point mantissa bits and exponent bits were unlimited, then any integer could be precisely represented, but unfortunately, the mantissa of a double-precision floating-point number has only 52 bits.
 Counter-example: Usually when an order number or transaction number is greater than or equal to 16 digits, there is a high probability that the front-end and back-end order data will be inconsistent.
 For example, the back end transmits "orderId": 362909601374617692, but the value the front end gets is: 362909601374617660

7. **[Mandatory]** When an HTTP request passes parameters via the URL, it cannot exceed 2048 bytes.
 Note: Different browsers have slightly different maximum length limits for URLs, and also differ in the handling logic for exceeding the maximum length; 2048 bytes is the minimum value taken across all browsers.
 Counter-example: A business put the list of returned-goods item ids in the URL as parameters to pass; when the quantity of returned goods in one return was too large, the URL parameters were over-length, and the parameters passed to the back end were truncated, causing some goods to not be correctly returned.

8. **[Mandatory]** When an HTTP request passes content via the body, the length must be controlled; after exceeding the maximum length, back-end parsing will error.
 Note: nginx's default limit is 1MB, tomcat's default limit is 2MB; when there is indeed a business need to pass larger content, you can increase the server-side limit.

9. **[Mandatory]** In pagination scenarios, if the parameter input by the user is less than 1, the front end returns the first-page parameter to the back end; if the back end finds that the parameter input by the user is greater than the total number of pages, it directly returns the last page.

10. **[Mandatory]** Internal server redirects must use forward; external redirect addresses must be generated using a unified URL proxy module, otherwise, because the online environment uses the HTTPS protocol, the browser will prompt "not secure", and it will also bring the problem of inconsistent URL maintenance.

11. **[Recommended]** The information returned by the server must be marked as to whether it can be cached; if cached, the client may reuse the result of the previous request.
Note: Caching helps reduce the number of interactions and reduce the average latency of interactions.
Positive example: In HTTP 1.1, s-maxage tells the server to cache, with the time unit in seconds, used as follows,
      response.setHeader("Cache-Control", "s-maxage=" + cacheSeconds);

12. **[Recommended]** Data returned by the server uses the JSON format rather than XML.
Note: Although HTTP supports using different output formats, such as plain text, JSON, CSV, XML, RSS, and even HTML. If we are using a user-facing service, we should choose JSON as the standard data exchange format used in communication, including requests and responses. Furthermore, application/JSON is a universal MIME type, with the characteristics of being practical, concise, and easy to read.

13. **[Recommended]** The time format for front-end/back-end is uniformly "yyyy-MM-dd HH:mm:ss", uniformly GMT.

14. **[Reference]** Do not add a version number in the interface path; version control is reflected in the HTTP header information, which is favorable for forward compatibility.
Note: When a user repeatedly switches work between a lower version and a higher version, it will increase the migration complexity, posing a risk of data confusion.

(11) Miscellaneous

1. **[Mandatory]** When using regular expressions, make good use of their precompilation function, which can effectively speed up regex matching.
  Note: Do not define within a method body: Pattern pattern = Pattern.compile("rule");

2. **[Mandatory]** Avoid using ApacheBeanutils to copy attributes.
  Note: ApacheBeanUtils has relatively poor performance; you can use other solutions such as SpringBeanUtils, CglibBeanCopier; note that all are shallow copies.

3. **[Mandatory]** When velocity calls the attributes of a POJO class, simply use the attribute name to get the value; the template engine will automatically call the POJO's getXxx() per the specification; if it is a boolean primitive data type variable (boolean naming does not need the is prefix), it will automatically call the isXxx() method.
  Note: Note that if it is a Boolean wrapper class object, the getXxx() method is preferentially called.

4. **[Mandatory]** Variables fed from the back end to the page must add $!{var} — with an exclamation mark in the middle.
  Note: If var equals null or does not exist, then ${var} will be displayed directly on the page.

5. **[Mandatory]** Note that the Math.random() method returns the double type; note the value range 0 ≤ x < 1 (it can take the value zero; watch out for divide-by-zero exceptions). If you want to get an integer-type random number, do not scale x up by some multiple of 10 and then round; directly use the nextInt or nextLong method of the Random object.

6. **[Mandatory]** The attribute fields of an enum (within the parentheses) must be private and immutable.

7. **[Recommended]** Do not put any complex logical operations into a view template.
  Note: According to MVC theory, the responsibility of the view is presentation; do not take over the work of the model and controller.

8. **[Recommended]** Any construction or initialization of a data structure should specify a size, to avoid the data structure growing infinitely and eating up memory.

9. **[Recommended]** Promptly clean up code segments or configuration information that are no longer used.
  Note: For garbage code or obsolete configuration, resolutely clean it up thoroughly, to avoid the program becoming overly bloated with redundant code.
  Positive example: For code fragments that are temporarily commented out and may be restored for use later, above the commented code, it is uniformly stipulated to use three slashes (///) to explain the reason for commenting out the code:
   public static void hello() {
         /// The business party notified that the event is paused
         // Business business = new Business();
         // business.active();
         System.out.println("it's finished");
   }
