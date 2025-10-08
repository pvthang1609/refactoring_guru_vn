# **Thay Đổi Liên Kết Một Chiều Thành Hai Chiều (Change Unidirectional Association to Bidirectional)**

## **Định Nghĩa**
Thay đổi liên kết một chiều thành hai chiều là kỹ thuật tái cấu trúc thêm liên kết ngược lại từ lớp đích về lớp nguồn khi cần truy cập theo cả hai hướng.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Cần truy cập từ lớp đích ngược về lớp nguồn
- Liên kết một chiều không đủ để hỗ trợ các use case mới
- Cần duyệt qua mối quan hệ theo cả hai hướng

### **Không nên sử dụng khi:**
- Liên kết một chiều đã đáp ứng đủ nhu cầu
- Muốn tránh sự phức tạp của việc duy trì tính nhất quán hai chiều

## **Các Bước Thực Hiện**

1. **Thêm trường liên kết ngược**: Thêm trường trong lớp đích để tham chiếu về lớp nguồn
2. **Quyết định lớp quản lý**: Xác định lớp nào sẽ quản lý mối quan hệ
3. **Tạo phương thức update**: Tạo phương thức trong lớp quản lý để cập nhật cả hai phía
4. **Cập nhật constructor/setter**: Đảm bảo cập nhật cả hai phía khi thiết lập quan hệ

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay đổi (Một chiều):**
```java
class Customer {
    private String name;
    // Chỉ có liên kết một chiều từ Order đến Customer
}

class Order {
    private Customer customer;
    
    public Order(Customer customer) {
        this.customer = customer;
    }
    
    public Customer getCustomer() {
        return customer;
    }
    
    public void setCustomer(Customer customer) {
        this.customer = customer;
    }
}

// Chỉ có thể truy cập từ Order đến Customer
Order order = new Order(customer);
Customer cust = order.getCustomer(); // OK

// Không thể truy cập từ Customer đến Order
// customer.getOrders() // KHÔNG THỂ!
```

### **✅ Sau khi thay đổi (Hai chiều):**
```java
class Customer {
    private String name;
    private Set<Order> orders = new HashSet<>(); // Liên kết ngược
    
    public Customer(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    // Phương thức để truy cập orders
    public Set<Order> getOrders() {
        return Collections.unmodifiableSet(orders);
    }
    
    // Phương thức để thêm order, đồng thời cập nhật cả hai phía
    void addOrder(Order order) {
        if (order == null) return;
        if (orders.contains(order)) return;
        
        orders.add(order);
        order.setCustomer(this); // Cập nhật phía Order
    }
    
    // Phương thức để xóa order
    void removeOrder(Order order) {
        if (order == null) return;
        if (!orders.contains(order)) return;
        
        orders.remove(order);
        order.setCustomer(null); // Cập nhật phía Order
    }
}

class Order {
    private Customer customer;
    
    public Order(Customer customer) {
        setCustomer(customer); // Sử dụng setter để cập nhật cả hai phía
    }
    
    public Customer getCustomer() {
        return customer;
    }
    
    public void setCustomer(Customer newCustomer) {
        // Xóa khỏi customer cũ (nếu có)
        if (this.customer != null) {
            this.customer.removeOrder(this);
        }
        
        // Thiết lập customer mới
        this.customer = newCustomer;
        
        // Thêm vào customer mới (nếu có)
        if (this.customer != null) {
            this.customer.addOrder(this);
        }
    }
    
    public double getAmount() {
        // logic tính toán
        return 0.0;
    }
}

// Sử dụng - có thể truy cập cả hai hướng
Customer customer = new Customer("John Doe");
Order order1 = new Order(customer);
Order order2 = new Order(customer);

// Truy cập từ Order đến Customer
Customer cust = order1.getCustomer(); // OK

// Truy cập từ Customer đến Order
Set<Order> orders = customer.getOrders(); // CŨNG OK!
for (Order order : orders) {
    System.out.println("Order amount: " + order.getAmount());
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với nhiều mối quan hệ:**
```java
class Department {
    private String name;
    private List<Employee> employees = new ArrayList<>();
    
    public Department(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    public List<Employee> getEmployees() {
        return Collections.unmodifiableList(employees);
    }
    
    public void addEmployee(Employee employee) {
        if (employee == null) return;
        if (employees.contains(employee)) return;
        
        employees.add(employee);
        employee.setDepartment(this);
    }
    
    public void removeEmployee(Employee employee) {
        if (employee == null) return;
        if (!employees.contains(employee)) return;
        
        employees.remove(employee);
        employee.setDepartment(null);
    }
    
    public double getTotalSalary() {
        return employees.stream()
            .mapToDouble(Employee::getSalary)
            .sum();
    }
}

class Employee {
    private String name;
    private double salary;
    private Department department;
    
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }
    
    public String getName() {
        return name;
    }
    
    public double getSalary() {
        return salary;
    }
    
    public void setSalary(double salary) {
        this.salary = salary;
    }
    
    public Department getDepartment() {
        return department;
    }
    
    public void setDepartment(Department newDepartment) {
        // Xóa khỏi department cũ
        if (this.department != null) {
            this.department.removeEmployee(this);
        }
        
        // Thiết lập department mới
        this.department = newDepartment;
        
        // Thêm vào department mới
        if (this.department != null) {
            this.department.addEmployee(this);
        }
    }
}

// Sử dụng
Department engineering = new Department("Engineering");
Department marketing = new Department("Marketing");

Employee emp1 = new Employee("Alice", 50000);
Employee emp2 = new Employee("Bob", 60000);

// Thiết lập quan hệ - tự động cập nhật cả hai phía
emp1.setDepartment(engineering);
emp2.setDepartment(engineering);

// Có thể truy cập cả hai hướng
System.out.println("Employees in Engineering: " + engineering.getEmployees().size());
System.out.println("Alice's department: " + emp1.getDepartment().getName());

// Chuyển department - tự động cập nhật
emp2.setDepartment(marketing);

System.out.println("Engineering employees: " + engineering.getEmployees().size()); // 1
System.out.println("Marketing employees: " + marketing.getEmployees().size()); // 1
```

## **Lợi Ích**

- **✅ Linh hoạt truy cập**: Có thể duyệt quan hệ theo cả hai hướng
- **✅ Đồng bộ tự động**: Thay đổi một phía tự động cập nhật phía kia
- **✅ Tính nhất quán**: Đảm bảo tính toàn vẹn của quan hệ
- **✅ Dễ dàng query**: Hỗ trợ các truy vấn phức tạp hơn

## **Điểm Cần Lưu Ý**

- **Độ phức tạp**: Tăng độ phức tạp do phải quản lý cả hai chiều
- **Performance**: Có thể ảnh hưởng hiệu năng với collections lớn
- **Memory overhead**: Tăng sử dụng bộ nhớ do lưu trữ thêm tham chiếu
- **Vòng lặp vô hạn**: Cẩn thận với serialization và toString() để tránh vòng lặp

## **Quyết Định Lớp Quản Lý**

- **Quan hệ một-nhiều**: Lớp "một" thường quản lý quan hệ
- **Quan hệ nhiều-nhiều**: Có thể chọn một lớp làm chủ, hoặc tạo lớp quản lý riêng
- **Phụ thuộc use case**: Lớp nào được sử dụng nhiều hơn nên quản lý

## **Kỹ Thuật Liên Quan**

- **Thay Đổi Liên Kết Hai Chiều Thành Một Chiều**: Ngược lại với kỹ thuật này
- **Encapsulate Collection**: Để quản lý collections an toàn hơn
- **Observer Pattern**: Để thông báo thay đổi giữa các đối tượng

## **Kết Luận**

Thay đổi liên kết một chiều thành hai chiều là kỹ thuật hữu ích khi cần truy cập mối quan hệ theo cả hai hướng. Tuy nhiên, cần cân nhắc kỹ giữa lợi ích và độ phức tạp, đồng thời đảm bảo tính nhất quán được duy trì thông qua các phương thức quản lý quan hệ tập trung.

**Nguồn:** [refactoring.guru/change-unidirectional-association-to-bidirectional](https://refactoring.guru/change-unidirectional-association-to-bidirectional)