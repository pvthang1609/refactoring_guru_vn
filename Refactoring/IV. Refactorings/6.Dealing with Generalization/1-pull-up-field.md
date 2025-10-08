# **Kéo Trường Lên (Pull Up Field)**

## **Định Nghĩa**
Kéo trường lên là kỹ thuật tái cấu trúc di chuyển một trường từ các lớp con lên lớp cha, khi trường đó được sử dụng bởi nhiều lớp con và thực sự thuộc về lớp cha.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các trường giống nhau trong nhiều lớp con
- Trường đại diện cho dữ liệu chung mà tất cả lớp con đều cần
- Muốn loại bỏ sự trùng lặp giữa các lớp con
- Trường thực sự thuộc về khái niệm của lớp cha

### **Không nên sử dụng khi:**
- Trường chỉ được sử dụng bởi một lớp con
- Trường có ý nghĩa khác nhau trong các lớp con khác nhau
- Việc kéo lên làm lớp cha trở nên quá phức tạp

## **Các Bước Thực Hiện**

1. **Xác định trường trùng lặp**: Tìm các trường giống nhau trong các lớp con
2. **Kiểm tra tính tương thích**: Đảm bảo trường có cùng ý nghĩa trong tất cả lớp con
3. **Tạo trường trong lớp cha**: Khai báo trường trong lớp cha
4. **Xóa trường trong lớp con**: Xóa trường khỏi các lớp con
5. **Điều chỉnh constructor**: Cập nhật constructor của lớp con để sử dụng trường từ lớp cha
6. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi kéo trường lên:**
```java
abstract class Employee {
    // Các trường chung đã có ở lớp cha
    private String name;
    private String id;
    
    public Employee(String name, String id) {
        this.name = name;
        this.id = id;
    }
}

class FullTimeEmployee extends Employee {
    // Trường trùng lặp ở các lớp con
    private String department;
    private double salary;
    private Date hireDate;  // Trùng lặp - có trong cả 2 lớp con
    
    public FullTimeEmployee(String name, String id, String department, 
                           double salary, Date hireDate) {
        super(name, id);
        this.department = department;
        this.salary = salary;
        this.hireDate = hireDate;
    }
    
    public Date getHireDate() {
        return hireDate;
    }
}

class PartTimeEmployee extends Employee {
    private double hourlyRate;
    private int hoursPerWeek;
    private Date hireDate;  // Trùng lặp - có trong cả 2 lớp con
    
    public PartTimeEmployee(String name, String id, double hourlyRate, 
                           int hoursPerWeek, Date hireDate) {
        super(name, id);
        this.hourlyRate = hourlyRate;
        this.hoursPerWeek = hoursPerWeek;
        this.hireDate = hireDate;
    }
    
    public Date getHireDate() {
        return hireDate;
    }
}

class ContractEmployee extends Employee {
    private Date contractStartDate;
    private Date contractEndDate;
    private Date hireDate;  // Trùng lặp - có trong cả 3 lớp con
    
    public ContractEmployee(String name, String id, Date contractStartDate, 
                           Date contractEndDate, Date hireDate) {
        super(name, id);
        this.contractStartDate = contractStartDate;
        this.contractEndDate = contractEndDate;
        this.hireDate = hireDate;
    }
    
    public Date getHireDate() {
        return hireDate;
    }
}
```

### **✅ Sau khi kéo trường lên:**
```java
abstract class Employee {
    private String name;
    private String id;
    // Trường hireDate được kéo lên lớp cha - không còn trùng lặp
    private Date hireDate;
    
    public Employee(String name, String id, Date hireDate) {
        this.name = name;
        this.id = id;
        this.hireDate = hireDate;
    }
    
    // Getter cho hireDate - có sẵn cho tất cả lớp con
    public Date getHireDate() {
        return hireDate;
    }
    
    // Các getter khác
    public String getName() { return name; }
    public String getId() { return id; }
}

class FullTimeEmployee extends Employee {
    private String department;
    private double salary;
    
    // Constructor không còn khởi tạo hireDate
    public FullTimeEmployee(String name, String id, String department, 
                           double salary, Date hireDate) {
        super(name, id, hireDate);  // Truyền hireDate lên lớp cha
        this.department = department;
        this.salary = salary;
    }
    
    // Không cần getHireDate() - kế thừa từ lớp cha
    public String getDepartment() { return department; }
    public double getSalary() { return salary; }
}

class PartTimeEmployee extends Employee {
    private double hourlyRate;
    private int hoursPerWeek;
    
    public PartTimeEmployee(String name, String id, double hourlyRate, 
                           int hoursPerWeek, Date hireDate) {
        super(name, id, hireDate);  // Truyền hireDate lên lớp cha
        this.hourlyRate = hourlyRate;
        this.hoursPerWeek = hoursPerWeek;
    }
    
    // Không cần getHireDate() - kế thừa từ lớp cha
    public double getHourlyRate() { return hourlyRate; }
    public int getHoursPerWeek() { return hoursPerWeek; }
}

class ContractEmployee extends Employee {
    private Date contractStartDate;
    private Date contractEndDate;
    
    public ContractEmployee(String name, String id, Date contractStartDate, 
                           Date contractEndDate, Date hireDate) {
        super(name, id, hireDate);  // Truyền hireDate lên lớp cha
        this.contractStartDate = contractStartDate;
        this.contractEndDate = contractEndDate;
    }
    
    // Không cần getHireDate() - kế thừa từ lớp cha
    public Date getContractStartDate() { return contractStartDate; }
    public Date getContractEndDate() { return contractEndDate; }
}

// Client code sử dụng nhất quán
Employee fullTime = new FullTimeEmployee("John", "FT001", "Engineering", 5000, new Date());
Employee partTime = new PartTimeEmployee("Jane", "PT001", 25.0, 20, new Date());

// Có thể sử dụng getHireDate() cho mọi loại employee
System.out.println("Full-time hire date: " + fullTime.getHireDate());
System.out.println("Part-time hire date: " + partTime.getHireDate());

// Tính số năm làm việc - có thể đưa lên lớp cha
public abstract class Employee {
    // ... các trường và constructor khác
    
    public int getYearsOfService() {
        Date now = new Date();
        long diff = now.getTime() - hireDate.getTime();
        return (int) (diff / (1000L * 60 * 60 * 24 * 365));
    }
}
```

## **Lợi Ích**

- **✅ Giảm trùng lặp**: Loại bỏ các trường giống nhau trong các lớp con
- **✅ Code nhất quán**: Đảm bảo trường có cùng ý nghĩa và cách sử dụng
- **✅ Dễ bảo trì**: Chỉ cần sửa một nơi thay vì nhiều nơi
- **✅ Tăng tính kế thừa**: Tận dụng tốt hơn tính kế thừa trong OOP
- **✅ Dễ mở rộng**: Thêm phương thức liên quan đến trường trong lớp cha

## **Điểm Cần Lưu ý**

- **Kiểm tra ý nghĩa**: Đảm bảo trường thực sự có cùng ý nghĩa trong tất cả lớp con
- **Cập nhật constructor**: Nhớ cập nhật tất cả constructor của lớp con
- **Xem xét phạm vi truy cập**: Chọn phạm vi truy cập phù hợp (private, protected)
- **Không phá vỡ encapsulation**: Đảm bảo không làm lộ implementation detail không cần thiết

## **Khi Nào Không Nên Sử Dụng**

- Khi trường có ý nghĩa khác nhau trong các lớp con khác nhau
- Khi chỉ một lớp con sử dụng trường đó
- Khi việc kéo lên làm lớp cha trở nên quá "béo" (god class)
- Khi trường là implementation detail cụ thể của lớp con

## **Kỹ Thuật Liên Quan**

- **Pull Up Method**: Kéo phương thức lên lớp cha
- **Pull Up Constructor Body**: Tối ưu constructor trong kế thừa
- **Push Down Field**: Đẩy trường xuống lớp con (ngược lại)
- **Extract Superclass**: Trích xuất lớp cha từ các lớp có chung trường

## **Kết Luận**

Kéo trường lên là kỹ thuật quan trọng để loại bỏ sự trùng lặp và tối ưu hóa hệ thống phân cấp kế thừa. Bằng cách di chuyển các trường chung lên lớp cha, chúng ta tạo ra code nhất quán, dễ bảo trì và tận dụng tốt hơn các nguyên lý OOP. Tuy nhiên, cần đảm bảo rằng trường thực sự thuộc về khái niệm của lớp cha và có cùng ý nghĩa trong tất cả lớp con trước khi thực hiện kỹ thuật này.

**Nguồn:** [refactoring.guru/pull-up-field](https://refactoring.guru/pull-up-field)