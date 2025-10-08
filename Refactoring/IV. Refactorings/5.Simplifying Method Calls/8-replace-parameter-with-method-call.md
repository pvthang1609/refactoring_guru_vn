# **Thay Thế Tham Số Bằng Lời Gọi Phương Thức (Replace Parameter with Method Call)**

## **Định Nghĩa**
Thay thế tham số bằng lời gọi phương thức là kỹ thuật tái cấu trúc thay thế việc truyền tham số bằng cách gọi một phương thức để lấy giá trị đó bên trong phương thức được gọi, giúp giảm số lượng tham số và đơn giản hóa lời gọi.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Phương thức được gọi có thể tự lấy được giá trị tham số từ các nguồn có sẵn
- Giá trị tham số có thể được tính toán từ dữ liệu đã có trong phương thức được gọi
- Muốn giảm số lượng tham số và đơn giản hóa interface
- Giá trị tham số luôn giống nhau trong hầu hết các lời gọi

### **Không nên sử dụng khi:**
- Giá trị tham số thay đổi thường xuyên và cần được truyền từ bên ngoài
- Phương thức được gọi không có quyền truy cập vào nguồn dữ liệu cần thiết
- Việc thay thế tạo ra sự phụ thuộc không mong muốn giữa các lớp

## **Các Bước Thực Hiện**

1. **Xác định tham số có thể thay thế**: Tìm tham số có thể được tính toán từ bên trong phương thức
2. **Thay thế bằng lời gọi phương thức**: Thay thế việc sử dụng tham số bằng lời gọi phương thức để lấy giá trị
3. **Xóa tham số**: Xóa tham số khỏi danh sách tham số của phương thức
4. **Cập nhật lời gọi**: Cập nhật tất cả các lời gọi để không truyền tham số đó nữa
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class Order {
    private List<OrderItem> items;
    private Customer customer;
    
    // Phương thức nhận tham số có thể tính toán được
    public double calculateTotal(double basePrice, double taxRate, double discountRate) {
        double subtotal = 0;
        for (OrderItem item : items) {
            subtotal += item.getPrice() * item.getQuantity();
        }
        
        double tax = subtotal * taxRate;
        double discount = subtotal * discountRate;
        return subtotal + tax - discount;
    }
    
    // Phương thức khác luôn truyền cùng một giá trị
    public void generateInvoice(Date currentDate, boolean includeDetails) {
        // Luôn sử dụng includeDetails = true
        if (includeDetails) {
            generateDetailedInvoice(currentDate);
        } else {
            generateSimpleInvoice(currentDate);
        }
    }
}

// Client code - phải truyền nhiều tham số có thể tính toán được
Order order = new Order();
double basePrice = order.calculateBasePrice(); // Có thể tính bên trong
double taxRate = order.getCustomer().getTaxRate(); // Có thể lấy được
double discount = order.getCustomer().getDiscountRate(); // Có thể lấy được

double total = order.calculateTotal(basePrice, taxRate, discount);

// Luôn phải truyền true cho includeDetails
order.generateInvoice(new Date(), true);
```

### **✅ Sau khi thay thế:**
```java
class Order {
    private List<OrderItem> items;
    private Customer customer;
    
    // Phương thức tự lấy các giá trị cần thiết
    public double calculateTotal() {
        double subtotal = calculateSubtotal();
        double taxRate = getTaxRate();
        double discountRate = getDiscountRate();
        
        double tax = subtotal * taxRate;
        double discount = subtotal * discountRate;
        return subtotal + tax - discount;
    }
    
    private double calculateSubtotal() {
        double subtotal = 0;
        for (OrderItem item : items) {
            subtotal += item.getPrice() * item.getQuantity();
        }
        return subtotal;
    }
    
    private double getTaxRate() {
        return customer.getTaxRate();
    }
    
    private double getDiscountRate() {
        return customer.getDiscountRate();
    }
    
    // Phương thức không còn tham số không cần thiết
    public void generateInvoice() {
        // Luôn generate detailed invoice - không cần tham số điều khiển
        generateDetailedInvoice(new Date());
    }
    
    // Cung cấp phương thức thay thế nếu cần linh hoạt
    public void generateSimpleInvoice() {
        generateSimpleInvoice(new Date());
    }
    
    private void generateDetailedInvoice(Date date) {
        // Logic tạo invoice chi tiết
    }
    
    private void generateSimpleInvoice(Date date) {
        // Logic tạo invoice đơn giản
    }
}

// Client code - đơn giản hơn nhiều
Order order = new Order();
double total = order.calculateTotal(); // Không cần truyền tham số
order.generateInvoice(); // Tự động tạo invoice chi tiết
```

## **Lợi Ích**

- **✅ Giảm số lượng tham số**: Phương thức có ít tham số hơn
- **✅ Đơn giản hóa lời gọi**: Client code dễ sử dụng hơn
- **✅ Giảm trùng lặp**: Tránh việc tính toán cùng giá trị ở nhiều nơi
- **✅ Tăng tính đóng gói**: Dữ liệu và hành vi được gói gọn hơn
- **✅ Code rõ ràng hơn**: Phương thức tự chủ hơn trong việc lấy dữ liệu cần thiết

## **Điểm Cần Lưu ý**

- **Không che giấu phụ thuộc**: Đảm bảo không tạo ra phụ thuộc ẩn giữa các lớp
- **Cân nhắc tính linh hoạt**: Đôi khi cần giữ lại tham số để phương thức linh hoạt hơn
- **Xem xét hiệu suất**: Nếu việc tính toán giá trị tốn kém, có thể cần cache kết quả
- **Bảo toàn khả năng test**: Đảm bảo phương thức vẫn dễ test sau khi thay đổi

## **Khi Nào Không Nên Sử Dụng**

- Khi giá trị tham số thay đổi thường xuyên và cần linh hoạt
- Khi phương thức được gọi không nên biết về cách lấy giá trị đó
- Khi việc thay thế tạo ra phụ thuộc chu trình (circular dependency)
- Khi giá trị cần được kiểm soát chặt chẽ từ bên ngoài

## **Kỹ Thuật Liên Quan**

- **Replace Method with Method Object**: Thay thế phương thức bằng đối tượng phương thức
- **Introduce Parameter Object**: Nhóm các tham số thành object
- **Preserve Whole Object**: Truyền toàn bộ đối tượng thay vì từng giá trị
- **Remove Parameter**: Xóa tham số không cần thiết

## **Kết Luận**

Thay thế tham số bằng lời gọi phương thức là kỹ thuật hiệu quả để đơn giản hóa interface của phương thức và giảm sự phức tạp cho client code. Bằng cách để phương thức tự lấy các giá trị cần thiết thay vì yêu cầu client cung cấp, code trở nên gọn gàng và dễ sử dụng hơn. Tuy nhiên, cần cân nhắc kỹ để không làm mất đi tính linh hoạt của phương thức hoặc tạo ra các phụ thuộc không mong muốn.

**Nguồn:** [refactoring.guru/replace-parameter-with-method-call](https://refactoring.guru/replace-parameter-with-method-call)