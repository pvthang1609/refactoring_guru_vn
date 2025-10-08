# **Thay Thế Constructor Với Factory Method (Replace Constructor with Factory Method)**

## **Định Nghĩa**
Thay thế constructor với factory method là kỹ thuật tái cấu trúc thay thế việc gọi constructor trực tiếp bằng việc gọi một phương thức factory, giúp linh hoạt hơn trong việc tạo đối tượng và che giấu logic khởi tạo phức tạp.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Cần thêm logic trước hoặc sau khi tạo đối tượng (validation, caching, registration)
- Muốn che giấu các lớp con cụ thể, chỉ trả về interface hoặc lớp cơ sở
- Cần đặt tên có ý nghĩa hơn cho việc tạo đối tượng
- Muốn kiểm soát số lượng instance (Singleton, Object Pool)
- Cần trả về các lớp con khác nhau dựa trên điều kiện

### **Không nên sử dụng khi:**
- Việc khởi tạo đơn giản và không cần thêm logic
- Constructor đã đủ rõ ràng và không cần che giấu implementation
- Không có nhu cầu thay đổi cách tạo đối tượng trong tương lai

## **Các Bước Thực Hiện**

1. **Tạo factory method**: Tạo phương thức static trả về đối tượng cùng kiểu với constructor
2. **Di chuyển logic khởi tạo**: Di chuyển code từ constructor sang factory method
3. **Thay thế lời gọi constructor**: Thay thế các lời gọi constructor bằng lời gọi factory method
4. **Ẩn constructor (tùy chọn)**: Đánh dấu constructor là private nếu cần
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class Employee {
    private String type;
    private double salary;
    private double bonus;
    
    // Constructor với tham số phức tạp
    public Employee(String type, double baseSalary, double overtime, int grade) {
        this.type = type;
        
        // Logic tính toán phức tạp trong constructor
        if ("manager".equals(type)) {
            this.salary = baseSalary * 1.5 + overtime * 2.0;
            this.bonus = baseSalary * 0.3;
        } else if ("engineer".equals(type)) {
            this.salary = baseSalary + overtime * 1.5;
            this.bonus = baseSalary * 0.2 + grade * 100;
        } else {
            this.salary = baseSalary + overtime;
            this.bonus = baseSalary * 0.1;
        }
    }
    
    // Nhiều constructor với ý nghĩa không rõ ràng
    public Employee(double monthlySalary) {
        this("regular", monthlySalary, 0, 1);
    }
    
    public Employee(double hourlyRate, double hours) {
        this("contract", hourlyRate * hours, 0, 1);
    }
}

// Client code - phải hiểu logic phức tạp khi tạo đối tượng
Employee manager = new Employee("manager", 5000, 10, 3);
Employee engineer = new Employee("engineer", 4000, 20, 2);
Employee regular = new Employee(3000);
```

### **✅ Sau khi thay thế:**
```java
class Employee {
    private String type;
    private double salary;
    private double bonus;
    
    // Constructor trở nên đơn giản, có thể private
    private Employee(String type, double salary, double bonus) {
        this.type = type;
        this.salary = salary;
        this.bonus = bonus;
    }
    
    // Factory methods với tên có ý nghĩa
    public static Employee createManager(double baseSalary, double overtime, int grade) {
        double salary = baseSalary * 1.5 + overtime * 2.0;
        double bonus = baseSalary * 0.3;
        return new Employee("manager", salary, bonus);
    }
    
    public static Employee createEngineer(double baseSalary, double overtime, int grade) {
        double salary = baseSalary + overtime * 1.5;
        double bonus = baseSalary * 0.2 + grade * 100;
        return new Employee("engineer", salary, bonus);
    }
    
    public static Employee createRegularEmployee(double monthlySalary) {
        return new Employee("regular", monthlySalary, monthlySalary * 0.1);
    }
    
    public static Employee createContractEmployee(double hourlyRate, double hours) {
        double salary = hourlyRate * hours;
        return new Employee("contract", salary, salary * 0.05);
    }
    
    // Có thể thêm validation và caching
    public static Employee createManagerWithValidation(double baseSalary, double overtime, int grade) {
        if (baseSalary < 0) {
            throw new IllegalArgumentException("Base salary cannot be negative");
        }
        if (grade < 1 || grade > 5) {
            throw new IllegalArgumentException("Grade must be between 1 and 5");
        }
        
        // Có thể thêm caching logic ở đây
        String cacheKey = "manager_" + baseSalary + "_" + overtime + "_" + grade;
        Employee cached = cache.get(cacheKey);
        if (cached != null) {
            return cached;
        }
        
        Employee newManager = createManager(baseSalary, overtime, grade);
        cache.put(cacheKey, newManager);
        return newManager;
    }
}

// Client code - rõ ràng và dễ hiểu
Employee manager = Employee.createManager(5000, 10, 3);
Employee engineer = Employee.createEngineer(4000, 20, 2);
Employee regular = Employee.createRegularEmployee(3000);
Employee contract = Employee.createContractEmployee(50, 160);

// Có thể sử dụng phiên bản với validation
try {
    Employee validatedManager = Employee.createManagerWithValidation(5000, 10, 3);
} catch (IllegalArgumentException e) {
    // Xử lý lỗi
}
```

## **Lợi Ích**

- **✅ Đặt tên có ý nghĩa**: Tên factory method mô tả rõ cách tạo đối tượng
- **✅ Che giấu implementation**: Client không cần biết lớp cụ thể được tạo
- **✅ Linh hoạt hơn**: Có thể trả về các lớp con khác nhau dựa trên điều kiện
- **✅ Kiểm soát tốt hơn**: Có thể thêm validation, caching, logging
- **✅ Giảm sự phụ thuộc**: Client code ít phụ thuộc vào lớp cụ thể

## **Điểm Cần Lưu ý**

- **Không lạm dụng**: Chỉ sử dụng khi thực sự cần thêm logic hoặc linh hoạt
- **Đặt tên rõ ràng**: Tên factory method nên bắt đầu với "create", "new", "get"
- **Cân nhắc static vs instance**: Thường dùng static, nhưng có thể dùng instance method nếu cần state
- **Xem xét subclassing**: Có thể tạo factory class riêng nếu logic phức tạp

## **Khi Nào Không Nên Sử Dụng**

- Khi việc khởi tạo đơn giản và không cần thêm logic
- Khi không có nhu cầu thay đổi cách tạo đối tượng
- Khi constructor đã đủ rõ ràng và dễ sử dụng
- Khi làm code trở nên phức tạp không cần thiết

## **Kỹ Thuật Liên Quan**

- **Replace Constructor with Builder**: Thay thế constructor với builder pattern
- **Replace Error Code with Exception**: Thay thế mã lỗi bằng ngoại lệ
- **Introduce Parameter Object**: Giới thiệu đối tượng tham số
- **Extract Method**: Tách phương thức để giảm độ phức tạp

## **Kết Luận**

Thay thế constructor với factory method là kỹ thuật mạnh mẽ để cải thiện khả năng kiểm soát và tính linh hoạt trong việc tạo đối tượng. Bằng cách che giấu logic khởi tạo phức tạp và cung cấp các phương thức có tên rõ ràng, code trở nên dễ đọc, dễ bảo trì và linh hoạt hơn. Tuy nhiên, cần cân nhắc sử dụng hợp lý để không làm code trở nên phức tạp không cần thiết.

**Nguồn:** [refactoring.guru/replace-constructor-with-factory-method](https://refactoring.guru/replace-constructor-with-factory-method)