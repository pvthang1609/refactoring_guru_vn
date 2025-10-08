# **Ẩn Phương Thức (Hide Method)**

## **Định Nghĩa**
Ẩn phương thức là kỹ thuật tái cấu trúc giảm phạm vi truy cập của phương thức từ public/protected xuống private hoặc package-private, khi phương thức đó chỉ được sử dụng nội bộ trong class và không nên được gọi từ bên ngoài.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Phương thức chỉ được sử dụng bởi các phương thức khác trong cùng class
- Phương thức là phần của implementation detail và không nên được biết đến bởi client code
- Muốn giảm sự phụ thuộc và đơn giản hóa public interface của class
- Phương thức có thể thay đổi thường xuyên và không muốn ảnh hưởng đến code bên ngoài

### **Không nên sử dụng khi:**
- Phương thức là một phần của public API và được sử dụng rộng rãi
- Phương thức cần được ghi đè (override) bởi các lớp con
- Phương thức cần được truy cập từ các class trong cùng package (khi đó dùng package-private)

## **Các Bước Thực Hiện**

1. **Xác định phương thức cần ẩn**: Tìm phương thức public/protected chỉ được sử dụng nội bộ
2. **Kiểm tra phạm vi sử dụng**: Đảm bảo phương thức không được gọi từ bên ngoài class
3. **Giảm phạm vi truy cập**: Thay đổi từ public/protected xuống private hoặc package-private
4. **Kiểm tra lỗi biên dịch**: Xác nhận không có lỗi do việc thay đổi phạm vi truy cập
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi ẩn phương thức:**
```java
public class OrderProcessor {
    private OrderValidator validator;
    private PaymentGateway paymentGateway;
    private EmailService emailService;
    
    // Phương thức public không cần thiết - chỉ dùng nội bộ
    public boolean validateOrder(Order order) {
        return validator.validate(order);
    }
    
    // Phương thức public không cần thiết - chỉ dùng nội bộ
    public void processPayment(Order order, PaymentInfo payment) {
        paymentGateway.processPayment(order.getTotal(), payment);
    }
    
    // Phương thức public không cần thiết - chỉ dùng nội bộ
    public void sendConfirmation(Order order) {
        String emailContent = createEmailContent(order);
        emailService.sendEmail(order.getCustomerEmail(), "Order Confirmation", emailContent);
    }
    
    // Phương thức chính - sử dụng các phương thức trên
    public void processOrder(Order order, PaymentInfo payment) {
        if (!validateOrder(order)) {
            throw new InvalidOrderException("Order validation failed");
        }
        
        processPayment(order, payment);
        order.markAsPaid();
        sendConfirmation(order);
    }
    
    // Phương thức helper chỉ dùng nội bộ - nhưng public
    public String createEmailContent(Order order) {
        return String.format(
            "Thank you for your order #%s. Total: $%.2f",
            order.getId(),
            order.getTotal()
        );
    }
}

// Client code có thể sử dụng các phương thức không nên dùng trực tiếp
OrderProcessor processor = new OrderProcessor();

// Có thể gọi các phương thức implementation detail - KHÔNG TỐT
boolean isValid = processor.validateOrder(order); // Nên là nội bộ
processor.processPayment(order, payment);         // Nên là nội bộ
processor.sendConfirmation(order);               // Nên là nội bộ
```

### **✅ Sau khi ẩn phương thức:**
```java
public class OrderProcessor {
    private OrderValidator validator;
    private PaymentGateway paymentGateway;
    private EmailService emailService;
    
    // Các phương thức được ẩn - chỉ dùng nội bộ
    private boolean validateOrder(Order order) {
        return validator.validate(order);
    }
    
    private void processPayment(Order order, PaymentInfo payment) {
        paymentGateway.processPayment(order.getTotal(), payment);
    }
    
    private void sendConfirmation(Order order) {
        String emailContent = createEmailContent(order);
        emailService.sendEmail(order.getCustomerEmail(), "Order Confirmation", emailContent);
    }
    
    // Phương thức chính vẫn public - interface rõ ràng
    public void processOrder(Order order, PaymentInfo payment) {
        if (!validateOrder(order)) {
            throw new InvalidOrderException("Order validation failed");
        }
        
        processPayment(order, payment);
        order.markAsPaid();
        sendConfirmation(order);
    }
    
    // Phương thức helper ẩn đi
    private String createEmailContent(Order order) {
        return String.format(
            "Thank you for your order #%s. Total: $%.2f",
            order.getId(),
            order.getTotal()
        );
    }
    
    // Có thể cung cấp phương thức public cho các use case cụ thể nếu cần
    public boolean canProcessOrder(Order order) {
        return validator.canValidate(order);
    }
}

// Client code chỉ có thể sử dụng interface được thiết kế cẩn thận
OrderProcessor processor = new OrderProcessor();

// Chỉ có phương thức chính được public - interface rõ ràng
processor.processOrder(order, payment);

// Có thể kiểm tra điều kiện trước khi xử lý
if (processor.canProcessOrder(order)) {
    processor.processOrder(order, payment);
}

// Không thể gọi các phương thức implementation detail
// processor.validateOrder(order);       // LỖI - private
// processor.processPayment(order, payment); // LỖI - private
// processor.sendConfirmation(order);    // LỖI - private
```

## **Lợi Ích**

- **✅ Đơn giản hóa interface**: Public interface nhỏ gọn và dễ hiểu hơn
- **✅ Giảm sự phụ thuộc**: Client code ít phụ thuộc vào implementation detail
- **✅ Bảo vệ encapsulation**: Implementation được bảo vệ khỏi sử dụng không đúng cách
- **✅ Dễ bảo trì**: Có thể thay đổi implementation mà không ảnh hưởng đến client
- **✅ Code rõ ràng hơn**: Thể hiện rõ ràng mục đích sử dụng của từng phương thức

## **Điểm Cần Lưu ý**

- **Kiểm tra kỹ phạm vi sử dụng**: Đảm bảo không có code bên ngoài đang sử dụng phương thức
- **Cân nhắc với testing**: Đôi khi cần giữ public cho mục đích test (nhưng nên ưu tiên test qua public interface)
- **Xem xét package structure**: Có thể sử dụng package-private cho các phương thức cần chia sẻ trong package
- **Documentation**: Cập nhật documentation khi thay đổi phạm vi truy cập

## **Khi Nào Không Nên Sử Dụng**

- Khi phương thức là phần của public API và được sử dụng rộng rãi
- Khi phương thức cần được ghi đè bởi lớp con (khi đó dùng protected)
- Khi phương thức cần được truy cập bởi các class trong cùng package
- Khi phương thức cần được sử dụng cho unit testing (mặc dù có cách khác)

## **Kỹ Thuật Liên Quan**

- **Extract Method**: Tách phương thức mới từ code hiện có
- **Inline Method**: Gom phương thức nhỏ vào nơi gọi
- **Encapsulate Field**: Đóng gói trường với getter/setter
- **Replace Method with Method Object**: Thay thế phương thức bằng đối tượng phương thức

## **Kết Luận**

Ẩn phương thức là kỹ thuật quan trọng để duy trì encapsulation và thiết kế interface rõ ràng. Bằng cách giảm phạm vi truy cập của các phương thức chỉ sử dụng nội bộ, chúng ta bảo vệ implementation detail, giảm sự phụ thuộc không cần thiết và làm cho code dễ bảo trì hơn. Kỹ thuật này giúp phân tách rõ ràng giữa public interface (những gì class làm) và implementation detail (cách class thực hiện).

**Nguồn:** [refactoring.guru/hide-method](https://refactoring.guru/hide-method)