# **Thay Thế Tham Số Bằng Phương Thức Rõ Ràng (Replace Parameter with Explicit Methods)**

## **Định Nghĩa**
Thay thế tham số bằng phương thức rõ ràng là kỹ thuật tái cấu trúc thay thế một tham số điều khiển logic bên trong phương thức bằng các phương thức riêng biệt, rõ ràng cho từng trường hợp.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Phương thức sử dụng tham số để quyết định thực hiện hành vi nào
- Có các giá trị tham số cố định, rõ ràng thay vì giá trị động
- Muốn loại bỏ các điều kiện kiểm tra giá trị tham số bên trong phương thức
- Cần cải thiện tính rõ ràng và an toàn của code

### **Không nên sử dụng khi:**
- Tham số có thể nhận nhiều giá trị động khác nhau
- Các giá trị tham số không cố định và có thể mở rộng
- Việc tạo nhiều phương thức làm code trở nên cồng kềnh

## **Các Bước Thực Hiện**

1. **Xác định phương thức có tham số điều khiển**: Tìm phương thức sử dụng tham số để quyết định logic
2. **Tạo phương thức rõ ràng**: Tạo phương thức mới cho mỗi giá trị quan trọng của tham số
3. **Di chuyển logic**: Di chuyển logic tương ứng vào từng phương thức mới
4. **Thay thế lời gọi**: Thay thế lời gọi đến phương thức cũ bằng phương thức mới
5. **Xóa phương thức cũ**: Xóa phương thức cũ khi không còn được sử dụng

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class Employee {
    private double salary;
    
    // Phương thức sử dụng tham số điều khiển
    public void setValue(String type, double value) {
        if ("salary".equals(type)) {
            if (value < 0) {
                throw new IllegalArgumentException("Salary cannot be negative");
            }
            this.salary = value;
        } else if ("bonus".equals(type)) {
            // Xử lý bonus - nhưng không có trường bonus trong class
            // Đây là vấn đề: tham số điều khiển không rõ ràng
        }
        // Có thể thêm nhiều type khác - khó kiểm soát
    }
    
    // Phương thức khác với tham số enum
    public void applyOperation(Operation operation, double amount) {
        switch (operation) {
            case RAISE:
                salary += amount;
                break;
            case CUT:
                salary -= amount;
                if (salary < 0) salary = 0;
                break;
            case MULTIPLY:
                salary *= amount;
                break;
            default:
                throw new IllegalArgumentException("Unknown operation");
        }
    }
}

enum Operation {
    RAISE, CUT, MULTIPLY
}

// Client code - không rõ ràng
Employee emp = new Employee();
emp.setValue("salary", 50000); // Khó biết các type hợp lệ
emp.applyOperation(Operation.RAISE, 1000); // Phải tra enum
```

### **✅ Sau khi thay thế:**
```java
class Employee {
    private double salary;
    
    // Các phương thức rõ ràng, chuyên biệt
    public void setSalary(double salary) {
        if (salary < 0) {
            throw new IllegalArgumentException("Salary cannot be negative");
        }
        this.salary = salary;
    }
    
    public void setBonus(double bonus) {
        // Xử lý bonus cụ thể
        // Có thể thêm validation riêng cho bonus
    }
    
    // Các phương thức thao tác rõ ràng
    public void raiseSalary(double amount) {
        salary += amount;
    }
    
    public void cutSalary(double amount) {
        salary -= amount;
        if (salary < 0) salary = 0;
    }
    
    public void multiplySalary(double factor) {
        salary *= factor;
    }
    
    // Có thể thêm các phương thức phức tạp hơn
    public void raiseSalaryByPercentage(double percentage) {
        if (percentage <= 0) {
            throw new IllegalArgumentException("Percentage must be positive");
        }
        salary *= (1 + percentage / 100);
    }
}

// Client code - rõ ràng và an toàn
Employee emp = new Employee();
emp.setSalary(50000);        // Rõ ràng
emp.raiseSalary(1000);       // Dễ hiểu
emp.raiseSalaryByPercentage(10); // Không cần nhớ enum
```

## **Lợi Ích**

- **✅ Code rõ ràng hơn**: Tên phương thức mô tả chính xác hành vi
- **✅ An toàn hơn**: Loại bỏ lỗi do truyền sai giá trị tham số
- **✅ Dễ mở rộng**: Dễ dàng thêm validation đặc thù cho từng phương thức
- **✅ Hỗ trợ IDE tốt hơn**: Auto-completion gợi ý các phương thức có sẵn
- **✅ Loại bỏ điều kiện**: Không cần kiểm tra giá trị tham số trong phương thức

## **Điểm Cần Lưu ý**

- **Không lạm dụng**: Chỉ áp dụng khi các giá trị tham số là cố định và hữu hạn
- **Cân nhắc với số lượng lớn**: Nếu có quá nhiều giá trị, có thể không khả thi
- **Giữ tính nhất quán**: Đặt tên phương thức theo cùng quy tắc
- **Xem xét trường hợp động**: Vẫn giữ phương thức gốc nếu cần xử lý giá trị động

## **Khi Nào Không Nên Sử Dụng**

- Khi tham số có thể nhận rất nhiều giá trị khác nhau
- Khi các giá trị tham số là động và không thể liệt kê hết
- Khi việc tạo nhiều phương thức làm class trở nên quá lớn
- Khi tham số thực sự cần tính linh hoạt

## **Kỹ Thuật Liên Quan**

- **Parameterize Method**: Kết hợp các phương thức tương tự bằng tham số
- **Replace Conditional with Polymorphism**: Thay thế điều kiện bằng đa hình
- **Extract Method**: Tách phần code thành phương thức mới

## **Kết Luận**

Thay thế tham số bằng phương thức rõ ràng là kỹ thuật hiệu quả khi một phương thức sử dụng tham số để điều khiển logic bên trong. Bằng cách tạo các phương thức chuyên biệt cho từng trường hợp, code trở nên rõ ràng, an toàn và dễ bảo trì hơn. Tuy nhiên, cần cân nhắc áp dụng khi số lượng trường hợp là hữu hạn và không quá lớn để tránh làm class trở nên cồng kềnh.

**Nguồn:** [refactoring.guru/replace-parameter-with-explicit-methods](https://refactoring.guru/replace-parameter-with-explicit-methods)