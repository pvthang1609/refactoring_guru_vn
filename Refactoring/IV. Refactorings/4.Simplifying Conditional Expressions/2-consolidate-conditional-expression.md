# **Hợp Nhất Biểu Thức Điều Kiện (Consolidate Conditional Expression)**

## **Định Nghĩa**
Hợp nhất biểu thức điều kiện là kỹ thuật tái cấu trúc kết hợp nhiều điều kiện liên quan thành một biểu thức duy nhất, thường bằng cách trích xuất chúng thành một phương thức có tên mô tả rõ ràng.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có nhiều điều kiện kiểm tra cùng một điều kiện nghiệp vụ
- Kết quả của tất cả các điều kiện là như nhau (cùng return value hoặc cùng action)
- Các điều kiện có mối quan hệ logic với nhau

### **Không nên sử dụng khi:**
- Các điều kiện không liên quan đến nhau
- Mỗi điều kiện dẫn đến các hành động khác nhau

## **Các Bước Thực Hiện**

1. **Xác định các điều kiện liên quan**: Tìm các điều kiện có cùng kết quả
2. **Kết hợp bằng toán tử logic**: Sử dụng AND (&&) hoặc OR (||) để kết hợp
3. **Trích xuất phương thức**: Tạo phương thức mới với tên mô tả ý nghĩa
4. **Thay thế điều kiện**: Thay thế các điều kiện riêng lẻ bằng phương thức mới

## **Ví Dụ Minh Họa**

### **❌ Trước khi hợp nhất:**
```java
class Employee {
    private int seniority;
    private int monthsDisabled;
    private boolean isPartTime;
    
    public double disabilityAmount() {
        if (seniority < 2) {
            return 0;
        }
        if (monthsDisabled > 12) {
            return 0;
        }
        if (isPartTime) {
            return 0;
        }
        // Tính toán số tiền trợ cấp
        return calculateBaseDisabilityAmount();
    }
}
```

### **✅ Sau khi hợp nhất:**
```java
class Employee {
    private int seniority;
    private int monthsDisabled;
    private boolean isPartTime;
    
    public double disabilityAmount() {
        if (isNotEligibleForDisability()) {
            return 0;
        }
        // Tính toán số tiền trợ cấp
        return calculateBaseDisabilityAmount();
    }
    
    private boolean isNotEligibleForDisability() {
        return seniority < 2 || monthsDisabled > 12 || isPartTime;
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với điều kiện AND:**
```java
class LoanApplication {
    private double income;
    private double creditScore;
    private int employmentYears;
    private boolean hasCriminalRecord;
    
    // ❌ Trước khi hợp nhất
    public boolean isApproved() {
        if (income < 30000) {
            return false;
        }
        if (creditScore < 650) {
            return false;
        }
        if (employmentYears < 2) {
            return false;
        }
        if (hasCriminalRecord) {
            return false;
        }
        return true;
    }
    
    // ✅ Sau khi hợp nhất
    public boolean isApproved() {
        return meetsMinimumRequirements() && hasCleanRecord();
    }
    
    private boolean meetsMinimumRequirements() {
        return income >= 30000 && creditScore >= 650 && employmentYears >= 2;
    }
    
    private boolean hasCleanRecord() {
        return !hasCriminalRecord;
    }
}
```

## **Ví Dụ Với Điều Kiện OR**

### **✅ Khi bất kỳ điều kiện nào cũng dẫn đến cùng kết quả:**
```java
class InsurancePolicy {
    private boolean isExpired;
    private boolean isCancelled;
    private boolean isSuspended;
    private double claimAmount;
    
    // ❌ Trước khi hợp nhất
    public boolean canProcessClaim() {
        if (isExpired) {
            return false;
        }
        if (isCancelled) {
            return false;
        }
        if (isSuspended) {
            return false;
        }
        if (claimAmount <= 0) {
            return false;
        }
        return true;
    }
    
    // ✅ Sau khi hợp nhất
    public boolean canProcessClaim() {
        return !isPolicyInvalid() && claimAmount > 0;
    }
    
    private boolean isPolicyInvalid() {
        return isExpired || isCancelled || isSuspended;
    }
}
```

## **Lợi Ích**

- **✅ Code rõ ràng hơn**: Một phương thức với tên có ý nghĩa thay vì nhiều điều kiện
- **✅ Giảm trùng lặp**: Tránh lặp lại cùng logic ở nhiều nơi
- **✅ Dễ bảo trì**: Thay đổi logic chỉ ở một nơi
- **✅ Dễ test**: Có thể test từng điều kiện tổng hợp độc lập
- **✅ Tái sử dụng**: Có thể sử dụng lại phương thức điều kiện ở nhiều nơi

## **Điểm Cần Lưu Ý**

- **Đặt tên có ý nghĩa**: Tên phương thức nên mô tả rõ điều kiện đang kiểm tra
- **Giữ nguyên logic**: Đảm bảo logic không thay đổi sau khi hợp nhất
- **Cân nhắc khả năng đọc**: Đôi khi hợp nhất quá nhiều có thể làm giảm khả năng đọc

## **Khi Nào Không Nên Sử Dụng**

- Khi các điều kiện thực sự độc lập và không liên quan
- Khi mỗi điều kiện cần được xử lý hoặc log riêng biệt
- Khi việc hợp nhất làm mất đi thông tin debug quan trọng

## **Biến Thể**

### **✅ Sử dụng guard clauses:**
```java
class OrderProcessor {
    public void processOrder(Order order) {
        if (isInvalidOrder(order)) {
            logInvalidOrder(order);
            return;
        }
        
        // Xử lý order hợp lệ
        processValidOrder(order);
    }
    
    private boolean isInvalidOrder(Order order) {
        return order == null || 
               order.getItems().isEmpty() || 
               order.getCustomer() == null;
    }
}
```

## **Kỹ Thuật Liên Quan**

- **Decompose Conditional**: Tách các điều kiện phức tạp thành phương thức
- **Replace Nested Conditional with Guard Clauses**: Thay thế điều kiện lồng nhau bằng guard clauses
- **Extract Method**: Trích xuất code thành phương thức mới

## **Kết Luận**

Hợp nhất biểu thức điều kiện là kỹ thuật hiệu quả để đơn giản hóa code khi có nhiều điều kiện dẫn đến cùng một kết quả. Bằng cách kết hợp các điều kiện liên quan và đặt tên có ý nghĩa, bạn tạo ra code dễ đọc, dễ bảo trì và ít lỗi hơn. Tuy nhiên, cần cân nhắc để không hợp nhất các điều kiện không thực sự liên quan với nhau.

**Nguồn:** [refactoring.guru/consolidate-conditional-expression](https://refactoring.guru/consolidate-conditional-expression)