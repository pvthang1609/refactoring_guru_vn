## Cách tái cấu trúc mã (How to Refactor)

Tái cấu trúc mã nên được thực hiện như một **loạt các thay đổi nhỏ**, trong đó mỗi thay đổi làm cho đoạn mã hiện có tốt hơn một chút mà vẫn giữ cho chương trình hoạt động bình thường.

### Danh sách kiểm tra (Checklist) để tái cấu trúc đúng cách

#### 1. Mã phải trở nên sạch hơn.

* Nếu mã vẫn bẩn như cũ sau khi tái cấu trúc... bạn đã lãng phí thời gian. Hãy tìm hiểu lý do tại sao điều này xảy ra.
* Điều này thường xảy ra khi bạn không thực hiện tái cấu trúc bằng các thay đổi nhỏ mà lại trộn lẫn nhiều lần tái cấu trúc thành một thay đổi lớn. Điều này rất dễ khiến bạn mất phương hướng, đặc biệt nếu bạn bị giới hạn về thời gian.
* Nó cũng có thể xảy ra khi làm việc với mã cực kỳ cẩu thả. Bất kể bạn cải thiện gì, mã tổng thể vẫn là một thảm họa.
* Trong trường hợp này, cần cân nhắc việc viết lại hoàn toàn một số phần của mã. Nhưng trước khi làm điều đó, bạn phải viết các bài kiểm thử (tests) và dành một khoảng thời gian đáng kể.

#### 2. Không nên tạo chức năng mới trong khi tái cấu trúc.

* Đừng trộn lẫn việc tái cấu trúc và việc phát triển trực tiếp các tính năng mới.
* Cố gắng tách biệt các quy trình này, ít nhất là trong phạm vi các commit riêng lẻ.

#### 3. Tất cả các bài kiểm thử hiện có phải vượt qua sau khi tái cấu trúc.

Có hai trường hợp các bài kiểm thử có thể bị hỏng sau khi tái cấu trúc:

1.  **Bạn đã mắc lỗi trong quá trình tái cấu trúc.**
    * Điều này rất rõ ràng: hãy tiến hành và sửa lỗi.
2.  **Các bài kiểm thử của bạn ở cấp độ quá thấp (low-level).**
    * Ví dụ: bạn đang kiểm thử các phương thức riêng tư (private methods) của các lớp.
    * Trong trường hợp này, chính các bài kiểm thử là nguyên nhân. Bạn có thể tái cấu trúc các bài kiểm thử đó hoặc viết một bộ bài kiểm thử cấp độ cao hơn hoàn toàn mới. Một cách tuyệt vời để tránh tình huống này là viết các bài kiểm thử theo phong cách **BDD (Behavior-Driven Development)**.

---
*Nguồn: Refactoring.Guru*