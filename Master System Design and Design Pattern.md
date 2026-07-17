# **Master System Design and Design Pattern**

## 🔷 Introduction

### Distributed System (DS)

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

- **Định lý CAP** được phát biểu rằng các distributed system không thể đồng thời đảm bảo 3 yếu tố : Tính nhất quán (Consistency - C), tính khả dụng (Availability - A) và khả năng chịu lỗi phân vùng (Partition Tolerance - P). Hệ thống chỉ có thể cung cấp tối đa hai trong ba tính chất này cùng lúc

- **Ba đặc tính của CAP**
  - Tính nhất quán (Consistency) nghĩa là mọi node trong hệ thống đều phải trả về cùng một dữ liệu tại cùng một thời điểm. Sau khi dữ liệu mới được ghi thành công, mọi client truy cập vào bất kì node nào cũng phải nhìn thấy phiên bản dữ liệu mới nhất thay vì dữ liệu cũ hoặc chưa đồng bộ
  - Tính sẵn sàng (Availability) thể hiện khả năng hệ thống luôn phản hồi yêu cầu từ người dùng, kể cả khi một hoặc nhiều node gặp lỗi. Mỗi request gửi tới hệ thống đều phải nhận được phản hồi hợp lệ thay vì bị từ chối hoặc treo kết nối

  - Tính chống chịu phân mảnh (Partition Tolerance)
    - Là khả năng hệ thống vẫn tiếp tục hoạt động ngay cả khi xảy ra lỗi mạng hoặc mất kết nốp giữa các node trong cluster. Trong môi trường distributed system, việc gián đoạn giao tiếp là điều gần như không thể tránh khỏi do lỗi hạ tầng, nghẽn mạng hoặc sự cố trung tâm dữ liệu. Khi xảy ra partition, các node có thể ko còn trao đổi dữ liệu với nhau nhưng hệ thống vẫn phải duy trì hoạt động thay vì dừng hoàn toàn

    - Đây là yếu tố cốt lõi của CAP bởi các DS luôn phải đối mặt với nguy cơ phân mảnh mạng. Chính vì vậy, khi partition xảy ra, kiến trúc hệ thống buộc phải lựa chọn ưu tiên giữa Consistency hoặc Availability để tiếp tục vận hành

- Để đạt được tính Consistency, hệ thống cần đồng bộ dữ liệu giữa các node gần như ngay lập tức trước khi xác nhận thao tác ghi hoàn tất, vì vậy việc ưu tiên Consistency thường làm tăng độ trễ xử lý hệ thống vì phải chờ quá trình đồng bộ hoàn tất. Để đảm bảo Availability, dữ liệu thường được sao chép trên nhiều node khác nhau nhằm duy trì khả năng phục vụ ngay cả khi xảy ra sự cố phần cứng hoặc mất kết nối cục bộ. Đổi lại, hệ thống có thể chấp nhận việc dữ liệu giữa các node chưa hoàn toàn đồng nhất trong một khoảng thời gian ngắn. Chính vì nguyên nhân trên nên hệ thống ko thể đồng thời có cả Consistency và Availability

### System design component: Data Partitioning techniques

- Data Partitioning là một phương pháp được sử dụng trong thiết kế hệ thống để chia nhỏ dữ liệu lớn thành các phần nhỏ hơn, dễ quản lý hơn. Nó không chỉ giúp tăng hiệu suất và khả năng mở rộng của hệ thống mà còn giúp giảm thiểu độ trễ và tăng cường hiệu suất truy vấn dữ liệu

- Các cách thức Data Partitioning:
  - **Horizontal Partitioning (còn được gọi là Sharding)** là cách chia dữ liệu của cùng một bảng ra nhiều DB khác nhau, Sharding giúp tăng hiệu suất truy vấn bằng cách giảm số lượng hàng dữ liệu cần xử lý trong mỗi truy vấn. Vấn đề gặp phải ở đây là tải trọng có thể ko được phần bố đều giữa tất cả các máy chủ

  - **Vertical Partitioning** là cách chia dữ liệu thành các phần dựa trên cột, với mỗi phần vùng chứa một tập hợp con của cột dữ liệu, Vertical Partitioning giúp tăng hiệu suất truy vấn bằng cách giảm số lượng cột dữ liệu cần xử lý trong mỗi truy vấn. Vấn đề gặp phải ở đây là nếu ứng dụng phát triển thêm, khi đó cần phải phân vùng thêm, ngoài ra nó cũng sẽ gặp vấn đề tương tự như Horizontal vì bản chất nó cũng là phân chia theo phạm vi

  - **Directory Based Partitioning** đây là kỹ thuật được sử dụng phổ biến nhất trong việc phân mảnh dữ liệu, nó sử dụng một lookup service (dịch vụ tra cứu) có tác dụng quyết định ánh xạ dữ liệu sẽ nằm ở đâu, DB nào. Mỗi khi có request ghi hoặc đọc sẽ thông qua lookup service để mapping vị trí sẽ đọc và ghi. Business mở rộng số lượng server có thể tăng lên mà không ảnh hưởng hay đồi hỏi ứng dụng phải thay đổi theo

## 🔷 Design Pattern

### SOLID Principal

- ** SOLID Principal** bao gồm 5 nguyên tắc quan trọng khi lập trình hướng đối tượng (OOP). 5 nguyên tắc này làm cho code trở nên dễ hiểu hơn, dễ bảo trì và dễ mở rộng hơn

- 5 nguyên tắc bao gồm:
  - **S-Single responsibility principle** Một class nên giữ một trách nhiệm cho một mục đích duy nhất
  - **O-Open/closed principle** Có thể thoải mái mở rộng 1 class nhưng ko được sửa đổi bên trong class đó
  - **L-Liskov substitution principle** Trong một chương trình, các object của class con có thể thay thế class cha mà không làm thay đổi tính đúng đắn của chương trình
  - **I-Interface segregation principle** Thay vì dùng 1 interface lớn, ta nên tách thành nhiều interface nhỏ, với nhiều mục đích cụ thể
  - **D-Dependency inversion principle** Các module cấp cao không nên phụ thuộc vào các module cấp thấp, cả 2 nên phụ thuộc vào abstraction. Interface (abstraction) ko nên phụ thuộc vào chi tiết, mà ngược lại, các class giao tiếp với nhau thông qua interface, ko phải thông qua implementation

### Singleton Design Pattern
- **Singleton Design Pattern** là một trong số 5 design patterns thuộc nhóm Creational Design Pattern - nhóm hỗ trợ khởi tạo class. Nó đảm bảo một class chỉ có duy nhất một instance được khởi tạo và nó cung cấp phương thức truy cập đến instance đó từ mọi nơi (global access)

- **Cách sử dụng**
  - Đặt contructor là private để client ko thể khởi tạo object của class
  - Tạo một biến static private là instance của class đó để đảm bảo rằng nó là duy nhất và chỉ được tạo ra trong class đó thôi
  - Tạo một public static method trả về instance vừa khởi tạo bên trên, đây là các duy nhất để các class khác có thể truy cập vào instance của class này

  ```ts
  // Logger class
  class Logger {
    private static instance: Logger;
    private contructor() {}
    public static getInstance(): Logger {
      if(!Logger.instance) {
        Logger.instance = new Logger();
      }

      return Logger.instance
    }

    add(log: string) {
      this.logs.push(log)
    }

    display() {
      console.log(this.logs)
    }
  }

  // Using
  productLogs = Logger.getInstance()
  productLogs.add('4:03:31 PM [vite] hmr update /src/pages/AdminProducts/ProductFormModal.tsx')

  centerLogs = Logger.getInstance()
  centerLogs.display()
  // Console.log: 4:03:31 PM [vite] hmr update /src/pages/AdminProducts/ProductFormModal.tsx
  ```

### Factory Design Pattern & Abtract Factory Design Pattern

- **Factory Design Pattern** (hay còn gọi là virtual contructor) là một mẫu thiết kế thuộc nhóm Creational Patterns - những mẫu thiết kế cho việc khởi tạo đối tượng của class. FDP cung cấp một interface, phương thức trong việc tạo nên một đối tượng trong class. Nhưng để cho class con kế thừa của nó có thể ghi đè để chỉ rõ đối tượng nào sẽ được tạo ra. FDP giao việc khởi tạo một đối tượng cụ thể cho lớp con

- **Mục đích**
  - Tạo ra một cách khởi tạo object mới thông qua một interface chung
  - Che giấu quá trình xử lý logic của phương thức khởi tạo
  - Giảm sự phụ thuộc, dễ dàng mở rộng
  - Giảm khả năng gây lỗi compile

  ```ts
  // Reader class
  class ReaderFactory {
    static create(type: string) {
      switch (type) {
        case type.EXCEL:
          return new ReaderForExcel();
        case type.WORD:
          return new ReaderForWord();
        case type.PDF:
          return new ReaderForPDF();
      }   
    }
  }

  // Using
  const reader = ReaderFactory.create(type)
  reader.read();

  ```

- **Abtract Factory Design Pattern** được xây dựng dựa trên FDP và nó được xem là một factory cao nhất trong hệ thống phân cấp. Pattern này sẽ tạo ra các factory là class con của nó và các factory này được tạo ra giống như cách mà factory tạo ra các sub-class

  ```ts
  // Writer class
  class WriterFactory {
    static create(type: string) {
      switch (type) {
        case type.EXCEL:
          return new WriterForExcel();
        case type.WORD:
          return new WriterForWord();
        case type.PDF:
          return new WriterForPDF();
      }   
    }
  }
  ```

  Tuy nhiên nếu tách biệt như vậy sẽ dẫn đến trường hợp

  ```ts
  // reader & writer conflict tool
  const reader = ReaderFactory.create(type.EXCEL)
  const writer = WriterFactory.create(type.WORD)
  ```

  Vì vậy, thay vì chọn Object ta sẽ chọn Factory

  ```ts
  interface Reader {
    read(): void;
  };
  interface Writer {
    write(): void;
  };

  // DocumentFactory - Abstract Factory
  interface DocumentFactory {
    createWriter(): Writer;
    createReader(): Reader;
  }
  ```
  ```ts
  class WriterForWord implements Writer {
    write() {
      console.log('WriterForWord');
    }
  };
  class ReaderForWord implements Reader {
    read() {
      console.log('ReaderForWord');
    }
  };

  // WordFactory
  class WordFactory implements DocumentFactory {
    createWriter() {
      return new WriterForWord();
    }

    createReader() {
      return new ReaderForWord();
    }
  }
  ```

  ```ts
  // Using
  const word = new WordFactory();
  
  // Word writer
  const writer = word.createWriter();
  writer.write()

  // Word reader
  const reader = word.createReader();
  reader.read();
  ```

### Strategy Design Pattern

- **Strategy Design Pattern** là một trong những mẫu thiết kế hành vi được sử dụng khi có nhiều thuật toán cần áp dụng cho một nhiệm vụ cụ thể , và khách hàng sẽ là người quyết định chọn thuật toán nào để sử dụng trong thời điểm chương trình đang được thực thi.

  ```ts
  interface PaymentStrategy {
    pay(amount: number): void;
  }
  ```

  ```ts
  // Visa payment method
  class VisaPayment implements PaymentStrategy {
    pay(amount: number) {
      console.log(`Payment with Pisa: ${amount}`);
    }
  }

  // Paypal payment method
  class PaypalPayment implements PaymentStrategy {
    pay(amount: number) {
      console.log(`Payment with Paypal: ${amount}`);
    }
  }
  ```

  ```ts
  // Payment service
  class PaymentService {
    constructor(private paymentStrategy: PaymentStrategy) {}
    
    pay(amount: number) {
      this.paymentStrategy.pay(amount);
    }
  }
  ```

  ```ts
  // Using
  const payment = new PaymentService(new PaypalPayment())
  payment.pay(50);
  ```