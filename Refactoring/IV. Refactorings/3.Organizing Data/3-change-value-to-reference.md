# **Thay Đổi Giá Trị Thành Tham Chiếu (Change Value to Reference)**

## **Định Nghĩa**
Thay đổi giá trị thành tham chiếu là kỹ thuật tái cấu trúc chuyển đổi một đối tượng giá trị (value object) thành đối tượng tham chiếu (reference object) khi cần chia sẻ một instance duy nhất cho nhiều đối tượng khác.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có nhiều đối tượng chứa cùng dữ liệu mà lẽ ra nên chia sẻ một instance duy nhất
- Cần đảm bảo tính nhất quán của dữ liệu trên toàn hệ thống
- Muốn tiết kiệm bộ nhớ bằng cách chia sẻ các đối tượng bất biến
- Dữ liệu đại diện cho một thực thể có tính duy nhất trong hệ thống

### **Dấu hiệu nhận biết:**
- Có nhiều instance của cùng một đối tượng với cùng dữ liệu
- Cần cập nhật dữ liệu và thay đổi phải được phản ánh ở mọi nơi
- Đối tượng đại diện cho một thực thể có ID hoặc khóa duy nhất

## **Các Bước Thực Hiện**

1. **Tạo repository**: Tạo nơi lưu trữ và quản lý các instance của đối tượng
2. **Xác định khóa**: Chọn thuộc tính dùng làm khóa để xác định đối tượng
3. **Thay đổi constructor**: Sửa constructor để sử dụng repository thay vì tạo instance mới
4. **Cập nhật factory method**: Tạo phương thức factory để lấy hoặc tạo instance
5. **Cập nhật client code**: Thay thế các lời gọi constructor bằng factory method

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay đổi (Value Object):**
```java
class Customer {
    private final String id;
    private final String name;
    private final String email;
    
    // Mỗi lần gọi constructor tạo instance mới
    public Customer(String id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }
    
    public String getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
}

class Order {
    private final String orderId;
    private final Customer customer; // Chứa bản sao của customer
    
    public Order(String orderId, Customer customer) {
        this.orderId = orderId;
        this.customer = customer;
    }
    
    public Customer getCustomer() {
        return customer;
    }
}

// Sử dụng - mỗi order có customer instance riêng
Customer customer1 = new Customer("C001", "John Doe", "john@email.com");
Customer customer2 = new Customer("C001", "John Doe", "john@email.com"); // Instance khác!

Order order1 = new Order("O001", customer1);
Order order2 = new Order("O002", customer2);

System.out.println(customer1 == customer2); // false - khác instance
System.out.println(customer1.equals(customer2)); // true - cùng dữ liệu
```

### **✅ Sau khi thay đổi (Reference Object):**
```java
class Customer {
    private final String id;
    private final String name;
    private final String email;
    
    // Constructor private - không cho phép tạo trực tiếp
    private Customer(String id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }
    
    public String getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
    
    // Repository quản lý các instance
    private static final Map<String, Customer> customers = new HashMap<>();
    
    // Factory method - luôn trả về cùng instance cho cùng ID
    public static Customer getCustomer(String id, String name, String email) {
        return customers.computeIfAbsent(id, k -> new Customer(id, name, email));
    }
    
    // Method để lấy customer đã tồn tại
    public static Customer getExistingCustomer(String id) {
        Customer customer = customers.get(id);
        if (customer == null) {
            throw new IllegalArgumentException("Customer không tồn tại: " + id);
        }
        return customer;
    }
    
    // Có thể thêm method để cập nhật thông tin
    public static void updateCustomer(String id, String name, String email) {
        Customer customer = customers.get(id);
        if (customer != null) {
            // Với immutable object, cần tạo mới hoặc sử dụng cách khác
            customers.put(id, new Customer(id, name, email));
        }
    }
}

class Order {
    private final String orderId;
    private final Customer customer; // Tham chiếu đến instance chia sẻ
    
    public Order(String orderId, Customer customer) {
        this.orderId = orderId;
        this.customer = customer;
    }
    
    public Customer getCustomer() {
        return customer;
    }
}

// Sử dụng - luôn dùng cùng instance cho cùng customer
Customer customer1 = Customer.getCustomer("C001", "John Doe", "john@email.com");
Customer customer2 = Customer.getCustomer("C001", "John Doe", "john@email.com"); // Cùng instance!

Order order1 = new Order("O001", customer1);
Order order2 = new Order("O002", customer2);

System.out.println(customer1 == customer2); // true - cùng instance
System.out.println(customer1.equals(customer2)); // true - cùng dữ liệu
```

## **Ví Dụ Phức Tạp Hơn Với Repository**

### **✅ Triển khai repository chuyên biệt:**
```java
class CustomerRepository {
    private static final CustomerRepository instance = new CustomerRepository();
    private final Map<String, Customer> customers = new HashMap<>();
    
    private CustomerRepository() {
        // Private constructor cho singleton
    }
    
    public static CustomerRepository getInstance() {
        return instance;
    }
    
    public Customer findById(String id) {
        return customers.get(id);
    }
    
    public Customer create(String id, String name, String email) {
        if (customers.containsKey(id)) {
            throw new IllegalArgumentException("Customer đã tồn tại: " + id);
        }
        Customer customer = new Customer(id, name, email);
        customers.put(id, customer);
        return customer;
    }
    
    public Customer findOrCreate(String id, String name, String email) {
        return customers.computeIfAbsent(id, k -> new Customer(id, name, email));
    }
    
    public void update(String id, String name, String email) {
        if (!customers.containsKey(id)) {
            throw new IllegalArgumentException("Customer không tồn tại: " + id);
        }
        customers.put(id, new Customer(id, name, email));
    }
    
    public Collection<Customer> findAll() {
        return Collections.unmodifiableCollection(customers.values());
    }
}

class Customer {
    private final String id;
    private final String name;
    private final String email;
    
    // Package-private constructor, chỉ repository được tạo
    Customer(String id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }
    
    // Getter methods...
}

// Sử dụng repository
CustomerRepository repo = CustomerRepository.getInstance();
Customer customer = repo.findOrCreate("C001", "John Doe", "john@email.com");
Order order = new Order("O001", customer);
```

## **Lợi Ích**

- **✅ Tính nhất quán**: Mọi thay đổi được phản ánh trên toàn hệ thống
- **✅ Tiết kiệm bộ nhớ**: Chia sẻ instance thay vì tạo nhiều bản sao
- **✅ Quản lý tập trung**: Dễ dàng theo dõi và quản lý các instance
- **✅ Kiểm soát vòng đời**: Có thể thêm logic khi tạo hoặc hủy instance
- **✅ Dễ cache**: Có thể triển khai caching hiệu quả

## **Điểm Cần Lưu Ý**

- **Thread safety**: Đảm bảo repository an toàn khi sử dụng trong môi trường đa luồng
- **Memory management**: Cần có chiến lược clear cache khi cần
- **Không dùng cho mutable objects**: Chỉ áp dụng cho immutable hoặc carefully managed mutable objects
- **Phức tạp hóa code**: Đánh đổi giữa lợi ích và độ phức tạp

## **Khi Nào Không Nên Sử Dụng**

- Khi đối tượng có trạng thái thay đổi thường xuyên và không muốn chia sẻ thay đổi
- Khi performance không phải vấn đề và code đơn giản quan trọng hơn
- Khi đối tượng nhẹ và việc tạo nhiều instance không ảnh hưởng

## **Kỹ Thuật Liên Quan**

- **Thay đổi Tham chiếu thành Giá trị (Change Reference to Value)**: Ngược lại với kỹ thuật này
- **Singleton Pattern**: Để quản lý repository
- **Factory Pattern**: Để tạo và quản lý instance
- **Flyweight Pattern**: Để chia sẻ các đối tượng nhẹ

## **Kết Luận**

Thay đổi giá trị thành tham chiếu là kỹ thuật mạnh mẽ để quản lý các đối tượng có tính duy nhất trong hệ thống. Bằng cách đảm bảo mọi nơi sử dụng cùng một instance, bạn có thể duy trì tính nhất quán của dữ liệu và tối ưu hiệu năng. Tuy nhiên, cần cân nhắc kỹ giữa lợi ích và độ phức tạp khi áp dụng kỹ thuật này.