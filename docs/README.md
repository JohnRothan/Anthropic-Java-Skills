# 源书库 · Source Library

本目录存放用于转换成 skills 的**源资料**,按技术领域归类。

> ⚠️ **重要说明**
> - 本目录下的实际文件**不纳入版本控制**(根 `.gitignore` 已忽略 `docs/*`,仅保留本 `README.md`)。
> - 这里**只收录官方发布或开源协议(可合法分发)的资料**,**不存放盗版商业书**(如 Effective Java、深入理解 JVM、Java 并发编程实战、高性能 MySQL、Spring 实战等受版权保护的书籍)。
> - 选用官方/开源资料,既合规,也让从中派生出的 skill 可以放心公开分发。

## 领域归类 · Catalog by domain

| 领域 | 资料 | 来源 / 出品方 | 许可 | 说明 |
|------|------|---------------|------|------|
| **编程规范** | 阿里巴巴 Java 开发手册(黄山版) | [alibaba/p3c](https://github.com/alibaba/p3c) | 官方免费 | 已转为 skill:[`alibaba-java-coding-guidelines`](../skills/alibaba-java-coding-guidelines) ✅ |
| **编程规范** | Google Java Style Guide | [google.github.io/styleguide](https://google.github.io/styleguide/javaguide.html) | CC-BY 3.0 | Google 官方 Java 代码风格 |
| **Java 语言与 JVM** | Java 语言规范 JLS(SE 21) | [Oracle docs](https://docs.oracle.com/javase/specs/jls/se21/jls21.pdf) | 官方免费 | 语言语义权威定义 |
| **Java 语言与 JVM** | Java 虚拟机规范 JVMS(SE 21) | [Oracle docs](https://docs.oracle.com/javase/specs/jvms/se21/jvms21.pdf) | 官方免费 | 字节码 / 类加载 / 内存模型权威定义 |
| **数据库** | MySQL 8.0 参考手册(6388 页) | [dev.mysql.com](https://dev.mysql.com/doc/) | 官方免费 | Oracle/MySQL 官方完整手册 |
| **高并发与分布式** | advanced-java(互联网 Java 进阶) | [doocs/advanced-java](https://github.com/doocs/advanced-java) | CC-BY-SA-4.0 | 高并发、分布式、缓存、消息队列、分库分表等(92 篇,开源) |
| **网络** | HTTP 标准 RFC 9110 / 9112 / 9113 | [rfc-editor.org](https://www.rfc-editor.org/) | IETF 标准(公开) | HTTP 语义、HTTP/1.1、HTTP/2 权威规范 |

## 目录结构 · Layout

```
docs/
├── README.md                  # 本文件（唯一被提交）
├── 编程规范/
│   ├── 阿里巴巴Java开发手册(黄山版).pdf
│   └── Google-Java-Style-Guide.html
├── Java语言与JVM/
│   ├── Java语言规范-JLS-SE21(官方).pdf
│   └── Java虚拟机规范-JVMS-SE21(官方).pdf
├── 数据库/
│   └── MySQL-8.0-参考手册(官方).pdf
├── 高并发与分布式/
│   └── advanced-java(doocs,CC-BY-SA-4.0)/   # 开源 markdown，含 LICENSE
└── 网络/
    ├── RFC9110-HTTP语义.txt
    ├── RFC9112-HTTP-1.1.txt
    └── RFC9113-HTTP-2.txt
```

## 想要的经典商业书怎么办?

像 *Effective Java*、*深入理解 Java 虚拟机*、*Java 并发编程实战*、*高性能 MySQL*、*Spring 实战* 等是受版权保护的商业书籍,不能下载盗版。建议:
- **合法购买**纸质 / 正版电子书后,放在本地(本目录已 gitignore,不会误传)用作个人学习与转换参考;
- 公开分发的 skill 应基于**官方/开源资料**或**你自己的总结**撰写,避免直接照搬受版权内容。

## 复现下载 · Re-download

这些文件不入库,换机器时可按上表来源重新获取。常用命令:

```bash
# JLS / JVMS（Oracle 官方）
curl -L -o "Java语言与JVM/Java语言规范-JLS-SE21(官方).pdf"   https://docs.oracle.com/javase/specs/jls/se21/jls21.pdf
curl -L -o "Java语言与JVM/Java虚拟机规范-JVMS-SE21(官方).pdf" https://docs.oracle.com/javase/specs/jvms/se21/jvms21.pdf
# MySQL 8.0 手册（官方，约 44MB）
curl -L -o "数据库/MySQL-8.0-参考手册(官方).pdf" https://downloads.mysql.com/docs/refman-8.0-en.a4.pdf
# 高并发开源书（doocs，CC-BY-SA-4.0）
git clone --depth 1 https://github.com/doocs/advanced-java.git
```
