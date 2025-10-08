# **Thay Thế Giá Trị Dữ Liệu Bằng Đối Tượng (Replace Data Value with Object)**

## **Định Nghĩa**
Thay thế giá trị dữ liệu bằng đối tượng là kỹ thuật tái cấu trúc chuyển đổi một kiểu dữ liệu đơn giản (như string, number) thành một đối tượng khi dữ liệu cần có hành vi hoặc xử lý phức tạp hơn.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một trường dữ liệu đơn giản cần có thêm hành vi hoặc validation
- Có code trùng lặp xử lý cùng một loại dữ liệu ở nhiều nơi
- Dữ liệu có cấu trúc phức tạp hơn một giá trị đơn lẻ
- Cần tăng khả năng biểu đạt của code

### **Dấu hiệu nhận biết:**
- Các phương thức static xử lý dữ liệu xuất hiện ở nhiều nơi
- Validation logic cho cùng kiểu dữ liệu bị lặp lại
- Dữ liệu có nhiều thuộc tính liên quan nhưng bị lưu rời rạc

## **Các Bước Thực Hiện**

1. **Tạo lớp mới**: Tạo lớp mới để đại diện cho khái niệm dữ liệu
2. **Chuyển trường**: Di chuyển trường dữ liệu sang lớp mới
3. **Thêm hành vi**: Di chuyển các phương thức xử lý liên quan sang lớp mới
4. **Cập nhật tham chiếu**: Thay thế tất cả các tham chiếu đến dữ liệu cũ

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class Order {
    private String customerName; // Chỉ là string đơn giản
    
    public Order(String customerName) {
        this.customerName = customerName;
    }
    
    public String getCustomerName() {
        return customerName;
    }
}

// Validation logic bị phân tán
class OrderValidator {
    public static boolean isValidCustomer(String customerName) {
        return customerName != null && 
               !customerName.trim().isEmpty() && 
               customerName.length() <= 100;
    }
}

// Format logic bị phân tán
class ReportGenerator {
    public static String formatCustomer(String customerName) {
        if (customerName == null) return "Unknown";
        return customerName.trim().toUpperCase();
    }
}
```

### **✅ Sau khi thay thế:**
```java
// Lớp Customer mới với đầy đủ hành vi
class Customer {
    private final String name;
    
    public Customer(String name) {
        if (!isValidName(name)) {
            throw new IllegalArgumentException("Tên khách hàng không hợp lệ");
        }
        this.name = name.trim();
    }
    
    private boolean isValidName(String name) {
        return name != null && 
               !name.trim().isEmpty() && 
               name.length() <= 100;
    }
    
    public String getName() {
        return name;
    }
    
    public String getFormattedName() {
        return name.toUpperCase();
    }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Customer customer = (Customer) obj;
        return name.equals(customer.name);
    }
    
    @Override
    public int hashCode() {
        return name.hashCode();
    }
}

class Order {
    private Customer customer; // Sử dụng đối tượng thay vì string
    
    public Order(Customer customer) {
        this.customer = customer;
    }
    
    public Customer getCustomer() {
        return customer;
    }
}

// Client code trở nên đơn giản và an toàn
Customer customer = new Customer("John Doe");
Order order = new Order(customer);

String formattedName = order.getCustomer().getFormattedName();
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Sử dụng primitive types cho địa chỉ:**
```java
class Person {
    private String street;
    private String city;
    private String country;
    private String zipCode;
    
    public Person(String street, String city, String country, String zipCode) {
        this.street = street;
        this.city = city;
        this.country = country;
        this.zipCode = zipCode;
    }
    
    // Validation logic phân tán
    public boolean isValidAddress() {
        return street != null && !street.trim().isEmpty() &&
               city != null && !city.trim().isEmpty() &&
               country != null && !country.trim().isEmpty() &&
               isValidZipCode(zipCode);
    }
    
    private boolean isValidZipCode(String zipCode) {
        return zipCode != null && zipCode.matches("\\d{5}(-\\d{4})?");
    }
    
    // Format logic phân tán
    public String getFullAddress() {
        return street + ", " + city + ", " + country + " " + zipCode;
    }
}
```

### **✅ Sau khi thay thế bằng đối tượng Address:**
```java
class Address {
    private final String street;
    private final String city;
    private final String country;
    private final String zipCode;
    
    public Address(String street, String city, String country, String zipCode) {
        if (!isValidStreet(street)) {
            throw new IllegalArgumentException("Đường phố không hợp lệ");
        }
        if (!isValidCity(city)) {
            throw new IllegalArgumentException("Thành phố không hợp lệ");
        }
        if (!isValidZipCode(zipCode)) {
            throw new IllegalArgumentException("Mã zip không hợp lệ");
        }
        
        this.street = street.trim();
        this.city = city.trim();
        this.country = country != null ? country.trim() : "US"; // default
        this.zipCode = zipCode.trim();
    }
    
    private boolean isValidStreet(String street) {
        return street != null && !street.trim().isEmpty();
    }
    
    private boolean isValidCity(String city) {
        return city != null && !city.trim().isEmpty();
    }
    
    private boolean isValidZipCode(String zipCode) {
        return zipCode != null && zipCode.matches("\\d{5}(-\\d{4})?");
    }
    
    public String getFullAddress() {
        return street + ", " + city + ", " + country + " " + zipCode;
    }
    
    public String getStreet() { return street; }
    public String getCity() { return city; }
    public String getCountry() { return country; }
    public String getZipCode() { return zipCode; }
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Address address = (Address) obj;
        return street.equals(address.street) &&
               city.equals(address.city) &&
               country.equals(address.country) &&
               zipCode.equals(address.zipCode);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(street, city, country, zipCode);
    }
}

class Person {
    private Address address;
    
    public Person(Address address) {
        this.address = address;
    }
    
    public Address getAddress() {
        return address;
    }
    
    // Không cần validation logic ở đây nữa
}

// Sử dụng
Address address = new Address("123 Main St", "New York", "US", "10001");
Person person = new Person(address);

String fullAddress = person.getAddress().getFullAddress();
```

## **Lợi Ích**

- **✅ Tăng tính đóng gói**: Dữ liệu và hành vi được nhóm lại với nhau
- **✅ Giảm code trùng lặp**: Logic xử lý tập trung ở một nơi
- **✅ Code dễ đọc hơn**: Tên lớp thể hiện rõ ý nghĩa của dữ liệu
- **✅ Dễ bảo trì**: Thay đổi logic chỉ ở một chỗ
- **✅ Tái sử dụng tốt**: Đối tượng mới có thể sử dụng ở nhiều nơi

## **Điểm Cần Lưu Ý**

- **Không tạo quá nhiều lớp nhỏ**: Chỉ tạo lớp mới khi thực sự cần thiết
- **Cân nhắc hiệu năng**: Trong một số trường hợp, đối tượng có thể nặng hơn kiểu primitive
- **Đảm bảo tính immutable**: Nếu có thể, làm cho đối tượng mới trở thành immutable

## **Khi Nào Không Nên Sử Dụng**

- Khi dữ liệu thực sự đơn giản và không cần hành vi đi kèm
- Khi performance là vấn đề quan trọng và cần tối ưu
- Khi dữ liệu chỉ được sử dụng tạm thời và cục bộ

## **Kỹ Thuật Liên Quan**

- **Thay thế Mảng bằng Đối tượng (Replace Array with Object)**: Khi có mảng chứa các phần tử có ý nghĩa khác nhau
- **Trích xuất Lớp (Extract Class)**: Khi cần tách một phần của lớp thành lớp độc lập
- **Giới thiệu Tham số Đối tượng (Introduce Parameter Object)**: Khi có nhiều tham số liên quan đến nhau

## **Kết Luận**

Thay thế giá trị dữ liệu bằng đối tượng là kỹ thuật mạnh mẽ để chống lại "Primitive Obsession" - việc sử dụng quá nhiều kiểu dữ liệu nguyên thủy. Bằng cách chuyển đổi các giá trị đơn giản thành các đối tượng có ý nghĩa, bạn tạo ra code dễ đọc, dễ bảo trì và linh hoạt hơn trước các thay đổi yêu cầu.

**Nguồn:** [refactoring.guru/replace-data-value-with-object](https://refactoring.guru/replace-data-value-with-object)