# **Vue JS**

## 🔷 MỤC LỤC

## 🔷 Tổng quan

### VueJS

- **VueJS** là một framework Javascript được sử dụng để xây dựng các giao diện người dùng. Nó được xây dựng dựa trên HTML, CSS và JS tiêu chuẩn, đồng thời cung cấp mô hình lập trình khai báo (declarative) và dựa trên component (component-based), giúp developer phát triển các giao diện người dùng với mọi mức độ phức tạp một cách hiệu quả, dễ bảo trì và có khả năng mở rộng

  ```js
  import { createApp, ref } from "vue";

  createApp({
    setup() {
      return {
        count: ref(0),
      };
    },
  }).mount("#app");
  ```

  ```html
  <div id="app">
    <button @click="count++">Count is: {{count}}</button>
  </div>
  ```

### Single-File Components (SFC)

- Vue Single-File Components (hay còn gọi là `*.vue` file, viết tắt là SFC) là một định dạng file đặc biệt cho phép chúng ta đóng gói mẫu, logic và kiểu dáng của một component Vue trong một file duy nhất

  ```html
  <script setup>
    import { ref } from "vue";
    const count = ref(0);
  </script>

  <template>
    <button @click="count++">Count is: {{count}}</button>
  </template>

  <style scoped>
    button {
      font-weight: bold;
    }
  </style>
  ```

### API Styles

- **Options API**
  - Định nghĩa logic của một component bằng cách sử dụng một object chứa các tùy chọn (options) như `data`, `methods` và `mounted`. Các thuộc tính được khai báo trong những option này sẽ được truy cập thông qua `this` bên trong các hàm, trong đó `this` sẽ tham chiếu đến instance của component.

  - Nó là phương pháp truyền thống và được sử dụng nhiều trong các dự án Vue.js cũ, thường phù hợp cho các dự án đơn giản, ko quá phức tạp và quy mô nhỏ

- **Composition API**
  - Định nghĩa logic của một component bằng cách sử dụng các hàm API được import từ Vue. Trong Single-File Components (SFC), Composition API thường được sử dụng cùng với `<script setup>`. Thuộc tính setup là một chỉ thị (hint) giúp Vue thực hiện các phép biến đổi (transform) ở thời điểm biên dịch (compile-time), cho phép chúng ta sử dụng Composition API với ít mã lặp (boilerplate) hơn. Ví dụ, các module được import và các biến hoặc hàm được khai báo ở cấp cao nhất (top-level) bên trong `<script setup>` có thể được sử dụng trực tiếp trong template mà không cần khai báo hoặc trả về thêm.

  - Nó là phương pháp mới, mang lại sự linh hoạt và khả năng tái sử dụng cao hơn. Ngoài ra, nó giúp giảm bớt sự phức tạp của các component lớn bằng cách chia nhỏ logic thành các phần nhỏ hơn, dễ quản lý hơn

## 🔷 Vue cơ bản

### Directives

- **Directives (chỉ thị)** là các thuộc tính đặc biệt có tiền tố `v-`. Vue cung cấp một số direcitive tích hợp, ví dụ như `v-html`, `v-bind`, ...

- **Arguments**: Một số directive có thể có một arguments, được ký hiệu bằng dấu 2 chấm đứng sau directive

  ```html
  <a v-bind:href="url"></a>
  ```

- **Dynamic Arguments** Cũng có thể sử dụng biêu thức JS trong agruments của directive bằng cách đặt vào trong dấu ngoặc vuông

  ```html
  <script setup>
    // Dynamic Arguments
    const attribute = "style";
    const value = {
      cursor: "help",
      padding: "10px",
    };
  </script>

  <template>
    <button :[attribute]="value">Dynamic Arguments</button>
  </template>
  ```

### Tại sao cần dùng ref?

- Khi sử dụng ref trong template và sau đó thay đổi giá trị của ref, Vue sẽ tự động phát hiện sử thay đổi và cập nhật DOM tương ứng

- Trong JS tiêu chuẩn, ko có cách nào để phát hiện truy cập hoặc biến đổi các biến đơn

- Khi một component được hiển thị lần đầu tiên, Vue theo dõi mỗi ref đã được sử dụng trong quá trình hiển thị. Sau đó, khi một ref bị biến đổi, nó sẽ kích hoạt để hiển thị lại cho các component đang theo dõi nó

### Một số Directive phổ biến

- **v-if** 
    - Được sử dụng để hiển thị một khối mã có điều kiện. 
    - Khối mã sẽ chỉ được hiển thị nếu biểu thức của chỉ thị trả về một giá trị đúng (truthy).
    - Ngoài ra còn có `v-else-if` và `v-else` cũng có chức năng tương tự trong các ngôn ngữ lập trình

- **v-show​** 
    - Một lựa chọn khác để hiển thị một phần tử có điều kiện là sử dụng `v-show` chỉ thị.

    - Cách sử dụng về cơ bản là tương tự:

        ```html
        <h1 v-show="ok">Hello!</h1>
        ```

    - Sự khác biệt là một phần tử có thuộc tính với `v-show` sẽ luôn được hiển thị và tồn tại trong DOM, thuộc tính ` v-show` chỉ thay đổi thuộc tính CSS display của phần tử đó.

- **v-for​** 
    - Để hiển thị danh sách các mục dựa trên một mảng, nó yêu cầu một cú pháp đặc biệt có dạng `item in items`, trong đó `items` là mảng dữ liệu nguồn và `item` là bí danh cho phần tử mảng đang được lặp lại

    - Chẳng hạn, có ví dụ sau
        
        ```html
        <script setup>
        import { ref, reactive } from 'vue'
        const AIs = reactive([
            { name: 'cursor', price: 20 },
            { name: 'claude', price: 17 }
        ])
        </script>

        <template>
            <ul>
                <li v-for="AI in AIs">
                    IDE AI: {{ AI.name }} pro plan with {{ AI.price }} $
                </li>
            </ul>
        </template>
        
        <!-- Output -->
        <!-- 
          IDE AI: cursor pro plan with 20 $
          IDE AI: claude pro plan with 17 $
        -->
        ```

### Event handling

- Để lắng nghe sự kiện DOM và chạy một số mã JS, có thể sử dụng directive `v-on`, thường được viết tắt bằng kí tự `@`

  ```html
  <script setup>

  const greeting = () => {
    alert('Greeting !!!');
  }

  </script>

  <template>
    <button @click="greeting">Greeting !</button>
  </template>
  ```

- **Event Modifier** là các hậu tố đặc biệt có thể thêm vào Event handling để dễ dàng kiểm soát hành vi của các sự kiện hơn. Chúng giúp đơn giản hóa các tác vụ thông thường như ngăn chặn sự lan truyền, ngăn chặn các hành động mặc định và đảm bảo rằng sự kiện chỉ được kích hoạt trong những điều kiện nhất định. Các event modifier phổ biến như là `.stop`, `.prevent`, `.self`, `.capture`, ...

  ```html
  <script setup>
  import { ref } from 'vue';

  const greeting = () => { /* Code */ }
  const callDiv = () => { /* Code */ }

  const styleDiv = ref({
    background: 'green',
    height: '100px',
  })

  </script>

  <template>
    <div @click="callDiv" :style="styleDiv">
      <button @click.self.stop="greeting">Greeting !</button>
    </div>

  </template>
  ```

- **Key Modifiers** Khi lắng nghe các sự kiện bàn phím, chúng ta thường cần kiểm tra các phím cụ thể. Vue cho phép thêm các phím bổ trợ khi `v-on` hoặc `@` để lắng nghe các sự kiện phím

  ```html
  <script setup>
  import { ref } from 'vue';
  const title = ref('')

  const header = ref('')
  const submit = () => {
    header.value = `This is ${title.value}`
  }

  </script>

  <template>
    <h3>{{ header }}</h3>
    <input @keyup.enter="submit" v-model="title"></input>
  </template>
  ```

## 🔷 Vue Lifecycle

### Virtual DOM (VDOM)

### Vue Lifecycle

