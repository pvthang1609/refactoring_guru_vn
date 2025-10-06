# **Thay Thế Lớp Con Bằng Trường (Replace Subclass with Fields)**

## **Định Nghĩa**
Thay thế lớp con bằng trường là kỹ thuật tái cấu trúc loại bỏ các lớp con chỉ khác biệt về giá trị trả về của một số phương thức, và thay thế chúng bằng các trường trong lớp cha.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Các lớp con chỉ khác nhau về giá trị trả về của một số phương thức
- Lớp con không có hành vi hoặc logic bổ sung nào khác
- Muốn đơn giản hóa hệ thống phân cấp không cần thiết

### **Không nên sử dụng khi:**
- Các lớp con có hành vi hoặc logic nghiệp vụ khác biệt
- Cần sử dụng tính đa hình thực sự
- Lớp con override các phương thức với implementation khác nhau

## **Các Bước Thực Hiện**

1. **Áp dụng Replace Constructor with Factory Method**: Tạo factory method trong lớp cha
2. **Thay thế bằng trường**: Thay thế các phương thức trong lớp con bằng trường trong lớp cha
3. **Tạo constructor**: Thêm constructor trong lớp cha để khởi tạo các trường
4. **Xóa lớp con**: Xóa các lớp con và cập nhật factory method
5. **Kiểm tra và dọn dẹp**: Xóa các phương thức abstract không còn cần thiết

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
abstract class Person {
    abstract boolean isMale();
    abstract String getCode();
}

class Male extends Person {
    @Override
    boolean isMale() {
        return true;
    }
    
    @Override
    String getCode() {
        return "M";
    }
}

class Female extends Person {
    @Override
    boolean isMale() {
        return false;
    }
    
    @Override
    String getCode() {
        return "F";
    }
}

// Sử dụng
Person male = new Male();
Person female = new Female();
System.out.println(male.isMale()); // true
System.out.println(female.isMale()); // false
```

### **✅ Sau khi thay thế:**
```java
class Person {
    private final boolean isMale;
    private final String code;
    
    // Constructor private
    private Person(boolean isMale, String code) {
        this.isMale = isMale;
        this.code = code;
    }
    
    // Factory methods
    public static Person createMale() {
        return new Person(true, "M");
    }
    
    public static Person createFemale() {
        return new Person(false, "F");
    }
    
    // Getter methods
    public boolean isMale() {
        return isMale;
    }
    
    public String getCode() {
        return code;
    }
}

// Sử dụng
Person male = Person.createMale();
Person female = Person.createFemale();
System.out.println(male.isMale()); // true
System.out.println(female.isMale()); // false
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Nhiều lớp con với các giá trị khác nhau:**
```java
abstract class BloodType {
    abstract String getType();
    abstract boolean canDonateTo(BloodType other);
    abstract boolean canReceiveFrom(BloodType other);
}

class BloodTypeA extends BloodType {
    @Override
    String getType() {
        return "A";
    }
    
    @Override
    boolean canDonateTo(BloodType other) {
        return other instanceof BloodTypeA || other instanceof BloodTypeAB;
    }
    
    @Override
    boolean canReceiveFrom(BloodType other) {
        return other instanceof BloodTypeA || other instanceof BloodTypeO;
    }
}

class BloodTypeB extends BloodType {
    @Override
    String getType() {
        return "B";
    }
    
    @Override
    boolean canDonateTo(BloodType other) {
        return other instanceof BloodTypeB || other instanceof BloodTypeAB;
    }
    
    @Override
    boolean canReceiveFrom(BloodType other) {
        return other instanceof BloodTypeB || other instanceof BloodTypeO;
    }
}

class BloodTypeO extends BloodType {
    @Override
    String getType() {
        return "O";
    }
    
    @Override
    boolean canDonateTo(BloodType other) {
        return true; // O có thể cho tất cả
    }
    
    @Override
    boolean canReceiveFrom(BloodType other) {
        return other instanceof BloodTypeO; // O chỉ nhận từ O
    }
}

class BloodTypeAB extends BloodType {
    @Override
    String getType() {
        return "AB";
    }
    
    @Override
    boolean canDonateTo(BloodType other) {
        return other instanceof BloodTypeAB; // AB chỉ cho AB
    }
    
    @Override
    boolean canReceiveFrom(BloodType other) {
        return true; // AB có thể nhận từ tất cả
    }
}
```

### **✅ Sau khi thay thế bằng enum:**
```java
enum BloodType {
    O("O", true, false),
    A("A", false, false),
    B("B", false, false),
    AB("AB", false, true);
    
    private final String type;
    private final boolean universalDonor;
    private final boolean universalRecipient;
    
    BloodType(String type, boolean universalDonor, boolean universalRecipient) {
        this.type = type;
        this.universalDonor = universalDonor;
        this.universalRecipient = universalRecipient;
    }
    
    public String getType() {
        return type;
    }
    
    public boolean canDonateTo(BloodType other) {
        if (this.universalDonor) return true;
        if (other.universalRecipient) return true;
        
        // Logic đơn giản hóa cho nhóm máu
        switch (this) {
            case A: return other == A || other == AB;
            case B: return other == B || other == AB;
            case AB: return other == AB;
            default: return false;
        }
    }
    
    public boolean canReceiveFrom(BloodType other) {
        if (this.universalRecipient) return true;
        if (other.universalDonor) return true;
        
        // Logic đơn giản hóa cho nhóm máu
        switch (this) {
            case A: return other == A || other == O;
            case B: return other == B || other == O;
            case O: return other == O;
            default: return false;
        }
    }
    
    // Factory method
    public static BloodType fromString(String type) {
        for (BloodType bloodType : values()) {
            if (bloodType.type.equalsIgnoreCase(type)) {
                return bloodType;
            }
        }
        throw new IllegalArgumentException("Nhóm máu không hợp lệ: " + type);
    }
}

// Sử dụng
BloodType typeA = BloodType.A;
BloodType typeO = BloodType.O;

System.out.println(typeA.canReceiveFrom(typeO)); // true
System.out.println(typeO.canDonateTo(typeA));    // true
```

## **Lợi Ích**

- **✅ Đơn giản hóa kiến trúc**: Giảm số lượng lớp không cần thiết
- **✅ Dễ bảo trì**: Logic tập trung ở một nơi
- **✅ Hiệu suất**: Tránh overhead của việc tạo nhiều lớp con
- **✅ Linh hoạt**: Dễ dàng thêm loại mới (trong enum)

## **Điểm Cần Lưu Ý**

- **Chỉ áp dụng khi phù hợp**: Không sử dụng nếu lớp con có hành vi thực sự khác biệt
- **Cân nhắc sử dụng enum**: Enum thường là lựa chọn tốt cho các giá trị cố định
- **Không làm mất tính đa hình**: Đảm bảo không loại bỏ tính đa hình thực sự cần thiết

## **Khi Nào Không Nên Sử Dụng**

- Khi các lớp con có implementation khác nhau cho các phương thức
- Khi cần mở rộng hành vi thông qua kế thừa
- Khi có sự khác biệt đáng kể về logic nghiệp vụ giữa các lớp con

## **Kỹ Thuật Liên Quan**

- **Replace Type Code with Class/Enum**: Khi có mã kiểu cần thay thế
- **Replace Constructor with Factory Method**: Để tạo đối tượng thay cho constructor
- **Inline Class**: Khi cần hợp nhất các lớp không cần thiết

## **Kết Luận**

Thay thế lớp con bằng trường là kỹ thuật hữu ích để đơn giản hóa hệ thống khi các lớp con chỉ khác biệt về giá trị dữ liệu mà không có hành vi khác biệt. Bằng cách chuyển đổi sang các trường hoặc enum, bạn tạo ra code gọn gàng hơn, dễ bảo trì hơn mà vẫn giữ được tính rõ ràng. Tuy nhiên, cần cân nhắc kỹ để không vô tình loại bỏ tính đa hình thực sự cần thiết.