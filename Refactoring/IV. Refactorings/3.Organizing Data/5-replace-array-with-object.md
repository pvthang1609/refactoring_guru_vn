# **Thay Thế Mảng Bằng Đối Tượng (Replace Array with Object)**

## **Định Nghĩa**
Thay thế mảng bằng đối tượng là kỹ thuật tái cấu trúc thay thế một mảng chứa các phần tử có ý nghĩa khác nhau bằng một đối tượng với các trường có tên rõ ràng.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một mảng chứa các phần tử có ý nghĩa khác nhau
- Các chỉ số mảng được sử dụng như "magic number" để truy cập dữ liệu cụ thể
- Code khó hiểu vì không rõ ý nghĩa của từng phần tử trong mảng

### **Dấu hiệu nhận biết:**
- Có comment giải thích ý nghĩa của từng chỉ số mảng
- Các hằng số được định nghĩa để đại diện cho chỉ số mảng
- Khó theo dõi và bảo trì code sử dụng mảng

## **Các Bước Thực Hiện**

1. **Tạo lớp mới**: Tạo lớp mới để chứa dữ liệu từ mảng
2. **Xác định trường**: Xác định các trường tương ứng với từng phần tử mảng
3. **Thay thế mảng**: Thay thế việc sử dụng mảng bằng đối tượng mới
4. **Cập nhật code**: Sửa tất cả các vị trí sử dụng mảng

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
// Sử dụng mảng - khó hiểu ý nghĩa các phần tử
String[] player = new String[3];
player[0] = "John Doe";    // Tên
player[1] = "25";          // Tuổi
player[2] = "forward";     // Vị trí

// Truy cập bằng magic number - khó đọc
String playerName = player[0];
int playerAge = Integer.parseInt(player[1]);
String position = player[2];

// Code xử lý phức tạp
if (player[2].equals("forward") && Integer.parseInt(player[1]) > 20) {
    System.out.println(player[0] + " có thể tham gia");
}
```

### **✅ Sau khi thay thế:**
```java
class Player {
    private String name;
    private int age;
    private String position;
    
    public Player(String name, int age, String position) {
        this.name = name;
        this.age = age;
        this.position = position;
    }
    
    // Getter methods
    public String getName() { return name; }
    public int getAge() { return age; }
    public String getPosition() { return position; }
    
    // Business logic
    public boolean canParticipate() {
        return "forward".equals(position) && age > 20;
    }
    
    // Setter methods nếu cần
    public void setPosition(String position) {
        this.position = position;
    }
}

// Sử dụng đối tượng - rõ ràng và dễ hiểu
Player player = new Player("John Doe", 25, "forward");

// Truy cập trực tiếp qua tên có ý nghĩa
String playerName = player.getName();
int playerAge = player.getAge();
String position = player.getPosition();

// Code rõ ràng hơn
if (player.canParticipate()) {
    System.out.println(player.getName() + " có thể tham gia");
}
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Mảng phức tạp với nhiều phần tử:**
```java
// Mảng chứa thông tin đơn hàng
Object[] orderData = new Object[5];
orderData[0] = "ORD-001";          // Mã đơn hàng
orderData[1] = "John Doe";         // Tên khách hàng
orderData[2] = 199.99;             // Tổng tiền
orderData[3] = new Date();         // Ngày đặt hàng
orderData[4] = "PROCESSING";       // Trạng thái

// Code xử lý rất khó đọc
if ("PROCESSING".equals(orderData[4]) && (Double)orderData[2] > 100.0) {
    System.out.println("Đơn hàng " + orderData[0] + " cần xử lý ưu tiên");
}

// Dễ bị lỗi chỉ số
String customerName = (String) orderData[1]; // Chỉ số 1 có thể nhầm
```

### **✅ Sau khi thay thế bằng đối tượng:**
```java
class Order {
    private String orderId;
    private String customerName;
    private double totalAmount;
    private Date orderDate;
    private OrderStatus status;
    
    public Order(String orderId, String customerName, double totalAmount, 
                 Date orderDate, OrderStatus status) {
        this.orderId = orderId;
        this.customerName = customerName;
        this.totalAmount = totalAmount;
        this.orderDate = orderDate;
        this.status = status;
    }
    
    // Getter methods
    public String getOrderId() { return orderId; }
    public String getCustomerName() { return customerName; }
    public double getTotalAmount() { return totalAmount; }
    public Date getOrderDate() { return orderDate; }
    public OrderStatus getStatus() { return status; }
    
    // Business logic
    public boolean isHighPriority() {
        return status == OrderStatus.PROCESSING && totalAmount > 100.0;
    }
    
    public boolean canBeCancelled() {
        return status == OrderStatus.PENDING || status == OrderStatus.PROCESSING;
    }
    
    // Setter methods
    public void setStatus(OrderStatus status) {
        this.status = status;
    }
}

enum OrderStatus {
    PENDING, PROCESSING, SHIPPED, DELIVERED, CANCELLED
}

// Sử dụng đối tượng - an toàn và rõ ràng
Order order = new Order("ORD-001", "John Doe", 199.99, new Date(), OrderStatus.PROCESSING);

// Code dễ đọc và an toàn
if (order.isHighPriority()) {
    System.out.println("Đơn hàng " + order.getOrderId() + " cần xử lý ưu tiên");
}

if (order.canBeCancelled()) {
    order.setStatus(OrderStatus.CANCELLED);
}
```

## **Lợi Ích**

- **✅ Code rõ ràng**: Tên trường thể hiện rõ ý nghĩa dữ liệu
- **✅ An toàn kiểu dữ liệu**: Tránh lỗi ép kiểu không an toàn
- **✅ Dễ bảo trì**: Thay đổi cấu trúc dữ liệu dễ dàng hơn
- **✅ Tái sử dụng**: Có thể thêm phương thức và logic nghiệp vụ
- **✅ Kiểm tra tại biên dịch**: Phát hiện lỗi sớm hơn runtime

## **Điểm Cần Lưu Ý**

- **Không áp dụng cho mảng đồng nhất**: Mảng chứa các phần tử cùng loại và ý nghĩa
- **Cân nhắc hiệu năng**: Trong một số trường hợp, mảng có thể hiệu quả hơn
- **Đảm bảo tính nhất quán**: Cập nhật tất cả các vị trí sử dụng mảng

## **Khi Nào Không Nên Sử Dụng**

- Khi mảng chứa các phần tử hoàn toàn đồng nhất (như danh sách số)
- Khi performance là yếu tố quan trọng và cần truy cập nhanh
- Khi làm việc với các API bên ngoài yêu cầu định dạng mảng

## **Kỹ Thuật Liên Quan**

- **Thay thế Giá trị Dữ liệu bằng Đối tượng (Replace Data Value with Object)**: Khi cần nâng cấp kiểu dữ liệu đơn giản
- **Thay thế Tham số bằng Đối tượng (Introduce Parameter Object)**: Khi có nhiều tham số liên quan
- **Trích xuất Lớp (Extract Class)**: Khi cần tách một phần của lớp thành lớp độc lập

## **Kết Luận**

Thay thế mảng bằng đối tượng là kỹ thuật quan trọng để cải thiện khả năng đọc và bảo trì code. Bằng cách chuyển đổi các mảng "đa nghĩa" thành các đối tượng có cấu trúc rõ ràng, bạn tạo ra code dễ hiểu, ít lỗi hơn và dễ dàng mở rộng khi yêu cầu thay đổi.