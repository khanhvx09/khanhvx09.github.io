---
layout: distill
title: Overview of Big Data Analytics
date: 2026-09-22 08:00:00 +0800
published: true
description: Tổng quan về phân tích dữ liệu lớn
tags: data-engineering data-lake big-data
categories: data-engineering
mermaid:
  enabled: false
  zoomable: false

authors:
  - name: Khanh Xuan Vu

toc:
  - name: Introduction to big data
  - name: The MapReduce Framework
  - name: Apache Spark
  - name: Transformation và Action
---

Bài viết này sẽ cung cấp một cái nhìn tổng quan về phân tích dữ liệu lớn (big data analytics), bao gồm các khái niệm cơ bản, các công nghệ phổ biến và cách chúng được sử dụng để xử lý và phân tích dữ liệu lớn.

## Introduction to big data
Theo [IBM](https://www.ibm.com/think/topics/big-data), dữ liệu lớn (big data) là một tập hợp các dữ liệu có kích thước lớn, tốc độ tăng nhanh và đa dạng, khiến các công cụ phân tích truyền thống không thể xử lý hiệu quả.

Dưới đây là 5 chữ V (5 Vs) của dữ liệu lớn, được sử dụng để mô tả các đặc tính của dữ liệu lớn:

### Variety of data
Data có thể đến từ nhiều nguồn dữ liệu khác nhau, như là thời tiết, mạng xã hội, cảm biến IoT, giao dịch tài chính, và nhiều nguồn khác. Dữ liệu có thể ở dạng cấu trúc (structured), bán cấu trúc (semi-structured) hoặc phi cấu trúc (unstructured).

### Velocity of data
Data có thể đến từ data warehouse, các file định kỳ hoặc các luồng dữ liệu trực tiếp (streaming data). Dữ liệu có thể được tạo ra với tốc độ cao, ví dụ như dữ liệu từ mạng xã hội hoặc cảm biến IoT. Velocity đề cập đến tốc độ mà dữ liệu được tạo ra và tốc độ mà dữ liệu cần được xử lý và phân tích.

### Volume of data
Dữ liệu lớn thường có kích thước rất lớn, từ hàng terabyte đến petabyte hoặc thậm chí exabyte. Volume đề cập đến kích thước của dữ liệu và khả năng lưu trữ và xử lý dữ liệu lớn.

### Veracity of data
Dữ liệu lớn có thể chứa nhiều thông tin không chính xác hoặc không đầy đủ. Veracity đề cập đến độ tin cậy và chất lượng của dữ liệu, và việc xử lý dữ liệu lớn đòi hỏi các kỹ thuật để làm sạch và xác thực dữ liệu.

### Value of data
Dữ liệu lớn có thể mang lại giá trị lớn nếu được phân tích và sử dụng đúng cách. Value đề cập đến khả năng khai thác giá trị từ dữ liệu lớn thông qua các kỹ thuật phân tích dữ liệu, học máy (machine learning) và trí tuệ nhân tạo (artificial intelligence).


## The MapReduce Framework
MapReduce là một mô hình (framework) được sử dụng để tính toán lượng lớn dữ liệu trong một cụm Hadoop. MapReduces chia quá trình xử lý dữ liệu thành hai bước chính: Map và Reduce. Trong bước Map, dữ liệu được chia thành các cặp key-value và được xử lý song song trên nhiều nút (nodes) trong cụm Hadoop. Trong bước Reduce, các kết quả từ bước Map được tổng hợp lại để tạo ra kết quả cuối cùng.

{% include figure.liquid loading="eager" path="/assets/img/posts/hadoop/hadoop-map-reduce.jpg" class="img-fluid rounded z-depth-1" title="MapReduce" %}



## Apache Spark
Apache Spark là một công cụ tính toán phân tán hợp nhất (unified distributed computing engine), được thiết kế để xử lý dữ liệu lớn một cách nhanh chóng và hiệu quả. 



{% include figure.liquid loading="eager" path="/assets/img/posts/spark/what-is-apache-spark.jpeg" class="img-fluid rounded z-depth-1" title="MapReduce" %}

Apache Spark là một engine xử lý dữ liệu nhanh chóng trên RAM, giúp tăng tốc độ xử lý dữ liệu lớn so với các công cụ truyền thống như Hadoop MapReduce. 

Spark cung cấp một API dễ sử dụng cho các ngôn ngữ lập trình phổ biến như Java, Scala, Python và R, và hỗ trợ nhiều loại dữ liệu khác nhau, bao gồm dữ liệu cấu trúc, bán cấu trúc và phi cấu trúc. Các thành phần chính của Spark bao gồm Spark Core, Spark SQL, Spark Streaming, MLlib và GraphX. Spark có thể chạy trên nhiều nền tảng khác nhau, bao gồm Hadoop YARN, Apache Mesos và Kubernetes.


### Spark Architecture

Một cụm máy tính (cluster) mà Spark sẽ sử dụng để chạy các tasks được quản lý bởi một cluster manager. Chúng ta sẽ submit Spark application đến cluster manager, và cluster manager sẽ phân bổ các resources (CPU, memory) cho Spark application. Spark application sẽ được chia thành nhiều tasks và được chạy trên các worker nodes trong cụm máy tính.

Spark Application bao gồm một driver program và nhiều executor processes. Driver program chịu trách nhiệm quản lý các tasks và phân bổ chúng cho các executor processes. Executor processes chịu trách nhiệm thực hiện các tasks và trả về kết quả cho driver program.

{% include figure.liquid loading="eager" path="/assets/img/posts/spark/spark-application.jpg" class="img-fluid rounded z-depth-1" title="Spark Application" %}

### Spark SQL
Trước Apache Spark, Apache Hive là một công cụ phổ biến để xử lý dữ liệu lớn. Hive cung cấp một ngôn ngữ truy vấn tương tự SQL. Apache Hive dịch các truy vấn SQL thành các công việc MapReduce để xử lý dữ liệu lớn. Tuy nhiên, MapReduce có thể chậm và không hiệu quả cho các truy vấn phức tạp.

Với sự ra đời của Apache Spark, Spark SQL đã trở thành một công cụ mạnh mẽ để xử lý dữ liệu lớn. Spark SQL cung cấp một API SQL tương tự như Hive, nhưng với hiệu suất cao hơn nhiều. Spark SQL sử dụng một engine tối ưu hóa (Catalyst optimizer) để tối ưu hóa các truy vấn SQL và thực hiện chúng trên Spark Core.

Dưới đây là một sơ đồ minh họa cách Spark SQL hoạt động:
{% include figure.liquid loading="eager" path="/assets/img/posts/spark/Catalyst-Optimizer-diagram.png" class="img-fluid rounded z-depth-1" title="Catalyst optimizer" %}


### Spark DataFrame
DataFrame là một cấu trúc dữ liệu trong Spark, tương tự như một bảng trong cơ sở dữ liệu quan hệ (relational database). DataFrame cung cấp một API dễ sử dụng để thao tác và phân tích dữ liệu, bao gồm các phép toán như lọc, nhóm, sắp xếp và tính toán các thống kê. DataFrame có thể được tạo ra từ nhiều nguồn dữ liệu khác nhau, bao gồm các file CSV, JSON, Parquet, Avro và các cơ sở dữ liệu quan hệ. DataFrame cũng hỗ trợ các phép toán SQL, cho phép người dùng viết các truy vấn SQL để thao tác và phân tích dữ liệu trong Spark.

Đây là một ví dụ về cách tạo một DataFrame và hiển thị nội dung của nó:
```python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

# Define the schema
schema = StructType([
    StructField("Name", StringType(), True),
    StructField("Age", IntegerType(), True),
    StructField("City", StringType(), True)
])

# Create some sample data
data = [
    ("Alice", 30, "New York"),
    ("Bob", 24, "London"),
    ("Charlie", 35, "New York"),
    ("David", 29, "Paris"),
    ("Eve", 30, "London"),
    ("Frank", 40, "New York")
]

# Create the DataFrame
df = spark.createDataFrame(data, schema)
print("DataFrame created.")
df.show()
```

| Name | Age | City |
| --- | --- | --- |
| Alice | 30 | New York |
| Bob | 24 | London |
| Charlie | 35 | New York |
| David | 29 | Paris |
| Eve | 30 | London |
| Frank | 40 | New York |

Chúng ta có thể thực hiện các phép toán trên DataFrame, ví dụ tính số lượng người ở mỗi thành phố:
```python
df_grouped = df.groupBy("City").count()

# Show the result
df_grouped.show()
```

| City |count |
| --- | --- |
| London | 2 |
| New York | 3 |
| Paris | 1 |

Sử dụng câu lệnh *explain* để xem kế hoạch thực thi của truy vấn:
```python
df_grouped.explain()
```

```
== Physical Plan ==
AdaptiveSparkPlan isFinalPlan=false
+- HashAggregate(keys=[City#2], functions=[count(1)])
   +- Exchange hashpartitioning(City#2, 200), ENSURE_REQUIREMENTS, [plan_id=71]
      +- HashAggregate(keys=[City#2], functions=[partial_count(1)])
         +- Project [City#2]
            +- Scan ExistingRDD[Name#0,Age#1,City#2]
```
[Source Code](https://colab.research.google.com/drive/1Ds8wTnH4l2cexnUO7ZxSdDtLqhL6TeHc?usp=sharing)

### Transformation và Action

Trong Spark, các phép toán trên DataFrame/RDD được chia thành hai loại: **Transformation** (biến đổi) và **Action** (hành động). Việc phân biệt rõ hai loại này là chìa khóa để hiểu cách Spark thực thi công việc.

**Transformation** là các phép toán tạo ra một DataFrame/RDD mới từ một DataFrame/RDD đã có, ví dụ như `select`, `filter`, `groupBy`, `join`, `map`. Transformation mang tính chất **lazy evaluation** (đánh giá trễ) — nghĩa là khi gọi một transformation, Spark không thực thi ngay mà chỉ ghi nhận lại phép biến đổi đó vào một kế hoạch thực thi (execution plan), cụ thể là **DAG (Directed Acyclic Graph)**. Đoạn code `df_grouped = df.groupBy("City").count()` ở trên chính là một transformation — nó chưa thực sự tính toán gì trên dữ liệu.

Transformation lại được chia thành hai loại nhỏ hơn:

- **Narrow transformation**: mỗi partition đầu vào chỉ đóng góp cho đúng một partition đầu ra (ví dụ `select`, `filter`, `map`). Loại này không cần trộn (shuffle) dữ liệu giữa các executor nên hiệu năng cao.

{% include figure.liquid loading="eager" path="/assets/img/posts/spark/narrow-transformation.jpg" class="img-fluid rounded z-depth-1" title="Narrow Transformation" %}

- **Wide transformation**: dữ liệu từ nhiều partition đầu vào cần được gộp lại để tạo ra partition đầu ra (ví dụ `groupBy`, `join`, `distinct`). Loại này đòi hỏi **shuffle** — di chuyển dữ liệu qua lại giữa các executor trên mạng — nên thường tốn kém hơn về hiệu năng.

{% include figure.liquid loading="eager" path="/assets/img/posts/spark/wide-transformation.jpg" class="img-fluid rounded z-depth-1" title="Wide Transformation" %}


**Action** là các phép toán kích hoạt Spark thực sự thực thi toàn bộ DAG đã được xây dựng từ các transformation trước đó, và trả về kết quả cho driver program hoặc ghi dữ liệu ra bên ngoài (file, database...). Một số action phổ biến: `show()`, `count()`, `collect()`, `explain()`, `write()`. Trong ví dụ trên, `df_grouped.show()` chính là action khiến Spark thực sự chạy phép `groupBy` và `count`.

Cơ chế lazy evaluation này cho phép Spark tối ưu hóa toàn bộ chuỗi transformation trước khi thực thi (thông qua Catalyst optimizer đã đề cập ở phần Spark SQL), thay vì thực thi tuần tự từng bước một cách kém hiệu quả.














