## Cụm Dữ liệu (Data Clumps)

**Data Clumps** là một loại **Mùi Mã (Code Smell)** thuộc nhóm **Bloaters** (Mã Phình To).

### 1. Dấu hiệu và Triệu chứng

* Đôi khi, các phần khác nhau của mã chứa **các nhóm biến giống hệt nhau** (ví dụ: các tham số để kết nối cơ sở dữ liệu như `ten_may_chu`, `cong`, `ten_nguoi_dung`, `mat_khau`).
* Những cụm này **nên được biến thành các lớp riêng**.

### 2. Nguyên nhân của Vấn đề

* Các nhóm dữ liệu này thường là do **cấu trúc chương trình kém** hoặc việc **"lập trình bằng cách copy-paste"** (copypasta programming).

### 3. Cách Xác định

* Để chắc chắn liệu một nhóm dữ liệu có phải là một "cụm dữ liệu" hay không, hãy thử **xóa một trong các giá trị dữ liệu** và xem liệu các giá trị còn lại có còn ý nghĩa hay không.
* Nếu các giá trị còn lại không còn ý nghĩa (ví dụ: `ten_may_chu` không có ý nghĩa nếu thiếu `cong`), thì đây là một dấu hiệu tốt cho thấy nhóm biến này **nên được kết hợp thành một đối tượng**.

### 4. Cách Xử lý (Điều trị)

Nguyên tắc chung là **đóng gói nhóm dữ liệu liên quan này vào một lớp đối tượng mới**.

| Kỹ thuật Tái cấu trúc | Khi sử dụng |
| :--- | :--- |
| **Extract Class** (Trích xuất Lớp) | Nếu dữ liệu lặp lại nằm trong các trường (fields) của một lớp. |
| **Introduce Parameter Object** (Giới thiệu Đối tượng Tham số) | Nếu các cụm dữ liệu giống nhau được truyền trong tham số của nhiều phương thức. |
| **Preserve Whole Object** (Bảo toàn Toàn bộ Đối tượng) | Nếu một số dữ liệu được truyền đến các phương thức khác, hãy nghĩ đến việc truyền **toàn bộ đối tượng dữ liệu** vào phương thức thay vì chỉ các trường riêng lẻ. |

* Ngoài ra, hãy xem xét mã được sử dụng bởi các trường này. Có thể là một ý hay để **di chuyển mã này vào lớp dữ liệu mới** (thay vì chỉ giữ dữ liệu) để tăng tính đóng gói.

### 5. Lợi ích (Payoff)

* **Cải thiện khả năng hiểu và tổ chức mã.** Các thao tác trên dữ liệu cụ thể giờ đây được tập hợp ở một nơi duy nhất, thay vì nằm rải rác trong mã.
* **Giảm kích thước mã.**

### 6. Khi nào nên Bỏ qua (Ignore)

* Việc truyền toàn bộ một đối tượng làm tham số của phương thức, thay vì chỉ truyền các giá trị của nó (kiểu nguyên thủy), có thể tạo ra **sự phụ thuộc không mong muốn** (undesirable dependency) giữa hai lớp. Bạn cần cân nhắc giữa việc giảm độ dài danh sách tham số và việc giới thiệu một sự phụ thuộc mới.

---
*Nguồn: Refactoring.Guru*