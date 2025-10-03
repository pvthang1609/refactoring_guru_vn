# **Các Mùi Code Lạm Dụng Hướng Đối Tượng**

**Nhóm mùi code này vi phạm các nguyên tắc cơ bản của lập trình hướng đối tượng, làm giảm tính linh hoạt và dễ bảo trì của code.**

## **📋 Danh sách các Mùi Code**

### **1. Câu lệnh Switch (Switch Statements)**
**Vấn đề:** Sử dụng các câu lệnh `switch` hoặc `if-else` dài thay vì tận dụng tính đa hình.

**Hậu quả:**
- Vi phạm nguyên tắc Mở/Đóng
- Khó mở rộng và bảo trì
- Code trùng lặp ở nhiều nơi

**Giải pháp:**
- Sử dụng đa hình thay thế
- Áp dụng State/Strategy Pattern
- Triển khai Factory Method

### **2. Từ Chối Kế Thừa (Refused Bequest)**
**Vấn đề:** Lớp con chỉ sử dụng một phần nhỏ các phương thức và thuộc tính được kế thừa từ lớp cha.

**Dấu hiệu nhận biết:**
- Lớp con override nhiều phương thức của lớp cha
- Quan hệ "is-a" không rõ ràng

**Giải pháp:**
- Thay thế kế thừa bằng ủy quyền (delegation)
- Tái cấu trúc hệ thống phân cấp kế thừa

### **3. Trường Tạm Thời (Temporary Field)**
**Vấn đề:** Các trường chỉ được sử dụng trong một số trường hợp cụ thể, thường xuyên ở trạng thái null hoặc không có giá trị.

**Tác hại:**
- Khó hiểu mục đích sử dụng của trường
- Tăng độ phức tạp không cần thiết
- Dễ gây lỗi runtime

**Giải pháp:**
- Trích xuất lớp (Extract Class)
- Di chuyển trường và phương thức liên quan

### **4. Quan hệ Thân Thiết Quá Mức (Feature Envy)**
**Vấn đề:** Một phương thức truy cập quá nhiều dữ liệu của đối tượng khác so với dữ liệu của chính nó.

**Biểu hiện:**
- Phương thức gọi nhiều getter của đối tượng khác
- Có vẻ "ghen tị" với dữ liệu của lớp khác

**Giải pháp:**
- Di chuyển phương thức (Move Method)
- Trích xuất phương thức (Extract Method)

## **🎯 Nguyên Nhân Chung**

Các mùi code này thường xuất phát từ:

### **Thiết kế không đúng với bản chất OOP**
- Sử dụng cấu trúc thủ tục trong môi trường hướng đối tượng
- Không tận dụng được tính đóng gói, kế thừa, đa hình

### **Áp dụng sai nguyên tắc SOLID**
- **S** (Single Responsibility): Một lớp làm quá nhiều việc
- **O** (Open/Closed): Khó mở rộng mà không sửa đổi code hiện có
- **L** (Liskov Substitution): Lớp con không thể thay thế lớp cha
- **I** (Interface Segregation): Interface quá lớn và phức tạp
- **D** (Dependency Inversion): Phụ thuộc trực tiếp vào implementation

## **🛠 Kỹ Thuật Tái Cấu Trúc Chính**

| Mùi Code | Kỹ Thuật Tái Cấu Trúc | Mẫu Thiết Kế Áp Dụng |
|----------|---------------------|-------------------|
| Switch Statements | Thay thế điều kiện bằng đa hình | Strategy, State, Factory |
| Refused Bequest | Thay thế kế thừa bằng ủy quyền | Composition over Inheritance |
| Temporary Field | Trích xuất lớp | Special Case, Null Object |
| Feature Envy | Di chuyển phương thức | - |

## **📊 Ví Dụ Tổng Hợp**

### **Vấn Đề: Switch Statement + Temporary Field**
```java
// ❌ Code có mùi
class Employee {
    private String type;
    private double bonus; // Temporary field - chỉ dùng cho một số type
    
    public double calculatePay() {
        switch (type) {
            case "REGULAR":
                return basePay + bonus;
            case "PART_TIME":
                return hourlyPay * hours;
            case "CONTRACTOR":
                return contractAmount;
            default:
                return 0;
        }
    }
}

// ✅ Code sau khi tái cấu trúc
abstract class Employee {
    public abstract double calculatePay();
}

class RegularEmployee extends Employee {
    private double basePay;
    private double bonus;
    
    public double calculatePay() {
        return basePay + bonus;
    }
}

class PartTimeEmployee extends Employee {
    private double hourlyPay;
    private double hours;
    
    public double calculatePay() {
        return hourlyPay * hours;
    }
}
```

## **💡 Lợi Ích Khi Khắc Phục**

### **Cải thiện Chất Lượng Code**
- **Tính module hóa cao**: Mỗi lớp có trách nhiệm rõ ràng
- **Dễ bảo trì**: Thay đổi ít ảnh hưởng đến toàn hệ thống
- **Tái sử dụng tốt**: Các thành phần có thể dùng lại trong nhiều ngữ cảnh

### **Tăng Hiệu Suất Phát Triển**
- **Giảm thời gian debug**: Code rõ ràng, ít lỗi tiềm ẩn
- **Dễ onboard thành viên mới**: Cấu trúc code dễ hiểu
- **Thuận tiện cho testing**: Có thể test từng thành phần độc lập

## **🚨 Khi Nào Có Thể Bỏ Qua?**

Các trường hợp có thể chấp nhận các mùi code này:

- **Prototype/MVP**: Khi cần deliver nhanh sản phẩm thử nghiệm
- **Code ít thay đổi**: Các thành phần ổn định, ít khi cần sửa đổi
- **Ràng buộc performance**: Trong một số trường hợp đặc biệt cần tối ưu hiệu năng
- **Integration code**: Khi làm việc với third-party library hoặc legacy system

## **🔚 Kết Luận**

Các mùi code "Object-Oriented Abusers" không chỉ là vấn đề kỹ thuật mà còn phản ánh **sự hiểu biết chưa sâu về các nguyên lý hướng đối tượng**. Việc:

- 🎯 **Nhận diện sớm** các dấu hiệu vi phạm
- 🛠 **Áp dụng đúng** kỹ thuật tái cấu trúc  
- 📚 **Hiểu rõ bản chất** các nguyên lý OOP

sẽ giúp xây dựng hệ thống **linh hoạt, dễ bảo trì và có khả năng mở rộng cao** theo thời gian.
