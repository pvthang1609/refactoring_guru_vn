# **Thay Đổi Liên Kết Hai Chiều Thành Một Chiều (Change Bidirectional Association to Unidirectional)**

## **Định Nghĩa**
Thay đổi liên kết hai chiều thành một chiều là kỹ thuật tái cấu trúc loại bỏ liên kết ngược từ lớp đích về lớp nguồn khi liên kết hai chiều không còn cần thiết.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Liên kết hai chiều không còn được sử dụng
- Muốn giảm sự phụ thuộc giữa các lớp
- Liên kết hai chiều gây ra độ phức tạp không cần thiết
- Chỉ cần truy cập một chiều là đủ

### **Không nên sử dụng khi:**
- Liên kết hai chiều vẫn còn được sử dụng tích cực
- Việc loại bỏ sẽ làm code phức tạp hơn

## **Các Bước Thực Hiện**

1. **Xác định lớp quản lý**: Tìm lớp quản lý mối quan hệ hai chiều
2. **Loại bỏ liên kết ngược**: Xóa trường và phương thức liên quan đến liên kết ngược
3. **Cập nhật logic quản lý**: Sửa code quản lý quan hệ để chỉ còn một chiều
4. **Kiểm tra phụ thuộc**: Đảm bảo không có code nào còn sử dụng liên kết ngược

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay đổi (Hai chiều):**
```java
class Customer {
    private String name;
    private Set<Order> orders = new HashSet<>();
    
    public Customer(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    public Set<Order> getOrders() {
        return Collections.unmodifiableSet(orders);
    }
    
    void addOrder(Order order) {
        if (order == null) return;
        if (orders.contains(order)) return;
        
        orders.add(order);
        order.setCustomer(this);
    }
    
    void removeOrder(Order order) {
        if (order == null) return;
        if (!orders.contains(order)) return;
        
        orders.remove(order);
        order.setCustomer(null);
    }
}

class Order {
    private Customer customer;
    
    public Order(Customer customer) {
        setCustomer(customer);
    }
    
    public Customer getCustomer() {
        return customer;
    }
    
    public void setCustomer(Customer newCustomer) {
        if (this.customer != null) {
            this.customer.removeOrder(this);
        }
        
        this.customer = newCustomer;
        
        if (this.customer != null) {
            this.customer.addOrder(this);
        }
    }
}
```

### **✅ Sau khi thay đổi (Một chiều):**
```java
class Customer {
    private String name;
    
    public Customer(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    // Không còn quản lý danh sách orders
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
    
    // Đơn giản, chỉ lưu trữ tham chiếu một chiều
}

// Sử dụng - chỉ truy cập từ Order đến Customer
Order order = new Order(customer);
Customer cust = order.getCustomer(); // Vẫn hoạt động

// Không thể truy cập từ Customer đến Order nữa
// customer.getOrders() // ĐÃ BỊ LOẠI BỎ
```

## **Ví Dụ Thực Tế**

### **✅ Khi không cần truy vấn ngược:**
```java
class Product {
    private String name;
    private double price;
    // Không còn tham chiếu đến OrderLine
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    public String getName() { return name; }
    public double getPrice() { return price; }
}

class OrderLine {
    private Product product;
    private int quantity;
    
    public OrderLine(Product product, int quantity) {
        this.product = product;
        this.quantity = quantity;
    }
    
    public Product getProduct() {
        return product;
    }
    
    public int getQuantity() {
        return quantity;
    }
    
    public double getSubtotal() {
        return product.getPrice() * quantity;
    }
}

class Order {
    private List<OrderLine> orderLines = new ArrayList<>();
    
    public void addOrderLine(OrderLine orderLine) {
        orderLines.add(orderLine);
    }
    
    public double getTotal() {
        return orderLines.stream()
            .mapToDouble(OrderLine::getSubtotal)
            .sum();
    }
    
    // Có thể truy vấn sản phẩm thông qua OrderLine
    public boolean containsProduct(Product product) {
        return orderLines.stream()
            .anyMatch(line -> line.getProduct().equals(product));
    }
}
```

## **Lợi Ích**

- **✅ Giảm độ phức tạp**: Loại bỏ code quản lý quan hệ hai chiều
- **✅ Giảm coupling**: Các lớp ít phụ thuộc vào nhau hơn
- **✅ Cải thiện performance**: Giảm overhead của việc duy trì đồng bộ
- **✅ Đơn giản hóa testing**: Dễ test hơn khi có ít phụ thuộc

## **Điểm Cần Lưu Ý**

- **Phân tích kỹ use case**: Đảm bảo liên kết ngược thực sự không cần thiết
- **Cập nhật tất cả client code**: Đảm bảo không có code nào đang sử dụng liên kết ngược
- **Cân nhắc truy vấn thay thế**: Đôi khi có thể dùng truy vấn để thay thế liên kết ngược

## **Khi Nào Không Nên Sử Dụng**

- Khi liên kết ngược vẫn thường xuyên được sử dụng
- Khi việc loại bỏ sẽ yêu cầu các truy vấn phức tạp để thay thế
- Khi hiệu năng truy vấn thay thế kém hơn

## **Phương Án Thay Thế**

```java
// Thay vì liên kết hai chiều, có thể dùng truy vấn
class OrderRepository {
    public List<Order> findOrdersByCustomer(Customer customer) {
        // Truy vấn database để lấy orders của customer
        return database.query("SELECT * FROM orders WHERE customer_id = ?", 
                             customer.getId());
    }
}
```

## **Kỹ Thuật Liên Quan**

- **Thay Đổi Liên Kết Một Chiều Thành Hai Chiều**: Ngược lại với kỹ thuật này
- **Encapsulate Collection**: Để quản lý collections an toàn
- **Introduce Foreign Method**: Khi cần truy cập dữ liệu theo cách khác

## **Kết Luận**

Thay đổi liên kết hai chiều thành một chiều giúp đơn giản hóa thiết kế khi liên kết ngược không còn cần thiết. Kỹ thuật này giảm độ phức tạp, giảm coupling và làm code dễ bảo trì hơn. Tuy nhiên, cần phân tích kỹ để đảm bảo liên kết ngược thực sự không còn được sử dụng trước khi loại bỏ.