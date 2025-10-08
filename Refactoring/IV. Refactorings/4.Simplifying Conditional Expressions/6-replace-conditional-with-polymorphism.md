# **Thay Thế Điều Kiện Bằng Đa Hình (Replace Conditional with Polymorphism)**

## **Định Nghĩa**
Thay thế điều kiện bằng đa hình là kỹ thuật tái cấu trúc thay thế các câu lệnh điều kiện phức tạp (switch-case, if-else) bằng tính đa hình trong hướng đối tượng.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các câu lệnh điều kiện phức tạp dựa trên kiểu của đối tượng
- Cần thêm loại mới thường xuyên và không muốn sửa đổi code hiện có
- Các hành vi khác nhau dựa trên loại đối tượng

### **Không nên sử dụng khi:**
- Điều kiện đơn giản và không thay đổi thường xuyên
- Chỉ có một vài trường hợp và không có kế hoạch mở rộng

## **Các Bước Thực Hiện**

1. **Tạo hệ thống phân cấp**: Tạo lớp cơ sở và các lớp con cho mỗi loại
2. **Di chuyển phương thức**: Di chuyển phương thức chứa điều kiện vào lớp cơ sở
3. **Ghi đè phương thức**: Trong mỗi lớp con, ghi đè phương thức với hành vi cụ thể
4. **Xóa điều kiện**: Thay thế lời gọi điều kiện bằng đa hình

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class Bird {
    private String type;
    private int numberOfCoconuts;
    private boolean isNailed;
    private double voltage;
    
    public double getSpeed() {
        switch (type) {
            case "EUROPEAN":
                return getBaseSpeed();
            case "AFRICAN":
                return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
            case "NORWEGIAN_BLUE":
                return (isNailed) ? 0 : getBaseSpeed(voltage);
            default:
                throw new RuntimeException("Unknown bird type");
        }
    }
    
    public String getPlumage() {
        switch (type) {
            case "EUROPEAN":
                return "average";
            case "AFRICAN":
                return (numberOfCoconuts > 2) ? "tired" : "average";
            case "NORWEGIAN_BLUE":
                return (voltage > 100) ? "scorched" : "beautiful";
            default:
                return "unknown";
        }
    }
}
```

### **✅ Sau khi thay thế:**
```java
// Lớp cơ sở
abstract class Bird {
    public abstract double getSpeed();
    public abstract String getPlumage();
    
    protected double getBaseSpeed() {
        return 10.0;
    }
}

// Các lớp con
class EuropeanBird extends Bird {
    @Override
    public double getSpeed() {
        return getBaseSpeed();
    }
    
    @Override
    public String getPlumage() {
        return "average";
    }
}

class AfricanBird extends Bird {
    private int numberOfCoconuts;
    
    public AfricanBird(int numberOfCoconuts) {
        this.numberOfCoconuts = numberOfCoconuts;
    }
    
    @Override
    public double getSpeed() {
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
    }
    
    @Override
    public String getPlumage() {
        return (numberOfCoconuts > 2) ? "tired" : "average";
    }
    
    private double getLoadFactor() {
        return 1.5;
    }
}

class NorwegianBlueBird extends Bird {
    private boolean isNailed;
    private double voltage;
    
    public NorwegianBlueBird(boolean isNailed, double voltage) {
        this.isNailed = isNailed;
        this.voltage = voltage;
    }
    
    @Override
    public double getSpeed() {
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
    
    @Override
    public String getPlumage() {
        return (voltage > 100) ? "scorched" : "beautiful";
    }
    
    private double getBaseSpeed(double voltage) {
        return Math.min(voltage / 10, 20.0);
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với factory method:**
```java
// Factory để tạo bird
class BirdFactory {
    public static Bird createBird(String type, int numberOfCoconuts, boolean isNailed, double voltage) {
        switch (type) {
            case "EUROPEAN":
                return new EuropeanBird();
            case "AFRICAN":
                return new AfricanBird(numberOfCoconuts);
            case "NORWEGIAN_BLUE":
                return new NorwegianBlueBird(isNailed, voltage);
            default:
                throw new IllegalArgumentException("Unknown bird type: " + type);
        }
    }
}

// Sử dụng
Bird european = BirdFactory.createBird("EUROPEAN", 0, false, 0);
Bird african = BirdFactory.createBird("AFRICAN", 3, false, 0);

System.out.println("European speed: " + european.getSpeed());
System.out.println("African plumage: " + african.getPlumage());
```

## **Lợi Ích**

- **✅ Tuân thủ OCP**: Có thể thêm loại mới mà không sửa code hiện có
- **✅ Code rõ ràng**: Mỗi lớp chỉ chịu trách nhiệm cho hành vi của mình
- **✅ Dễ bảo trì**: Thay đổi hành vi chỉ ảnh hưởng đến lớp liên quan
- **✅ Dễ test**: Có thể test từng lớp con độc lập
- **✅ Giảm điều kiện**: Loại bỏ các câu lệnh switch/case phức tạp

## **Điểm Cần Lưu Ý**

- **Không lạm dụng**: Với các điều kiện đơn giản, đôi khi if/else là đủ
- **Cân nhắc độ phức tạp**: Thêm nhiều lớp con có thể làm hệ thống phức tạp hơn
- **Sử dụng factory**: Để che giấu việc tạo đối tượng cụ thể

## **Khi Nào Không Nên Sử Dụng**

- Khi chỉ có 1-2 trường hợp đơn giản
- Khi các hành vi không thực sự khác biệt nhiều
- Khi performance là yếu tố quan trọng và cần tối ưu

## **Biến Thể**

### **✅ Sử dụng interface:**
```java
interface Flyable {
    double getSpeed();
    String getPlumage();
}

class EuropeanBird implements Flyable {
    public double getSpeed() { return 10.0; }
    public String getPlumage() { return "average"; }
}

class AfricanBird implements Flyable {
    private int numberOfCoconuts;
    
    public AfricanBird(int numberOfCoconuts) {
        this.numberOfCoconuts = numberOfCoconuts;
    }
    
    public double getSpeed() {
        return 10.0 - 1.5 * numberOfCoconuts;
    }
    
    public String getPlumage() {
        return (numberOfCoconuts > 2) ? "tired" : "average";
    }
}
```

## **Kỹ Thuật Liên Quan**

- **Replace Type Code with Subclasses**: Thay thế mã kiểu bằng lớp con
- **Replace Conditional with State/Strategy**: Thay thế điều kiện bằng state/strategy
- **Factory Pattern**: Để tạo đối tượng thay cho constructor

## **Kết Luận**

Thay thế điều kiện bằng đa hình là kỹ thuật mạnh mẽ để xử lý các hành vi khác nhau dựa trên loại đối tượng. Bằng cách chuyển đổi từ các câu lệnh điều kiện cứng nhắc sang hệ thống phân cấp lớp linh hoạt, bạn tạo ra code dễ mở rộng, dễ bảo trì và tuân thủ tốt các nguyên tắc SOLID.

**Nguồn:** [refactoring.guru/replace-conditional-with-polymorphism](https://refactoring.guru/replace-conditional-with-polymorphism)