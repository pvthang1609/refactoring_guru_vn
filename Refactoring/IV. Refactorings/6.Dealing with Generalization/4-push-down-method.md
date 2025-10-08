# **Đẩy Phương Thức Xuống (Push Down Method)**

## **Định Nghĩa**
Đẩy phương thức xuống là kỹ thuật tái cấu trúc di chuyển một phương thức từ lớp cha xuống các lớp con, khi phương thức đó chỉ được sử dụng bởi một số lớp con cụ thể và không thực sự thuộc về lớp cha.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Phương thức trong lớp cha chỉ được sử dụng bởi một số lớp con
- Phương thức không phù hợp với tất cả các lớp con
- Muốn giữ cho lớp cha gọn gàng và chỉ chứa những phương thức thực sự chung
- Phương thức đại diện cho behavior cụ thể của một số lớp con

### **Không nên sử dụng khi:**
- Phương thức được sử dụng bởi hầu hết hoặc tất cả các lớp con
- Phương thức thực sự thuộc về khái niệm của lớp cha
- Việc đẩy xuống tạo ra sự trùng lặp code giữa các lớp con

## **Các Bước Thực Hiện**

1. **Xác định phương thức cần đẩy xuống**: Tìm phương thức trong lớp cha chỉ liên quan đến một số lớp con
2. **Tạo phương thức trong lớp con**: Sao chép phương thức xuống các lớp con cần nó
3. **Xóa phương thức khỏi lớp cha**: Xóa phương thức khỏi lớp cha sau khi đã di chuyển
4. **Điều chỉnh tham chiếu**: Cập nhật các tham chiếu đến phương thức
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi đẩy phương thức xuống:**
```java
abstract class Employee {
    protected String name;
    protected double baseSalary;
    
    public Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }
    
    // Phương thức chung - phù hợp cho tất cả
    public String getBasicInfo() {
        return String.format("Name: %s, Base Salary: $%.2f", name, baseSalary);
    }
    
    // Phương thức chỉ phù hợp với Manager - KHÔNG NÊN ở lớp cha
    public void approveLeave(String employeeId, int days) {
        // Logic phê duyệt nghỉ phép
        System.out.println("Leave approved for " + employeeId + " for " + days + " days");
    }
    
    // Phương thức chỉ phù hợp với Engineer - KHÔNG NÊN ở lớp cha
    public void writeCode() {
        // Logic viết code
        System.out.println("Writing code...");
    }
    
    // Phương thức chỉ phù hợp với Salesperson - KHÔNG NÊN ở lớp cha
    public void makeSale(double amount) {
        // Logic bán hàng
        System.out.println("Making sale of $" + amount);
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
    
    // Manager sử dụng approveLeave - hợp lệ
    public void manageTeam() {
        approveLeave("EMP001", 5); // OK
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
    
    // Engineer sử dụng writeCode - hợp lệ
    public void developFeature() {
        writeCode(); // OK
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
    
    // Salesperson sử dụng makeSale - hợp lệ
    public void meetClient() {
        makeSale(5000); // OK
    }
}

class Intern extends Employee {
    public Intern(String name, double baseSalary) {
        super(name, baseSalary);
    }
    
    @Override
    public double calculatePay() {
        return baseSalary;
    }
    
    // Intern KHÔNG sử dụng approveLeave, writeCode, hay makeSale
    // Nhưng vẫn kế thừa chúng - KHÔNG PHÙ HỢP
}
```

### **✅ Sau khi đẩy phương thức xuống:**
```java
abstract class Employee {
    protected String name;
    protected double baseSalary;
    
    public Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }
    
    // Chỉ giữ lại phương thức thực sự chung
    public String getBasicInfo() {
        return String.format("Name: %s, Base Salary: $%.2f", name, baseSalary);
    }
    
    public abstract double calculatePay();
    
    // Các getter
    public String getName() { return name; }
    public double getBaseSalary() { return baseSalary; }
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
    
    // Phương thức được đẩy xuống từ lớp cha - chỉ Manager có
    public void approveLeave(String employeeId, int days) {
        // Logic phê duyệt nghỉ phép cụ thể cho Manager
        System.out.println("Manager " + name + " approved leave for " + 
                          employeeId + " for " + days + " days");
    }
    
    // Phương thức riêng của Manager
    public void manageTeam() {
        System.out.println("Managing team...");
    }
    
    public void conductPerformanceReview(String employeeId) {
        System.out.println("Conducting performance review for " + employeeId);
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
    
    // Phương thức được đẩy xuống từ lớp cha - chỉ Engineer có
    public void writeCode() {
        // Logic viết code cụ thể cho Engineer
        System.out.println("Engineer " + name + " is writing code...");
    }
    
    // Phương thức riêng của Engineer
    public void debugCode() {
        System.out.println("Debugging code...");
    }
    
    public void attendTechMeeting() {
        System.out.println("Attending technical meeting...");
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
    
    // Phương thức được đẩy xuống từ lớp cha - chỉ Salesperson có
    public void makeSale(double amount) {
        // Logic bán hàng cụ thể cho Salesperson
        this.salesAmount += amount;
        System.out.println("Salesperson " + name + " made a sale of $" + amount);
    }
    
    // Phương thức riêng của Salesperson
    public void contactLead(String leadName) {
        System.out.println("Contacting lead: " + leadName);
    }
    
    public void prepareProposal(String clientName) {
        System.out.println("Preparing proposal for " + clientName);
    }
}

class Intern extends Employee {
    public Intern(String name, double baseSalary) {
        super(name, baseSalary);
    }
    
    @Override
    public double calculatePay() {
        return baseSalary;
    }
    
    // Intern chỉ có phương thức phù hợp với mình
    public void learnSkills() {
        System.out.println("Learning new skills...");
    }
    
    public void shadowMentor() {
        System.out.println("Shadowing mentor...");
    }
}

// Client code sử dụng cụ thể từng loại
Manager manager = new Manager("Alice", 6000, 1000);
manager.approveLeave("EMP001", 5); // Chỉ Manager có phương thức này

Engineer engineer = new Engineer("Bob", 5000, 10, 50);
engineer.writeCode(); // Chỉ Engineer có phương thức này

Salesperson salesperson = new Salesperson("Charlie", 4000, 0.1, 0);
salesperson.makeSale(2500); // Chỉ Salesperson có phương thức này

Intern intern = new Intern("David", 2000);
intern.learnSkills(); // Chỉ Intern có phương thức này

// Tất cả đều có phương thức chung
System.out.println(manager.getBasicInfo());
System.out.println(engineer.getBasicInfo());
System.out.println(salesperson.getBasicInfo());
System.out.println(intern.getBasicInfo());
```

## **Lợi Ích**

- **✅ Lớp cha gọn gàng**: Chỉ chứa những phương thức thực sự chung
- **✅ Tránh interface không phù hợp**: Các lớp con không kế thừa phương thức không liên quan
- **✅ Code rõ ràng hơn**: Mỗi lớp chỉ có phương thức phù hợp với nhiệm vụ của nó
- **✅ Dễ bảo trì**: Thay đổi phương thức chỉ ảnh hưởng đến các lớp con thực sự cần nó
- **✅ Tuân thủ LSP**: Đảm bảo lớp con có thể thay thế lớp cha mà không vi phạm behavior

## **Điểm Cần Lưu ý**

- **Kiểm tra sử dụng**: Đảm bảo phương thức thực sự chỉ được dùng bởi một số lớp con
- **Tránh trùng lặp**: Nếu nhiều lớp con cần cùng phương thức, xem xét tách interface
- **Cân nhắc composition**: Đôi khi sử dụng composition thay vì kế thừa
- **Giữ tính nhất quán**: Đảm bảo phương thức có cùng signature trong các lớp con

## **Khi Nào Không Nên Sử Dụng**

- Khi phương thức được sử dụng bởi hầu hết các lớp con
- Khi phương thức thực sự thuộc về khái niệm của lớp cha
- Khi việc đẩy xuống tạo ra sự trùng lặp code
- Khi có thể sử dụng interface để giải quyết vấn đề tốt hơn

## **Kỹ Thuật Liên Quan**

- **Push Down Field**: Đẩy trường xuống lớp con
- **Pull Up Method**: Kéo phương thức lên lớp cha (ngược lại)
- **Extract Interface**: Trích xuất interface từ các phương thức chung
- **Replace Inheritance with Delegation**: Thay thế kế thừa bằng ủy quyền

## **Kết Luận**

Đẩy phương thức xuống là kỹ thuật quan trọng để duy trì hệ thống phân cấp kế thừa sạch sẽ và có ý nghĩa. Bằng cách di chuyển các phương thức cụ thể xuống các lớp con thực sự cần chúng, chúng ta đảm bảo rằng mỗi lớp chỉ chứa những behavior phù hợp với nhiệm vụ của nó. Điều này giúp code dễ hiểu, dễ bảo trì và tuân thủ tốt hơn các nguyên tắc thiết kế hướng đối tượng.

**Nguồn:** [refactoring.guru/push-down-method](https://refactoring.guru/push-down-method)