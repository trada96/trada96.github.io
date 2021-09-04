---
layout: post 
title: SQL vs NoSQL Khác biệt là gì và được sử dụng khi nào ? 
group: big_data
state: publish

---


Hôm nay chúng ta cùng nhau tìm hiểu về sql và nosql xem chúng là gì, và sự khác nhau giữa chúng là gì nhé.

Các phần chính trong bài viết

**1. SQL và NoSQL là gì ?**

**2. Sự khác nhau giữa SQL và NoSQL là gì ?**

**3. Khi nào thì sử dụng SQL hay NoSQL ?**


Nội dung chi tiết: 

## 1. SQL và NoSQL là gì ?

- SQL (Structured Query Language) là ngôn ngữ truy vấn có cấu trúc, là dạng database có quan hệ và mối quan hệ được thể hiện qua dạng bảng. SQL được sử dụng hiệu quả để chèn chèn, tìm kiếm, cập nhật, xóa bản ghi trong database. Các database SQL thường sử dụng là MySQL, Oracle, Ms SQL Server, Sybase, etc...

- NoSQL (Non SQL or Not Only SQL) được phát triển từ cuối những năm 2000 với trọng tâm là khả năng mở rộng quy mô (scale), truy vấn nhanh, cho phép thay đổi ứng dụng thường xuyên và giúp lập trình đơn giản hơn cho các nhà phát triển. NoSQL không yêu cầu một schema cố định và nó được sử dụng cho các kho dữ liệu phân tán với nhu cầu lưu trữ dữ liệu lớn, ứng dụng thời gian thực.

## 2. Sự khác nhau giữa SQL và NoSQL là gì ?

**Những điểm khác nhau chính**

- SQL làn cơ sở dữ liệu quan hệ trong khi nosql là cơ sở dữ liệu phi quan hệ hoặc phân tán.
- SQL là cơ sở dữ liệu dạng bảng, còn nosql là cơ sở dữ liệu bao gồm dạng document, cặp key-value, graph.
- SQL có khả năng scale theo chiều dọc còn nosql mở rộng theo chiều ngang.
- SQL sử dụng với dữ liệu có cấu trúc được định nghĩa sẵn, còn nosql có thể sử dụng với dữ liệu phi cấu trúc.


- Chi tiết:

| Parameters      | SQL     | NoSQL
| -----------     | ----------- |--|
| Definition      | SQL là cơ sở dữ liệu quan hệ       | NoSQL là cơ sở dữ liệu phi quan hệ|
| Design for        | SQL truyền thống sử dụng truy vấn để phân tích dữ liệu. Thường được sử dụng cho các hệ thống OLAP        |NoSQL được phát triển để đáp ứng nhu cầu của dữ liệu lớn, đa dạng của các ứng dụng hiện đại |
|Query Language|Ngôn ngữ truy vấn có cấu trúc| Không có ngôn ngữ truy vấn|
|Type|SQL có kiểu dữ liệu dạng bảng|NoSQL có kiểu dữ liệu đa dạng như document, cặp key-value, graph|
|Schema|SQL cần phải có schema định nghĩa sẵn trước khi sử dụng|NoSQL có schema động để sử dụng với dữ liệu phi cấu trúc|
|Khả năng mở rộng|SQL databases có khả năng mở rộng theo chiều dọc|NoSQL có khả năng mở rộng theo chiều ngang|
|Exampes| Oracle, Postgres, MS SQL|MongoDB, Redis, Neo4j, Cassandra, Hbase|
|Phù hợp nhất |Phù hợp với các nghiệp vụ chuyên sâu, phức tạp, mối quan hệ giữa các records, tables|Không phù hợp với truy vấn phức tạp|
|Lưu trữ dữ liệu phân cấp|SQL không phù hợp với dữ liệu phân cấp|Phù hợp hơn với dữ liệu phân cấp vì nó hỗ trợ định dạng key-value (json)|
|[Tính nhất quán](http://tiepvut.blogspot.com/2016/07/nhat-quan-du-lieu-trong-nosql.html)|Nó phải được cấu hình để có tính nhất quán mạnh mẽ|Nó tùy thuộc vào loại DBMS vì một số cung cấp tính nhất quán như mongoDB, trong khi có cái khác lại chỉ cung cấp tính nhất quán cuối cùng như cassandra|
|Best Used for|SQL là lựa chọn đúng đắn để giải quyết các vấn đề [ACID](https://vi.wikipedia.org/wiki/ACID)|NoSQL được sử dụng tốt nhất để giải quyết vấn đề dữ liệu lớn|
|Important|Nên được sử dụng khi tính hợp lệ vô cùng quan trọng|Sử dụng khi dữ liệu nhanh quan trọng hơn dữ liệu chính xác|
|Best Option|Khi bạn cần hỗ trợ các try vấn động, phức tạp|Sử dụng khi bạn cần mở rộng quy mô dựa trên yêu cầu thay đổi|
|Best Features|Hỗ trợ đa nền tảng, bảo mật và miễn phí|Dễ dàng sử dụng, hiệu suât cao và công cụ linh hoạt|

## 3. Khi nào thì sử dụng SQL hay NoSQL ?

### 3.1 Khi nào sử dụng SQL?

- Được ưu tiên khi muốn thực hiện join các bảng hay các câu truy vấn phức tạp
- Thương được sử dụng với các giao dịch tài chính
- Kiểu dữ liệu đã có cấu trúc định dạng sẵn
- Sử dụng phù hợp với OLAP

### 3.2 Khi nào sử dụng NoSQL ?

- Sử dụng với dữ liệu lớn vì khả năng scale dễ dàng.
- Sử dụng với các dữ liệu phân cấp hoặc dữ liệu phi cấu trúc


# Tư liệu tham khảo:

- [https://www.guru99.com/sql-vs-nosql.html](https://www.guru99.com/sql-vs-nosql.html)
