## Lớp Lớn (Large Class)

**Large Class** là một loại **Mùi Mã (Code Smell)** thuộc nhóm **Bloaters** (Mã Phình To).

### 1. Dấu hiệu và Triệu chứng

* Một lớp chứa **quá nhiều trường (fields) / phương thức (methods) / dòng mã**.

### 2. Nguyên nhân của Vấn đề

* Các lớp thường bắt đầu nhỏ. Nhưng theo thời gian, chúng bị phình to ra khi chương trình phát triển.
* Tương tự như với Phương thức Dài, các lập trình viên thường cảm thấy **ít tốn công sức tinh thần** hơn khi đặt một tính năng mới vào một lớp đã tồn tại thay vì tạo một lớp mới cho tính năng đó.
* Vấn đề cốt lõi là lớp đó đang gánh vác **quá nhiều trách nhiệm** (vi phạm Nguyên tắc Trách nhiệm Đơn nhất - Single Responsibility Principle).

### 3. Cách Xử lý (Điều trị)

Khi một lớp đang đảm nhiệm quá nhiều vai trò (chức năng), hãy nghĩ đến việc **tách nó ra**:

| Kỹ thuật Tái cấu trúc | Mục đích |
| :--- | :--- |
| **Extract Class** (Trích xuất Lớp) | Giúp tách một phần hành vi của lớp lớn thành một thành phần riêng biệt. |
| **Extract Subclass** (Trích xuất Lớp con) | Giúp nếu một phần hành vi của lớp lớn có thể được triển khai theo nhiều cách khác nhau hoặc chỉ được sử dụng trong các trường hợp hiếm. |
| **Extract Interface** (Trích xuất Giao diện) | Giúp nếu cần có một danh sách các hoạt động và hành vi mà client có thể sử dụng. |
| **Duplicate Observed Data** (Nhân đôi Dữ liệu Được Quan sát) | Nếu lớp lớn chịu trách nhiệm về giao diện đồ họa, bạn có thể cố gắng di chuyển một số dữ liệu và hành vi của nó sang một đối tượng miền (domain object) riêng biệt. Kỹ thuật này giúp giữ cho dữ liệu nhất quán giữa hai nơi. |

### 4. Lợi ích (Payoff)

* Tái cấu trúc các lớp lớn giúp các nhà phát triển không cần phải nhớ một số lượng lớn thuộc tính cho một lớp duy nhất.
* Trong nhiều trường hợp, việc chia lớp lớn thành các phần nhỏ giúp **tránh trùng lặp mã** và chức năng.
* Mã được chia thành các lớp nhỏ hơn sẽ dễ hiểu, dễ kiểm thử (test) và dễ bảo trì hơn nhiều.

---
*Nguồn: Refactoring.Guru*