---
name: alibaba-java-coding-guidelines
description: >-
  阿里巴巴《Java 开发手册（黄山版）》编码规约。在编写、审查、重构或评审任何 Java
  代码时都应使用本技能——包括命名风格、常量定义、代码格式、OOP 规约、日期时间、集合处理、
  并发与多线程、控制语句、注释、异常处理、日志、单元测试、安全规约、MySQL 建表/索引/SQL/ORM、
  以及应用分层等工程结构。无论用户是否明确提到“阿里规范 / P3C / 黄山版 / 开发手册”，只要任务涉及
  Java 代码的产出或质量把关（如“写个 Java 类 / Service / DAO”“review 这段 Java”“这段代码符合规范吗”
  “建一张 MySQL 表”“帮我命名”“处理一下异常和日志”），都要主动参考本技能给出的规则与等级。
---

# 阿里巴巴 Java 开发手册（黄山版）编码规约

本技能把阿里巴巴官方《Java 开发手册（黄山版）》转化为可执行的编码与代码审查指南，覆盖编程规约、
异常日志、单元测试、安全规约、MySQL 数据库、工程结构、设计规约七大维度。完整条文按维度拆分在
`references/` 目录中，**先用下方速查表覆盖高频场景，遇到具体维度再按需读取对应参考文件**。

## 规则等级（务必在产出中体现）

每条规则带有等级标签，处理冲突和给建议时按此优先级：

- **【强制】**——必须无条件遵守。违反会埋下故障、安全或可维护性隐患。写代码时不得违反；审查时**必须**作为问题（blocker）指出。
- **【推荐】**——强烈建议遵守。除非有充分理由，否则应采纳；审查时作为改进建议提出。
- **【参考】**——供参考的良好实践，可结合团队情况酌情采用。

## 使用方式

### 编写 Java 代码时
1. 动手前先扫一遍下面的「高频强制规则速查」，确保命名、POJO、空指针、集合、并发、SQL 等高频点不踩坑。
2. 任务集中在某个维度时（如写 DAO/建表 → MySQL；写并发工具 → 并发处理），**读取对应的 `references/` 文件**再落笔，确保细节准确。
3. 产出代码后自查：是否有【强制】被违反？若为兼顾可读性而偏离【推荐】，在说明中讲清理由。

### 审查 / 评审 Java 代码时
1. 逐条对照相关维度的规则，按等级归类发现的问题：**【强制】违规 = 必须修改**，**【推荐】违规 = 建议改进**。
2. 每条意见尽量**引用规则出处**（如“命名风格第 9 条【强制】：POJO 布尔字段不加 is 前缀”），便于对方理解与查证。
3. 给出问题的同时给**正例**修法，不要只说“不符合规范”。

## 高频强制规则速查（覆盖最常见场景，免于每次翻参考文件）

**命名**
- 类名 `UpperCamelCase`；方法/参数/成员/局部变量 `lowerCamelCase`；常量 `UPPER_SNAKE_CASE`。
- 禁止拼音与英文混用、禁止中文命名；禁止 `_`/`$` 开头或结尾；杜绝不规范缩写。
- POJO 布尔字段**不要加 `is` 前缀**（如用 `deleted` 而非 `isDeleted`），否则部分框架序列化出错。
- 抽象类用 `Abstract`/`Base` 开头，异常类以 `Exception` 结尾，测试类以 `Test` 结尾。
- 包名全小写、单数；Service/DAO 接口实现类用 `Impl` 后缀。

**常量与类型**
- 不允许魔法值直接出现在代码中；`long` 赋值用大写 `L`（如 `2L`）。
- POJO 类属性、RPC 入参/返回值必须用**包装类型**；局部变量用基本类型。
- 整型包装类值比较用 `equals`；`BigDecimal` 等值比较用 `compareTo()`，禁止 `new BigDecimal(double)`，用 `new BigDecimal("0.1")` 或 `BigDecimal.valueOf()`。
- 浮点数不用 `==` 直接比较。金额用最小货币单位的整型存储。
- POJO 不设属性默认值；必须写 `toString()`；覆写方法必须加 `@Override`。

**集合**
- `Arrays.asList()` 的返回不可做 add/remove；判空用 `isEmpty()` 而非 `size()==0`。
- 用 `Collection.toArray(new T[0])`；`foreach` 循环里不要 remove/add 元素（用 `Iterator` 或并发容器）。
- 重写 `equals` 必须同时重写 `hashCode`；用 `Map.entrySet` 遍历而非 keySet 二次取值。
- 集合转 `Map` 注意 value 为 null 会 NPE；`Collectors.toMap` 注意 key 重复抛异常。

**并发**
- 线程池**不允许用 `Executors` 创建**，必须用 `new ThreadPoolExecutor(...)`，明确队列与拒绝策略，避免 OOM。
- 线程/线程池必须命名（`ThreadFactory`）；`SimpleDateFormat` 非线程安全，用 `DateTimeFormatter`。
- 加锁顺序一致避免死锁；并发修改用原子类/锁；`ThreadLocal` 用完 `remove()`。

**控制语句 / OOP**
- `switch` 每个 `case` 要么 `break`/`return`，要么注释说明穿透；必须有 `default`。
- `if/else/for/while` 即使单行也必须用大括号；避免超过 3 层嵌套（用卫语句/状态模式）。
- 用常量或确定非空对象调 `equals`（`"x".equals(param)`）。

**异常与日志**
- 不要 catch `RuntimeException`（如 `NullPointerException`/`IndexOutOfBounds`）来代替预检查，应预先判断。
- `try-catch` 不要包住大段无关逻辑；catch 后不要只 `printStackTrace` 或吞掉；`finally` 中释放资源（或 try-with-resources）。
- 日志用 SLF4J 门面 + 占位符 `logger.info("id={}", id)`，不要字符串拼接；异常日志要打出上下文与堆栈。

**MySQL（写 DDL/SQL/实体时）**
- 表必备字段 `id`(bigint unsigned 主键自增)、`gmt_create`、`gmt_modified`；表名、字段名小写下划线，禁用保留字。
- 表达是否概念的字段用 `is_xxx`（unsigned tinyint，1 是 0 否）。
- 小数类型用 `decimal`，禁用 `float`/`double` 存金额；`varchar` 超 5000 用 `text` 并拆表。
- SQL 用参数绑定防注入；`count(*)` 统计行数；分页、索引、`like` 左模糊会失效等见参考文件。

> 速查表是高频提醒，不是全部。涉及具体细节、边界与正反例时，请读取下方对应的参考文件确认。

## 参考文件索引（按需读取）

| 维度 | 文件 | 何时阅读 |
|------|------|----------|
| 编程规约 | `references/01-编程规约.md` | 命名、常量、格式、OOP、日期、集合、并发、控制语句、注释、前后端、其它——写或评任何 Java 代码的主参考 |
| 异常日志 | `references/02-异常日志.md` | 设计错误码、异常处理策略、日志打印规范 |
| 单元测试 | `references/03-单元测试.md` | 编写或评审单元测试（AIR / BCDE 原则等） |
| 安全规约 | `references/04-安全规约.md` | 涉及用户输入、权限、SQL 注入、XSS/CSRF、文件上传、脱敏、敏感信息 |
| MySQL 数据库 | `references/05-MySQL数据库.md` | 建表、索引设计、SQL 语句、ORM/MyBatis 映射 |
| 工程结构 | `references/06-工程结构.md` | 应用分层（DO/DTO/VO、Manager 层）、二方库依赖、服务器配置 |
| 设计规约 | `references/07-设计规约.md` | 架构与设计层面的约定 |
| 专有名词 | `references/08-专有名词解释.md` | 遇到 POJO/DO/DTO/CAS/IDE 等术语需要确认含义时查阅 |

每个参考文件保留了官方条文的等级标签与正反例，引用时可直接标注“第 X 条【强制/推荐/参考】”。
