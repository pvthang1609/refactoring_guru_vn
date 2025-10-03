# **Mùi Code: Các Lớp Thay Thế với Giao Diện Khác nhau**

## **Dấu hiệu Nhận biết**
Hai lớp thực hiện các chức năng giống nhau nhưng lại có **tên phương thức** hoặc **tham số** khác nhau.

## **Nguyên nhân**
Bạn thấy có hai lớp thực hiện một công việc giống hệt nhau (hoặc gần giống nhau) nhưng lại khác biệt về:
- Tên phương thức
- Tham số
- Định dạng dữ liệu

## **Hậu quả**
Khi hai lớp hoàn thành cùng một mục đích nhưng sử dụng các giao diện công khai (public interfaces) khác nhau:
- Gây **nhầm lẫn** cho người dùng (lập trình viên sử dụng lớp đó)
- Phải ghi nhớ **hai cách khác nhau** mà không có lý do rõ ràng
- Giảm tính **thống nhất** của codebase

## **Giải pháp**
### **Chuẩn hóa các giao diện:**

**Nếu các lớp không liên quan:**
- Thực hiện **Đổi Tên Phương Thức (Rename Method)** để làm cho tên của chúng thống nhất

**Nếu các lớp chỉ tương tự nhau một phần:**
- **Trích xuất Siêu lớp (Extract Superclass)** để tách biệt phần giao diện chung

**Trường hợp đặc biệt:**
- Hợp nhất các lớp nếu chúng thực sự giống nhau

## **Kỹ thuật Tái cấu trúc**
- 🔧 **Đổi Tên Phương Thức** (Rename Method)
- 🔧 **Thêm Tham Số** (Add Parameter)
- 🔧 **Tham Số Hóa Phương Thức** (Parameterize Method)
- 🔧 **Trích xuất Siêu lớp** (Extract Superclass)
- 🔧 **Hợp nhất Lớp** (Inline Class)

## **Lợi ích**
- ✅ **Giảm độ phức tạp**
- ✅ **Code đơn giản và dễ hiểu hơn**
- ✅ **Cải thiện khả năng tái sử dụng**
- ✅ **Dễ dàng hoán đổi giữa các lớp**

## **Ngoại lệ**
Có thể bỏ qua việc tái cấu trúc khi:
- ❌ Các lớp thuộc về **thư viện hoặc framework khác nhau**
- ❌ **Không có quyền kiểm soát** mã nguồn
- ❌ Sự khác biệt về giao diện là **có chủ ý và cần thiết**

## **Kết luận**
"Mùi" này xảy ra khi có **nhiều hơn một cách** để thực hiện cùng một hành động mà không có sự khác biệt rõ ràng về ngữ nghĩa. Việc chuẩn hóa giao diện giúp:
- 🎯 Hệ thống **trực quan hơn**
- 🎯 Giảm thiểu **dư thừa không cần thiết**
- 🎯 Code **dễ bảo trì** hơn

**Nguồn:** [refactoring.guru/smells/alternative-classes-with-different-interfaces](https://refactoring.guru/smells/alternative-classes-with-different-interfaces)
