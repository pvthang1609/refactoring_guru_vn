# **Mùi Code: Ngăn Cản Thay Đổi (Change Preventers)**

## **Định Nghĩa**
Đây là những mùi code khiến việc thay đổi trở nên khó khăn, buộc bạn phải sửa đổi nhiều lớp chỉ để thực hiện một thay đổi nhỏ.

## **Các Loại Chính**

### **1. Thay Đổi Phân Kỳ (Divergent Change)**
**Vấn đề:** Một lớp thường xuyên bị thay đổi vì nhiều lý do khác nhau.

**Giải pháp:**
- Trích xuất lớp (Extract Class)
- Tách theo từng lý do thay đổi

### **2. Phẫu Thuật Shotgun (Shotgun Surgery)**
**Vấn đề:** Một thay đổi nhỏ buộc phải sửa nhiều lớp khác nhau.

**Giải pháp:**
- Di chuyển phương thức (Move Method)
- Di chuyển trường (Move Field)
- Hợp nhất lớp (Inline Class)

### **3. Hệ Thống Kế Thừa Song Song (Parallel Inheritance Hierarchies)**
**Vấn đề:** Mỗi khi tạo lớp con cho một lớp, bạn phải tạo lớp con cho một lớp khác.

**Giải pháp:**
- Hợp nhất hệ thống phân cấp
- Di chuyển phương thức và trường

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Trích xuất lớp (Extract Class)
- 🔧 Di chuyển phương thức (Move Method)
- 🔧 Di chuyển trường (Move Field)
- 🔧 Hợp nhất lớp (Inline Class)

## **Kết Luận**
Các mùi "Change Preventers" làm giảm đáng kể khả năng bảo trì code. Nhận diện và xử lý chúng kịp thời giúp hệ thống linh hoạt hơn khi có yêu cầu thay đổi.
