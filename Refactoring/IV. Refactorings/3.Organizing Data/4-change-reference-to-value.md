# **Thay Đổi Tham Chiếu Thành Giá Trị (Change Reference to Value)**

## **Định Nghĩa**
Thay đổi tham chiếu thành giá trị là kỹ thuật tái cấu trúc chuyển đổi một đối tượng tham chiếu (reference object) thành đối tượng giá trị (value object) khi đối tượng nhỏ, không thay đổi và không cần quản lý danh tính.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Đối tượng nhỏ, bất biến (immutable) và khó quản lý vòng đời
- Danh tính của đối tượng không quan trọng, chỉ giá trị mới quan trọng
- Có quá nhiều đối tượng tham chiếu gây khó khăn cho việc quản lý
- Muốn đơn giản hóa hệ thống bằng cách loại bỏ repository

### **Dấu hiệu nhận biết:**
- Đối tượng không có hành vi phức tạp, chỉ chứa dữ liệu
- So sánh đối tượng dựa trên giá trị, không phải danh tính
- Mỗi instance độc lập và không cần chia sẻ giữa các client

## **Các Bước Thực Hiện**

1. **Kiểm tra tính bất biến**: Đảm bảo đối tượng có thể trở thành immutable
2. **Triển khai equals() và hashCode()**: Đảm bảo so sánh giá trị hoạt động chính xác
3. **Loại bỏ factory/repository**: Thay thế bằng constructor thông thường
4. **Xóa quản lý danh tính**: Loại bỏ ID và các cơ chế quản lý instance
5. **Cập nhật client code**: Thay thế các lời gọi factory bằng constructor

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay đổi (Reference Object):**
```java
class Currency {
    private final String code;
    private static final Map<String, Currency> instances = new HashMap<>();
    
    // Constructor private với quản lý instance
    private Currency(String code) {
        this.code = code;
    }
    
    // Factory method quản lý instance
    public static Currency getInstance(String code) {
        return instances.computeIfAbsent(code, Currency::new);
    }
    
    public String getCode() {
        return code;
    }
}

class Money {
    private final double amount;
    private final Currency currency; // Reference object
    
    public Money(double amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }
    
    // So sánh dựa trên danh tính (identity)
    public boolean isSameCurrency(Money other) {
        return this.currency == other.currency;
    }
}

// Sử dụng - phải dùng factory method
Currency usd1 = Currency.getInstance("USD");
Currency usd2 = Currency.getInstance("USD");
Money money1 = new Money(100, usd1);
Money money2 = new Money(200, usd2);

System.out.println(usd1 == usd2); // true - cùng instance
System.out.println(money1.isSameCurrency(money2)); // true
```

### **✅ Sau khi thay đổi (Value Object):**
```java
class Currency {
    private final String code;
    
    // Constructor public - có thể tạo trực tiếp
    public Currency(String code) {
        if (code == null || code.length() != 3) {
            throw new IllegalArgumentException("Mã tiền tệ phải có 3 ký tự");
        }
        this.code = code.toUpperCase();
    }
    
    public String getCode() {
        return code;
    }
    
    // Triển khai equals() và hashCode() để so sánh giá trị
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Currency currency = (Currency) o;
        return code.equals(currency.code);
    }
    
    @Override
    public int hashCode() {
        return code.hashCode();
    }
}

class Money {
    private final double amount;
    private final Currency currency; // Value object
    
    public Money(double amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }
    
    // So sánh dựa trên giá trị
    public boolean isSameCurrency(Money other) {
        return this.currency.equals(other.currency);
    }
    
    public Money add(Money other) {
        if (!this.currency.equals(other.currency)) {
            throw new IllegalArgumentException("Khác loại tiền tệ");
        }
        return new Money(this.amount + other.amount, this.currency);
    }
    
    // Triển khai equals() và hashCode()
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Money money = (Money) o;
        return Double.compare(money.amount, amount) == 0 &&
               currency.equals(money.currency);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(amount, currency);
    }
}

// Sử dụng - đơn giản với constructor
Currency usd1 = new Currency("USD");
Currency usd2 = new Currency("USD");
Money money1 = new Money(100, usd1);
Money money2 = new Money(200, usd2);

System.out.println(usd1.equals(usd2)); // true - cùng giá trị
System.out.println(money1.isSameCurrency(money2)); // true
System.out.println(money1.equals(new Money(100, new Currency("USD")))); // true
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Đối tượng giá trị với nhiều thuộc tính:**
```java
class Address {
    private final String street;
    private final String city;
    private final String zipCode;
    private final String country;
    
    public Address(String street, String city, String zipCode, String country) {
        this.street = street;
        this.city = city;
        this.zipCode = zipCode;
        this.country = country;
    }
    
    // Getter methods
    public String getStreet() { return street; }
    public String getCity() { return city; }
    public String getZipCode() { return zipCode; }
    public String getCountry() { return country; }
    
    // Business logic
    public boolean isDomestic() {
        return "US".equals(country);
    }
    
    public String getFullAddress() {
        return street + ", " + city + ", " + zipCode + ", " + country;
    }
    
    // Triển khai equals() và hashCode()
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Address address = (Address) o;
        return Objects.equals(street, address.street) &&
               Objects.equals(city, address.city) &&
               Objects.equals(zipCode, address.zipCode) &&
               Objects.equals(country, address.country);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(street, city, zipCode, country);
    }
    
    @Override
    public String toString() {
        return getFullAddress();
    }
}

class Customer {
    private String name;
    private Address address; // Value object
    
    public Customer(String name, Address address) {
        this.name = name;
        this.address = address;
    }
    
    public void updateAddress(Address newAddress) {
        this.address = newAddress; // Có thể gán trực tiếp vì immutable
    }
    
    // Có thể tạo bản sao an toàn
    public Address getAddress() {
        return address; // Trả về trực tiếp vì immutable
    }
}

// Sử dụng
Address address1 = new Address("123 Main St", "New York", "10001", "US");
Address address2 = new Address("123 Main St", "New York", "10001", "US");

Customer customer1 = new Customer("John", address1);
Customer customer2 = new Customer("Jane", address2);

System.out.println(address1.equals(address2)); // true
System.out.println(customer1.getAddress().isDomestic()); // true
```

## **Lợi Ích**

- **✅ Đơn giản hóa**: Không cần quản lý repository hoặc factory
- **✅ Dễ sử dụng**: Có thể tạo instance trực tiếp với constructor
- **✅ An toàn đa luồng**: Đối tượng bất biến an toàn khi chia sẻ
- **✅ Dễ test**: Có thể tạo instance mới cho mỗi test
- **✅ Minh bạch**: Hành vi rõ ràng, không có side effect ẩn

## **Điểm Cần Lưu Ý**

- **Bất biến là bắt buộc**: Value object phải là immutable
- **Triển khai equals()/hashCode()**: Phải chính xác và nhất quán
- **Hiệu năng**: Có thể tạo nhiều instance, nhưng thường không thành vấn đề với object nhỏ
- **Không phù hợp cho object lớn**: Object có trạng thái phức tạp hoặc cần quản lý vòng đời

## **Khi Nào Không Nên Sử Dụng**

- Khi object có trạng thái thay đổi thường xuyên
- Khi cần quản lý danh tính hoặc vòng đời của object
- Khi object lớn và việc tạo nhiều instance tốn kém
- Khi cần theo dõi các thay đổi trên cùng một instance

## **Kỹ Thuật Liên Quan**

- **Thay đổi Giá trị Thành Tham chiếu (Change Value to Reference)**: Ngược lại với kỹ thuật này
- **Đóng gói Bộ sưu tập (Encapsulate Collection)**: Khi làm việc với collections
- **Tự Đóng gói Trường (Self Encapsulate Field)**: Để chuẩn bị cho thay đổi

## **Kết Luận**

Thay đổi tham chiếu thành giá trị là kỹ thuật đơn giản hóa hệ thống khi đối tượng không cần quản lý danh tính phức tạp. Bằng cách chuyển sang value object, bạn tạo ra code dễ hiểu, dễ test và an toàn hơn. Tuy nhiên, kỹ thuật này chỉ phù hợp cho các đối tượng nhỏ, bất biến và không yêu cầu quản lý vòng đời.