# **Giới Thiệu Đối Tượng Null (Introduce Null Object)**

## **Định Nghĩa**
Giới thiệu đối tượng Null là kỹ thuật tái cấu trúc tạo một đối tượng đặc biệt có hành vi mặc định để thay thế cho việc kiểm tra null trong code.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có nhiều kiểm tra null lặp lại trong code
- Muốn loại bỏ các điều kiện kiểm tra null rải rác
- Cần cung cấp hành vi mặc định cho trường hợp không có đối tượng

### **Không nên sử dụng khi:**
- Việc null thực sự đại diện cho lỗi và cần được xử lý đặc biệt
- Chỉ có một vài kiểm tra null đơn lẻ

## **Các Bước Thực Hiện**

1. **Tạo lớp null**: Tạo lớp con hoặc lớp triển khai interface với hành vi mặc định
2. **Triển khai hành vi mặc định**: Trong lớp null, triển khai các phương thức với hành vi an toàn
3. **Thay thế kiểm tra null**: Thay thế các kiểm tra null bằng việc sử dụng đối tượng null
4. **Cập nhật factory/constructor**: Đảm bảo trả về đối tượng null thay vì null

## **Ví Dụ Minh Họa**

### **❌ Trước khi giới thiệu null object:**
```java
class Customer {
    private String name;
    private BillingPlan plan;
    private PaymentHistory history;
    
    public String getName() {
        return name;
    }
    
    public BillingPlan getPlan() {
        return plan;
    }
    
    public PaymentHistory getHistory() {
        return history;
    }
    
    public boolean isNull() {
        return false;
    }
}

// Client code với nhiều kiểm tra null
Customer customer = getCustomerById(id);
String customerName;
if (customer == null) {
    customerName = "Occupant";
} else {
    customerName = customer.getName();
}

BillingPlan plan;
if (customer == null) {
    plan = BillingPlan.basic();
} else {
    plan = customer.getPlan();
}

int weeksDelinquent;
if (customer == null) {
    weeksDelinquent = 0;
} else {
    weeksDelinquent = customer.getHistory().getWeeksDelinquentInLastYear();
}
```

### **✅ Sau khi giới thiệu null object:**
```java
// Lớp cơ sở hoặc interface
abstract class Customer {
    public abstract String getName();
    public abstract BillingPlan getPlan();
    public abstract PaymentHistory getHistory();
    public abstract boolean isNull();
}

// Lớp thực thể
class RealCustomer extends Customer {
    private String name;
    private BillingPlan plan;
    private PaymentHistory history;
    
    public RealCustomer(String name, BillingPlan plan, PaymentHistory history) {
        this.name = name;
        this.plan = plan;
        this.history = history;
    }
    
    @Override
    public String getName() {
        return name;
    }
    
    @Override
    public BillingPlan getPlan() {
        return plan;
    }
    
    @Override
    public PaymentHistory getHistory() {
        return history;
    }
    
    @Override
    public boolean isNull() {
        return false;
    }
}

// Lớp null object
class NullCustomer extends Customer {
    @Override
    public String getName() {
        return "Occupant";
    }
    
    @Override
    public BillingPlan getPlan() {
        return BillingPlan.basic();
    }
    
    @Override
    public PaymentHistory getHistory() {
        return new NullPaymentHistory();
    }
    
    @Override
    public boolean isNull() {
        return true;
    }
}

// Null object cho PaymentHistory
class NullPaymentHistory extends PaymentHistory {
    @Override
    public int getWeeksDelinquentInLastYear() {
        return 0;
    }
}

// Factory method
class CustomerFactory {
    public static Customer getCustomerById(String id) {
        Customer customer = repository.findCustomerById(id);
        if (customer == null) {
            return new NullCustomer();
        }
        return customer;
    }
}

// Client code trở nên đơn giản
Customer customer = CustomerFactory.getCustomerById(id);
String customerName = customer.getName();
BillingPlan plan = customer.getPlan();
int weeksDelinquent = customer.getHistory().getWeeksDelinquentInLastYear();
```

## **Lợi Ích**

- **✅ Giảm kiểm tra null**: Loại bỏ các điều kiện kiểm tra null lặp lại
- **✅ Code an toàn hơn**: Tránh NullPointerException
- **✅ Hành vi nhất quán**: Cung cấp hành vi mặc định thống nhất
- **✅ Code sạch hơn**: Logic nghiệp vụ không bị lẫn với kiểm tra null
- **✅ Dễ test**: Có thể test với cả đối tượng thật và đối tượng null

## **Điểm Cần Lưu Ý**

- **Không che giấu lỗi thực sự**: Đảm bảo null object chỉ dùng cho trường hợp hợp lệ
- **Hành vi mặc định phù hợp**: Hành vi mặc định phải có ý nghĩa trong ngữ cảnh
- **Cân nhắc với collections**: Đôi khi empty collection tốt hơn null collection

## **Khi Nào Không Nên Sử Dụng**

- Khi null thực sự biểu thị lỗi và cần được xử lý đặc biệt
- Khi không có hành vi mặc định phù hợp
- Khi việc giới thiệu null object làm code phức tạp hơn

## **Kỹ Thuật Liên Quan**

- **Introduce Special Case**: Mở rộng của null object cho các trường hợp đặc biệt khác
- **Optional Pattern**: Sử dụng Optional trong Java 8+
- **Replace Error Code with Exception**: Thay thế mã lỗi bằng ngoại lệ

## **Kết Luận**

Giới thiệu đối tượng Null là kỹ thuật hiệu quả để xử lý vấn đề null trong code. Bằng cách cung cấp một đối tượng với hành vi mặc định an toàn, bạn loại bỏ được các kiểm tra null lặp lại và làm cho code trở nên sạch sẽ, an toàn hơn. Tuy nhiên, cần sử dụng một cách thận trọng để không che giấu các lỗi thực sự cần được xử lý.