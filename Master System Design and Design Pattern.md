# **Master System Design and Design Pattern**

## 🔷 Giới thiệu

### Distributed System
- **Distributed System (Hệ thống phân tán)** là một tập hợp các tiến trình địện toán (process) độc lập, được kết nối với nhau bởi một hệ thống mạng (network) để các process này có thể truyền nhận thông tin. Các process này phối hợp hoạt động với nhau như một thực thể duy nhất đối với người dùng bên ngoài nhằm thực thi một task nào đó

### BE System techniques for distributed system - SQL / NoSQL

- **NoSQL - Document DB**
    - Là một cơ sở dữ liệu lưu trữ thông tin trong các dạng tài liệu (chẳng hạn như JSON hoặc BSON)

    - Nó mang đến nhiều ưu điểm, bao gồm:
        - Một mô hình dữ liệu trực quan, nhanh chóng và dễ dàng cho các nhà phát triển làm việc
        - Một lược đồ linh hoạt cho phép mô hình dữ liệu phát triển khi nhu cầu của ứng dụng thay đổi
        - Khả năng mở rộng theo chiều ngang


- **NoSQL - Graph DB**
    - Đây là loại cơ sở có cấu trúc phức tạp nhất, là ngôn ngữ truy vấn dành cho API và là một môi trường thực thi phía server (server-side runtime), cho phép máy khách yêu cầu chính xác dữ liệu mà họ cần. Khác với REST, GraphQL sử dụng hệ thống kiểu dữ liệu (type system) để định nghĩa cấu trúc dữ liệu và cho phép truy xuất nhiều tài nguyênchỉ trong một yêu cầu duy nhất, từ đó giảm thiểu các vấn đề như lấy dư dữ liệu (over-fetching) và lấy thiếu dữ liệu (under-fetching).

### CAP Theorem

- Định lý CAP được phát biểu rằng các hệ thống dữ liệu phân tán không thể đồng thời đảm bảo 3 yếu tố : Tính nhất quán (Consistency - C), tính khả dụng (Availability - A) và khả năng chịu lỗi phân vùng (Partition Tolerance - P). Hệ thống chỉ có thể cung cấp tối đa hai trong ba tính chất này cùng lúc