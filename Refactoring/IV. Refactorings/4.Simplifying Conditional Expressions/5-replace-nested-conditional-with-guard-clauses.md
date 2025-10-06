# **Thay Thế Điều Kiện Lồng Nhau Bằng Mệnh Đề Bảo Vệ (Replace Nested Conditional with Guard Clauses)**

## **Định Nghĩa**
Thay thế điều kiện lồng nhau bằng mệnh đề bảo vệ là kỹ thuật tái cấu trúc sử dụng các điều kiện bảo vệ (guard clauses) để xử lý các trường hợp đặc biệt sớm, thay vì lồng các điều kiện trong nhau.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các điều kiện lồng nhau làm code khó đọc
- Có các trường hợp đặc biệt cần được xử lý sớm
- Muốn làm phẳng cấu trúc điều kiện và giảm độ sâu thụt lề

### **Không nên sử dụng khi:**
- Các điều kiện thực sự có quan hệ cha-con và việc lồng nhau là tự nhiên
- Tất cả các nhánh điều kiện đều quan trọng như nhau

## **Các Bước Thực Hiện**

1. **Xác định điều kiện bảo vệ**: Tìm các điều kiện kiểm tra trường hợp đặc biệt
2. **Tách điều kiện bảo vệ**: Đưa các điều kiện này lên đầu và sử dụng return/throw
3. **Đảo điều kiện nếu cần**: Đôi khi cần đảo điều kiện để xử lý trường hợp đặc biệt trước
4. **Tiếp tục với logic chính**: Sau các điều kiện bảo vệ, xử lý logic chính

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class PaymentProcessor {
    public double calculatePayment(Employee employee, int hours) {
        double result;
        if (employee != null) {
            if (employee.isActive()) {
                if (hours > 0) {
                    double baseRate = employee.getPayRate();
                    if (hours > 40) {
                        result = baseRate * 40 + baseRate * 1.5 * (hours - 40);
                    } else {
                        result = baseRate * hours;
                    }
                } else {
                    result = 0;
                }
            } else {
                result = 0;
            }
        } else {
            result = 0;
        }
        return result;
    }
}
```

### **✅ Sau khi thay thế:**
```java
class PaymentProcessor {
    public double calculatePayment(Employee employee, int hours) {
        // Guard clauses - xử lý các trường hợp đặc biệt trước
        if (employee == null) return 0;
        if (!employee.isActive()) return 0;
        if (hours <= 0) return 0;
        
        // Logic chính - không còn lồng nhau
        double baseRate = employee.getPayRate();
        if (hours > 40) {
            return baseRate * 40 + baseRate * 1.5 * (hours - 40);
        }
        return baseRate * hours;
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với validation phức tạp:**
```java
class OrderValidator {
    // ❌ Trước khi thay thế
    public ValidationResult validateOrder(Order order) {
        ValidationResult result = new ValidationResult();
        
        if (order != null) {
            if (order.getCustomer() != null) {
                if (order.getItems() != null && !order.getItems().isEmpty()) {
                    if (order.getTotalAmount() > 0) {
                        if (order.getCustomer().isActive()) {
                            // Tất cả validation passed
                            result.setValid(true);
                        } else {
                            result.setValid(false);
                            result.addError("Khách hàng không hoạt động");
                        }
                    } else {
                        result.setValid(false);
                        result.addError("Tổng tiền phải lớn hơn 0");
                    }
                } else {
                    result.setValid(false);
                    result.addError("Đơn hàng phải có ít nhất một sản phẩm");
                }
            } else {
                result.setValid(false);
                result.addError("Khách hàng không được null");
            }
        } else {
            result.setValid(false);
            result.addError("Đơn hàng không được null");
        }
        return result;
    }
    
    // ✅ Sau khi thay thế
    public ValidationResult validateOrder(Order order) {
        ValidationResult result = new ValidationResult();
        
        // Guard clauses
        if (order == null) {
            result.addError("Đơn hàng không được null");
            return result;
        }
        
        if (order.getCustomer() == null) {
            result.addError("Khách hàng không được null");
            return result;
        }
        
        if (order.getItems() == null || order.getItems().isEmpty()) {
            result.addError("Đơn hàng phải có ít nhất một sản phẩm");
            return result;
        }
        
        if (order.getTotalAmount() <= 0) {
            result.addError("Tổng tiền phải lớn hơn 0");
            return result;
        }
        
        if (!order.getCustomer().isActive()) {
            result.addError("Khách hàng không hoạt động");
            return result;
        }
        
        // Tất cả validation passed
        result.setValid(true);
        return result;
    }
}
```

## **Lợi Ích**

- **✅ Code rõ ràng hơn**: Dễ đọc và theo dõi luồng thực thi
- **✅ Giảm độ phức tạp**: Loại bỏ các cấp độ lồng nhau
- **✅ Tập trung vào logic chính**: Các trường hợp đặc biệt được xử lý riêng
- **✅ Dễ bảo trì**: Thêm/xóa điều kiện dễ dàng hơn
- **✅ Hiệu suất tốt hơn**: Thoát sớm khỏi phương thức khi có lỗi

## **Điểm Cần Lưu Ý**

- **Sử dụng return sớm**: Không ngại sử dụng return/throw sớm trong guard clauses
- **Đặt tên có ý nghĩa**: Có thể trích xuất guard clauses thành phương thức riêng nếu phức tạp
- **Không lạm dụng**: Với logic đơn giản, đôi khi điều kiện lồng nhau tự nhiên hơn

## **Khi Nào Không Nên Sử Dụng**

- Khi tất cả các nhánh điều kiện đều quan trọng như nhau
- Khi logic thực sự có cấu trúc phân cấp rõ ràng
- Khi guard clauses làm code trở nên rời rạc

## **Kỹ Thuật Liên Quan**

- **Consolidate Conditional Expression**: Hợp nhất các biểu thức điều kiện
- **Introduce Null Object**: Giới thiệu đối tượng null
- **Replace Exception with Test**: Thay thế ngoại lệ bằng kiểm tra

## **Kết Luận**

Thay thế điều kiện lồng nhau bằng mệnh đề bảo vệ là kỹ thuật mạnh mẽ để đơn giản hóa code phức tạp. Bằng cách xử lý các trường hợp đặc biệt sớm và trả về ngay lập tức, bạn tạo ra code dễ đọc, dễ bảo trì và ít lỗi hơn. Kỹ thuật này đặc biệt hữu ích trong các phương thức validation và xử lý nghiệp vụ có nhiều điều kiện tiên quyết.