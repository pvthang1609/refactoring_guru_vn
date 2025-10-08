# **Đổi Tên Phương Thức (Rename Method)**

## **Định Nghĩa**
Đổi tên phương thức là kỹ thuật tái cấu trúc thay đổi tên của phương thức thành tên mới rõ ràng và dễ hiểu hơn, phản ánh chính xác mục đích của phương thức.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Tên phương thức không mô tả rõ những gì phương thức thực hiện
- Tên phương thức gây hiểu nhầm về chức năng
- Code được sử dụng rộng rãi và cần cải thiện tính rõ ràng
- Tên phương thức quá ngắn hoặc sử dụng từ viết tắt khó hiểu

### **Không nên sử dụng khi:**
- Tên hiện tại đã đủ rõ ràng và mô tả đúng chức năng
- Phương thức ít được sử dụng và không ảnh hưởng đến khả năng đọc code
- Không có đủ thời gian để cập nhật tất cả các lời gọi

## **Các Bước Thực Hiện**

1. **Kiểm tra phương thức**: Xác định phương thức cần đổi tên
2. **Tạo tên mới**: Chọn tên mô tả rõ ràng mục đích của phương thức
3. **Tìm tất cả lời gọi**: Tìm tất cả các nơi gọi phương thức này
4. **Thay đổi tên**: Đổi tên phương thức và cập nhật tất cả lời gọi
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi đổi tên:**
```java
class Customer {
    private String name;
    private List<Order> orders;
    
    // Tên phương thức không rõ ràng
    public double getInv() {
        double total = 0;
        for (Order order : orders) {
            total += order.getAmount();
        }
        return total;
    }
    
    // Tên gây hiểu nhầm
    public boolean check() {
        return orders.size() > 0;
    }
}

// Sử dụng - khó hiểu
Customer customer = getCustomer();
double amount = customer.getInv(); // getInv là gì?
boolean status = customer.check(); // check cái gì?
```

### **✅ Sau khi đổi tên:**
```java
class Customer {
    private String name;
    private List<Order> orders;
    
    // Tên mới rõ ràng
    public double getTotalInvoiceAmount() {
        double total = 0;
        for (Order order : orders) {
            total += order.getAmount();
        }
        return total;
    }
    
    // Tên mới mô tả đúng chức năng
    public boolean hasActiveOrders() {
        return orders.size() > 0;
    }
}

// Sử dụng - dễ hiểu
Customer customer = getCustomer();
double amount = customer.getTotalInvoiceAmount(); // Rõ ràng
boolean hasOrders = customer.hasActiveOrders();   // Dễ hiểu
```

## **Lợi Ích**

- **✅ Cải thiện khả năng đọc**: Code tự ghi chú (self-documenting)
- **✅ Giảm hiểu nhầm**: Tên rõ ràng giúp giảm lỗi do hiểu sai
- **✅ Dễ bảo trì**: Người khác dễ hiểu code hơn
- **✅ Phát hiện vấn đề**: Trong quá trình đổi tên có thể phát hiện code smell khác

## **Điểm Cần Lưu ý**

- **Đảm bảo cập nhật tất cả**: Cập nhật tất cả các lời gọi đến phương thức
- **Sử dụng IDE hỗ trợ**: Các IDE hiện đại có tính năng rename refactoring tự động
- **Kiểm tra API public**: Cẩn thận khi đổi tên phương thức trong API public
- **Cập nhật tài liệu**: Đảm bảo cập nhật tài liệu liên quan

## **Khi Nào Không Nên Sử Dụng**

- Khi phương thức là phần của API public và không thể thay đổi
- Khi có quá nhiều phụ thuộc và không đủ thời gian cập nhật
- Khi tên hiện tại đã đủ tốt và phổ biến

## **Kỹ Thuật Liên Quan**

- **Extract Method**: Tách phương thức phức tạp thành nhiều phương thức nhỏ
- **Inline Method**: Gom các phương thức quá nhỏ
- **Change Signature**: Thay đổi signature của phương thức

## **Kết Luận**

Đổi tên phương thức là một trong những kỹ thuật tái cấu trúc đơn giản nhưng hiệu quả nhất. Một tên phương thức tốt giúp code trở nên dễ đọc, dễ bảo trì và ít lỗi hơn. Luôn chọn tên mô tả rõ ràng mục đích của phương thức và đảm bảo cập nhật tất cả các lời gọi khi thực hiện thay đổi.

**Nguồn:** [refactoring.guru/rename-method](https://refactoring.guru/rename-method)