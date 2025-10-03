## Khi nào nên tái cấu trúc Mã (Refactor)

Tái cấu trúc mã là một công việc cần được thực hiện liên tục, không phải là một nhiệm vụ đặc biệt chỉ làm một lần. Dưới đây là những quy tắc và tình huống quan trọng để quyết định khi nào nên thực hiện việc tái cấu trúc.

### 1. Quy tắc số ba (Rule of Three)

Đây là một quy tắc đơn giản để giải quyết sự lặp lại (Duplicate Code):

* **Lần thứ nhất (First time):** Chỉ cần hoàn thành công việc.
* **Lần thứ hai (Second time):** Cảm thấy khó chịu vì phải lặp lại, nhưng vẫn làm theo cách cũ.
* **Lần thứ ba (Third time):** Bắt đầu tái cấu trúc.

### 2. Khi thêm một tính năng mới

Tái cấu trúc mã trong quá trình thêm tính năng giúp ích cho bạn rất nhiều:

* **Giúp bạn hiểu mã của người khác:** Nếu bạn phải xử lý đoạn mã "bẩn" của ai đó, hãy thử tái cấu trúc nó trước. Mã sạch sẽ dễ nắm bắt hơn nhiều. Bạn sẽ cải thiện nó không chỉ cho bản thân mà còn cho những người sử dụng nó sau này.
* **Giúp việc thêm tính năng mới dễ dàng hơn:** Việc thực hiện các thay đổi trong mã sạch sẽ dễ dàng hơn rất nhiều.

### 3. Khi sửa một lỗi

* Các lỗi trong mã cũng giống như trong đời thực: chúng sống ở những nơi tối tăm, bẩn thỉu nhất trong mã.
* Hãy làm sạch mã của bạn, và các lỗi gần như sẽ tự "lộ diện".

### 4. Trong quá trình duyệt mã (Code Review)

* Duyệt mã có thể là cơ hội cuối cùng để dọn dẹp mã trước khi nó được đưa vào sử dụng công khai.
* Tốt nhất là thực hiện việc xem xét này theo cặp với tác giả. Bằng cách này, bạn có thể nhanh chóng khắc phục các vấn đề đơn giản và ước tính thời gian để khắc phục các vấn đề khó khăn hơn.

### Tái cấu trúc chủ động (Proactive Refactoring)

Các nhà quản lý đánh giá cao việc tái cấu trúc chủ động vì nó loại bỏ nhu cầu về các nhiệm vụ tái cấu trúc đặc biệt sau này. Sếp vui vẻ sẽ khiến lập trình viên cũng vui vẻ!

---
*Nguồn: Refactoring.Guru*