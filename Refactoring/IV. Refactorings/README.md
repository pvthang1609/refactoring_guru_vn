# **Các Kỹ Thuật Tái Cấu Trúc**

## **Giới Thiệu**
Tái cấu trúc (refactoring) là quá trình cấu trúc lại code mà không thay đổi hành vi bên ngoài, nhằm cải thiện tính dễ đọc, dễ bảo trì và hiệu suất của phần mềm.

## **Nhóm Kỹ Thuật Chính**

### **1. Tổ Chức Phương Thức (Composing Methods)**
**Mục đích:** Đơn giản hóa và tối ưu hóa các phương thức

- **Trích xuất Phương thức (Extract Method)**: Tách đoạn code thành phương thức mới
- **Hợp nhất Phương thức (Inline Method)**: Thay thế lời gọi phương thức bằng nội dung của nó
- **Trích xuất Biến (Extract Variable)**: Tạo biến có tên rõ ràng cho biểu thức phức tạp
- **Thay thế Biến tạm bằng Truy vấn (Replace Temp with Query)**: Thay biến tạm bằng phương thức

### **2. Di Chuyển Tính Năng (Moving Features)**
**Mục đích:** Tái cấu trúc cấu trúc lớp và phân phối trách nhiệm

- **Di chuyển Phương thức (Move Method)**: Chuyển phương thức sang lớp phù hợp hơn
- **Di chuyển Trường (Move Field)**: Di chuyển trường dữ liệu giữa các lớp
- **Trích xuất Lớp (Extract Class)**: Tách một phần của lớp thành lớp mới
- **Hợp nhất Lớp (Inline Class)**: Gộp lớp không cần thiết vào lớp khác

### **3. Tổ Chức Dữ Liệu (Organizing Data)**
**Mục đích:** Cải thiện việc quản lý và đóng gói dữ liệu

- **Đóng gói Trường (Encapsulate Field)**: Chuyển trường public thành private
- **Thay thế Kiểu mã bằng Lớp con (Replace Type Code with Subclasses)**: Tạo lớp con cho từng giá trị
- **Thay thế Kiểu mã bằng State/Strategy**: Áp dụng mẫu thiết kế State hoặc Strategy

### **4. Đơn Giản Hóa Biểu Thức Điều Kiện**
**Mục đích:** Làm cho các điều kiện dễ hiểu hơn

- **Tách Biến điều kiện (Decompose Conditional)**: Tách điều kiện phức tạp thành phương thức
- **Hợp nhất Biểu thức điều kiện (Consolidate Conditional Expression)**: Gộp các điều kiện trùng lặp
- **Thay thế Điều kiện bằng Đa hình (Replace Conditional with Polymorphism)**: Sử dụng đa hình thay vì điều kiện

### **5. Đơn Giản Hóa Lời Gọi Phương Thức**
**Mục đích:** Cải thiện giao diện và tương tác giữa các đối tượng

- **Đổi tên Phương thức (Rename Method)**: Thay đổi tên cho rõ nghĩa hơn
- **Thêm/Xóa Tham số (Add/Remove Parameter)**: Điều chỉnh danh sách tham số
- **Tách Truy vấn và Sửa đổi (Separate Query from Modifier)**: Tách phương thức có tác dụng phụ

### **6. Xử Lý Tính Tổng Quát (Dealing with Generalization)**
**Mục đích:** Tối ưu hóa hệ thống phân cấp kế thừa

- **Kéo lên Phương thức (Pull Up Method)**: Di chuyển phương thức lên lớp cha
- **Đẩy xuống Phương thức (Push Down Method)**: Di chuyển phương thức xuống lớp con
- **Trích xuất Siêu lớp (Extract Superclass)**: Tạo lớp cha chung cho các lớp liên quan
- **Trích xuất Interface**: Tách giao diện từ các lớp hiện có

## **Lợi Ích Chính**

✅ **Cải thiện khả năng đọc hiểu** - Code rõ ràng, dễ bảo trì  
✅ **Giảm độ phức tạp** - Phân tách các mối quan tâm  
✅ **Tăng khả năng tái sử dụng** - Component linh hoạt hơn  
✅ **Dễ dàng mở rộng** - Hỗ trợ thay đổi yêu cầu  
✅ **Nâng cao chất lượng** - Ít bug, dễ kiểm thử  

## **Nguyên Tắc Áp Dụng**

1. **Luôn có test** - Đảm bảo không phá vỡ chức năng hiện có
2. **Thực hiện từng bước nhỏ** - Mỗi lần chỉ thay đổi một thứ
3. **Hiểu rõ code** - Phân tích kỹ trước khi tái cấu trúc
4. **Ưu tiên vấn đề nghiêm trọng** - Xử lý các mùi code ảnh hưởng lớn nhất

## **Kết Luận**

Các kỹ thuật tái cấu trúc cung cấp bộ công cụ toàn diện để cải thiện chất lượng code. Việc áp dụng thường xuyên và có hệ thống giúp duy trì codebase sạch sẽ, linh hoạt và dễ bảo trì theo thời gian.

**Lưu ý:** Luôn đảm bảo có đầy đủ test case trước khi thực hiện tái cấu trúc để tránh giới thiệu lỗi mới.