## Ám ảnh Kiểu Dữ liệu Nguyên thủy (Primitive Obsession)

**Primitive Obsession** là một loại **Mùi Mã (Code Smell)** thuộc nhóm **Bloaters** (Mã Phình To).

### 1. Dấu hiệu và Triệu chứng

* Sử dụng **kiểu dữ liệu nguyên thủy** (primitives) thay vì các **đối tượng nhỏ** cho các tác vụ đơn giản (ví dụ: tiền tệ, phạm vi giá trị, chuỗi đặc biệt cho số điện thoại, v.v.).
* Sử dụng **hằng số** để mã hóa thông tin (ví dụ: hằng số `USER_ADMIN_ROLE = 1` để chỉ người dùng có quyền quản trị).
* Sử dụng **hằng số chuỗi** làm tên trường để sử dụng trong các mảng dữ liệu.

### 2. Nguyên nhân của Vấn đề

* Giống như hầu hết các mùi mã khác, ám ảnh kiểu nguyên thủy sinh ra trong những khoảnh khắc "yếu lòng" của lập trình viên. "Chỉ là một trường để lưu trữ dữ liệu thôi!" lập trình viên tự nhủ. Tạo một trường kiểu nguyên thủy dễ dàng hơn nhiều so với việc tạo cả một lớp mới, đúng không?
* Sau đó, một trường khác được thêm vào theo cùng một cách, và lớp dần trở nên lớn và khó quản lý.
* Các kiểu nguyên thủy thường được sử dụng để "giả lập" các kiểu dữ liệu. Thay vì một kiểu dữ liệu riêng biệt, bạn có một tập hợp các số hoặc chuỗi tạo thành danh sách các giá trị cho phép cho một thực thể nào đó. Các tên dễ hiểu sau đó được đặt cho các số và chuỗi cụ thể này thông qua các **hằng số**, và chúng được lan truyền rộng rãi.

### 3. Cách Xử lý (Điều trị)

Tái cấu trúc cốt lõi là **thay thế các kiểu nguyên thủy bằng các lớp đối tượng chuyên biệt** để đóng gói dữ liệu và hành vi liên quan.

| Kỹ thuật Tái cấu trúc | Khi sử dụng |
| :--- | :--- |
| **Replace Data Value with Object** (Thay thế Giá trị Dữ liệu bằng Đối tượng) | Nếu bạn có nhiều trường kiểu nguyên thủy, hãy nhóm một số trường trong số đó một cách logic thành lớp riêng, đồng thời chuyển cả hành vi liên quan đến dữ liệu đó vào lớp mới. |
| **Introduce Parameter Object** (Giới thiệu Đối tượng Tham số) hoặc **Preserve Whole Object** (Bảo toàn Toàn bộ Đối tượng) | Khi các giá trị của trường kiểu nguyên thủy được sử dụng trong tham số phương thức. |
| **Replace Type Code with Class** (Thay thế Mã Kiểu bằng Lớp) | Khi dữ liệu phức tạp được mã hóa trong các biến (ví dụ: hằng số để chỉ trạng thái, vai trò, v.v.). |
| **Replace Type Code with Subclasses** (Thay thế Mã Kiểu bằng Lớp con) hoặc **Replace Type Code with State/Strategy** (Thay thế Mã Kiểu bằng State/Strategy) | Khi cần xử lý mã kiểu dữ liệu phức tạp. |
| **Replace Array with Object** (Thay thế Mảng bằng Đối tượng) | Nếu có mảng trong số các biến đang mô phỏng một cấu trúc đối tượng (ví dụ: mảng dữ liệu có các chỉ mục là chuỗi hằng số). |

### 4. Lợi ích (Payoff)

* Mã trở nên **linh hoạt** hơn nhờ sử dụng đối tượng thay vì kiểu nguyên thủy.
* **Tăng khả năng hiểu** và tổ chức mã tốt hơn. Các thao tác trên dữ liệu cụ thể nằm ở cùng một nơi, thay vì bị phân tán. Không còn phải đoán về lý do cho tất cả các hằng số lạ lùng và tại sao chúng lại ở trong một mảng nữa.
* **Dễ dàng tìm thấy mã trùng lặp** hơn.

---
*Nguồn: Refactoring.Guru*