Linkis
============

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

English | [中文](zh_CN)

# Introduction

&nbsp; &nbsp; &nbsp; &nbsp;Linkis builds a layer of computation middleware between upper applications and underlying engines. By using standard interfaces such as REST/WS/JDBC provided by Linkis, the upper applications can easily access the underlying engines such as MySQL/Spark/Hive/Presto/Flink, etc., and achieve the intercommunication of user resources like unified variables, scripts, UDFs, functions and resource files at the same time.
                           
&nbsp; &nbsp; &nbsp; &nbsp;As a computation middleware, Linkis provides powerful connectivity, reuse, expansion, and computation governance capabilities. By decoupling the application layer and the engine layer, it simplifies the complex network call relationship, and thus reduces the overall complexity and saves the development and maintenance costs as well.
                           
&nbsp; &nbsp; &nbsp; &nbsp;Since the first release of Linkis in 2019, it has accumulated more than 700 trial companies and 1000+ sandbox trial users, which involving diverse industries, from finance, banking, tele-communication, to manufactory, internet companies and so on. Lots of companies have already used Linkis as a unified entrance for the underlying computation and storage engines of the big data platform.


![linkis-intro-01](en_US/Images/Architecture/linkis-intro-01.png)

![linkis-intro-02](en_US/Images/Architecture/linkis-intro-02.png)
<br>
<br>

&nbsp; &nbsp; &nbsp; &nbsp;Based on the concept of the computation middleware architecture of Linkis, we have built a large amount of applications and systems on top of it. The following is the currently available open-source projects:  
 
 - [**DataSphere Studio - Data Application Development & Management Framework**](https://github.com/WeBankFinTech/DataSphereStudio)
 
 - [**Qualitis - Data Quality Tool**](https://github.com/WeBankFinTech/Qualitis)
 
 - [**Scriptis - Data Development IDE Tool**](https://github.com/WeBankFinTech/Scriptis)
 
 - [**Visualis - Data Visualization Tool**](https://github.com/WeBankFinTech/Visualis)

 - [**Schedulis - Workflow Task Scheduling Tool**](https://github.com/WeBankFinTech/Schedulis)

&nbsp; &nbsp; &nbsp; &nbsp;More upcoming tools  ready to be open-sourced, please stay tuned!
 
----
 
 # Integrated engines

| Engine     | Description                                                          | Contributor                                                           | Version    | Released? | User Manual |
| --------------- | -------------------------------------------------------------------- | ------------------ | ------------- | ------------ |  ---------------------- |
| **Flink**  | Flink EngineConn. Supports the submission of FlinkSQL code, and also supports the submission of Flink Jar to Linkis Manager to start a new Yarn application. | [hui zhu](https://github.com/liangqilang) & WeDataSphere | Linkis-1.1.0 | No, Expected in mid-2021 | To be perfected |
| **Impala**     | Impala EngineConn. Supports the submission of Impala SQL code. | [hui zhu](https://github.com/liangqilang) | Linkis-0.12.0 | No, Expected in April | To be perfected |
| **Presto**  | Presto EngineConn. Supports the submission of Presto SQL code. | [wForget](https://github.com/wForget)  | Linkis-0.11.0 | Open sourced | [Presto User Manual](zh_CN/Engine Usage Documentations/Presto_User_Manual.md) |
| **ElasticSearch** | ElasticSearch EngineConn. Supports the submission of SQL and DSL code. | [wForget](https://github.com/wForget)  | Linkis-0.11.0 | Open sourced | [ElasticSearch User Manual](zh_CN/Engine Usage Documentations/ElasticSearch_User_Manual.md) |
| **Shell**  | Shell EngineConn. Supports the submission of shell code. | WeDataSphere | Linkis-0.9.3 | Open sourced | [Shell User Manual](zh_CN/Engine Usage Documentations/Shell_User_Manual.md) |
| **MLSQL**   | MLSQL EngineConn. Supports the submission of MLSQL code. | [WilliamZhu](https://github.com/allwefantasy) | Linkis-0.9.1 | Open sourced | [MLSQL User Manual](zh_CN/Engine Usage Documentations/MLSQL_User_Manual.md) |
| **JDBC**   | JDBC EngineConn. Supports the submission of MySQL and HiveQL code. | WeDataSphere | Linkis-0.9.0 | Open sourced | [JDBC User Manual](zh_CN/Engine Usage Documentations/JDBC_User_Manual.md) |
| **Spark**   | Spark EngineConn. Supports the submission of SQL, Scala, Pyspark and R code. | WeDataSphere | Linkis-0.5.0 | Open sourced | [Spark User Manual](zh_CN/Engine Usage Documentations/Spark_User_Manual.md) |
| **Hive**   | Hive EngineConn. Supports the submission of HiveQL code. | WeDataSphere | Linkis-0.5.0 | Open sourced | [Hive User Manual](zh_CN/Engine Usage Documentations/Hive_User_Manual.md) |
| **Python**   | Python EngineConn. Supports the submission of python code. | WeDataSphere | Linkis-0.5.0 | Open sourced | [Python User Manual](zh_CN/Engine Usage Documentations/Python_User_Manual.md) |
| **TiSpark**   | TiSpark EngineConn. Support querying TiDB data by SparkSQL. | WeDataSphere | Linkis-0.5.0 | Open sourced | To be perfected |

----
 
 # Who is using Linkis
 
 
----
 
 # Document Announcement
 
&nbsp; &nbsp; &nbsp; &nbsp;Linkis uses GitBook for management, and the entire project will be organized into a GitBook e-book, which is convenient for everyone to download and use.
 
&nbsp; &nbsp; &nbsp; &nbsp;WeDataSphere will provide a unified document reading entrance in the future. For the usage of GitBook, please refer to: [GitBook User Manual](https://docs.gitbook.com/)

----

# Contributing

&nbsp; &nbsp; &nbsp; &nbsp;Before contributing document, please read the [Document Contribution Specification]() to understand how to contribute document and submit your contribution.

----

# Architecture

![Architecture](en_US/Images/Architecture/Linkis1.0-architecture.png)

----

# License

&nbsp; &nbsp; &nbsp; &nbsp;Linkis is under the Apache 2.0 license. See the [License](https://github.com/WeBankFinTech/Linkis/LICENSE) file for details.