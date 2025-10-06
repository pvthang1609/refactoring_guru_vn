# **Tổ Chức Dữ Liệu (Organizing Data)**

## **Giới Thiệu**
Nhóm kỹ thuật tái cấu trúc giúp tổ chức và quản lý dữ liệu hiệu quả hơn, cải thiện tính đóng gói và giảm sự phụ thuộc không cần thiết giữa các đối tượng.

## **Các Kỹ Thuật Chính**

### **1. Tự Đóng Gói Trường (Self Encapsulate Field)**
**Mục đích:** Tạo getter/setter cho trường và chỉ sử dụng chúng để truy cập trường ngay cả trong cùng lớp.

**Khi nào sử dụng:**
- Khi cần thêm logic khi truy cập trường (validation, logging)
- Khi kế thừa lớp và cần ghi đè cách truy cập trường

```java
// ❌ Trước
class Account {
    private double balance;
    
    public void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
        }
    }
}

// ✅ Sau
class Account {
    private double balance;
    
    public void withdraw(double amount) {
        if (amount <= getBalance()) {
            setBalance(getBalance() - amount);
        }
    }
    
    private double getBalance() {
        return balance;
    }
    
    private void setBalance(double balance) {
        this.balance = balance;
    }
}
```

### **2. Thay Thế Kiểu Mã Bằng Lớp Con (Replace Type Code with Subclasses)**
**Mục đích:** Tạo lớp con cho mỗi giá trị của kiểu mã.

**Khi nào sử dụng:**
- Có kiểu mã ảnh hưởng đến hành vi
- Giá trị kiểu mã không thay đổi sau khi tạo đối tượng

```java
// ❌ Trước
class Employee {
    private static final int ENGINEER = 0;
    private static final int MANAGER = 1;
    private int type;
    
    public int getType() {
        return type;
    }
}

// ✅ Sau
abstract class Employee {
    abstract int getType();
}

class Engineer extends Employee {
    int getType() {
        return ENGINEER;
    }
}

class Manager extends Employee {
    int getType() {
        return MANAGER;
    }
}
```

### **3. Thay Thế Kiểu Mã Bằng State/Strategy (Replace Type Code with State/Strategy)**
**Mục đích:** Sử dụng mẫu State hoặc Strategy thay vì kiểu mã.

**Khi nào sử dụng:**
- Kiểu mã thay đổi trong thời gian chạy
- Cần tránh tạo quá nhiều lớp con

```java
// ❌ Trước
class Order {
    private String status; // "pending", "shipped", "delivered"
    
    public void process() {
        if ("pending".equals(status)) {
            // logic xử lý
        } else if ("shipped".equals(status)) {
            // logic khác
        }
    }
}

// ✅ Sau
interface OrderState {
    void process(Order order);
}

class PendingState implements OrderState {
    public void process(Order order) {
        // logic xử lý pending
    }
}

class Order {
    private OrderState state;
    
    public void process() {
        state.process(this);
    }
}
```

### **4. Thay Thế Trường Bằng Đối Tượng (Replace Data Value with Object)**
**Mục đích:** Thay thế kiểu dữ liệu đơn giản bằng đối tượng khi dữ liệu trở nên phức tạp.

```java
// ❌ Trước
class Customer {
    private String address; // chỉ là string
}

// ✅ Sau
class Customer {
    private Address address; // đối tượng chuyên biệt
}

class Address {
    private String street;
    private String city;
    private String zipCode;
}
```

### **5. Thay Thế Mảng Bằng Đối Tượng (Replace Array with Object)**
**Mục đích:** Thay thế mảng bằng đối tượng khi các phần tử mảng có ý nghĩa khác nhau.

```java
// ❌ Trước
String[] player = new String[3];
player[0] = "John"; // name
player[1] = "25";   // age
player[2] = "forward"; // position

// ✅ Sau
class Player {
    private String name;
    private int age;
    private String position;
}
```

### **6. Thay Thế Biến Số Bằng Hằng Số (Replace Magic Number with Symbolic Constant)**
**Mục đích:** Thay thế các số "ma thuật" bằng hằng số có tên ý nghĩa.

```java
// ❌ Trước
double circumference = 2 * 3.14159 * radius;
if (status == 1) { // 1 có nghĩa là gì?
    // ...
}

// ✅ Sau
final double PI = 3.14159;
final int STATUS_ACTIVE = 1;

double circumference = 2 * PI * radius;
if (status == STATUS_ACTIVE) {
    // ...
}
```

## **Lợi Ích**

✅ **Tính đóng gói tốt hơn** - Kiểm soát truy cập dữ liệu chặt chẽ  
✅ **Code linh hoạt** - Dễ dàng thay đổi và mở rộng  
✅ **Giảm sự phụ thuộc** - Các lớp ít phụ thuộc vào implementation chi tiết  
✅ **Dễ bảo trì** - Thay đổi cấu trúc dữ liệu ít ảnh hưởng đến code khác  
✅ **Tái sử dụng tốt** - Các đối tượng dữ liệu có thể sử dụng ở nhiều nơi  

## **Khi Nào Sử Dụng**

- Khi phát hiện "Primitive Obsession" - ám ảnh sử dụng kiểu nguyên thủy
- Khi có các "magic number" hoặc "magic string" trong code
- Khi cấu trúc dữ liệu hiện tại không đủ biểu đạt ý nghĩa kinh doanh
- Khi cần thêm validation hoặc logic xử lý khi truy cập dữ liệu

## **Kết Luận**

Tổ chức dữ liệu hiệu quả là nền tảng cho thiết kế hướng đối tượng tốt. Bằng cách áp dụng các kỹ thuật này, bạn tạo ra hệ thống dễ hiểu, dễ bảo trì và linh hoạt trước các thay đổi yêu cầu. Hãy bắt đầu với việc nhận diện các "mùi code" liên quan đến dữ liệu và áp dụng kỹ thuật phù hợp để cải thiện.