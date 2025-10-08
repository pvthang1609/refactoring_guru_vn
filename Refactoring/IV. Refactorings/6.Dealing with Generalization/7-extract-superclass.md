# **Trích Xuất Lớp Cha (Extract Superclass)**

## **Định Nghĩa**
Trích xuất lớp cha là kỹ thuật tái cấu trúc tạo một lớp cha mới từ các lớp có chung tính năng, giúp loại bỏ sự trùng lặp code và tạo ra hệ thống phân cấp kế thừa hợp lý.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có hai hoặc nhiều lớp có chung các trường, phương thức hoặc behavior
- Muốn loại bỏ sự trùng lặp code giữa các lớp liên quan
- Cần tạo ra một khái niệm chung (abstraction) cho các lớp cụ thể
- Muốn sử dụng tính đa hình trong hệ thống

### **Không nên sử dụng khi:**
- Các lớp không có đủ điểm chung đáng kể
- Sự khác biệt giữa các lớp lớn hơn sự tương đồng
- Có thể sử dụng composition thay vì inheritance

## **Các Bước Thực Hiện**

1. **Xác định các lớp có điểm chung**: Tìm các lớp có trường và phương thức giống nhau
2. **Tạo lớp cha mới**: Tạo lớp abstract hoặc lớp cha chứa các tính năng chung
3. **Làm cho các lớp kế thừa**: Cho các lớp hiện có kế thừa từ lớp cha mới
4. **Di chuyển tính năng chung**: Sử dụng Pull Up để di chuyển trường và phương thức chung lên lớp cha
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi trích xuất lớp cha:**
```java
class Employee {
    private String name;
    private String id;
    private double baseSalary;
    
    public Employee(String name, String id, double baseSalary) {
        this.name = name;
        this.id = id;
        this.baseSalary = baseSalary;
    }
    
    public double calculatePay() {
        return baseSalary;
    }
    
    public String getDetails() {
        return String.format("Employee: %s (ID: %s), Salary: $%.2f", name, id, baseSalary);
    }
    
    public void saveToDatabase() {
        // Logic lưu vào database
        System.out.println("Saving employee to database: " + name);
    }
    
    // Getters
    public String getName() { return name; }
    public String getId() { return id; }
    public double getBaseSalary() { return baseSalary; }
}

class Department {
    private String name;
    private String code;
    private double budget;
    
    public Department(String name, String code, double budget) {
        this.name = name;
        this.code = code;
        this.budget = budget;
    }
    
    public double calculateBudget() {
        return budget;
    }
    
    public String getDetails() {
        return String.format("Department: %s (Code: %s), Budget: $%.2f", name, code, budget);
    }
    
    public void saveToDatabase() {
        // Logic lưu vào database - TRÙNG LẶP với Employee
        System.out.println("Saving department to database: " + name);
    }
    
    // Getters
    public String getName() { return name; }
    public String getCode() { return code; }
    public double getBudget() { return budget; }
}

class Project {
    private String name;
    private String projectId;
    private double cost;
    
    public Project(String name, String projectId, double cost) {
        this.name = name;
        this.projectId = projectId;
        this.cost = cost;
    }
    
    public double calculateCost() {
        return cost;
    }
    
    public String getDetails() {
        return String.format("Project: %s (ID: %s), Cost: $%.2f", name, projectId, cost);
    }
    
    public void saveToDatabase() {
        // Logic lưu vào database - TRÙNG LẶP với Employee và Department
        System.out.println("Saving project to database: " + name);
    }
    
    // Getters
    public String getName() { return name; }
    public String getProjectId() { return projectId; }
    public double getCost() { return cost; }
}
```

### **✅ Sau khi trích xuất lớp cha:**
```java
// Lớp cha chung cho các entity có thể lưu vào database
abstract class DatabaseEntity {
    private String name;
    private String id;
    
    public DatabaseEntity(String name, String id) {
        this.name = name;
        this.id = id;
    }
    
    // Phương thức chung - không còn trùng lặp
    public void saveToDatabase() {
        // Logic lưu vào database chung
        System.out.println("Saving " + getEntityType() + " to database: " + name);
    }
    
    public String getDetails() {
        return String.format("%s: %s (ID: %s)", getEntityType(), name, id);
    }
    
    // Phương thức abstract - lớp con phải triển khai
    public abstract double calculateFinancials();
    public abstract String getEntityType();
    
    // Các getter chung
    public String getName() { return name; }
    public String getId() { return id; }
}

class Employee extends DatabaseEntity {
    private double baseSalary;
    
    public Employee(String name, String id, double baseSalary) {
        super(name, id);
        this.baseSalary = baseSalary;
    }
    
    @Override
    public double calculateFinancials() {
        return baseSalary;
    }
    
    @Override
    public String getEntityType() {
        return "Employee";
    }
    
    @Override
    public String getDetails() {
        return super.getDetails() + String.format(", Salary: $%.2f", baseSalary);
    }
    
    public double getBaseSalary() { return baseSalary; }
}

class Department extends DatabaseEntity {
    private double budget;
    
    public Department(String name, String code, double budget) {
        super(name, code);
        this.budget = budget;
    }
    
    @Override
    public double calculateFinancials() {
        return budget;
    }
    
    @Override
    public String getEntityType() {
        return "Department";
    }
    
    @Override
    public String getDetails() {
        return super.getDetails() + String.format(", Budget: $%.2f", budget);
    }
    
    public double getBudget() { return budget; }
}

class Project extends DatabaseEntity {
    private double cost;
    
    public Project(String name, String projectId, double cost) {
        super(name, projectId);
        this.cost = cost;
    }
    
    @Override
    public double calculateFinancials() {
        return cost;
    }
    
    @Override
    public String getEntityType() {
        return "Project";
    }
    
    @Override
    public String getDetails() {
        return super.getDetails() + String.format(", Cost: $%.2f", cost);
    }
    
    public double getCost() { return cost; }
}

// Client code có thể xử lý chung các entity
public class OrganizationManager {
    public void processEntities(List<DatabaseEntity> entities) {
        for (DatabaseEntity entity : entities) {
            System.out.println(entity.getDetails());
            System.out.println("Financials: $" + entity.calculateFinancials());
            entity.saveToDatabase();
            System.out.println("---");
        }
    }
    
    public double calculateTotalFinancials(List<DatabaseEntity> entities) {
        double total = 0;
        for (DatabaseEntity entity : entities) {
            total += entity.calculateFinancials();
        }
        return total;
    }
    
    public void saveAllToDatabase(List<DatabaseEntity> entities) {
        for (DatabaseEntity entity : entities) {
            entity.saveToDatabase();
        }
    }
}

// Sử dụng
List<DatabaseEntity> entities = Arrays.asList(
    new Employee("John Doe", "EMP001", 50000),
    new Department("Engineering", "DEPT001", 1000000),
    new Project("Website Redesign", "PROJ001", 50000)
);

OrganizationManager manager = new OrganizationManager();
manager.processEntities(entities);
double total = manager.calculateTotalFinancials(entities);
System.out.println("Total financials: $" + total);
```

## **Lợi Ích**

- **✅ Giảm trùng lặp**: Loại bỏ code giống nhau giữa các lớp
- **✅ Code nhất quán**: Các lớp có cùng interface và behavior chung
- **✅ Dễ bảo trì**: Thay đổi logic chung chỉ ở một nơi
- **✅ Tận dụng đa hình**: Có thể xử lý các đối tượng khác nhau thông qua interface chung
- **✅ Dễ mở rộng**: Thêm lớp mới dễ dàng bằng cách kế thừa từ lớp cha

## **Điểm Cần Lưu ý**

- **Chọn tên có ý nghĩa**: Tên lớp cha nên thể hiện khái niệm chung
- **Không ép kế thừa**: Chỉ tạo lớp cha khi có đủ điểm chung
- **Cân nhắc composition**: Đôi khi sử dụng composition tốt hơn inheritance
- **Sử dụng abstract method**: Cho các behavior khác nhau giữa các lớp con

## **Khi Nào Không Nên Sử Dụng**

- Khi các lớp không có đủ điểm chung đáng kể
- Khi sự khác biệt lớn hơn sự tương đồng
- Khi có thể sử dụng interface thay vì lớp abstract
- Khi hệ thống phân cấp làm code phức tạp hơn

## **Kỹ Thuật Liên Quan**

- **Extract Subclass**: Trích xuất lớp con từ lớp hiện có
- **Extract Interface**: Trích xuất interface từ các lớp có chung behavior
- **Pull Up Method**: Kéo phương thức lên lớp cha
- **Pull Up Field**: Kéo trường lên lớp cha
- **Replace Inheritance with Delegation**: Thay thế kế thừa bằng ủy quyền

## **Kết Luận**

Trích xuất lớp cha là kỹ thuật mạnh mẽ để tạo ra hệ thống phân cấp kế thừa hợp lý và loại bỏ sự trùng lặp code. Bằng cách xác định các điểm chung giữa các lớp và di chuyển chúng lên lớp cha, chúng ta tạo ra code dễ bảo trì, dễ mở rộng và tận dụng được tính đa hình. Tuy nhiên, cần đảm bảo rằng các lớp thực sự có đủ điểm chung và việc tạo lớp cha không làm hệ thống trở nên phức tạp không cần thiết.

**Nguồn:** [refactoring.guru/extract-superclass](https://refactoring.guru/extract-superclass)