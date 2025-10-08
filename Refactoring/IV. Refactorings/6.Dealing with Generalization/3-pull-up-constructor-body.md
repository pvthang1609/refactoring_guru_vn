# **Kéo Thân Constructor Lên (Pull Up Constructor Body)**

## **Định Nghĩa**
Kéo thân constructor lên là kỹ thuật tái cấu trúc di chuyển phần code giống nhau từ các constructor của lớp con lên constructor của lớp cha, giúp loại bỏ sự trùng lặp và tận dụng tính kế thừa.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Các lớp con có constructor chứa code giống nhau
- Muốn loại bỏ sự trùng lặp code trong việc khởi tạo
- Cần đảm bảo một số bước khởi tạo luôn được thực hiện bởi tất cả lớp con
- Muốn tập trung logic khởi tạo chung vào một nơi

### **Không nên sử dụng khi:**
- Các constructor không có code chung đáng kể
- Logic khởi tạo quá khác biệt giữa các lớp con
- Việc kéo lên làm constructor của lớp cha trở nên quá phức tạp

## **Các Bước Thực Hiện**

1. **Xác định code chung**: Tìm phần code giống nhau trong các constructor của lớp con
2. **Tạo constructor trong lớp cha**: Tạo constructor trong lớp cha chứa code chung
3. **Gọi constructor cha**: Sử dụng `super()` để gọi constructor của lớp cha từ lớp con
4. **Xóa code trùng lặp**: Xóa code chung khỏi constructor của lớp con
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi kéo thân constructor lên:**
```java
abstract class Employee {
    protected String name;
    protected String id;
    protected Date hireDate;
    
    // Các getter
    public String getName() { return name; }
    public String getId() { return id; }
    public Date getHireDate() { return hireDate; }
}

class FullTimeEmployee extends Employee {
    private String department;
    private double salary;
    
    public FullTimeEmployee(String name, String id, Date hireDate, 
                           String department, double salary) {
        // Code trùng lặp - có trong cả 3 lớp con
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        if (id == null || id.trim().isEmpty()) {
            throw new IllegalArgumentException("ID cannot be empty");
        }
        if (hireDate == null) {
            throw new IllegalArgumentException("Hire date cannot be null");
        }
        if (hireDate.after(new Date())) {
            throw new IllegalArgumentException("Hire date cannot be in the future");
        }
        
        this.name = name;
        this.id = id;
        this.hireDate = hireDate;
        
        // Code riêng của lớp con
        this.department = department;
        this.salary = salary;
    }
}

class PartTimeEmployee extends Employee {
    private double hourlyRate;
    private int hoursPerWeek;
    
    public PartTimeEmployee(String name, String id, Date hireDate,
                           double hourlyRate, int hoursPerWeek) {
        // Code trùng lặp - giống hệt trong FullTimeEmployee
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        if (id == null || id.trim().isEmpty()) {
            throw new IllegalArgumentException("ID cannot be empty");
        }
        if (hireDate == null) {
            throw new IllegalArgumentException("Hire date cannot be null");
        }
        if (hireDate.after(new Date())) {
            throw new IllegalArgumentException("Hire date cannot be in the future");
        }
        
        this.name = name;
        this.id = id;
        this.hireDate = hireDate;
        
        // Code riêng của lớp con
        this.hourlyRate = hourlyRate;
        this.hoursPerWeek = hoursPerWeek;
    }
}

class ContractEmployee extends Employee {
    private Date contractEndDate;
    
    public ContractEmployee(String name, String id, Date hireDate, Date contractEndDate) {
        // Code trùng lặp - giống hệt trong các lớp con khác
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        if (id == null || id.trim().isEmpty()) {
            throw new IllegalArgumentException("ID cannot be empty");
        }
        if (hireDate == null) {
            throw new IllegalArgumentException("Hire date cannot be null");
        }
        if (hireDate.after(new Date())) {
            throw new IllegalArgumentException("Hire date cannot be in the future");
        }
        
        this.name = name;
        this.id = id;
        this.hireDate = hireDate;
        
        // Code riêng của lớp con
        if (contractEndDate == null || contractEndDate.before(hireDate)) {
            throw new IllegalArgumentException("Invalid contract end date");
        }
        this.contractEndDate = contractEndDate;
    }
}
```

### **✅ Sau khi kéo thân constructor lên:**
```java
abstract class Employee {
    protected String name;
    protected String id;
    protected Date hireDate;
    
    // Constructor chung trong lớp cha
    protected Employee(String name, String id, Date hireDate) {
        // Validation chung - chỉ ở một nơi
        validateCommonFields(name, id, hireDate);
        
        this.name = name;
        this.id = id;
        this.hireDate = new Date(hireDate.getTime()); // Defensive copy
    }
    
    private void validateCommonFields(String name, String id, Date hireDate) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        if (id == null || id.trim().isEmpty()) {
            throw new IllegalArgumentException("ID cannot be empty");
        }
        if (hireDate == null) {
            throw new IllegalArgumentException("Hire date cannot be null");
        }
        if (hireDate.after(new Date())) {
            throw new IllegalArgumentException("Hire date cannot be in the future");
        }
    }
    
    // Có thể thêm phương thức tiện ích chung
    protected final void validateSalary(double salary) {
        if (salary < 0) {
            throw new IllegalArgumentException("Salary cannot be negative");
        }
    }
    
    protected final void validateHourlyRate(double hourlyRate) {
        if (hourlyRate < 0) {
            throw new IllegalArgumentException("Hourly rate cannot be negative");
        }
    }
    
    // Các getter
    public String getName() { return name; }
    public String getId() { return id; }
    public Date getHireDate() { return new Date(hireDate.getTime()); } // Defensive copy
}

class FullTimeEmployee extends Employee {
    private String department;
    private double salary;
    
    public FullTimeEmployee(String name, String id, Date hireDate, 
                           String department, double salary) {
        // Gọi constructor của lớp cha
        super(name, id, hireDate);
        
        // Validation riêng
        if (department == null || department.trim().isEmpty()) {
            throw new IllegalArgumentException("Department cannot be empty");
        }
        validateSalary(salary); // Sử dụng phương thức từ lớp cha
        
        // Code riêng của lớp con
        this.department = department;
        this.salary = salary;
    }
}

class PartTimeEmployee extends Employee {
    private double hourlyRate;
    private int hoursPerWeek;
    
    public PartTimeEmployee(String name, String id, Date hireDate,
                           double hourlyRate, int hoursPerWeek) {
        // Gọi constructor của lớp cha
        super(name, id, hireDate);
        
        // Validation riêng
        validateHourlyRate(hourlyRate); // Sử dụng phương thức từ lớp cha
        if (hoursPerWeek < 0 || hoursPerWeek > 168) {
            throw new IllegalArgumentException("Invalid hours per week");
        }
        
        // Code riêng của lớp con
        this.hourlyRate = hourlyRate;
        this.hoursPerWeek = hoursPerWeek;
    }
}

class ContractEmployee extends Employee {
    private Date contractEndDate;
    
    public ContractEmployee(String name, String id, Date hireDate, Date contractEndDate) {
        // Gọi constructor của lớp cha
        super(name, id, hireDate);
        
        // Validation riêng
        if (contractEndDate == null || contractEndDate.before(hireDate)) {
            throw new IllegalArgumentException("Invalid contract end date");
        }
        
        // Code riêng của lớp con
        this.contractEndDate = new Date(contractEndDate.getTime()); // Defensive copy
    }
    
    public Date getContractEndDate() {
        return new Date(contractEndDate.getTime()); // Defensive copy
    }
}
```

## **Lợi Ích**

- **✅ Giảm trùng lặp**: Loại bỏ code validation và khởi tạo giống nhau
- **✅ Code nhất quán**: Đảm bảo validation chung được áp dụng cho tất cả lớp con
- **✅ Dễ bảo trì**: Chỉ cần sửa validation ở một nơi
- **✅ Tăng tính đóng gói**: Tập trung logic khởi tạo chung vào lớp cha
- **✅ Tuân thủ DRY**: Không lặp lại code (Don't Repeat Yourself)

## **Điểm Cần Lưu ý**

- **Sử dụng super()**: Luôn gọi super() ở đầu constructor của lớp con
- **Phạm vi constructor cha**: Constructor của lớp cha nên là protected
- **Defensive copying**: Với các đối tượng mutable như Date, cần tạo bản sao
- **Validation riêng**: Giữ validation cụ thể của lớp con trong constructor của chúng

## **Khi Nào Không Nên Sử Dụng**

- Khi các constructor không có đủ code chung
- Khi logic khởi tạo quá khác biệt giữa các lớp con
- Khi việc tạo constructor cha làm phức tạp hệ thống phân cấp
- Khi sử dụng composition thay vì kế thừa

## **Kỹ Thuật Liên Quan**

- **Pull Up Method**: Kéo phương thức lên lớp cha
- **Pull Up Field**: Kéo trường lên lớp cha
- **Extract Method**: Trích xuất phương thức để tái cấu trúc code
- **Replace Constructor with Factory Method**: Thay thế constructor bằng factory method

## **Kết Luận**

Kéo thân constructor lên là kỹ thuật hiệu quả để loại bỏ sự trùng lặp trong việc khởi tạo các đối tượng trong hệ thống phân cấp kế thừa. Bằng cách di chuyển code chung lên constructor của lớp cha, chúng ta đảm bảo tính nhất quán, dễ bảo trì và tuân thủ nguyên tắc DRY. Tuy nhiên, cần đảm bảo rằng code thực sự chung và có ý nghĩa ở mức độ lớp cha trước khi thực hiện kỹ thuật này.

**Nguồn:** [refactoring.guru/pull-up-constructor-body](https://refactoring.guru/pull-up-constructor-body)