# **Loại Bỏ Người Trung Gian (Remove Middle Man)**

## **Định Nghĩa**
Loại bỏ người trung gian là kỹ thuật tái cấu trúc xóa bỏ các phương thức ủy quyền không cần thiết và để client gọi trực tiếp đến đối tượng mục tiêu.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một lớp có quá nhiều phương thức chỉ để ủy quyền cho lớp khác
- Các phương thức ủy quyền làm cho code trở nên phức tạp không cần thiết
- Client cần truy cập trực tiếp vào các tính năng của đối tượng được ủy quyền
- Lớp trung gian trở thành "Middle Man" không có giá trị gia tăng

### **Dấu hiệu nhận biết:**
- Lớp có hầu hết các phương thức chỉ gọi phương thức của lớp khác
- Client vẫn cần truy cập đến đối tượng được ủy quyền cho các tác vụ khác
- Phương thức ủy quyền không thêm bất kỳ giá trị nào ngoài việc chuyển tiếp lời gọi

## **Các Bước Thực Hiện**

1. **Xác định phương thức ủy quyền**: Tìm các phương thức chỉ ủy quyền công việc cho đối tượng khác
2. **Tạo getter cho đối tượng ủy quyền**: Cung cấp cách truy cập trực tiếp đến đối tượng được ủy quyền
3. **Cập nhật client**: Thay thế lời gọi phương thức ủy quyền bằng lời gọi trực tiếp
4. **Xóa phương thức ủy quyền**: Loại bỏ phương thức ủy quyền khỏi lớp trung gian

## **Ví Dụ Minh Họa**

### **❌ Trước khi loại bỏ (có quá nhiều ủy quyền):**
```java
class Person {
    private Department department;
    
    // Quá nhiều phương thức ủy quyền
    public String getManagerName() {
        return department.getManager().getName();
    }
    
    public String getManagerPhone() {
        return department.getManager().getPhone();
    }
    
    public String getManagerEmail() {
        return department.getManager().getEmail();
    }
    
    public String getDepartmentName() {
        return department.getName();
    }
    
    public String getDepartmentCode() {
        return department.getCode();
    }
    
    // ... nhiều phương thức ủy quyền khác
}

class Department {
    private Person manager;
    private String name;
    private String code;
    
    public Person getManager() {
        return manager;
    }
    
    public String getName() {
        return name;
    }
    
    public String getCode() {
        return code;
    }
}

// Client code sử dụng ủy quyền
Person person = new Person();
String managerName = person.getManagerName();
String deptName = person.getDepartmentName();
```

### **✅ Sau khi loại bỏ người trung gian:**
```java
class Person {
    private Department department;
    
    // Cung cấp truy cập trực tiếp đến department
    public Department getDepartment() {
        return department;
    }
    
    // Giữ lại chỉ những phương thức thực sự thêm giá trị
    public String getManagerContactInfo() {
        // Có thể thêm logic định dạng hoặc xử lý đặc biệt ở đây
        return department.getManager().getName() + " - " + 
               department.getManager().getPhone();
    }
}

class Department {
    private Person manager;
    private String name;
    private String code;
    
    public Person getManager() {
        return manager;
    }
    
    public String getName() {
        return name;
    }
    
    public String getCode() {
        return code;
    }
}

// Client code gọi trực tiếp
Person person = new Person();
String managerName = person.getDepartment().getManager().getName();
String deptName = person.getDepartment().getName();

// Vẫn có thể sử dụng phương thức thêm giá trị
String contactInfo = person.getManagerContactInfo();
```

## **Ví Dụ Thực Tế**

### **❌ Middle Man quá mức:**
```java
class OrderProcessor {
    private PaymentGateway paymentGateway;
    private EmailService emailService;
    
    // Quá nhiều ủy quyền không cần thiết
    public boolean processPayment(double amount) {
        return paymentGateway.charge(amount);
    }
    
    public void sendConfirmationEmail(String email) {
        emailService.send(email, "Order Confirmation");
    }
    
    public void sendReceiptEmail(String email) {
        emailService.send(email, "Order Receipt");
    }
    
    public String getPaymentStatus(String transactionId) {
        return paymentGateway.getStatus(transactionId);
    }
}

// Client bị giới hạn bởi các phương thức ủy quyền
OrderProcessor processor = new OrderProcessor();
processor.processPayment(100.0);
processor.sendConfirmationEmail("customer@example.com");
```

### **✅ Sau khi loại bỏ:**
```java
class OrderProcessor {
    private PaymentGateway paymentGateway;
    private EmailService emailService;
    
    // Cung cấp truy cập trực tiếp
    public PaymentGateway getPaymentGateway() {
        return paymentGateway;
    }
    
    public EmailService getEmailService() {
        return emailService;
    }
    
    // Giữ lại phương thức phức tạp thực sự cần xử lý
    public void processCompleteOrder(Order order) {
        // Logic phức tạp kết hợp nhiều dịch vụ
        boolean paymentSuccess = paymentGateway.charge(order.getTotal());
        if (paymentSuccess) {
            emailService.send(order.getCustomerEmail(), "Order Confirmed");
            // ... các bước khác
        }
    }
}

// Client có thể truy cập trực tiếp khi cần
OrderProcessor processor = new OrderProcessor();
processor.getPaymentGateway().charge(100.0);
processor.getEmailService().send("customer@example.com", "Custom Message");

// Hoặc sử dụng phương thức phức tạp
processor.processCompleteOrder(order);
```

## **Lợi Ích**

- **✅ Giảm độ phức tạp**: Loại bỏ các phương thức không cần thiết
- **✅ Linh hoạt hơn**: Client có thể truy cập trực tiếp đến các tính năng
- **✅ Code minh bạch**: Client hiểu rõ hơn về cấu trúc hệ thống
- **✅ Dễ bảo trì**: Ít code hơn để duy trì

## **Điểm Cần Lưu Ý**

- **Giữ lại phương thức có giá trị**: Chỉ xóa các phương thức ủy quyền thuần túy
- **Cân nhắc tính đóng gói**: Đảm bảo không vi phạm nguyên tắc đóng gói
- **Đánh giá tác động**: Xem xét ảnh hưởng đến tất cả client code

## **Khi Không Nên Sử Dụng**

- Khi phương thức ủy quyền thực sự thêm giá trị (như validation, logging)
- Khi cần duy trì tính đóng gói chặt chẽ
- Khi cung cấp interface đơn giản hóa cho client

## **Kỹ Thuật Liên Quan**

- **Ẩn Đối Tượng Ủy quyền (Hide Delegate)**: Ngược lại với loại bỏ người trung gian
- **Hợp nhất Lớp (Inline Class)**: Có thể sử dụng kết hợp nếu lớp chỉ toàn ủy quyền
- **Di chuyển Phương thức (Move Method)**: Để di chuyển phương thức đến lớp phù hợp hơn

## **Kết Luận**

Loại bỏ người trung gian giúp đơn giản hóa hệ thống khi có quá nhiều lớp chỉ thực hiện ủy quyền mà không thêm giá trị. Tuy nhiên, cần cân nhắc kỹ để giữ lại những phương thức ủy quyền thực sự hữu ích và đảm bảo không làm mất đi tính đóng gói cần thiết.