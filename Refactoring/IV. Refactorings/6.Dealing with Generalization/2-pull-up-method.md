# **Kéo Phương Thức Lên (Pull Up Method)**

## **Định Nghĩa**
Kéo phương thức lên là kỹ thuật tái cấu trúc di chuyển một phương thức từ các lớp con lên lớp cha, khi phương thức đó có cùng implementation trong nhiều lớp con và thực sự thuộc về lớp cha.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các phương thức giống nhau trong nhiều lớp con
- Phương thức có cùng implementation trong tất cả lớp con
- Muốn loại bỏ sự trùng lặp code giữa các lớp con
- Phương thức thực sự thuộc về khái niệm của lớp cha

### **Không nên sử dụng khi:**
- Phương thức chỉ được sử dụng bởi một lớp con
- Phương thức có implementation khác nhau trong các lớp con
- Việc kéo lên làm lớp cha trở nên quá phức tạp

## **Các Bước Thực Hiện**

1. **Xác định phương thức trùng lặp**: Tìm các phương thức giống nhau trong các lớp con
2. **Kiểm tra tính tương thích**: Đảm bảo phương thức có cùng implementation trong tất cả lớp con
3. **Tạo phương thức trong lớp cha**: Di chuyển phương thức lên lớp cha
4. **Xóa phương thức trong lớp con**: Xóa phương thức khỏi các lớp con
5. **Điều chỉnh tham chiếu**: Cập nhật các tham chiếu đến phương thức
6. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi kéo phương thức lên:**
```java
abstract class Employee {
    protected String name;
    protected double baseSalary;
    
    public Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }
}

class Manager extends Employee {
    private double bonus;
    
    public Manager(String name, double baseSalary, double bonus) {
        super(name, baseSalary);
        this.bonus = bonus;
    }
    
    // Phương thức trùng lặp - có trong cả 3 lớp con
    public String getDetails() {
        return String.format("Name: %s, Base Salary: $%.2f, Bonus: $%.2f", 
                           name, baseSalary, bonus);
    }
    
    public double calculatePay() {
        return baseSalary + bonus;
    }
}

class Engineer extends Employee {
    private int overtimeHours;
    private double overtimeRate;
    
    public Engineer(String name, double baseSalary, int overtimeHours, double overtimeRate) {
        super(name, baseSalary);
        this.overtimeHours = overtimeHours;
        this.overtimeRate = overtimeRate;
    }
    
    // Phương thức trùng lặp - có trong cả 3 lớp con
    public String getDetails() {
        return String.format("Name: %s, Base Salary: $%.2f, Overtime: %d hours", 
                           name, baseSalary, overtimeHours);
    }
    
    public double calculatePay() {
        return baseSalary + (overtimeHours * overtimeRate);
    }
}

class Salesperson extends Employee {
    private double commission;
    private double salesAmount;
    
    public Salesperson(String name, double baseSalary, double commission, double salesAmount) {
        super(name, baseSalary);
        this.commission = commission;
        this.salesAmount = salesAmount;
    }
    
    // Phương thức trùng lặp - có trong cả 3 lớp con
    public String getDetails() {
        return String.format("Name: %s, Base Salary: $%.2f, Commission: $%.2f", 
                           name, baseSalary, commission);
    }
    
    public double calculatePay() {
        return baseSalary + (salesAmount * commission);
    }
}
```

### **✅ Sau khi kéo phương thức lên:**
```java
abstract class Employee {
    protected String name;
    protected double baseSalary;
    
    public Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }
    
    // Phương thức getDetails được kéo lên lớp cha
    public String getDetails() {
        return String.format("Name: %s, Base Salary: $%.2f", name, baseSalary);
    }
    
    // Phương thức abstract - lớp con phải triển khai
    public abstract double calculatePay();
}

class Manager extends Employee {
    private double bonus;
    
    public Manager(String name, double baseSalary, double bonus) {
        super(name, baseSalary);
        this.bonus = bonus;
    }
    
    @Override
    public double calculatePay() {
        return baseSalary + bonus;
    }
    
    // Có thể override getDetails nếu cần thông tin chi tiết hơn
    @Override
    public String getDetails() {
        return super.getDetails() + String.format(", Bonus: $%.2f", bonus);
    }
}

class Engineer extends Employee {
    private int overtimeHours;
    private double overtimeRate;
    
    public Engineer(String name, double baseSalary, int overtimeHours, double overtimeRate) {
        super(name, baseSalary);
        this.overtimeHours = overtimeHours;
        this.overtimeRate = overtimeRate;
    }
    
    @Override
    public double calculatePay() {
        return baseSalary + (overtimeHours * overtimeRate);
    }
    
    @Override
    public String getDetails() {
        return super.getDetails() + String.format(", Overtime: %d hours", overtimeHours);
    }
}

class Salesperson extends Employee {
    private double commission;
    private double salesAmount;
    
    public Salesperson(String name, double baseSalary, double commission, double salesAmount) {
        super(name, baseSalary);
        this.commission = commission;
        this.salesAmount = salesAmount;
    }
    
    @Override
    public double calculatePay() {
        return baseSalary + (salesAmount * commission);
    }
    
    @Override
    public String getDetails() {
        return super.getDetails() + String.format(", Commission: $%.2f", commission);
    }
}

// Client code sử dụng nhất quán
Employee manager = new Manager("John", 5000, 1000);
Employee engineer = new Engineer("Jane", 4000, 10, 50);
Employee salesperson = new Salesperson("Bob", 3000, 0.1, 50000);

// Có thể sử dụng getDetails() cho mọi loại employee
System.out.println(manager.getDetails());
System.out.println(engineer.getDetails());
System.out.println(salesperson.getDetails());

// Tính toán lương theo cách riêng của từng loại
System.out.println("Manager pay: $" + manager.calculatePay());
System.out.println("Engineer pay: $" + engineer.calculatePay());
System.out.println("Salesperson pay: $" + salesperson.calculatePay());
```

## **Lợi Ích**

- **✅ Giảm trùng lặp**: Loại bỏ các phương thức giống nhau trong các lớp con
- **✅ Code nhất quán**: Đảm bảo phương thức có cùng behavior trong tất cả lớp con
- **✅ Dễ bảo trì**: Chỉ cần sửa một nơi thay vì nhiều nơi
- **✅ Tăng tính kế thừa**: Tận dụng tốt hơn tính kế thừa trong OOP
- **✅ Đa hình tự nhiên**: Có thể sử dụng phương thức chung thông qua lớp cha

## **Điểm Cần Lưu ý**

- **Kiểm tra implementation**: Đảm bảo phương thức thực sự có cùng implementation
- **Xem xét override**: Cho phép lớp con override nếu cần behavior khác
- **Phạm vi truy cập**: Chọn phạm vi truy cập phù hợp (public, protected)
- **Sử dụng abstract method**: Nếu behavior khác nhau, để phương thức là abstract

## **Khi Nào Không Nên Sử Dụng**

- Khi phương thức có implementation khác nhau trong các lớp con
- Khi chỉ một lớp con sử dụng phương thức đó
- Khi việc kéo lên làm lớp cha trở nên quá phức tạp
- Khi phương thức là implementation detail cụ thể của lớp con

## **Kỹ Thuật Liên Quan**

- **Pull Up Field**: Kéo trường lên lớp cha
- **Pull Up Constructor Body**: Tối ưu constructor trong kế thừa
- **Push Down Method**: Đẩy phương thức xuống lớp con (ngược lại)
- **Form Template Method**: Định nghĩa bộ khung thuật toán trong lớp cha
- **Extract Superclass**: Trích xuất lớp cha từ các lớp có chung phương thức

## **Kết Luận**

Kéo phương thức lên là kỹ thuật quan trọng để loại bỏ sự trùng lặp và tối ưu hóa hệ thống phân cấp kế thừa. Bằng cách di chuyển các phương thức chung lên lớp cha, chúng ta tạo ra code nhất quán, dễ bảo trì và tận dụng tốt hơn các nguyên lý OOP. Tuy nhiên, cần đảm bảo rằng phương thức thực sự thuộc về khái niệm của lớp cha và có cùng implementation trong tất cả lớp con trước khi thực hiện kỹ thuật này.

**Nguồn:** [refactoring.guru/pull-up-method](https://refactoring.guru/pull-up-method)