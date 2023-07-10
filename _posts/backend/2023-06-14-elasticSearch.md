---
layout: post
title: Tìm hiểu về Elastic Search
group: backend
state: publish
---

Có rất nhiều công cụ phải trải qua khi làm backend như mysql, postgresql, docker, rabbitmq và Elastic Search cũng là một trong số nó. Vậy hôm nay chúng ta sẽ cùng tìm hiểu chi tiết về Elastic Search

# Các câu hỏi về ES (Elastic Search)

## 1. ElasticSearch là gì?

-   ElasticSearch là một công cụ tìm kiếm và phân tích dũ liệu mã nguồn mở, được phát triển bởi công ty Elastic. Nó được thiết kế để xử lý và truy vấn tập dữ liệu lớn, phức tạp và có cấu trúc khác nhau.

-   ES là Search Engine số 1 thế giới. Được viết bằng Java.
-   Phiên bản đầu tiên ra đời vào năm 2010.
-   Chung hệ sinh thái cùng Kibana, Beat, Logstash, ...

## 2. Ưu nhược điểm của Elastic Search

### Ưu điểm:

-   Mã nguồn mở, miễn phí, cộng đồng lớn mạnh.
-   Tốc độ truy vấn nhanh, kể cả truy vấn phức tạp.
-   Hỗ trợ full-text search: tách từ, tách câu và tạo chỉ mục cho dữ liệu.
-   Hỗ trợ fuzzy search, auto-complete giúp tìm kiếm kể cả khi sai chính tả.
-   Chạy trên server riêng, tương tác qua RESTful API, tích hợp với nhiều hệ thống khác nhau.
-   Lưu data dạng JSON NoSQL.
-   Khả năng mở rộng cao có mô hình cluster.

### Nhược điểm:

-   Yêu cầu kiến thức về cấu hình và quản lý để triển khai và vận hành. Việc cấu hình sai có thể ảnh hưởng tới hiệu suất và tính sẵn sàng của hệ thống. Ví dụ lựa chọn cấu hình RAM, cấu hình shard, node, phân chia dữ liệu vào node.
-   Không phù hợp với các ứng dụng quá nhỏ. ES là một hệ thống tìm kiếm và phân tích phức tạp và ES yêu cầu một số tài nguyên nhất định để triển khai và vận hành vậy nên không có nhu cầu tìm kiếm với dữ liệu lớn thì việc sử dụng ES là không cần thiết.
-   Không có schema, dữ liệu không có sự đồng nhất nên sẽ khó khăn trong việc xử lý câu truy vấn phức tạp
-   ES cung cấp các API để sao lưu và phục hồi dữ liệu, nhưng không có tính năng backup và recovery tích hợp. Tuy nhiên có thể sử dụng công cụ ngoài để tích hợp.

## 3. Dữ liệu được xử lý thế nào trong Elasticsearch ?

    -   Elastic Search sử dụng Inverted Index ( chỉ mục đảo ngược là cơ sở dữ liệu ánh xạ tư nội dung đến vị trí trong bản ghi)

<p align="center">
  <img src="/images/backend/ElasticSeach/index.webp" />
</p>

Sau khi loại bỏ những từ mang ít ý nghĩa ví dụ như (a, an, each, for, to, by, the, ...) còn các term ( từ mang ý nghĩa) được giữ lại. Như hình trên thì "blue" được lưu dưới dạng 1:3, 3:2 (document:postion) được giải thích là "blue" nằm ở vị trí thứ 3 trong document 1 và vị trí thứ 2 ở document 3.

Khi có query full-text search, ví dụ tìm xem document nào chứa "blue sky". Cụm từ trên được tách thành 2 term riêng biệt "blue", "sky". Dựa vào bảng Inverted Index "blue" nằm ở document 1 và 3. "sky" nằm ở document cũng ở 2 và 3. Kết quả trả về là document 3.

## 4. Thuật toán tìm kiếm Term trong Inverted Index là gì ?

        * Thuật toán tìm kiếm term trong inverted Index được sử dụng bởi ES khá đơn giản và hiệu quả. Các bước chính của thuật toán như sau:

-   Bước 1: Tách các term: Văn bản cần tìm kiếm được tách thành các term riêng lẻ. Các term có thể là các từ đơn, cụm từ hoặc ký tự đặc biệt.

-   Bước 2: Tìm kiếm các term trong inverted index: Với mỗi term, ES tìm kiếm nó trong Inverted Index. nverted index là một danh sách các term được sắp xếp theo thứ tự bảng chữ cái và cho biết vị trí của các term trong các tài liệu. Cụ thể các invered index bao gồm các thông tin sau: + Term: Tên của các term cần tìm kiếm. + Posting list: Danh sách các vị trí của term trong các bản ghi. + Term frequency: Tần xuất xuất hiện của term trong các bản ghi. + Document frequency: Số lượng bản ghi chứa term đó.

    Ví dụ, nếu truy vấn của bạn là 'macbook', ES sẽ tìm kiếm 'macbook' trong inverted index và trả về 1 posting list bao gồm các vị trí của 'macbook' trong các bản ghi.

-   Bước 3: Tìm kiếm các tài liệu chứa các tearm: Với mỗi term, ES tìm kiếm danh sách các tài liệu chứa term đó và trả về kết quả.

-   Bước 4: Lọc kết quả: ES sử dụng các thuật toán lọc để loại bỏ các thông tin không phù hợp. Các thuật toán lọc này bao gồm tính toán điểm số của tài liệu dựa trên tuần suất xuất hiện của các term trong tài liệu, tính toán khoảng cách các term trong truy vấn và data và các thuật toán tương tự.

-   Bước 5: Trả về kết quả cuối cùng: ES trả về kết quả cho truy vấn tìm kiếm. Kết quả được sắp xếp theo thứ tự ưu tiên hoặc thứ tự mặc định tuỳ vào điều kiện trong truy vấn.

4. Mỗi Query GET gọi vào Elastic Search, các Inverted Index được đọc Disk hay Memory ?

-   Khi thực hiện truy vấn trên ElasticSearch, Inverted Index sẽ được sử dụng để tìm kiếm các term và trả về kết quả tương ứng. Việc Inverted Index được đọc từ Disk hay memory phụ thuộc vào cấu hình của ES và trạng thái của Inverted Index.

-   Mặc định, khi ES được cài đặt, Inverted Index được lưu trữ trên disk và được đọc từ đó khi cần thiết. Tuy nhiên Elasticsearch cung cấp khả năng đọc index từ bộ nhớ Memory để tăng tốc độ truy vấn. Khi ES đọc Inverted Index từ Disk, nó sẽ sử dụng bộ nhớ đệm(cache) để lưu trữ một phần của của Invered Index trong RAM để tăng tốc độ truy vấn. Elastic Search sẽ cố gắng tối ưu hoá việc sử dụng bộ nhớ đệm bằng các sử dụng các chiến lược Least Recently Used (LRU) để quản lý bộ nhớ và loại bỏ các phần của Inverted Index không được sử dụng nhiều. Khi bộ nhớ đệm đầy, ES sẽ sử dụng LRU để loại bỏ các term ít được sử dụng nhất và giải phóng không gian cho các term mới được sử dụng trong truy vấn. Các term được sử dụng nhiều sẽ được giữ lại trong bộ nhớ đệm để đảm bảo hiệu quả truy vấn cao.

-   Nếu Inverted Index của Elasticsearch không vượt quá dung lượng bộ nhớ có sẵn, ES sẽ tải toàn bộ Inverted Index lên bộ nhớ và sẽ sử dụng để truy vấn. Việc đọc này sẽ tăng tốc độ truy vấn lên đáng kể, đặc biết khi có nhiều truy vấn đồng thời hoặc Inverted có kích thước lớn.
