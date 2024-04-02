
https://topdev.vn/blog/phan-chia-du-lieu-sharding-data-partitioning/
https://www.singlestore.com/blog/database-sharding-vs-partitioning-whats-the-difference/
# Overview
Sharding và partition là hai kỹ thuật được sử dụng trong cơ sở dữ liệu để tăng hiệu suất và khả năng mở rộng của hệ thống. Tuy nhiên, chúng có mục tiêu và cách thức hoạt động khác nhau.

1. **Sharding (phân mảnh)**:
    
    - Sharding là quá trình phân chia dữ liệu của một cơ sở dữ liệu thành nhiều phần nhỏ hơn được gọi là shard và lưu trữ mỗi shard trên một máy chủ hoặc một nhóm máy chủ riêng biệt.
    - Mỗi shard có thể được quản lý và truy cập một cách độc lập, giảm bớt áp lực cho một máy chủ cụ thể và tăng khả năng mở rộng của hệ thống.
    - Dữ liệu được phân chia dựa trên một khóa hoặc thuộc tính cụ thể (ví dụ: theo giá trị của một trường khóa chính) để đảm bảo rằng các dòng dữ liệu có liên quan được lưu trữ cùng một shard.
2. **Partition (phân vùng)**:
    
    - Partition là quá trình phân chia một bảng hoặc một chỉ mục của cơ sở dữ liệu thành nhiều phần nhỏ hơn được gọi là partition dựa trên một tiêu chí nhất định.
    - Mỗi partition có thể lưu trữ một tập hợp con của dữ liệu và được quản lý một cách độc lập. Các partition có thể được lưu trữ trên cùng một máy chủ hoặc trên các máy chủ khác nhau.
    - Dữ liệu có thể được phân vùng dựa trên các tiêu chí như giá trị của một trường cụ thể, phạm vi giá trị hoặc một thuộc tính khác.

Tóm lại, sharding và partition đều là các kỹ thuật để phân chia dữ liệu thành các phần nhỏ hơn để tăng hiệu suất và khả năng mở rộng của hệ thống. Tuy nhiên, sharding thường áp dụng cho toàn bộ cơ sở dữ liệu và lưu trữ mỗi phần trên các máy chủ riêng biệt, trong khi partition thường áp dụng cho các đối tượng cụ thể như bảng hoặc chỉ mục và có thể lưu trữ trên cùng một máy chủ hoặc trên các máy chủ khác nhau.
