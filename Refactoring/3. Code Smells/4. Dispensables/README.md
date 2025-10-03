# **Mùi Code: Có Thể Loại Bỏ (Dispensables)**

## **Định Nghĩa**
Những thành phần trong code không còn cần thiết, làm code phức tạp hơn mà không mang lại giá trị.

## **Các Loại Chính**

### **1. Mã Thừa (Comments)**
**Vấn đề:** Comment giải thích code phức tạp thay vì làm code dễ hiểu.

**Giải pháp:**
- Trích xuất phương thức (Extract Method)
- Đổi tên biến/phương thức cho rõ nghĩa

### **2. Code Trùng Lặp (Duplicate Code)**
**Vấn đề:** Cùng một code xuất hiện ở nhiều nơi.

**Giải pháp:**
- Trích xuất phương thức (Extract Method)
- Trích xuất lớp (Extract Class)

### **3. Lười Nhác (Lazy Class)**
**Vấn đề:** Lớp thực hiện quá ít chức năng, không đáng để duy trì.

**Giải pháp:**
- Hợp nhất lớp (Inline Class)

### **4. Dữ Liệu Thừa (Data Class)**
**Vấn đề:** Lớp chỉ chứa trường và getter/setter.

**Giải pháp:**
- Di chuyển phương thức (Move Method)
- Đóng gói hành vi liên quan

### **5. Nguyên Thủy Ám Ảnh (Primitive Obsession)**
**Vấn đề:** Sử dụng kiểu dữ liệu nguyên thủy thay vì đối tượng.

**Giải pháp:**
- Thay thế kiểu nguyên thủy bằng lớp (Replace Primitive with Object)

### **6. Tạp Trữ (Dead Code)**
**Vấn đề:** Code không bao giờ được sử dụng.

**Giải pháp:**
- Xóa bỏ

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Trích xuất phương thức (Extract Method)
- 🔧 Trích xuất lớp (Extract Class)
- 🔧 Hợp nhất lớp (Inline Class)
- 🔧 Thay thế kiểu nguyên thủy bằng lớp

## **Kết Luận**
Các thành phần "có thể loại bỏ" làm tăng độ phức tạp không cần thiết. Xác định và loại bỏ chúng giúp code sạch sẽ, dễ bảo trì hơn.
