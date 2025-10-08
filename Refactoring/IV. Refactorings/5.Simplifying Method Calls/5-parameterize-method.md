# **Tham Số Hóa Phương Thức (Parameterize Method)**

## **Định Nghĩa**
Tham số hóa phương thức là kỹ thuật tái cấu trúc kết hợp nhiều phương thức tương tự nhau thành một phương thức duy nhất bằng cách thêm tham số để điều khiển sự khác biệt giữa chúng.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có nhiều phương thức thực hiện công việc tương tự nhau, chỉ khác nhau ở một vài giá trị
- Muốn loại bỏ sự trùng lặp code giữa các phương thức
- Cần tạo phương thức linh hoạt hơn có thể xử lý nhiều trường hợp

### **Không nên sử dụng khi:**
- Các phương thức có logic quá khác biệt
- Việc tham số hóa làm phương thức trở nên quá phức tạp
- Các phương thức đã đủ đơn giản và rõ ràng

## **Các Bước Thực Hiện**

1. **Xác định phương thức trùng lặp**: Tìm các phương thức có logic tương tự
2. **Tạo phương thức thống nhất**: Tạo phương thức mới với các tham số cần thiết
3. **Thay thế lời gọi**: Thay thế các lời gọi phương thức cũ bằng phương thức mới
4. **Xóa phương thức cũ**: Xóa các phương thức trùng lặp đã không còn sử dụng
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi tham số hóa:**
```java
class Employee {
    private double salary;
    
    // Các phương thức trùng lặp
    public void raiseSalary5Percent() {
        salary *= 1.05;
    }
    
    public void raiseSalary10Percent() {
        salary *= 1.10;
    }
    
    public void raiseSalary15Percent() {
        salary *= 1.15;
    }
    
    // Các phương thức tính bonus trùng lặp
    public double calculateYearlyBonus() {
        return salary * 0.1;
    }
    
    public double calculatePerformanceBonus() {
        return salary * 0.15;
    }
    
    public double calculateSpecialBonus() {
        return salary * 0.2;
    }
}

// Client code - phải chọn phương thức cụ thể
Employee emp = new Employee();
emp.raiseSalary5Percent();  // Tăng 5%
emp.raiseSalary10Percent(); // Tăng 10%

double bonus1 = emp.calculateYearlyBonus();
double bonus2 = emp.calculatePerformanceBonus();
```

### **✅ Sau khi tham số hóa:**
```java
class Employee {
    private double salary;
    
    // Một phương thức duy nhất với tham số
    public void raiseSalary(double percentage) {
        if (percentage <= 0) {
            throw new IllegalArgumentException("Percentage must be positive");
        }
        salary *= (1 + percentage / 100);
    }
    
    // Một phương thức tính bonus với tham số
    public double calculateBonus(double bonusRate) {
        if (bonusRate < 0 || bonusRate > 1) {
            throw new IllegalArgumentException("Bonus rate must be between 0 and 1");
        }
        return salary * bonusRate;
    }
    
    // Có thể giữ lại các phương thức cũ như wrapper methods nếu cần
    public void raiseSalary5Percent() {
        raiseSalary(5);
    }
    
    public void raiseSalary10Percent() {
        raiseSalary(10);
    }
}

// Client code - linh hoạt hơn
Employee emp = new Employee();
emp.raiseSalary(5);   // Tăng 5%
emp.raiseSalary(7.5); // Tăng 7.5% - không có phương thức riêng trước đây
emp.raiseSalary(10);  // Tăng 10%

double yearlyBonus = emp.calculateBonus(0.1);     // Bonus 10%
double perfBonus = emp.calculateBonus(0.15);      // Bonus 15%
double customBonus = emp.calculateBonus(0.12);    // Bonus 12% - linh hoạt
```

## **Lợi Ích**

- **✅ Giảm trùng lặp**: Loại bỏ code trùng lặp giữa các phương thức
- **✅ Linh hoạt hơn**: Có thể xử lý nhiều trường hợp mà không cần tạo phương thức mới
- **✅ Dễ bảo trì**: Chỉ cần sửa một phương thức thay vì nhiều phương thức
- **✅ Code gọn hơn**: Giảm số lượng phương thức trong class

## **Điểm Cần Lưu ý**

- **Validation**: Thêm validation cho tham số để đảm bảo tính đúng đắn
- **Đặt tên rõ ràng**: Đặt tên tham số và phương thức sao cho dễ hiểu
- **Giá trị mặc định**: Xem xét thêm tham số mặc định nếu phù hợp
- **Bảo toàn tính rõ ràng**: Đảm bảo phương thức vẫn dễ sử dụng sau khi tham số hóa

## **Khi Nào Không Nên Sử Dụng**

- Khi các phương thức có logic quá khác biệt
- Khi việc tham số hóa làm phương thức trở nên phức tạp và khó hiểu
- Khi cần giữ lại các phương thức cụ thể cho mục đích API public

## **Kỹ Thuật Liên Quan**

- **Extract Method**: Tách code thành phương thức mới
- **Inline Method**: Gom phương thức nhỏ vào nơi gọi
- **Replace Parameter with Explicit Methods**: Thay thế tham số bằng các phương thức rõ ràng

## **Kết Luận**

Tham số hóa phương thức là kỹ thuật hiệu quả để loại bỏ sự trùng lặp và tạo ra các phương thức linh hoạt hơn. Bằng cách kết hợp các phương thức tương tự thành một phương thức duy nhất với tham số, chúng ta giảm được số lượng code, dễ bảo trì và có thể xử lý nhiều trường hợp khác nhau. Tuy nhiên, cần cân nhắc để không làm phương thức trở nên quá phức tạp và khó sử dụng.

**Nguồn:** [refactoring.guru/parameterize-method](https://refactoring.guru/parameterize-method)