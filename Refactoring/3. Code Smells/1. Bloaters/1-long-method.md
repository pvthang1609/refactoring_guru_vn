## Phương thức Dài (Long Method)

**Long Method** là một loại **Mùi Mã (Code Smell)** thuộc nhóm **Bloaters** (Mã Phình To).

### 1. Dấu hiệu và Triệu chứng

* Một phương thức chứa **quá nhiều dòng mã**.
* Quy tắc chung là bất kỳ phương thức nào **dài hơn mười dòng** đều nên khiến bạn bắt đầu đặt câu hỏi.

### 2. Nguyên nhân của Vấn đề

* Giống như Khách sạn California, có điều gì đó luôn được thêm vào một phương thức nhưng không có gì được loại bỏ.
* Vì việc viết mã dễ hơn việc đọc mã, nên "mùi" này thường không bị chú ý cho đến khi phương thức đó biến thành một con quái vật quá khổ và khó coi.
* Về mặt tâm lý, thường khó tạo một phương thức mới hơn là thêm vào một phương thức hiện có: "Chỉ có hai dòng thôi, đâu cần thiết phải tạo cả một phương thức riêng cho nó..." Điều này có nghĩa là thêm một dòng, rồi thêm một dòng nữa, sinh ra một mớ mã **"spaghetti code"** (mã rối rắm).

### 3. Cách Xử lý (Điều trị)

Nguyên tắc chung là: **Nếu bạn cảm thấy cần phải chú thích (comment) về điều gì đó bên trong một phương thức, bạn nên lấy đoạn mã đó và đặt nó vào một phương thức mới.**

* Ngay cả một dòng duy nhất cũng **có thể và nên** được tách ra thành một phương thức riêng nếu nó cần giải thích. Nếu phương thức mới có một cái tên mô tả rõ ràng, sẽ không ai cần phải xem mã để biết nó làm gì.

Các kỹ thuật tái cấu trúc cụ thể:

| Kỹ thuật Tái cấu trúc | Khi sử dụng |
| :--- | :--- |
| **Extract Method** (Trích xuất Phương thức) | Để giảm độ dài của thân phương thức. |
| **Replace Temp with Query** (Thay thế Biến Tạm bằng Truy vấn) | Nếu các biến cục bộ và tham số cản trở việc trích xuất phương thức. |
| **Introduce Parameter Object** (Giới thiệu Đối tượng Tham số) hoặc **Preserve Whole Object** (Bảo toàn Toàn bộ Đối tượng) | Nếu các biến cục bộ và tham số cản trở việc trích xuất phương thức. |
| **Replace Method with Method Object** (Thay thế Phương thức bằng Đối tượng Phương thức) | Nếu không có công thức nào ở trên giúp ích, hãy thử di chuyển toàn bộ phương thức sang một đối tượng riêng biệt. |
| **Decompose Conditional** (Phân tách Điều kiện) | Các toán tử điều kiện (`if/else`) là dấu hiệu tốt cho thấy mã có thể được chuyển sang một phương thức riêng. |
| **Extract Method** (Trích xuất Phương thức) | Nếu các vòng lặp (loops) cản trở. |

### 4. Lợi ích (Payoff)

* Trong số tất cả các loại mã hướng đối tượng, các lớp có **phương thức ngắn** có tuổi thọ dài nhất. Phương thức hoặc hàm càng dài, càng khó hiểu và khó bảo trì.
* Ngoài ra, các phương thức dài là nơi ẩn náu hoàn hảo cho **mã trùng lặp** không mong muốn.

### 5. Hiệu suất (Performance)

* Liệu việc tăng số lượng phương thức có làm giảm hiệu suất, như nhiều người tuyên bố không? Trong hầu hết các trường hợp, tác động **rất không đáng kể** đến mức không cần phải lo lắng.
* Hơn nữa, với mã rõ ràng và dễ hiểu, bạn có nhiều khả năng tìm ra các phương pháp thực sự hiệu quả để cấu trúc lại mã và đạt được hiệu suất thực sự nếu cần.

---
*Nguồn: Refactoring.Guru*