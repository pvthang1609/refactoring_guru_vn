# **Thay Thế Mã Kiểu Bằng Lớp (Replace Type Code with Class)**

## **Định Nghĩa**
Thay thế mã kiểu bằng lớp là kỹ thuật tái cấu trúc thay thế các số hoặc chuỗi đại diện cho kiểu dữ liệu bằng một lớp chuyên biệt để tăng tính an toàn kiểu dữ liệu và khả năng biểu đạt của code.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các hằng số số hoặc chuỗi đại diện cho các loại khác nhau
- Mã kiểu không an toàn (có thể nhầm lẫn giá trị)
- Cần thêm hành vi hoặc validation cho các kiểu

### **Dấu hiệu nhận biết:**
- Các hằng số như `TYPE_ADMIN = 1`, `TYPE_USER = 2`
- Tham số kiểu int hoặc String có thể nhận nhiều giá trị không liên quan
- Có switch-case hoặc if-else dựa trên mã kiểu

## **Các Bước Thực Hiện**

1. **Tạo lớp mới**: Tạo lớp mới để đại diện cho mã kiểu
2. **Thay thế mã kiểu**: Thay thế sử dụng mã kiểu cũ bằng lớp mới
3. **Thêm hành vi**: Di chuyển các phương thức liên quan đến mã kiểu vào lớp mới
4. **Cập nhật client**: Sửa tất cả các vị trí sử dụng mã kiểu cũ

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class User {
    public static final int TYPE_ADMIN = 1;
    public static final int TYPE_MODERATOR = 2;
    public static final int TYPE_USER = 3;
    
    private String name;
    private int type;  // Mã kiểu số - không an toàn
    
    public User(String name, int type) {
        this.name = name;
        this.type = type;
    }
    
    public boolean canDeleteContent() {
        return type == TYPE_ADMIN || type == TYPE_MODERATOR;
    }
    
    public boolean canManageUsers() {
        return type == TYPE_ADMIN;
    }
    
    // Có thể truyền bất kỳ số nào - không an toàn
}

// Sử dụng không an toàn
User user1 = new User("Admin", User.TYPE_ADMIN);  // Đúng
User user2 = new User("Hacker", 999);            // Sai nhưng không bị phát hiện
```

### **✅ Sau khi thay thế:**
```java
// Lớp đại diện cho kiểu người dùng
class UserType {
    private final String name;
    private final int level;
    
    // Các instance được định nghĩa trước
    public static final UserType ADMIN = new UserType("ADMIN", 1);
    public static final UserType MODERATOR = new UserType("MODERATOR", 2);
    public static final UserType USER = new UserType("USER", 3);
    
    private UserType(String name, int level) {
        this.name = name;
        this.level = level;
    }
    
    // Phương thức factory để tạo từ chuỗi (nếu cần)
    public static UserType fromString(String type) {
        switch (type.toUpperCase()) {
            case "ADMIN": return ADMIN;
            case "MODERATOR": return MODERATOR;
            case "USER": return USER;
            default: throw new IllegalArgumentException("Kiểu người dùng không hợp lệ: " + type);
        }
    }
    
    // Business logic
    public boolean canDeleteContent() {
        return this == ADMIN || this == MODERATOR;
    }
    
    public boolean canManageUsers() {
        return this == ADMIN;
    }
    
    public boolean hasHigherPermissionThan(UserType other) {
        return this.level < other.level; // level thấp hơn = quyền cao hơn
    }
    
    // Getter
    public String getName() {
        return name;
    }
    
    @Override
    public String toString() {
        return name;
    }
}

class User {
    private String name;
    private UserType type;  // Sử dụng lớp thay vì số
    
    public User(String name, UserType type) {
        if (type == null) {
            throw new IllegalArgumentException("Kiểu người dùng không được null");
        }
        this.name = name;
        this.type = type;
    }
    
    // Delegation đến UserType
    public boolean canDeleteContent() {
        return type.canDeleteContent();
    }
    
    public boolean canManageUsers() {
        return type.canManageUsers();
    }
    
    public String getName() {
        return name;
    }
    
    public UserType getType() {
        return type;
    }
}

// Sử dụng an toàn
User user1 = new User("Admin", UserType.ADMIN);        // Đúng
User user2 = new User("Moderator", UserType.MODERATOR); // Đúng
// User user3 = new User("Hacker", 999);               // LỖI BIÊN DỊCH - không thể truyền số

// Có thể sử dụng các phương thức
if (user1.getType().canManageUsers()) {
    System.out.println("Có thể quản lý người dùng");
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với enum (Java 5+):**
```java
// Sử dụng enum - cách tiếp cận hiện đại hơn
enum UserType {
    ADMIN(1, "Administrator") {
        @Override
        public boolean canDeleteContent() { return true; }
        @Override
        public boolean canManageUsers() { return true; }
    },
    MODERATOR(2, "Moderator") {
        @Override
        public boolean canDeleteContent() { return true; }
        @Override
        public boolean canManageUsers() { return false; }
    },
    USER(3, "User") {
        @Override
        public boolean canDeleteContent() { return false; }
        @Override
        public boolean canManageUsers() { return false; }
    };
    
    private final int code;
    private final String displayName;
    
    UserType(int code, String displayName) {
        this.code = code;
        this.displayName = displayName;
    }
    
    // Abstract methods - mỗi enum phải implement
    public abstract boolean canDeleteContent();
    public abstract boolean canManageUsers();
    
    // Có thể thêm các phương thức cụ thể
    public boolean hasHigherPermissionThan(UserType other) {
        return this.code < other.code;
    }
    
    // Getter
    public int getCode() { return code; }
    public String getDisplayName() { return displayName; }
    
    // Tìm theo code
    public static UserType fromCode(int code) {
        for (UserType type : values()) {
            if (type.code == code) {
                return type;
            }
        }
        throw new IllegalArgumentException("Mã không hợp lệ: " + code);
    }
}

class User {
    private String name;
    private UserType type;
    
    public User(String name, UserType type) {
        this.name = name;
        this.type = type;
    }
    
    public boolean canDeleteContent() {
        return type.canDeleteContent();
    }
    
    public boolean canManageUsers() {
        return type.canManageUsers();
    }
    
    // Getter
    public String getName() { return name; }
    public UserType getType() { return type; }
}
```

## **Lợi Ích**

- **✅ An toàn kiểu dữ liệu**: Không thể truyền giá trị không hợp lệ
- **✅ Code rõ ràng**: Tên có ý nghĩa thay vì số hoặc chuỗi
- **✅ Dễ mở rộng**: Dễ dàng thêm loại mới và hành vi
- **✅ Tập trung logic**: Logic liên quan đến kiểu được nhóm lại một chỗ
- **✅ Dễ test**: Có thể test từng kiểu độc lập

## **Điểm Cần Lưu Ý**

- **Sử dụng enum khi có thể**: Enum trong Java cung cấp nhiều tính năng hữu ích
- **Cân nhắc performance**: Với số lượng nhỏ, sự khác biệt không đáng kể
- **Không lạm dụng**: Với các mã kiểu đơn giản, đôi khi số/chuỗi là đủ

## **Khi Nào Không Nên Sử Dụng**

- Khi mã kiểu thực sự đơn giản và không có hành vi liên quan
- Khi mã kiểu chỉ được sử dụng tạm thời và cục bộ
- Khi performance là yếu tố quan trọng và cần tối ưu

## **Kỹ Thuật Liên Quan**

- **Replace Type Code with Subclasses**: Khi các kiểu có hành vi khác nhau đáng kể
- **Replace Type Code with State/Strategy**: Khi kiểu có thể thay đổi trong thời gian chạy
- **Introduce Parameter Object**: Khi có nhiều tham số liên quan

## **Kết Luận**

Thay thế mã kiểu bằng lớp là kỹ thuật quan trọng để cải thiện tính an toàn và khả năng bảo trì của code. Bằng cách chuyển đổi các số và chuỗi "ma thuật" thành các lớp có tên ý nghĩa, bạn tạo ra code dễ đọc hơn, ít lỗi hơn và dễ dàng mở rộng khi yêu cầu thay đổi.