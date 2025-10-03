## Danh sách Tham số Dài (Long Parameter List)

**Long Parameter List** là một loại **Mùi Mã (Code Smell)** thuộc nhóm **Bloaters** (Mã Phình To).

### 1. Dấu hiệu và Triệu chứng

* Một phương thức có **hơn ba hoặc bốn tham số**.

### 2. Nguyên nhân của Vấn đề

* **Hợp nhất thuật toán:** Danh sách tham số dài có thể xảy ra sau khi một vài loại thuật toán được gộp vào một phương thức duy nhất. Danh sách dài được tạo ra để kiểm soát thuật toán nào sẽ được chạy và cách thức hoạt động của nó.
* **Giảm sự phụ thuộc (Dependency):** Các lập trình viên cố gắng làm cho các lớp độc lập với nhau hơn. Ví dụ, mã tạo các đối tượng cụ thể cần thiết trong một phương thức đã được chuyển từ phương thức đó sang mã gọi phương thức (caller code), nhưng các đối tượng được tạo lại được truyền vào phương thức dưới dạng tham số. Mặc dù lớp ban đầu không còn biết về mối quan hệ giữa các đối tượng (giảm sự phụ thuộc), nhưng nếu tạo ra nhiều đối tượng, mỗi đối tượng sẽ yêu cầu một tham số riêng, dẫn đến danh sách tham số dài hơn.

### 3. Hậu quả

* Rất khó để hiểu và sử dụng các danh sách tham số dài, và chúng trở nên mâu thuẫn khi phát triển.
* Thay vì một danh sách tham số dài, một phương thức có thể sử dụng **dữ liệu của chính đối tượng của nó**. Nếu đối tượng hiện tại không chứa tất cả dữ liệu cần thiết, một đối tượng khác (chứa dữ liệu cần thiết) có thể được truyền dưới dạng tham số.

### 4. Cách Xử lý (Điều trị)

| Kỹ thuật Tái cấu trúc | Mục đích |
| :--- | :--- |
| **Replace Parameter with Method Call** (Thay thế Tham số bằng Lời gọi Phương thức) | Kiểm tra các giá trị được truyền vào tham số. Nếu một số đối số chỉ là kết quả của các lời gọi phương thức của một đối tượng khác, hãy loại bỏ chúng và gọi phương thức đó trực tiếp bên trong phương thức hiện tại. |
| **Preserve Whole Object** (Bảo toàn Toàn bộ Đối tượng) | Thay vì truyền một nhóm dữ liệu nhận được từ một đối tượng khác dưới dạng các tham số riêng lẻ, hãy truyền **toàn bộ đối tượng đó** vào phương thức. |
| **Introduce Parameter Object** (Giới thiệu Đối tượng Tham số) | Nếu các tham số đến từ các nguồn khác nhau nhưng có mối liên hệ logic với nhau, hãy tạo một **Đối tượng Tham số** duy nhất để đóng gói tất cả các giá trị đó và truyền nó dưới dạng một tham số duy nhất. |

### 5. Lợi ích (Payoff)

* Mã **dễ đọc hơn** và **ngắn gọn hơn**.
* Tái cấu trúc có thể tiết lộ **mã trùng lặp** chưa từng được chú ý trước đây.

### 6. Khi nào nên Bỏ qua (Ignore)

* **Đừng loại bỏ tham số nếu việc đó sẽ gây ra sự phụ thuộc không mong muốn** (unwanted dependency) giữa các lớp. Mục tiêu là làm cho mã dễ bảo trì hơn, không phải tạo ra sự phụ thuộc phức tạp.

---
*Nguồn: Refactoring.Guru*