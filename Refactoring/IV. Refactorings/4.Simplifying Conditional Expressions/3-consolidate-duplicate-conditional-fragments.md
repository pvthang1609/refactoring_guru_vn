# **Hợp Nhất Điều Kiện Trùng Lặp (Consolidate Duplicate Conditional Fragments)**

## **Định Nghĩa**
Hợp nhất điều kiện trùng lặp là kỹ thuật tái cấu trúc di chuyển các đoạn code giống nhau trong tất cả các nhánh của câu lệnh điều kiện ra bên ngoài.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có cùng một đoạn code xuất hiện trong tất cả các nhánh của điều kiện
- Code trùng lặp làm giảm khả năng bảo trì
- Muốn làm nổi bật sự khác biệt thực sự giữa các nhánh

### **Không nên sử dụng khi:**
- Các đoạn code chỉ tương tự nhau một phần
- Việc di chuyển code ra ngoài làm thay đổi logic chương trình

## **Các Bước Thực Hiện**

1. **Xác định code trùng lặp**: Tìm code giống nhau trong các nhánh if và else
2. **Di chuyển ra ngoài**: Di chuyển code trùng lặp đến trước hoặc sau câu lệnh điều kiện
3. **Kiểm tra logic**: Đảm bảo thứ tự thực hiện không thay đổi hành vi

## **Ví Dụ Minh Họa**

### **❌ Trước khi hợp nhất:**
```java
class OrderProcessor {
    public void processOrder(Order order, boolean isPriority) {
        if (isPriority) {
            // Code trùng lặp
            validateOrder(order);
            updateInventory(order);
            calculateShipping(order);
            // Code đặc biệt cho priority
            expediteShipping(order);
            sendPriorityConfirmation(order);
        } else {
            // Code trùng lặp
            validateOrder(order);
            updateInventory(order);
            calculateShipping(order);
            // Code đặc biệt cho standard
            scheduleStandardShipping(order);
            sendStandardConfirmation(order);
        }
    }
}
```

### **✅ Sau khi hợp nhất:**
```java
class OrderProcessor {
    public void processOrder(Order order, boolean isPriority) {
        // Code chung được di chuyển ra ngoài
        validateOrder(order);
        updateInventory(order);
        calculateShipping(order);
        
        // Chỉ code khác biệt được giữ lại trong điều kiện
        if (isPriority) {
            expediteShipping(order);
            sendPriorityConfirmation(order);
        } else {
            scheduleStandardShipping(order);
            sendStandardConfirmation(order);
        }
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với nhiều nhánh điều kiện:**
```java
class PaymentProcessor {
    // ❌ Trước khi hợp nhất
    public void processPayment(Payment payment, String paymentType) {
        if ("CREDIT_CARD".equals(paymentType)) {
            validatePayment(payment);
            logTransaction(payment);
            processCreditCard(payment);
            updateAccountBalance(payment);
            sendReceipt(payment);
        } else if ("PAYPAL".equals(paymentType)) {
            validatePayment(payment);
            logTransaction(payment);
            processPayPal(payment);
            updateAccountBalance(payment);
            sendReceipt(payment);
        } else if ("BANK_TRANSFER".equals(paymentType)) {
            validatePayment(payment);
            logTransaction(payment);
            processBankTransfer(payment);
            updateAccountBalance(payment);
            sendReceipt(payment);
        }
    }
    
    // ✅ Sau khi hợp nhất
    public void processPayment(Payment payment, String paymentType) {
        // Code chung được di chuyển ra ngoài
        validatePayment(payment);
        logTransaction(payment);
        
        // Chỉ xử lý đặc thù cho từng loại thanh toán
        if ("CREDIT_CARD".equals(paymentType)) {
            processCreditCard(payment);
        } else if ("PAYPAL".equals(paymentType)) {
            processPayPal(payment);
        } else if ("BANK_TRANSFER".equals(paymentType)) {
            processBankTransfer(payment);
        } else {
            throw new IllegalArgumentException("Loại thanh toán không hợp lệ");
        }
        
        // Code chung ở cuối
        updateAccountBalance(payment);
        sendReceipt(payment);
    }
}
```

## **Ví Dụ Với Try-Catch**

### **✅ Xử lý ngoại lệ:**
```java
class FileProcessor {
    // ❌ Trước khi hợp nhất
    public void processFile(String filename) {
        try {
            File file = new File(filename);
            String content = readFileContent(file);
            validateContent(content);
            processContent(content);
            // Code trùng lặp
            logSuccess(filename);
            cleanupTempFiles();
        } catch (IOException e) {
            // Code trùng lặp
            logError(filename, e);
            cleanupTempFiles();
            throw new RuntimeException("Xử lý file thất bại", e);
        }
    }
    
    // ✅ Sau khi hợp nhất
    public void processFile(String filename) {
        try {
            File file = new File(filename);
            String content = readFileContent(file);
            validateContent(content);
            processContent(content);
            logSuccess(filename);
        } catch (IOException e) {
            logError(filename, e);
            throw new RuntimeException("Xử lý file thất bại", e);
        } finally {
            // Code chung được di chuyển vào finally
            cleanupTempFiles();
        }
    }
}
```

## **Lợi Ích**

- **✅ Giảm trùng lặp**: Loại bỏ code giống nhau trong các nhánh
- **✅ Code ngắn gọn**: Giảm kích thước và độ phức tạp của code
- **✅ Dễ bảo trì**: Thay đổi code chung chỉ ở một nơi
- **✅ Làm nổi bật sự khác biệt**: Dễ dàng thấy được điểm khác biệt thực sự giữa các nhánh
- **✅ Giảm lỗi**: Ít code trùng lặp = ít cơ hội mắc lỗi

## **Điểm Cần Lưu Ý**

- **Đảm bảo thứ tự thực thi**: Di chuyển code không được thay đổi thứ tự thực hiện
- **Kiểm tra side effects**: Đảm bảo code di chuyển không gây ra side effect không mong muốn
- **Phân biệt code thực sự giống nhau**: Chỉ di chuyển code giống hệt nhau, không phải code tương tự

## **Khi Nào Không Nên Sử Dụng**

- Khi code chỉ tương tự nhau nhưng không giống hệt
- Khi việc di chuyển code làm thay đổi logic chương trình
- Khi các nhánh có thứ tự thực hiện quan trọng khác nhau

## **Kỹ Thuật Liên Quan**

- **Extract Method**: Trích xuất code trùng lặp thành phương thức riêng
- **Consolidate Conditional Expression**: Hợp nhất các biểu thức điều kiện
- **Decompose Conditional**: Tách biến điều kiện phức tạp

## **Kết Luận**

Hợp nhất điều kiện trùng lặp là kỹ thuật đơn giản nhưng hiệu quả để loại bỏ sự trùng lặp trong các câu lệnh điều kiện. Bằng cách di chuyển code chung ra ngoài, bạn không chỉ làm code ngắn gọn hơn mà còn làm nổi bật được sự khác biệt thực sự giữa các nhánh, giúp code dễ đọc và dễ bảo trì hơn.