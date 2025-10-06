# **Giới Thiệu Phương Thức Ngoại Lai (Introduce Foreign Method)**

## **Định Nghĩa**
Giới thiệu phương thức ngoại lai là kỹ thuật tái cấu trúc tạo một phương thức trong lớp client để thực hiện chức năng mà lẽ ra nên có trong lớp server, khi bạn không thể sửa đổi lớp server.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Bạn cần thực hiện một chức năng trên lớp server nhưng không thể sửa đổi nó (third-party library, sealed class)
- Chức năng này chỉ được sử dụng trong một lớp client cụ thể
- Không muốn tạo một lớp con chỉ để thêm một phương thức đơn giản

### **Dấu hiệu nhận biết:**
- Code lặp lại việc kết hợp các lời gọi phương thức từ lớp server
- Có logic xử lý phức tạp trên dữ liệu của lớp server
- Không thể thêm phương thức trực tiếp vào lớp server

## **Các Bước Thực Hiện**

1. **Xác định chức năng thiếu**: Tìm code client cần chức năng mà lớp server nên có
2. **Tạo phương thức trong client**: Tạo phương thức mới trong lớp client
3. **Truyền đối tượng server làm tham số**: Đối tượng server trở thành tham số chính
4. **Chuyển code vào phương thức**: Di chuyển code xử lý vào phương thức mới
5. **Thay thế code gốc**: Thay thế code trùng lặp bằng lời gọi phương thức mới

## **Ví Dụ Minh Họa**

### **❌ Trước khi giới thiệu phương thức ngoại lai:**
```java
// Lớp Date từ thư viện - không thể sửa đổi
Date startDate = new Date();

// Client code phức tạp để thêm ngày
Calendar calendar = Calendar.getInstance();
calendar.setTime(startDate);
calendar.add(Calendar.DATE, 7); // Thêm 7 ngày
Date nextWeek = calendar.getTime();

// Code lặp lại ở nhiều nơi
Calendar calendar2 = Calendar.getInstance();
calendar2.setTime(startDate);
calendar2.add(Calendar.DATE, 30); // Thêm 30 ngày
Date nextMonth = calendar2.getTime();
```

### **✅ Sau khi giới thiệu phương thức ngoại lai:**
```java
// Lớp tiện ích với phương thức ngoại lai
class DateUtils {
    
    // Phương thức ngoại lai - thêm chức năng cho Date
    public static Date addDays(Date date, int days) {
        Calendar calendar = Calendar.getInstance();
        calendar.setTime(date);
        calendar.add(Calendar.DATE, days);
        return calendar.getTime();
    }
    
    // Có thể thêm các phương thức ngoại lai khác
    public static Date addMonths(Date date, int months) {
        Calendar calendar = Calendar.getInstance();
        calendar.setTime(date);
        calendar.add(Calendar.MONTH, months);
        return calendar.getTime();
    }
}

// Client code trở nên đơn giản
Date startDate = new Date();
Date nextWeek = DateUtils.addDays(startDate, 7);
Date nextMonth = DateUtils.addDays(startDate, 30);
```

## **Ví Dụ Thực Tế Khác**

### **❌ Xử lý string phức tạp:**
```java
// Client code xử lý string
String name = "  john doe  ";
String formattedName = name.trim().toLowerCase();
formattedName = formattedName.substring(0, 1).toUpperCase() + 
                formattedName.substring(1);

// Lặp lại ở nơi khác
String productName = "  laptop computer  ";
String formattedProduct = productName.trim().toLowerCase();
formattedProduct = formattedProduct.substring(0, 1).toUpperCase() + 
                   formattedProduct.substring(1);
```

### **✅ Với phương thức ngoại lai:**
```java
// Lớp tiện ích cho String
class StringUtils {
    
    // Phương thức ngoại lai cho String
    public static String toTitleCase(String input) {
        if (input == null || input.trim().isEmpty()) {
            return input;
        }
        String trimmed = input.trim().toLowerCase();
        return trimmed.substring(0, 1).toUpperCase() + 
               trimmed.substring(1);
    }
    
    public static boolean isNullOrEmpty(String input) {
        return input == null || input.trim().isEmpty();
    }
}

// Client code đơn giản
String name = "  john doe  ";
String formattedName = StringUtils.toTitleCase(name);

String productName = "  laptop computer  ";
String formattedProduct = StringUtils.toTitleCase(productName);
```

## **Ví Dụ Với Đối Tượng Phức Tạp**

### **❌ Xử lý danh sách phức tạp:**
```java
List<Order> orders = getOrders();

// Tính tổng tiền - code lặp lại
double total = 0;
for (Order order : orders) {
    if (order.getStatus() == OrderStatus.COMPLETED) {
        total += order.getAmount();
    }
}

// Đếm số đơn hàng completed - code tương tự
int completedCount = 0;
for (Order order : orders) {
    if (order.getStatus() == OrderStatus.COMPLETED) {
        completedCount++;
    }
}
```

### **✅ Với phương thức ngoại lai:**
```java
class OrderUtils {
    
    // Phương thức ngoại lai cho List<Order>
    public static double calculateTotalCompleted(List<Order> orders) {
        return orders.stream()
            .filter(order -> order.getStatus() == OrderStatus.COMPLETED)
            .mapToDouble(Order::getAmount)
            .sum();
    }
    
    public static long countCompletedOrders(List<Order> orders) {
        return orders.stream()
            .filter(order -> order.getStatus() == OrderStatus.COMPLETED)
            .count();
    }
    
    public static List<Order> getPendingOrders(List<Order> orders) {
        return orders.stream()
            .filter(order -> order.getStatus() == OrderStatus.PENDING)
            .collect(Collectors.toList());
    }
}

// Client code đơn giản
List<Order> orders = getOrders();
double total = OrderUtils.calculateTotalCompleted(orders);
long completedCount = OrderUtils.countCompletedOrders(orders);
List<Order> pendingOrders = OrderUtils.getPendingOrders(orders);
```

## **Lợi Ích**

- **✅ Giảm code trùng lặp**: Tập trung logic vào một nơi
- **✅ Code rõ ràng hơn**: Client code trở nên dễ đọc và dễ hiểu
- **✅ Dễ bảo trì**: Thay đổi logic chỉ ở một chỗ
- **✅ Tái sử dụng**: Có thể sử dụng ở nhiều nơi trong ứng dụng

## **Điểm Cần Lưu Ý**

- **Đặt tên rõ ràng**: Tên phương thức nên thể hiện rõ chức năng
- **Nhóm theo chức năng**: Tạo các lớp tiện ích theo nhóm chức năng liên quan
- **Tránh lạm dụng**: Chỉ sử dụng khi không thể sửa đổi lớp server
- **Xem xét Local Extension**: Nếu cần nhiều phương thức, cân nhắc tạo lớp mở rộng

## **Khi Nên Chuyển Sang Local Extension**

- Khi cần thêm nhiều phương thức ngoại lai cho cùng một lớp
- Khi muốn gom nhóm các chức năng liên quan
- Khi cần kế thừh hoặc mở rộng thêm chức năng phức tạp

## **Kỹ Thuật Liên Quan**

- **Giới Thiệu Local Extension (Introduce Local Extension)**: Khi cần nhiều phương thức hơn
- **Trích xuất Phương thức (Extract Method)**: Để tạo phương thức từ code trùng lặp
- **Di chuyển Phương thức (Move Method)**: Khi có thể sửa đổi lớp server

## **Kết Luận**

Giới thiệu phương thức ngoại lai là giải pháp linh hoạt khi bạn cần thêm chức năng cho lớp không thể sửa đổi. Kỹ thuật này giúp giảm code trùng lặp và cải thiện khả năng bảo trì, nhưng cần sử dụng hợp lý để tránh tạo ra quá nhiều phương thức rời rạc.