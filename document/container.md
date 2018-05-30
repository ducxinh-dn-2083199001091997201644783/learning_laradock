# List container

## Backend
1. PHP-FPM HHVM(facebook)

## Database engines
    - MySQL, PostgreSQL, MariaDB, MongoDB, Neo4j, RethinkDB
## Cache Engines
    - Redis, Memcached, Aerospike
## Server
    - NGINX, Apache2, Caddy
## Message Queueing Systems
    - Beanstalkd, RabbitMQ
## Công cụ
    - Composer, Git, Node, Gulp, xDebug, PhpMyAdmin, PgAdmin, ElasticSearch, Selenium, Envoy, Vim...

1. Memcached
```
Memcached  là một hệ thống lưu trữ bản sao các đối tượng (objects) và dữ liệu được truy cập nhiều lần để tăng tốc độc truy xuất. Mục đích chính của nó là để tăng tốc độ ứng dụng web bằng cách truy vấn cơ sở dữ liệu bộ nhớ đệm, nội dung, hoặc kết quả tính toán khác.
- Cách lưu trữ và get dữ liệu giống như session.
- được lưu trong ổ cứng (HDD)
```
    - Ưu điểm
        - Ở mức nhỏ người ta thường dùng memcached để làm nơi lưu trữ dữ liệu chia sẻ, thường là lưu session. Cái này rất tiện lợi nhất là trong các kiểu loadbalancing đơn giản như nginx hay pound, khỏi phải lo tới vấn đề persistence session.
        - Ở mức lớn hơn một chút, người ta dùng memcached để giảm thiểu read từ db, cho những dữ liệu ít thay đổi và cần tính toán, query phức tạp, tốn tài nguyên.
        - Các thể cài đặt trên môi trường: Windows, Linux

    - Nhược điểm
        - Memcached không có cơ chế thẩm định tính chính xác của dữ liệu lưu trong nó. Điều này có thể thấy quá cấu trúc hệ thống (memcached không có bất cứ sự liên hệ nào với db, mà nằm độc lập).
        - Muốn dùng thì phải cài memcached vào máy chủ.
        - Chưa đồng bộ tự động với dữ liệu database khi dữ liệu thay đổi. Ví dụ: Database có dữ liệu là A và Memcached cũng có dữ liệu là A. Lúc database đổi giá trị sang B nhưng memcached vẫn là A. Các bạn có thể tham khảo giảm pháp sử dụng Sqlcachedependency

## Database
2. Percona: (Database management)
```
Có Percona Server for MySQL, Percona Server for MongoDB
Percona là nhánh của mysql. nhằm mục đích tối ưu hóa lại các CSDL nền tảng
Percona Server for MySQL tập trung vào việc tăng hiệu suất cho MySQL, và đặc biệt tối ưu cấu trúc lưu trữ InnoDB (InnoDB storage engine) - với tên gọi XtraDB. Percona bám sát sự phát triển của các phiên bản MySQL, so với MariaDB thì Percona chỉ tối ưu hóa MySQL chứ không phát triển thêm các chức năng.
```

3. MariaDB: (Database management)
```
Nhánh của Mysql
```

4. Mssql: MicroSoft Mysql Server (Database management)
```
MicroSoft Mysql Server
```

5. Postgres: PostgreSql (Database management)
```
Postgresql is  object-relational database management system (ORDBMS)
```

6. Elasticsearch(ES)
- Elasticsearch Là search engine open source viết bằng java kế thừa từ Apache Lucene(root) cũng như Laravel kế thừa php
- Thực chất hoặt động như 1 web server, có khả năng tìm kiếm nhanh chóng (near realtime) thông qua giao thức RESTful.
- có khả năng phân tích và thống kê dữ liệu
- chạy trên server riêng và đồng thời giao tiếp thông qua RESTful. bạn chỉ cần gửi request http lên là nó trả về kết quả.
- là 1 hệ thống phân tán và có khả năng mở rộng tuyệt vời (horizontal scalability). Lắp thêm node cho nó là nó tự động auto mở rộng cho bạn.
- Hổ trợ: Java, PhP, Javascript, Ruby, .NET, Python
- Ưu điểm: search nhanh
- nhược điệm: CRUD chậm
- Full text search. nên k phù hợp với ứng dụng thường xuyên cập nhập dữ liệu

### No sql
1. MongoDB,
2. RavenDB
3. Redis
4. Neo4j(Graph database)

## Server
1. Caddy Server
```
    Caddy server được viết bằng Go tài liệu đầy đủ [Caddy Server](https://caddyserver.com/)
```

## Common
6. MINIO
```
Minio is an object storage server released under Apache License v2.0. It is compatible with Amazon S3 cloud storage service. It is best suited for storing unstructured data such as photos, videos, log files, backups and container / VM images. Size of an object can range from a few KBs to a maximum of 5TB.

Minio server is light enough to be bundled with the application stack, similar to NodeJS, Redis and MySQL.
```

7. Varnish
```
Varnish là một ứng dụng mã nguồn mở (Open source) có tác dụng lưu lại bộ nhớ đệm của website bằng phương thức làm proxy trung gian giữa nội dung website gốc và trình duyệt, và Varnish sẽ tạo một bản cache ngoài frontend. Hãy hiểu đơn giản hơn là, mặc định các webserver sẽ sử dụng cổng 80 để gửi dữ liệu tới trình duyệt để người dùng đọc nó, nhưng khi sử dụng Varnish thì chúng ta sẽ muốn cho người dùng nhận các dữ liệu trong cache nên sẽ sử dụng Varnish làm cổng 80, còn dữ liệu website gốc sẽ được trả về một cổng nào đó mà Varnish sẽ nhận dữ liệu trực tiếp từ đó rồi lưu lại và gửi cho người dùng. Nhìn chung Varnish sẽ làm việc tương tự như việc sử dụng NGINX làm proxy cho Apache vậy nhưng Varnish là một ứng dụng cache nên sẽ làm việc đó tốt hơn và có tốc độ truy xuất tốt hơn.
```

8. JENKINS
```
    Jenkins là một máy chủ tích hợp liên tục có thể mở rộng. Nó build và test phần mềm của bạn một cách liên tục và theo dõi sự thi hành và trạng thái của các remote jobs. Nó giúp cho team members và users thường xuyên có được code chạy ổn định.
    Giống CI.
```
9. Grafana
```
Grafana là open source có khả năng hiển thị metric, phân tích ứng dụng ... Grafana hỗ trợ các data source, phục vụ việc query metric, hiển thị lên dashboard.
Giám sát CPU,...
```
10. Adminer
```
Giống PHPMyAdmin, Windows SQL Management Studio nhưng thằng này k cần phải cài đặt
Là công cụ quản lý database(phpMinAdmin), được viết dưới dạng một File Script PHP duy nhất

```

11. Blackfire: debug and profiling 
```
debug từng đoạn từng function, số lượng gọi
```
12. XDebug: debug and profiling
```
debug từng đoạn từng function, số lượng gọi
```