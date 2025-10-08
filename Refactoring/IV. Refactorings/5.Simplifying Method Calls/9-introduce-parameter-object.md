# **Giới Thiệu Đối Tượng Tham Số (Introduce Parameter Object)**

## **Định Nghĩa**
Giới thiệu đối tượng tham số là kỹ thuật tái cấu trúc thay thế một nhóm các tham số liên quan bằng một đối tượng duy nhất, giúp giảm số lượng tham số và nhóm các dữ liệu liên quan với nhau.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một phương thức có quá nhiều tham số, đặc biệt là các tham số liên quan với nhau
- Có cùng một nhóm tham số xuất hiện trong nhiều phương thức khác nhau
- Muốn nhóm các dữ liệu liên quan thành một khái niệm duy nhất
- Cần thêm validation hoặc hành vi cho nhóm dữ liệu

### **Không nên sử dụng khi:**
- Các tham số không thực sự liên quan đến nhau
- Chỉ có một hoặc hai tham số đơn lẻ
- Việc tạo đối tượng mới làm code phức tạp hơn

## **Các Bước Thực Hiện**

1. **Xác định nhóm tham số**: Tìm các tham số thường xuất hiện cùng nhau
2. **Tạo lớp đối tượng tham số**: Tạo lớp mới để đại diện cho nhóm tham số
3. **Thêm thuộc tính và validation**: Thêm các thuộc tính tương ứng và validation cần thiết
4. **Thay thế trong phương thức**: Thay thế nhóm tham số bằng đối tượng mới
5. **Cập nhật lời gọi**: Cập nhật tất cả các lời gọi phương thức
6. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi giới thiệu parameter object:**
```java
class CustomerService {
    // Quá nhiều tham số liên quan đến địa chỉ
    public void updateCustomerAddress(
        Long customerId, 
        String street, 
        String city, 
        String state, 
        String zipCode, 
        String country,
        boolean isPrimary
    ) {
        // Validation rải rác
        if (street == null || street.trim().isEmpty()) {
            throw new IllegalArgumentException("Street cannot be empty");
        }
        if (zipCode == null || !zipCode.matches("\\d{5}")) {
            throw new IllegalArgumentException("Invalid zip code");
        }
        
        // Logic xử lý
        Customer customer = findCustomer(customerId);
        customer.updateAddress(street, city, state, zipCode, country, isPrimary);
    }
    
    // Cùng nhóm tham số xuất hiện ở nhiều phương thức
    public void validateAddress(
        String street, 
        String city, 
        String state, 
        String zipCode, 
        String country
    ) {
        // Validation logic trùng lặp
    }
    
    public void calculateShipping(
        String street, 
        String city, 
        String state, 
        String zipCode, 
        String country
    ) {
        // Logic tính phí vận chuyển
    }
}

// Client code - khó nhớ thứ tự và ý nghĩa tham số
service.updateCustomerAddress(
    123L, 
    "123 Main St", 
    "Springfield", 
    "IL", 
    "62701", 
    "USA", 
    true
);
```

### **✅ Sau khi giới thiệu parameter object:**
```java
// Lớp đối tượng tham số mới
class Address {
    private final String street;
    private final String city;
    private final String state;
    private final String zipCode;
    private final String country;
    
    public Address(String street, String city, String state, String zipCode, String country) {
        // Validation tập trung
        if (street == null || street.trim().isEmpty()) {
            throw new IllegalArgumentException("Street cannot be empty");
        }
        if (zipCode == null || !zipCode.matches("\\d{5}")) {
            throw new IllegalArgumentException("Invalid zip code");
        }
        
        this.street = street;
        this.city = city;
        this.state = state;
        this.zipCode = zipCode;
        this.country = country;
    }
    
    // Có thể thêm các phương thức tiện ích
    public boolean isDomestic() {
        return "USA".equalsIgnoreCase(country);
    }
    
    public String getFullAddress() {
        return String.format("%s, %s, %s %s, %s", street, city, state, zipCode, country);
    }
    
    // Getters
    public String getStreet() { return street; }
    public String getCity() { return city; }
    public String getState() { return state; }
    public String getZipCode() { return zipCode; }
    public String getCountry() { return country; }
}

class CustomerService {
    // Phương thức đơn giản hơn với parameter object
    public void updateCustomerAddress(Long customerId, Address address, boolean isPrimary) {
        Customer customer = findCustomer(customerId);
        customer.updateAddress(address, isPrimary);
    }
    
    // Các phương thức khác cũng sử dụng cùng parameter object
    public void validateAddress(Address address) {
        // Validation đã được xử lý trong constructor của Address
        // Có thể thêm validation nghiệp vụ cụ thể ở đây
    }
    
    public double calculateShipping(Address address) {
        // Logic tính phí vận chuyển
        if (address.isDomestic()) {
            return calculateDomesticShipping(address);
        } else {
            return calculateInternationalShipping(address);
        }
    }
    
    private double calculateDomesticShipping(Address address) {
        // Tính phí nội địa
        return 5.0;
    }
    
    private double calculateInternationalShipping(Address address) {
        // Tính phí quốc tế
        return 25.0;
    }
}

// Client code - rõ ràng và dễ sử dụng
Address address = new Address("123 Main St", "Springfield", "IL", "62701", "USA");
service.updateCustomerAddress(123L, address, true);

// Có thể tái sử dụng đối tượng Address
service.validateAddress(address);
double shippingCost = service.calculateShipping(address);
```

## **Lợi Ích**

- **✅ Giảm số lượng tham số**: Phương thức có ít tham số hơn
- **✅ Nhóm dữ liệu liên quan**: Các tham số liên quan được nhóm thành một khái niệm
- **✅ Tái sử dụng code**: Có thể sử dụng cùng đối tượng ở nhiều phương thức
- **✅ Validation tập trung**: Logic validation được tập trung ở một nơi
- **✅ Dễ mở rộng**: Thêm thuộc tính mới dễ dàng mà không thay đổi signature
- **✅ Code rõ ràng hơn**: Thể hiện rõ ý nghĩa của nhóm dữ liệu

## **Điểm Cần Lưu ý**

- **Không lạm dụng**: Chỉ áp dụng cho các tham số thực sự liên quan
- **Đặt tên có ý nghĩa**: Tên lớp nên thể hiện rõ khái niệm mà nó đại diện
- **Xem xét tính bất biến**: Thường nên làm đối tượng bất biến (immutable)
- **Cân nhắc performance**: Trong một số trường hợp, tạo object mới có thể ảnh hưởng hiệu suất

## **Khi Nào Không Nên Sử Dụng**

- Khi các tham số không thực sự liên quan đến nhau
- Khi chỉ có một hoặc hai tham số đơn lẻ
- Khi việc tạo lớp mới là quá mức cần thiết cho nhu cầu đơn giản
- Khi cần tương thích với API hiện có

## **Kỹ Thuật Liên Quan**

- **Preserve Whole Object**: Truyền toàn bộ đối tượng thay vì từng thuộc tính
- **Replace Data Value with Object**: Thay thế giá trị dữ liệu đơn giản bằng đối tượng
- **Remove Parameter**: Xóa tham số không cần thiết
- **Add Parameter**: Thêm tham số mới khi cần

## **Kết Luận**

Giới thiệu đối tượng tham số là kỹ thuật mạnh mẽ để quản lý các phương thức có quá nhiều tham số, đặc biệt khi các tham số này liên quan với nhau và xuất hiện trong nhiều phương thức. Bằng cách nhóm các tham số thành một đối tượng có ý nghĩa, code trở nên rõ ràng, dễ bảo trì và ít lỗi hơn. Kỹ thuật này cũng giúp tập trung validation và thêm các hành vi liên quan đến nhóm dữ liệu.

**Nguồn:** [refactoring.guru/introduce-parameter-object](https://refactoring.guru/introduce-parameter-object)