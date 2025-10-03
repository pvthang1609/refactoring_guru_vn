# **Các Mùi Code Khác**

## **Định Nghĩa**
Những mùi code không thuộc các nhóm chính nhưng vẫn ảnh hưởng đến chất lượng code.

## **Các Mùi Code Chính**

### **1. Lạm Dụng Biến Toàn Cục (Global Data)**
**Vấn đề:** Biến toàn cục có thể được truy cập và thay đổi từ bất kỳ đâu.

**Giải pháp:**
- Đóng gói biến (Encapsulate Variable)
- Giới hạn phạm vi truy cập

### **2. Tham Số Thừa (Long Parameter List)**
**Vấn đề:** Phương thức có quá nhiều tham số, khó hiểu và bảo trì.

**Giải pháp:**
- Thay thế tham số bằng phương thức (Replace Parameter with Method Call)
- Giới thiệu đối tượng tham số (Introduce Parameter Object)

### **3. Modifiers Thừa (Mutable Data)**
**Vấn đề:** Dữ liệu có thể thay đổi từ nhiều nơi, khó kiểm soát.

**Giải pháp:**
- Đóng băng đối tượng (Encapsulate Collection)
- Tách truy vấn và sửa đổi (Separate Query from Modifier)

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Đóng gói biến (Encapsulate Variable)
- 🔧 Thay thế tham số bằng phương thức
- 🔧 Giới thiệu đối tượng tham số
- 🔧 Tách truy vấn và sửa đổi

## **Kết Luận**
Các mùi code "khác" tuy không thuộc nhóm chính nhưng vẫn cần được xử lý để đảm bảo chất lượng code tổng thể.