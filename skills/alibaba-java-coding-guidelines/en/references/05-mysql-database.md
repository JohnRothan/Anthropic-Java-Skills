# 5. MySQL Database

> From the Alibaba Java Development Manual (Huangshan Edition). Severity levels: [Mandatory] must follow · [Recommended] should follow · [Reference] good practice for reference.

(1) Table Creation Specification

1. **[Mandatory]** Fields that express a yes/no concept must be named using the is_xxx convention, with the data type being unsigned tinyint (1 means yes, 0 means no).
  Note: Any boolean variable in a POJO class should not carry the is prefix, so a mapping from is_xxx to Xxx must be configured in `<resultMap>`. The database uses the tinyint type to represent yes/no values, and the is_xxx naming convention is maintained to make the meaning and range of the values explicit.
  Note: Any field that is non-negative must be unsigned.
  Positive example: A field expressing logical deletion is named is_deleted, where 1 means deleted and 0 means not deleted.

2. **[Mandatory]** Table names and field names must use lowercase letters or digits. They must not start with a digit, and digits must not be the only characters appearing between two underscores. Modifying a database field name is very costly because it cannot be pre-released, so field names must be chosen carefully.
  Note: MySQL is case-insensitive on Windows, but case-sensitive by default on Linux. Therefore, database names, table names, and field names must not contain any uppercase letters, to avoid complications.
  Positive example: aliyun_admin, rdc_config, level3_name
  Counter-example: AliyunAdmin, rdcConfig, level_3_name

3. **[Mandatory]** Table names must not use plural nouns.
  Note: A table name should represent only the entity content within the table, not the quantity of entities. The corresponding DO class name is also in singular form, which conforms to expression conventions.

4. **[Mandatory]** Reserved words such as desc, range, match, delayed, etc. are prohibited. Please refer to the MySQL official reserved words list.

5. **[Mandatory]** A primary key index is named pk_fieldname; a unique index is named uk_fieldname; an ordinary index is named idx_fieldname.
  Note: pk_ stands for primary key; uk_ stands for unique key; idx_ is the abbreviation for index.

6. **[Mandatory]** The type for decimals is decimal. Using float and double is prohibited.
  Note: When stored, both float and double suffer from precision loss, which can easily produce incorrect results when comparing values. If the data range to be stored exceeds the range of decimal, it is recommended to split the data into integer and fractional parts and store them separately.

7. **[Mandatory]** If the lengths of the stored strings are almost equal, use the char fixed-length string type.

8. **[Mandatory]** varchar is a variable-length string that does not pre-allocate storage space. Its length should not exceed 5000. If the stored length is greater than this value, define the field type as text, separate it into a dedicated table, and use a primary key to correspond to it, to avoid affecting the index efficiency of other fields.

9. **[Mandatory]** Every table must have three fields: id, create_time, update_time.
  Note: Here, id must be the primary key, with type bigint unsigned; for a single table it auto-increments with a step of 1. The types of create_time and update_time are both datetime; if time zone information needs to be recorded, set the type to timestamp.

10. **[Mandatory]** Physical deletion operations must not be used in the database; logical deletion must be used instead.
   Note: With logical deletion, the behavior can be traced back after data is deleted. However, in some cases it makes a unique primary key no longer unique, which needs to be resolved as appropriate depending on the situation.

11. **[Recommended]** Table naming should preferably follow "business_name_table_purpose".
   Positive example: alipay_task / force_project / trade_config / tes_question

12. **[Recommended]** The database name should be as consistent as possible with the application name.

13. **[Recommended]** When modifying the meaning of a field or appending to the states a field represents, the field comment needs to be updated promptly.

14. **[Recommended]** Fields may have appropriate redundancy to improve query performance, but data consistency must be considered. Redundant fields should follow:
   1) Not a frequently modified field.
   2) Not a unique-index field.
   3) Not an overly long varchar field, and certainly not a text field.
   Positive example: Various business lines often redundantly store the product name to avoid calling the IC service to obtain it during queries.

15. **[Recommended]** Only when the number of rows in a single table exceeds 5 million or the size of a single table exceeds 2GB is database/table sharding recommended.
  Note: If the projected data volume in three years will not reach this level at all, do not shard the database/table when creating the table.

16. **[Reference]** An appropriate character storage length not only saves database table space and index storage, but more importantly improves retrieval speed.
  Positive example: Unsigned values can avoid mistakenly storing negative numbers and expand the representable range:

          Object          Age range                Type                Bytes                  Representable range

           Human          within 150 years         tinyint unsigned    1              Unsigned value: 0 to 255

           Tortoise        hundreds of years        smallint unsigned   2             Unsigned value: 0 to 65535

        Dinosaur fossil    tens of millions of years int unsigned        4             Unsigned value: 0 to about 4.3 billion

          Sun             about 5 billion years     bigint unsigned     8         Unsigned value: 0 to about 10 to the 19th power

(2) Index Specification

1. **[Mandatory]** Fields that have uniqueness characteristics in the business must be built as unique indexes, even if they are composite fields.
 Note: Do not assume that a unique index affects insert speed; this speed cost is negligible, but the improvement in lookup speed is obvious. Moreover, even with very thorough validation control at the application layer, as long as there is no unique index, by Murphy's Law dirty data will inevitably be produced.

2. **[Mandatory]** Joining more than three tables is prohibited. The fields that need to be joined must have absolutely consistent data types; in multi-table join queries, ensure that the joined fields have indexes.
 Note: Even for a two-table join, pay attention to table indexes and SQL performance.

3. **[Mandatory]** When building an index on a varchar field, the index length must be specified. There is no need to build an index on the full field; determine the index length based on the actual text discrimination.
 Note: Index length and discrimination are a pair of contradictions. Generally, for string-type data, an index of length 20 will have a discrimination as high as over 90%. You can use the discrimination of count(distinct left(column_name, index_length)) / count(*) to determine it.

4. **[Mandatory]** Left-fuzzy or full-fuzzy matching is strictly prohibited in page searches. If needed, please use a search engine to solve it.
 Note: Index files have the leftmost-prefix matching characteristic of B-Tree. If the value on the left is not determined, then this index cannot be used.

5. **[Recommended]** If there is an order by scenario, please be careful to leverage the ordering of the index. The last field of order by is part of the composite index and is placed at the end of the index combination order, to avoid the filesort situation that affects query performance.
 Positive example: where a = ? and b = ? order by c; index: a_b_c
 Counter-example: If the index has a range query, then the ordering of the index cannot be leveraged, e.g.: WHERE a > 10 ORDER BY b; index a_b cannot be sorted.

6. **[Recommended]** Use covering indexes for query operations to avoid table lookups (回表).
 Note: If you need to know the title of Chapter 11 in a book, would you open the page corresponding to Chapter 11? Just browse the table of contents; this table of contents acts as a covering index.
 Positive example: The kinds of indexes that can be built are divided into three types: primary key index, unique index, and ordinary index. A covering index is merely an effect of a query; in the result of explain, the extra column will show: using index.

7. **[Recommended]** Use deferred joins or subqueries to optimize scenarios with very large pagination.
 Note: MySQL does not skip offset rows; instead it fetches offset+N rows, then returns N rows after discarding the first offset rows. So when offset is especially large, efficiency becomes very low. Either control the total number of returned pages, or rewrite the SQL for page numbers exceeding a specific threshold.
 Positive example: First quickly locate the id segment to be obtained, then join:
 SELECT t1.* FROM table1 as t1 , (select id from table1 where condition LIMIT 100000 , 20) as t2 where t1.id = t2.id

8. **[Recommended]** The goal of SQL performance optimization: at minimum reach the range level, the requirement is the ref level, and const if possible is best.
 Note:
  1) consts: there is at most one matching row in a single table (primary key or unique index), and data can be read during the optimization phase.

   2) ref: refers to using an ordinary index (normal index).
   3) range: performs range retrieval on an index.
  Counter-example: In the result of explain, type = index means a full scan of the index physical file, which is very slow. This index level is even lower than range; compared with a full table scan it is a case of small magic versus big magic (negligible difference).

9. **[Recommended]** When building a composite index, place the one with the highest discrimination on the leftmost.
  Positive example: If where a = ? and b = ?, and the values of column a are almost unique, then it suffices to build only the idx_a index.
  Note: When there is a mix of non-equality and equality conditions, when building the index, please place the equality-condition columns first. For example: where c > ? and d = ?, then even if c has higher discrimination, d must be placed in the leftmost column of the index, i.e., build the composite index idx_d_c.

10. **[Recommended]** Prevent implicit conversion caused by different field types, which causes indexes to become ineffective.

11. **[Reference]** Avoid the following extreme misconceptions when creating indexes:
   1) Better too many indexes than too few. Believing that every query needs an index.
   2) Being stingy with index creation. Believing that indexes consume space and severely slow down record updates and row inserts.
   3) Resisting unique indexes. Believing that unique indexes should always be resolved at the application layer via a "query first, then insert" approach.

(3) SQL Statements

1. **[Mandatory]** Do not use count(column_name) or count(constant) to replace count(*). count(*) is the standard syntax for counting rows defined by SQL92, independent of the database and independent of NULL versus non-NULL.
  Note: count(*) counts rows whose value is NULL, whereas count(column_name) does not count rows where this column is NULL.

2. **[Mandatory]** count(distinct col) computes the number of distinct rows in that column excluding NULL. Note that for count(distinct col1 , col2), if one of the columns is entirely NULL, then even if the other column has different values, it returns 0.

3. **[Mandatory]** When the values of a column are all NULL, count(col) returns 0; but sum(col) returns NULL, so when using sum() pay attention to the NPE issue.
  Positive example: You can use the following approach to avoid the NPE issue with sum: SELECT IFNULL(SUM(column) , 0) FROM table;

4. **[Mandatory]** Use ISNULL() to determine whether a value is NULL.
  Note: A direct comparison of NULL with any value is NULL.
   1) The result of NULL<>NULL is NULL, not false.
   2) The result of NULL=NULL is NULL, not true.
   3) The result of NULL<>1 is NULL, not true.
  Counter-example: In an SQL statement, breaking the line before null hurts readability.
  select * from table where column1 is null and column3 is not null; whereas ISNULL(column) is a whole, concise and easy to understand.
  Analyzing from performance data, ISNULL(column) executes somewhat faster.

5. **[Mandatory]** When writing pagination query logic in code, if count is 0 you should return directly, avoiding the execution of the subsequent pagination statement.

6. **[Mandatory]** Foreign keys and cascading must not be used; all foreign key concepts must be resolved at the application layer.
  Note: (Concept explanation) student_id in the student table is the primary key, then student_id in the grade table is a foreign key. If updating student_id in the student table simultaneously triggers an update of student_id in the grade table, that is a cascading update. Foreign keys and cascading updates are suitable for single-machine, low-concurrency scenarios, but not suitable for distributed, high-concurrency clusters; cascading updates are strongly blocking and carry the risk of a database update storm; foreign keys affect the database insert speed.

7. **[Mandatory]** Using stored procedures is prohibited. Stored procedures are difficult to debug and extend, and have no portability.

8. **[Mandatory]** When correcting data (especially delete or modify record operations), select first to avoid accidental deletion, and only execute the update statement after confirming there are no errors.

9. **[Mandatory]** For querying and changing table records in the database, as long as multiple tables are involved, column names must be qualified with the table alias (or table name) as a prefix.

  Note: When querying, updating, or deleting records across multiple tables, if the operated columns are not qualified with a table alias (or table name) and the operated columns exist in multiple tables, an exception will be thrown.
  Positive example: select t1.name from first_table as t1 , second_table as t2 where t1.id = t2.id;
  Counter-example: In a certain business, because a multi-table join query statement did not add the restriction of a table alias (or table name), after running normally for two years, a field with the same name was recently added to one of the tables. After making the database change in the pre-release environment, the online query statement produced a 1052 exception:
  Column 'name' in field list is ambiguous.

10. **[Recommended]** Add as before the table alias in an SQL statement, and name them sequentially in the order t1, t2, t3, ...
   Note:
   1) An alias can be an abbreviation of the table, or, following the order in which the tables appear in the SQL statement, named in the manner t1, t2, t3.
   2) Adding as before the alias makes the alias easier to recognize.
   Positive example: select t1.name from first_table as t1 , second_table as t2 where t1.id = t2.id;

11. **[Recommended]** Avoid the in operation if possible. If it really cannot be avoided, carefully evaluate the number of set elements after in, keeping it within 1000.

12. **[Reference]** Due to internationalization needs, all character storage and representation use the utf8mb4 character set; pay attention to the character counting method.
   Note:
        SELECT LENGTH("轻松工作"); -- returns 12
        SELECT CHARACTER_LENGTH("轻松工作"); -- returns 4
        Emojis need to be stored using utf8mb4; note its difference from utf8 encoding.

13. **[Reference]** TRUNCATE TABLE is faster than DELETE and uses fewer system and transaction log resources, but TRUNCATE has no transaction and does not trigger triggers, which can potentially cause accidents, so it is not recommended to use this statement in development code.
   Note: TRUNCATE TABLE is functionally identical to a DELETE statement without a WHERE clause.

(4) ORM Mapping

1. **[Mandatory]** In table queries, never use * as the list of query fields; the needed fields must be written out explicitly.
  Note:
  1) It increases the parsing cost of the query analyzer.
  2) Adding or removing fields easily becomes inconsistent with the resultMap configuration.
  3) Useless fields increase network consumption, especially text-type fields.

2. **[Mandatory]** Boolean properties of a POJO class must not have is, while database fields must have is_, and a mapping between fields and properties is required in the resultMap.
  Note: See the rules for defining POJO classes and database fields; adding the mapping in sql.xml is mandatory.

3. **[Mandatory]** Do not use resultClass as the return parameter. Even if all class property names correspond one-to-one with database fields, a `<resultMap>` still needs to be defined; conversely, every table must necessarily have a `<resultMap>` corresponding to it.
  Note: Configuring the mapping relationship decouples fields from the DO class, making maintenance convenient.

4. **[Mandatory]** sql.xml configuration parameters use: #{}, #param#. Do not use ${}; this approach easily leads to SQL injection.

5. **[Mandatory]** iBATIS's built-in queryForList(String statementName, int start, int size) is not recommended.
  Note: Its implementation fetches all records of the SQL statement corresponding to statementName from the database, then uses subList to take the subset of start, size. There has been an OOM online because of this reason.
  Positive example:
     Map<String, Object> map = new HashMap<>(16);
     map.put("start", start);
     map.put("size", size);

6. **[Mandatory]** Directly using HashMap and Hashtable as the output of a query result set is not allowed.
 Counter-example: To avoid writing a `<resultMap>xxx</resultMap>`, a developer directly used Hashtable to receive the database return result. The result was that normally bigint was converted to a Long value, but online, because of a different database version, it was parsed as BigInteger, causing an online problem.

7. **[Mandatory]** When updating a table record, the record's corresponding update_time field value must be simultaneously updated to the current time.

8. **[Recommended]** Do not write a large all-encompassing data update interface. Passing in a POJO class and, regardless of whether they are your intended target fields to update, performing update table set c1 = value1 , c2 = value2 , c3 = value3 — this is wrong. When executing SQL, do not update fields that have not changed. First, it is error-prone; second, it is inefficient; third, it increases binlog storage.

9. **[Reference]** Do not abuse @Transactional transactions. Transactions affect the database's QPS. In addition, where transactions are used, rollback plans must be considered in all aspects, including cache rollback, search engine rollback, message compensation, statistics correction, etc.

10. **[Reference]** The compareValue in `<isEqual>` is a constant compared against the property value, generally a number, meaning this condition is included when equal; `<isNotEmpty>` means execute when not empty and not null; `<isNotNull>` means execute when not a null value.
