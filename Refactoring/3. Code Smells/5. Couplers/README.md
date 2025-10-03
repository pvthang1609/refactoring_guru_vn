# **Mùi Code: Kết Nối Quá Mức (Couplers)**

## **Định Nghĩa**
Nhóm mùi code thể hiện sự phụ thuộc quá mức giữa các lớp, làm giảm tính module hóa và khả năng tái sử dụng.

## **Các Loại Chính**

### **1. Ghen Tị Tính Năng (Feature Envy)**
**Vấn đề:** Một phương thức sử dụng nhiều dữ liệu của lớp khác hơn chính lớp của nó.

**Giải pháp:**
- Di chuyển phương thức (Move Method)

### **2. Quá Thân Mật (Inappropriate Intimacy)**
**Vấn đề:** Hai lớp phụ thuộc quá nhiều vào chi tiết nội bộ của nhau.

**Giải pháp:**
- Di chuyển phương thức/trường (Move Method/Field)
- Trích xuất lớp (Extract Class)
- Thay đổi tham chiếu hai chiều thành một chiều

### **3. Chuỗi Thông Điệp (Message Chains)**
**Vấn đề:** Chuỗi dài các lời gọi phương thức (a.b().c().d())

**Giải pháp:**
- Ẩn đối tượng ủy quyền (Hide Delegate)
- Trích xuất phương thức (Extract Method)

### **4. Người Trung Gian (Middle Man)**
**Vấn đề:** Một lớp chỉ ủy quyền hầu hết công việc cho lớp khác.

**Giải pháp:**
- Loại bỏ người trung gian (Remove Middle Man)
- Hợp nhất lớp (Inline Method)

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Di chuyển phương thức (Move Method)
- 🔧 Di chuyển trường (Move Field)
- 🔧 Trích xuất lớp (Extract Class)
- 🔧 Ẩn đối tượng ủy quyền (Hide Delegate)
- 🔧 Loại bỏ người trung gian (Remove Middle Man)

## **Kết Luận**
Các mùi "Couplers" làm giảm tính linh hoạt của hệ thống. Giảm kết nối giữa các lớp giúp code dễ bảo trì, mở rộng và tái sử dụng hơn.