# **Xóa Phương Thức Thiết Lập (Remove Setting Method)**

## **Định Nghĩa**
Xóa phương thức thiết lập là kỹ thuật tái cấu trúc loại bỏ phương thức setter khi giá trị của trường không nên thay đổi sau khi đối tượng được tạo, giúp đảm bảo tính bất biến và tính toàn vẹn của đối tượng.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Trường chỉ nên được thiết lập một lần khi đối tượng được tạo
- Muốn đảm bảo tính bất biến (immutability) cho đối tượng
- Giá trị trường không nên thay đổi trong vòng đời của đối tượng
- Muốn ngăn chặn việc thay đổi trạng thái không mong muốn

### **Không nên sử dụng khi:**
- Trường cần được thay đổi trong suốt vòng đời của đối tượng
- Đối tượng cần cập nhật trạng thái theo nghiệp vụ
- Việc xóa setter làm mất đi tính linh hoạt cần thiết

## **Các Bước Thực Hiện**

1. **Xác định setter cần xóa**: Tìm phương thức setter cho trường không nên thay đổi
2. **Kiểm tra tính sử dụng**: Đảm bảo setter không được sử dụng hoặc có thể thay thế
3. **Khởi tạo trong constructor**: Di chuyển việc thiết lập giá trị vào constructor
4. **Xóa setter**: Xóa phương thức setter khỏi class
5. **Đánh dấu trường là final**: Đánh dấu trường là final nếu có thể
6. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi xóa setter:**
```java
class Customer {
    private String id;
    private String name;
    private String email;
    private Date registrationDate;
    
    public Customer() {
        this.registrationDate = new Date();
    }
    
    // Setter cho trường không nên thay đổi
    public void setId(String id) {
        this.id = id;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
    
    // Setter nguy hiểm - ngày đăng ký không nên thay đổi
    public void setRegistrationDate(Date date) {
        this.registrationDate = date;
    }
    
    // Các getter
    public String getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
    public Date getRegistrationDate() { return registrationDate; }
}

// Client code có thể thay đổi trạng thái không mong muốn
Customer customer = new Customer();
customer.setId("CUST-001");
customer.setName("John Doe");
customer.setEmail("john@example.com");

// Vấn đề: có thể thay đổi ngày đăng ký - không hợp lý về nghiệp vụ
customer.setRegistrationDate(someOtherDate); // KHÔNG NÊN CHO PHÉP
```

### **✅ Sau khi xóa setter:**
```java
class Customer {
    private final String id;
    private final String name;
    private final String email;
    private final Date registrationDate;
    
    // Khởi tạo tất cả giá trị trong constructor
    public Customer(String id, String name, String email) {
        if (id == null || id.trim().isEmpty()) {
            throw new IllegalArgumentException("ID cannot be null or empty");
        }
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be null or empty");
        }
        
        this.id = id;
        this.name = name;
        this.email = email;
        this.registrationDate = new Date(); // Tự động thiết lập
    }
    
    // Constructor với ngày đăng ký tùy chỉnh (nếu cần)
    public Customer(String id, String name, String email, Date registrationDate) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.registrationDate = new Date(registrationDate.getTime()); // Defensive copy
    }
    
    // Chỉ có getter, không có setter
    public String getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
    public Date getRegistrationDate() { 
        return new Date(registrationDate.getTime()); // Defensive copy
    }
    
    // Có thể cung cấp phương thức "thay đổi" thông qua đối tượng mới
    public Customer withEmail(String newEmail) {
        return new Customer(this.id, this.name, newEmail, this.registrationDate);
    }
}

// Client code - an toàn hơn, trạng thái được bảo vệ
Customer customer = new Customer("CUST-001", "John Doe", "john@example.com");

// Không thể thay đổi các trường quan trọng
// customer.setId("NEW-ID"); // LỖI BIÊN DỊCH - không có setter
// customer.setRegistrationDate(new Date()); // LỖI BIÊN DỊCH

// Cách duy nhất "thay đổi" email là tạo đối tượng mới
Customer updatedCustomer = customer.withEmail("newemail@example.com");
```

## **Lợi Ích**

- **✅ Đảm bảo tính bất biến**: Đối tượng an toàn hơn trong môi trường đa luồng
- **✅ Tính toàn vẹn dữ liệu**: Ngăn chặn thay đổi trạng thái không hợp lệ
- **✅ Code dễ hiểu hơn**: Đối tượng có trạng thái có thể dự đoán được
- **✅ Giảm lỗi**: Không có nguy cơ thay đổi trạng thái không mong muốn
- **✅ Dễ test hơn**: Trạng thái đối tượng luôn ổn định

## **Điểm Cần Lưu ý**

- **Không áp dụng cho tất cả trường hợp**: Chỉ cho trường thực sự không nên thay đổi
- **Cân nhắc với ORM**: Một số framework ORM yêu cầu setter
- **Xem xét tính linh hoạt**: Đảm bảo không làm mất tính linh hoạt cần thiết
- **Sử dụng defensive copy**: Với kiểu dữ liệu mutable như Date, cần tạo bản sao

## **Khi Nào Không Nên Sử Dụng**

- Khi đối tượng cần thay đổi trạng thái theo yêu cầu nghiệp vụ
- Khi sử dụng framework yêu cầu setter (như JPA/Hibernate)
- Khi đối tượng là DTO hoặc bean cần setter
- Khi cần linh hoạt thay đổi trạng thái

## **Kỹ Thuật Liên Quan**

- **Encapsulate Field**: Đóng gói trường bằng getter/setter
- **Change Value to Reference**: Thay đổi giá trị thành tham chiếu
- **Replace Constructor with Factory Method**: Thay thế constructor bằng factory method
- **Introduce Parameter Object**: Giới thiệu đối tượng tham số

## **Kết Luận**

Xóa phương thức thiết lập là kỹ thuật quan trọng để thiết kế các đối tượng bất biến và đảm bảo tính toàn vẹn dữ liệu. Bằng cách loại bỏ các setter không cần thiết và khởi tạo giá trị trong constructor, chúng ta tạo ra các đối tượng an toàn hơn, dễ dự đoán hơn và ít lỗi hơn. Tuy nhiên, cần cân nhắc kỹ lưỡng để không làm mất đi tính linh hoạt cần thiết của đối tượng, đặc biệt khi làm việc với các framework yêu cầu setter.

**Nguồn:** [refactoring.guru/remove-setting-method](https://refactoring.guru/remove-setting-method)