# **Xử Lý Tính Tổng Quát Hóa (Dealing with Generalization)**

## **Định Nghĩa**
Xử lý tính tổng quát hóa là nhóm kỹ thuật tái cấu trúc liên quan đến việc tổ chức và tối ưu hóa hệ thống phân cấp kế thừa, giúp quản lý mối quan hệ giữa các lớp cha và lớp con một cách hiệu quả.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có sự trùng lặp code giữa các lớp liên quan
- Cần tạo ra các mức độ trừu tượng phù hợp
- Muốn cải thiện khả năng tái sử dụng code
- Cần tổ chức lại hệ thống kế thừa cho rõ ràng

## **Các Kỹ Thuật Chính**

### **1. Kéo Lên (Pull Up)**
**Pull Up Field**: Di chuyển trường từ lớp con lên lớp cha
**Pull Up Method**: Di chuyển phương thức từ lớp con lên lớp cha
**Pull Up Constructor Body**: Tối ưu constructor trong hệ thống kế thừa

### **2. Đẩy Xuống (Push Down)**
**Push Down Field**: Di chuyển trường từ lớp cha xuống lớp con
**Push Down Method**: Di chuyển phương thức từ lớp cha xuống lớp con

### **3. Trích Xuất (Extract)**
**Extract Subclass**: Tách lớp con từ lớp hiện có
**Extract Superclass**: Tách lớp cha từ các lớp có chung tính năng
**Extract Interface**: Trích xuất interface từ các lớp có chung hành vi

### **4. Tổ Chức Lại Hệ Thống Phân Cấp**
**Collapse Hierarchy**: Gom lớp cha và lớp con không cần thiết
**Form Template Method**: Định nghĩa bộ khung thuật toán trong lớp cha

### **5. Thay Thế Quan Hệ**
**Replace Inheritance with Delegation**: Thay thế kế thừa bằng ủy quyền
**Replace Delegation with Inheritance**: Thay thế ủy quyền bằng kế thừa

## **Ví Dụ Minh Họa**

### **❌ Trước khi tái cấu trúc:**
```java
class Employee {
    private String name;
    private double salary;
    
    // Có quá nhiều logic trong lớp cơ sở
    public double calculatePay() {
        // Logic tính lương chung - nhưng không phù hợp cho tất cả
        return salary;
    }
}

class Manager extends Employee {
    private double bonus;
    
    // Trùng lặp code
    public double calculateManagerPay() {
        return getSalary() + bonus;
    }
}

class Engineer extends Employee {
    private int overtimeHours;
    private double overtimeRate;
    
    // Trùng lặp code
    public double calculateEngineerPay() {
        return getSalary() + (overtimeHours * overtimeRate);
    }
}
```

### **✅ Sau khi tái cấu trúc:**
```java
// Sử dụng Pull Up Method và Form Template Method
abstract class Employee {
    private String name;
    private double baseSalary;
    
    public final double calculatePay() {
        return baseSalary + calculateAdditionalPay();
    }
    
    // Template method - lớp con phải triển khai
    protected abstract double calculateAdditionalPay();
    
    protected double getBaseSalary() {
        return baseSalary;
    }
}

class Manager extends Employee {
    private double bonus;
    
    @Override
    protected double calculateAdditionalPay() {
        return bonus;
    }
}

class Engineer extends Employee {
    private int overtimeHours;
    private double overtimeRate;
    
    @Override
    protected double calculateAdditionalPay() {
        return overtimeHours * overtimeRate;
    }
}
```

## **Lợi Ích**

- **✅ Giảm trùng lặp**: Code chung được đưa lên lớp cha
- **✅ Tăng tính linh hoạt**: Dễ dàng mở rộng và bảo trì
- **✅ Code rõ ràng hơn**: Hệ thống phân cấp được tổ chức hợp lý
- **✅ Dễ test**: Có thể test từng phần riêng biệt
- **✅ Tái sử dụng tốt**: Code chung được sử dụng ở nhiều nơi

## **Điểm Cần Lưu ý**

- **Không lạm dụng kế thừa**: Ưu tiên composition over inheritance
- **Giữ nguyên tắc LSP**: Lớp con phải thay thế được lớp cha
- **Tránh phân cấp quá sâu**: Hệ thống phân cấp quá sâu khó bảo trì
- **Sử dụng đúng mục đích**: Mỗi kỹ thuật có trường hợp sử dụng riêng

## **Khi Nào Không Nên Sử Dụng**

- Khi hệ thống phân cấp đã đủ đơn giản và rõ ràng
- Khi không có sự trùng lặp code đáng kể
- Khi việc thay đổi sẽ phá vỡ nhiều code hiện có
- Khi sử dụng composition phù hợp hơn inheritance

## **Kết Luận**

Xử lý tính tổng quát hóa là nhóm kỹ thuật quan trọng để xây dựng và duy trì hệ thống phân cấp kế thừa hiệu quả. Bằng cách áp dụng các kỹ thuật phù hợp, chúng ta có thể tạo ra code linh hoạt, dễ bảo trì và giảm thiểu sự trùng lặp. Tuy nhiên, cần cân nhắc kỹ lưỡng giữa kế thừa và composition, đồng thời đảm bảo tuân thủ các nguyên tắc SOLID trong thiết kế hướng đối tượng.

**Nguồn:** [refactoring.guru/refactoring/techniques/dealing-with-generalization](https://refactoring.guru/refactoring/techniques/dealing-with-generalization)