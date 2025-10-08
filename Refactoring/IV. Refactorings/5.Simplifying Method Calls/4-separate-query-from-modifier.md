# **Tách Truy Vấn Khỏi Bộ Điều Chỉnh (Separate Query from Modifier)**

## **Định Nghĩa**
Tách truy vấn khỏi bộ điều chỉnh là kỹ thuật tái cấu trúc tách một phương thức vừa trả về giá trị (truy vấn) vừa thay đổi trạng thái đối tượng (bộ điều chỉnh) thành hai phương thức riêng biệt.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Phương thức vừa trả về giá trị vừa thay đổi trạng thái đối tượng
- Cần tuân thủ nguyên tắc Command-Query Separation (CQS)
- Muốn code dễ đọc và dễ bảo trì hơn
- Cần tái sử dụng phương thức truy vấn mà không thay đổi trạng thái

### **Không nên sử dụng khi:**
- Phương thức thực sự cần vừa trả về giá trị vừa thay đổi trạng thái (như pop() trong stack)
- Việc tách làm code phức tạp hơn thay vì đơn giản hơn

## **Các Bước Thực Hiện**

1. **Xác định phương thức hỗn hợp**: Tìm phương thức vừa truy vấn vừa thay đổi trạng thái
2. **Tạo phương thức truy vấn**: Tách phần trả về giá trị thành phương thức mới
3. **Tạo phương thức điều chỉnh**: Giữ phần thay đổi trạng thái trong phương thức gốc hoặc tạo phương thức mới
4. **Thay thế lời gọi**: Cập nhật các lời gọi đến phương thức cũ
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi tách:**
```java
class Customer {
    private List<Order> orders = new ArrayList<>();
    
    // Phương thức vừa truy vấn vừa thay đổi trạng thái
    public Order getLastOrderAndMarkPriority() {
        if (orders.isEmpty()) {
            return null;
        }
        
        Order lastOrder = orders.get(orders.size() - 1);
        
        // Side effect: thay đổi trạng thái
        lastOrder.setPriority(true);
        updateCustomerStatus();
        
        return lastOrder;
    }
    
    private void updateCustomerStatus() {
        // Cập nhật trạng thái khách hàng
    }
}

// Client code - không rõ phương thức có side effect
Customer customer = getCustomer();
Order lastOrder = customer.getLastOrderAndMarkPriority(); // Có side effect ẩn
processOrder(lastOrder);

// Nếu chỉ muốn lấy đơn hàng cuối mà không đánh dấu ưu tiên?
// Không có cách nào, phải chấp nhận side effect
```

### **✅ Sau khi tách:**
```java
class Customer {
    private List<Order> orders = new ArrayList<>();
    
    // Phương thức truy vấn thuần túy - không side effect
    public Order getLastOrder() {
        if (orders.isEmpty()) {
            return null;
        }
        return orders.get(orders.size() - 1);
    }
    
    // Phương thức điều chỉnh thuần túy - không trả về giá trị
    public void markLastOrderAsPriority() {
        if (orders.isEmpty()) {
            return;
        }
        
        Order lastOrder = orders.get(orders.size() - 1);
        lastOrder.setPriority(true);
        updateCustomerStatus();
    }
    
    // Phương thức kết hợp (nếu cần) sử dụng 2 phương thức trên
    public Order getAndMarkLastOrderAsPriority() {
        markLastOrderAsPriority();
        return getLastOrder();
    }
    
    private void updateCustomerStatus() {
        // Cập nhật trạng thái khách hàng
    }
}

// Client code - rõ ràng về side effect
Customer customer = getCustomer();

// Chỉ lấy đơn hàng cuối - không side effect
Order lastOrder = customer.getLastOrder();
processOrder(lastOrder);

// Chỉ đánh dấu ưu tiên - không quan tâm giá trị trả về
customer.markLastOrderAsPriority();

// Cả hai nếu cần
Order prioritizedOrder = customer.getAndMarkLastOrderAsPriority();
```

## **Lợi Ích**

- **✅ Tuân thủ CQS**: Phân tách rõ ràng giữa truy vấn và điều chỉnh
- **✅ Code dễ đọc hơn**: Biết ngay phương thức nào có side effect
- **✅ Dễ tái sử dụng**: Có thể sử dụng truy vấn mà không gây side effect
- **✅ Dễ test hơn**: Có thể test riêng truy vấn và điều chỉnh
- **✅ Tránh lỗi bất ngờ**: Không có side effect ẩn trong truy vấn

## **Điểm Cần Lưu ý**

- **Hiệu suất**: Đôi khi cần tối ưu để tránh tính toán trùng lặp
- **Tính nguyên tử**: Trong một số trường hợp cần giữ tính nguyên tử của thao tác
- **Interface cũ**: Có thể cần giữ phương thức cũ cho tương thích ngược

## **Khi Nào Không Nên Sử Dụng**

- Khi thao tác thực sự cần tính nguyên tử cao
- Khi việc tách làm giảm hiệu suất đáng kể
- Khi phương thức là phần của API public và không thể thay đổi

## **Kỹ Thuật Liên Quan**

- **Extract Method**: Tách một phần code thành phương thức mới
- **Inline Method**: Gom phương thức nhỏ vào nơi gọi
- **Remove Setting Method**: Xóa phương thức setter không cần thiết

## **Kết Luận**

Tách truy vấn khỏi bộ điều chỉnh là kỹ thuật quan trọng để viết code rõ ràng và dễ bảo trì. Bằng cách phân tách rõ ràng giữa các phương thức truy vấn (trả về giá trị, không side effect) và phương thức điều chỉnh (thay đổi trạng thái, không trả về giá trị), chúng ta làm cho code dễ đọc, dễ test và ít lỗi hơn. Kỹ thuật này đặc biệt hữu ích khi tuân thủ nguyên tắc Command-Query Separation.

**Nguồn:** [refactoring.guru/separate-query-from-modifier](https://refactoring.guru/separate-query-from-modifier)