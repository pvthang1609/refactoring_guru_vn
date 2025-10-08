# **Thay Thế Mã Kiểu Bằng State/Strategy (Replace Type Code with State/Strategy)**

## **Định Nghĩa**
Thay thế mã kiểu bằng State/Strategy là kỹ thuật tái cấu trúc thay thế các mã kiểu (số hoặc chuỗi) bằng các đối tượng State hoặc Strategy riêng biệt, cho phép hành vi thay đổi động trong thời gian chạy.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Mã kiểu ảnh hưởng đến hành vi và có thể thay đổi trong thời gian chạy
- Không thể sử dụng kế thừa do mã kiểu thay đổi sau khi đối tượng được tạo
- Có nhiều điều kiện phức tạp dựa trên mã kiểu

### **Không nên sử dụng khi:**
- Mã kiểu đơn giản, không thay đổi và không ảnh hưởng đến hành vi
- Có thể sử dụng kế thừa đơn giản với các lớp con

## **Các Bước Thực Hiện**

1. **Tạo interface**: Tạo interface chung cho tất cả các hành vi
2. **Tạo lớp cụ thể**: Tạo lớp cho mỗi giá trị mã kiểu, triển khai interface
3. **Thay thế mã kiểu**: Thay thế trường mã kiểu bằng tham chiếu đến interface
4. **Di chuyển logic**: Di chuyển các phương thức phụ thuộc mã kiểu vào các lớp cụ thể
5. **Cập nhật client**: Sửa các vị trí sử dụng mã kiểu

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class Document {
    public static final int STATE_DRAFT = 1;
    public static final int STATE_MODERATION = 2;
    public static final int STATE_PUBLISHED = 3;
    
    private int state;
    private String content;
    
    public Document(String content) {
        this.content = content;
        this.state = STATE_DRAFT;
    }
    
    public void publish() {
        switch (state) {
            case STATE_DRAFT:
                if (isContentValid()) {
                    state = STATE_MODERATION;
                }
                break;
            case STATE_MODERATION:
                if (isModerated()) {
                    state = STATE_PUBLISHED;
                }
                break;
            case STATE_PUBLISHED:
                // Đã published - không làm gì
                break;
        }
    }
    
    public void reject() {
        switch (state) {
            case STATE_MODERATION:
                state = STATE_DRAFT;
                break;
            case STATE_PUBLISHED:
                state = STATE_MODERATION;
                break;
        }
    }
    
    public boolean canEdit() {
        return state == STATE_DRAFT || state == STATE_MODERATION;
    }
    
    private boolean isContentValid() {
        return content != null && content.length() > 0;
    }
    
    private boolean isModerated() {
        // Logic kiểm duyệt
        return true;
    }
}
```

### **✅ Sau khi thay thế bằng State Pattern:**
```java
// State interface
interface DocumentState {
    void publish(Document document);
    void reject(Document document);
    boolean canEdit();
}

// Concrete states
class DraftState implements DocumentState {
    @Override
    public void publish(Document document) {
        if (document.isContentValid()) {
            document.setState(new ModerationState());
        }
    }
    
    @Override
    public void reject(Document document) {
        // Draft không thể bị reject
    }
    
    @Override
    public boolean canEdit() {
        return true;
    }
    
    @Override
    public String toString() {
        return "Draft";
    }
}

class ModerationState implements DocumentState {
    @Override
    public void publish(Document document) {
        if (document.isModerated()) {
            document.setState(new PublishedState());
        }
    }
    
    @Override
    public void reject(Document document) {
        document.setState(new DraftState());
    }
    
    @Override
    public boolean canEdit() {
        return true;
    }
    
    @Override
    public String toString() {
        return "Moderation";
    }
}

class PublishedState implements DocumentState {
    @Override
    public void publish(Document document) {
        // Đã published - không làm gì
    }
    
    @Override
    public void reject(Document document) {
        document.setState(new ModerationState());
    }
    
    @Override
    public boolean canEdit() {
        return false;
    }
    
    @Override
    public String toString() {
        return "Published";
    }
}

// Document class với state pattern
class Document {
    private DocumentState state;
    private String content;
    
    public Document(String content) {
        this.content = content;
        this.state = new DraftState();
    }
    
    public void publish() {
        state.publish(this);
    }
    
    public void reject() {
        state.reject(this);
    }
    
    public boolean canEdit() {
        return state.canEdit();
    }
    
    // Phương thức để state thay đổi state
    void setState(DocumentState state) {
        this.state = state;
    }
    
    // Các phương thức helper cho states
    boolean isContentValid() {
        return content != null && content.length() > 0;
    }
    
    boolean isModerated() {
        // Logic kiểm duyệt
        return true;
    }
    
    public String getStateName() {
        return state.toString();
    }
}
```

## **Ví Dụ Với Strategy Pattern**

### **✅ Thay thế mã kiểu tính toán:**
```java
// Strategy interface
interface PricingStrategy {
    double calculatePrice(double basePrice, int quantity);
}

// Concrete strategies
class RegularPricing implements PricingStrategy {
    @Override
    public double calculatePrice(double basePrice, int quantity) {
        return basePrice * quantity;
    }
}

class BulkDiscountPricing implements PricingStrategy {
    private static final int BULK_THRESHOLD = 10;
    private static final double DISCOUNT_RATE = 0.1;
    
    @Override
    public double calculatePrice(double basePrice, int quantity) {
        double total = basePrice * quantity;
        if (quantity >= BULK_THRESHOLD) {
            total *= (1 - DISCOUNT_RATE);
        }
        return total;
    }
}

class SeasonalPricing implements PricingStrategy {
    private static final double SEASONAL_MULTIPLIER = 1.2;
    
    @Override
    public double calculatePrice(double basePrice, int quantity) {
        return basePrice * quantity * SEASONAL_MULTIPLIER;
    }
}

// Context class
class Order {
    private PricingStrategy pricingStrategy;
    private double basePrice;
    private int quantity;
    
    public Order(double basePrice, int quantity) {
        this.basePrice = basePrice;
        this.quantity = quantity;
        this.pricingStrategy = new RegularPricing(); // Mặc định
    }
    
    public void setPricingStrategy(PricingStrategy strategy) {
        this.pricingStrategy = strategy;
    }
    
    public double calculateTotal() {
        return pricingStrategy.calculatePrice(basePrice, quantity);
    }
}

// Sử dụng
Order order = new Order(100.0, 5);
System.out.println("Regular price: " + order.calculateTotal());

order.setPricingStrategy(new BulkDiscountPricing());
System.out.println("Bulk price: " + order.calculateTotal());

order.setPricingStrategy(new SeasonalPricing());
System.out.println("Seasonal price: " + order.calculateTotal());
```

## **Lợi Ích**

- **✅ Linh hoạt**: Có thể thay đổi hành vi trong thời gian chạy
- **✅ Tuân thủ OCP**: Dễ dàng thêm hành vi mới mà không sửa code hiện có
- **✅ Giảm điều kiện**: Loại bỏ các câu lệnh switch/case phức tạp
- **✅ Tách biệt concerns**: Mỗi lớp chỉ chịu trách nhiệm cho một hành vi
- **✅ Dễ test**: Có thể test từng strategy/state độc lập

## **Khi Nào Dùng State vs Strategy**

### **State Pattern:**
- Khi hành vi phụ thuộc vào trạng thái của đối tượng
- Khi có nhiều điều kiện kiểm tra trạng thái
- Khi trạng thái thay đổi và ảnh hưởng đến nhiều hành vi

### **Strategy Pattern:**
- Khi có nhiều thuật toán/thuộc tính khác nhau cho một tác vụ
- Khi muốn client chọn thuật toán cụ thể
- Khi cần tách biệt thuật toán khỏi logic nghiệp vụ

## **Điểm Cần Lưu Ý**

- **Performance**: Có thể tạo nhiều đối tượng, nhưng thường không thành vấn đề
- **Độ phức tạp**: Tăng số lượng lớp trong hệ thống
- **Quản lý vòng đời**: Cần quản lý việc tạo và hủy các state/strategy object

## **Khi Nào Không Nên Sử Dụng**

- Khi mã kiểu đơn giản và không thay đổi
- Khi chỉ có 1-2 trường hợp và không có kế hoạch mở rộng
- Khi performance là yếu tố quan trọng và cần tối ưu

## **Kỹ Thuật Liên Quan**

- **Replace Type Code with Subclasses**: Khi mã kiểu không thay đổi và có thể dùng kế thừa
- **Replace Conditional with Polymorphism**: Để thay thế các điều kiện phức tạp
- **State Pattern**: Cho các hành vi phụ thuộc trạng thái
- **Strategy Pattern**: Cho các thuật toán thay thế được

## **Kết Luận**

Thay thế mã kiểu bằng State/Strategy là kỹ thuật mạnh mẽ để xử lý các hành vi thay đổi động. Bằng cách chuyển đổi từ mã kiểu cứng nhắc sang các đối tượng linh hoạt, bạn tạo ra hệ thống dễ mở rộng, dễ bảo trì và tuân thủ tốt các nguyên tắc thiết kế hướng đối tượng.

**Nguồn:** [refactoring.guru/replace-type-code-with-state-strategy](https://refactoring.guru/replace-type-code-with-state-strategy)