# **Mùi Code: Phình To (Bloaters)**

## **Định Nghĩa**
Các mùi code làm cho code phình to một cách không cần thiết, khiến chúng khó sử dụng và bảo trì.

## **Các Loại Chính**

### **1. Phương Thức Quá Dài (Long Method)**
**Vấn đề:** Phương thức chứa quá nhiều dòng code.

**Giải pháp:**
- Trích xuất phương thức (Extract Method)
- Thay thế biến tạm bằng truy vấn

### **2. Lớp Quá Lớn (Large Class)**
**Vấn đề:** Lớp có quá nhiều trường, phương thức.

**Giải pháp:**
- Trích xuất lớp (Extract Class)
- Trích xuất lớp con (Extract Subclass)

### **3. Ám Ảnh Kiểu Nguyên Thủy (Primitive Obsession)**
**Vấn đề:** Sử dụng quá nhiều kiểu dữ liệu nguyên thủy.

**Giải pháp:**
- Thay thế kiểu nguyên thủy bằng đối tượng
- Nhóm dữ liệu thành lớp

### **4. Danh Sách Tham Số Dài (Long Parameter List)**
**Vấn đề:** Phương thức có quá nhiều tham số.

**Giải pháp:**
- Giới thiệu đối tượng tham số
- Bảo toàn toàn bộ đối tượng

### **5. Cụm Dữ Liệu (Data Clumps)**
**Vấn đề:** Nhóm biến luôn xuất hiện cùng nhau.

**Giải pháp:**
- Trích xuất lớp
- Giới thiệu đối tượng tham số

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Trích xuất phương thức
- 🔧 Trích xuất lớp
- 🔧 Thay thế kiểu nguyên thủy bằng đối tượng
- 🔧 Giới thiệu đối tượng tham số

## **Kết Luận**
Các mùi "Bloaters" làm code cồng kềnh và khó bảo trì. Nhận diện và xử lý chúng kịp thời giúp code gọn gàng và hiệu quả hơn.

**Nguồn:** [refactoring.guru/refactoring/smells/bloaters](https://refactoring.guru/refactoring/smells/bloaters)
