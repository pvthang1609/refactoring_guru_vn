# **Đẩy Trường Xuống (Push Down Field)**

## **Định Nghĩa**
Đẩy trường xuống là kỹ thuật tái cấu trúc di chuyển một trường từ lớp cha xuống các lớp con, khi trường đó chỉ được sử dụng bởi một số lớp con cụ thể và không thực sự thuộc về lớp cha.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Trường trong lớp cha chỉ được sử dụng bởi một số lớp con
- Trường không phù hợp với tất cả các lớp con
- Muốn giữ cho lớp cha gọn gàng và chỉ chứa những trường thực sự chung
- Trường đại diện cho dữ liệu cụ thể của một số lớp con

### **Không nên sử dụng khi:**
- Trường được sử dụng bởi hầu hết hoặc tất cả các lớp con
- Trường thực sự thuộc về khái niệm của lớp cha
- Việc đẩy xuống tạo ra sự trùng lặp code giữa các lớp con

## **Các Bước Thực Hiện**

1. **Xác định trường cần đẩy xuống**: Tìm trường trong lớp cha chỉ liên quan đến một số lớp con
2. **Tạo trường trong lớp con**: Khai báo trường trong các lớp con cần nó
3. **Xóa trường khỏi lớp cha**: Xóa trường khỏi lớp cha sau khi đã di chuyển
4. **Điều chỉnh constructor và phương thức**: Cập nhật constructor và các phương thức sử dụng trường
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi đẩy trường xuống:**
```java
abstract class Employee {
    protected String name;
    protected double baseSalary;
    // Trường chỉ phù hợp với một số lớp con - KHÔNG NÊN ở lớp cha
    protected String projectCode; // Chỉ Engineer và Manager có dự án
    protected double commissionRate; // Chỉ Salesperson có tỷ lệ hoa hồng

    public Employee(String name, double baseSalary, String projectCode, double commissionRate) {
        this.name = name;
        this.baseSalary = baseSalary;
        this.projectCode = projectCode;
        this.commissionRate = commissionRate;
    }

    // Các phương thức sử dụng các trường không phù hợp
    public String getProjectCode() {
        return projectCode;
    }

    public double getCommissionRate() {
        return commissionRate;
    }
}

class Engineer extends Employee {
    public Engineer(String name, double baseSalary, String projectCode) {
        super(name, baseSalary, projectCode, 0); // Engineer không có commissionRate
    }

    public void workOnProject() {
        System.out.println("Engineer " + name + " working on project " + projectCode);
    }
}

class Salesperson extends Employee {
    public Salesperson(String name, double baseSalary, double commissionRate) {
        super(name, baseSalary, null, commissionRate); // Salesperson không có projectCode
    }

    public void makeSale() {
        System.out.println("Salesperson " + name + " makes sale with commission " + commissionRate);
    }
}

class Manager extends Employee {
    public Manager(String name, double baseSalary, String projectCode) {
        super(name, baseSalary, projectCode, 0); // Manager không có commissionRate
    }

    public void manageProject() {
        System.out.println("Manager " + name + " managing project " + projectCode);
    }
}

class Intern extends Employee {
    public Intern(String name, double baseSalary) {
        super(name, baseSalary, null, 0); // Intern không có cả projectCode và commissionRate
    }

    // Intern không sử dụng projectCode và commissionRate, nhưng vẫn kế thừa
}
```

### **✅ Sau khi đẩy trường xuống:**
```java
abstract class Employee {
    protected String name;
    protected double baseSalary;

    public Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }

    // Chỉ giữ lại các trường thực sự chung
    public String getName() { return name; }
    public double getBaseSalary() { return baseSalary; }
}

class Engineer extends Employee {
    private String projectCode; // Trường được đẩy xuống - chỉ Engineer có

    public Engineer(String name, double baseSalary, String projectCode) {
        super(name, baseSalary);
        this.projectCode = projectCode;
    }

    public String getProjectCode() { return projectCode; }

    public void workOnProject() {
        System.out.println("Engineer " + name + " working on project " + projectCode);
    }
}

class Salesperson extends Employee {
    private double commissionRate; // Trường được đẩy xuống - chỉ Salesperson có

    public Salesperson(String name, double baseSalary, double commissionRate) {
        super(name, baseSalary);
        this.commissionRate = commissionRate;
    }

    public double getCommissionRate() { return commissionRate; }

    public void makeSale() {
        System.out.println("Salesperson " + name + " makes sale with commission " + commissionRate);
    }
}

class Manager extends Employee {
    private String projectCode; // Trường được đẩy xuống - chỉ Manager có

    public Manager(String name, double baseSalary, String projectCode) {
        super(name, baseSalary);
        this.projectCode = projectCode;
    }

    public String getProjectCode() { return projectCode; }

    public void manageProject() {
        System.out.println("Manager " + name + " managing project " + projectCode);
    }
}

class Intern extends Employee {
    public Intern(String name, double baseSalary) {
        super(name, baseSalary);
    }

    // Intern không còn kế thừa các trường không cần thiết
}
```

## **Lợi Ích**

- **✅ Lớp cha gọn gàng**: Chỉ chứa những trường thực sự chung
- **✅ Tránh dữ liệu thừa**: Các lớp con không kế thừa trường không liên quan
- **✅ Code rõ ràng hơn**: Mỗi lớp chỉ có trường phù hợp với nhiệm vụ của nó
- **✅ Dễ bảo trì**: Thay đổi trường chỉ ảnh hưởng đến các lớp con thực sự cần nó
- **✅ Tuân thủ nguyên tắc đơn trách nhiệm**: Mỗi lớp chỉ chứa dữ liệu cần thiết

## **Điểm Cần Lưu ý**

- **Kiểm tra sử dụng**: Đảm bảo trường thực sự chỉ được dùng bởi một số lớp con
- **Cập nhật constructor**: Nhớ cập nhật tất cả constructor của lớp con
- **Tránh trùng lặp**: Nếu nhiều lớp con cần cùng trường, xem xét lại hệ thống phân cấp
- **Xem xét tính nhất quán**: Đảm bảo trường có cùng ý nghĩa trong các lớp con

## **Khi Nào Không Nên Sử Dụng**

- Khi trường được sử dụng bởi hầu hết các lớp con
- Khi trường thực sự thuộc về khái niệm của lớp cha
- Khi việc đẩy xuống tạo ra sự trùng lặp code
- Khi có thể tái cấu trúc hệ thống phân cấp để phản ánh đúng hơn

## **Kỹ Thuật Liên Quan**

- **Push Down Method**: Đẩy phương thức xuống lớp con
- **Pull Up Field**: Kéo trường lên lớp cha (ngược lại)
- **Extract Subclass**: Trích xuất lớp con cho các trường và phương thức cụ thể
- **Replace Inheritance with Delegation**: Thay thế kế thừa bằng ủy quyền

## **Kết Luận**

Đẩy trường xuống là kỹ thuật quan trọng để duy trì hệ thống phân cấp kế thừa sạch sẽ và có ý nghĩa. Bằng cách di chuyển các trường cụ thể xuống các lớp con thực sự cần chúng, chúng ta đảm bảo rằng mỗi lớp chỉ chứa những dữ liệu phù hợp với nhiệm vụ của nó. Điều này giúp code dễ hiểu, dễ bảo trì và tuân thủ tốt hơn các nguyên tắc thiết kế hướng đối tượng.

**Nguồn:** [refactoring.guru/push-down-field](https://refactoring.guru/push-down-field)