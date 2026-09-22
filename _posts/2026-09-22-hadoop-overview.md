---
layout: distill
title: Introduction to Hadoop Distributed File System
date: 2026-09-22 08:00:00 +0800
published: true
description: Tổng quan về HDFS
tags: data-engineering data-lake hadoop hdfs
categories: data-engineering
mermaid:
  enabled: false
  zoomable: false

authors:
  - name: Khanh Xuan Vu

toc:
  - name: Blocks
  - name: Name Node
  - name: Data Node
  - name: High Availability
  - name: Small Files Problem
---

HDFS là hệ thống file phân tán được thiết kế để chạy trên phần cứng thông thường. Ý tưởng chính phía sau HDFS đó là chia nhỏ file thành các khối (blocks) - thường là 128MB và lưu trữ các khối này trên nhiều máy khác nhau. Điều này giúp cho HDFS cung cấp các tính năng quan trọng như: phân tán (distribution), nhân bản (replication), và khả năng chịu lỗi (fault tolerance). Dưới đây là một số concept quan trọng trong HDFS:

## Blocks
Trong HDFS, block là đơn vị lưu trữ dữ liệu vật lý nhỏ nhất. Thay vì lưu trữ một tệp tin lớn nguyên khối trên một máy chủ duy nhất, HDFS sẽ tự động chia nhỏ tệp đó thành các khối (blocks) độc lập và phân tán chúng trên nhiều máy chủ (DataNode) khác nhau trong cụm.

Lưu ý rằng, nếu một file có kích thước nhỏ hơn kích thước block (ví dụ file dung lượng 30 MB lưu vào block 128 MB), HDFS chỉ chiếm đúng 30 MB dung lượng ổ đĩa, chứ không chiếm trọn 128 MB.

Khi có một lớp trừu lượng (abstraction block) trên hệ thống tệp phân tán mang lại nhiều lợi ích:
- Một file có thể lớn hơn dung lượng ổ đĩa của một máy chủ duy nhất: bởi vì các khối được phân tán trên nhiều máy chủ, HDFS có thể lưu trữ các tệp tin lớn mà không bị giới hạn bởi dung lượng của một máy chủ duy nhất.
- Quản lý lưu trữ dễ dàng hơn: Vì kích thước block cố định nên rất dễ tính toán số lượng block có thể chứa trên một ổ đĩa.

## Name Node
NameNode là máy chủ chính trong HDFS, chịu trách nhiệm quản lý thông tin metadata của hệ thống file: filename, file permission và vị trí các block (block location) trên các DataNode. NameNode không lưu trữ dữ liệu thực tế mà chỉ lưu trữ thông tin về cách dữ liệu được phân tán trên các DataNode.

NameNode lưu trữ thông tin metadata trong bộ nhớ (RAM) để truy cập nhanh chóng. Khi một client muốn đọc hoặc ghi dữ liệu, nó sẽ liên hệ với NameNode để lấy thông tin về vị trí các block cần thiết. Bởi vì, NameNode là một điểm quan trọng trong HDFS, nếu NameNode gặp sự cố, toàn bộ hệ thống HDFS có thể bị gián đoạn. Do đó, các triển khai HDFS thường sử dụng cơ chế sao lưu (backup) hoặc triển khai NameNode dự phòng (standby NameNode) để đảm bảo tính sẵn sàng cao.

## Data Node

DataNode là các máy chủ trong HDFS chịu trách nhiệm lưu trữ dữ liệu thực tế. Mỗi DataNode quản lý các block dữ liệu trên ổ đĩa của nó và thực hiện các thao tác đọc/ghi dữ liệu theo yêu cầu từ client hoặc NameNode. Khi DataNode lỗi, NameNode sẽ phát hiện trình trạng giảm số lượng bản sao (replication) và thực hiện các hành động cần thiết để đảm bảo tính toàn vẹn của dữ liệu. Thông thường, số lượng bản sao của mỗi block mặc định là 3.

Dưới đây là một sơ đồ minh họa cách HDFS hoạt động:

{% include figure.liquid loading="eager" path="/assets/img/posts/hadoop/hdfs-architecture.jpg" class="img-fluid rounded z-depth-1" title="HDFS Architecture" %}

## High Availability
Như đã đề cập ở trên, NameNode là một điểm quan trọng trong HDFS. Nếu NameNode gặp sự cố, toàn bộ hệ thống HDFS có thể bị gián đoạn. Từ Hadoop 2.x trở đi, HDFS hỗ trợ cơ chế High Availability (HA) bằng cách triển khai hai NameNode: một NameNode hoạt động (Active) và một NameNode dự phòng (Standby). Khi NameNode hoạt động gặp sự cố, NameNode dự phòng sẽ tự động tiếp quản vai trò của NameNode hoạt động, đảm bảo hệ thống HDFS vẫn tiếp tục hoạt động mà không bị gián đoạn.



{% include figure.liquid loading="eager" path="/assets/img/posts/hadoop/hdfs-ha.jpg" class="img-fluid rounded z-depth-1" title="High Availability" %}

Trong sơ đồ trên, NameNode hoạt động (Active) xử lý tất cả các yêu cầu từ client, trong khi NameNode dự phòng (Standby) luôn đồng bộ với NameNode hoạt động thông qua cơ chế JournalNode. Khi NameNode hoạt động gặp sự cố, NameNode dự phòng sẽ tự động tiếp quản vai trò của NameNode hoạt động, đảm bảo hệ thống HDFS vẫn tiếp tục hoạt động mà không bị gián đoạn.

## Small Files Problem

Trong hệ thống thực tế, một vấn đề rất hay gặp phải với HDFS đó là "small files problem" - tình trạng có rất nhiều file có kích thước nhỏ, nhỏ hơn nhiều lần (<<) so với kích thước block mặc định (128 MB). Ví dụ: hàng triệu file log chỉ vài KB.

Vấn đề nằm ở chỗ, dù file nhỏ chỉ chiếm đúng dung lượng ổ đĩa thực tế của nó (như đã nói ở phần Blocks), NameNode vẫn phải cấp phát và lưu trong RAM một lượng metadata **cố định** cho mỗi file và mỗi block, bất kể file đó lớn hay nhỏ: bao gồm một object cho inode (tên file, quyền, thư mục cha...) và một object cho mỗi block thuộc về file đó. Do đó, càng nhiều file nhỏ thì tổng số object metadata càng lớn, trong khi tổng dung lượng dữ liệu thực tế có thể rất khiêm tốn - gây lãng phí RAM của NameNode, làm chậm thời gian khởi động (NameNode phải load toàn bộ metadata vào RAM khi start), và tạo áp lực lớn lên NameNode khi có quá nhiều request nhỏ lẻ.

Một con số ước lượng thường được dùng (theo tài liệu của Cloudera/Hortonworks) là mỗi object metadata (file, thư mục, hoặc block) chiếm khoảng **150 bytes** trong RAM của NameNode. Với một file nhỏ (chỉ có 1 block), ta cần 2 object: 1 inode + 1 block, tức khoảng 300 bytes/file.

**Ví dụ:** với NameNode có 1 GB RAM dành cho việc lưu metadata:

$$
\frac{1 \text{ GB}}{300 \text{ bytes/file}} = \frac{1{,}073{,}741{,}824 \text{ bytes}}{300} \approx 3.58 \text{ triệu file (block)}
$$

Để thấy rõ hơn tác động của việc chia nhỏ file, so sánh cùng 1 GB dữ liệu được lưu theo hai cách:

| Cách lưu | Số file/block | Số object metadata | RAM cần cho metadata |
|---|---|---|---|
| Gộp thành các file 128 MB (đúng kích thước block) | 8 file → 8 block | 16 | ~2.4 KB |
| Chia thành các file nhỏ 1 MB | 1024 file → 1024 block | 2048 | ~300 KB |

Cùng một lượng dữ liệu, nhưng cách chia nhỏ file tốn RAM NameNode gấp ~128 lần. Đây là lý do các hệ thống thực tế thường có bước gộp file nhỏ (compaction) trước khi ghi vào HDFS, hoặc dùng các định dạng lưu trữ theo container (như SequenceFile, Avro, Parquet) để nhiều bản ghi nhỏ được đóng gói chung vào một file lớn thay vì tạo ra hàng loạt file riêng lẻ.