# **Tách Biến Điều Kiện (Decompose Conditional)**

## **Định Nghĩa**
Tách biến điều kiện là kỹ thuật tái cấu trúc tách các điều kiện phức tạp thành các phương thức riêng biệt có tên mô tả rõ ràng ý nghĩa của điều kiện.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các điều kiện phức tạp với nhiều toán tử logic (&&, ||)
- Điều kiện khó hiểu và cần comment để giải thích
- Cùng một điều kiện được lặp lại ở nhiều nơi

### **Không nên sử dụng khi:**
- Điều kiện đã đủ đơn giản và dễ hiểu
- Việc tách phương thức làm code phức tạp hơn

## **Các Bước Thực Hiện**

1. **Tách điều kiện**: Tách phần điều kiện thành phương thức riêng
2. **Tách then/else**: Tách các khối then và else thành phương thức riêng
3. **Đặt tên có ý nghĩa**: Đặt tên phương thức mô tả rõ mục đích

## **Ví Dụ Minh Họa**

### **❌ Trước khi tách:**
```java
class Order {
    private Date orderDate;
    private double amount;
    private Customer customer;
    
    public double calculateDiscount() {
        // Điều kiện phức tạp, khó hiểu
        if (orderDate.getMonth() >= 11 || orderDate.getMonth() <= 1) {
            if (amount > 1000 && customer.isPremium()) {
                return amount * 0.15;
            } else if (amount > 500) {
                return amount * 0.1;
            } else {
                return amount * 0.05;
            }
        } else if (customer.isPremium() && amount > 800) {
            return amount * 0.1;
        } else {
            return amount * 0.02;
        }
    }
}
```

### **✅ Sau khi tách:**
```java
class Order {
    private Date orderDate;
    private double amount;
    private Customer customer;
    
    public double calculateDiscount() {
        if (isHolidaySeason()) {
            return calculateHolidayDiscount();
        } else if (isPremiumHighValueOrder()) {
            return calculatePremiumDiscount();
        } else {
            return calculateStandardDiscount();
        }
    }
    
    private boolean isHolidaySeason() {
        int month = orderDate.getMonth();
        return month >= 11 || month <= 1; // Tháng 11, 12, 1
    }
    
    private boolean isPremiumHighValueOrder() {
        return customer.isPremium() && amount > 800;
    }
    
    private double calculateHolidayDiscount() {
        if (isHighValuePremiumOrder()) {
            return amount * 0.15;
        } else if (isHighValueOrder()) {
            return amount * 0.1;
        } else {
            return amount * 0.05;
        }
    }
    
    private boolean isHighValuePremiumOrder() {
        return amount > 1000 && customer.isPremium();
    }
    
    private boolean isHighValueOrder() {
        return amount > 500;
    }
    
    private double calculatePremiumDiscount() {
        return amount * 0.1;
    }
    
    private double calculateStandardDiscount() {
        return amount * 0.02;
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với validation phức tạp:**
```java
class LoanApplication {
    private Applicant applicant;
    private double loanAmount;
    private int loanTerm;
    private double annualIncome;
    
    public ValidationResult validate() {
        // ❌ Điều kiện validation phức tạp
        if ((applicant.getAge() < 18 || applicant.getAge() > 65) ||
            (annualIncome < 30000 && loanAmount > 50000) ||
            (loanTerm > 30 || loanTerm < 1) ||
            (applicant.getCreditScore() < 600 && loanAmount > 10000) ||
            (applicant.hasBankruptcy() && loanAmount > 5000)) {
            return ValidationResult.rejected("Điều kiện không hợp lệ");
        }
        return ValidationResult.approved();
    }
}

// ✅ Sau khi tách
class LoanApplication {
    private Applicant applicant;
    private double loanAmount;
    private int loanTerm;
    private double annualIncome;
    
    public ValidationResult validate() {
        if (isAgeInvalid() ||
            isIncomeInsufficient() ||
            isLoanTermInvalid() ||
            isCreditScoreTooLow() ||
            hasBankruptcyRestriction()) {
            return ValidationResult.rejected("Điều kiện không hợp lệ");
        }
        return ValidationResult.approved();
    }
    
    private boolean isAgeInvalid() {
        return applicant.getAge() < 18 || applicant.getAge() > 65;
    }
    
    private boolean isIncomeInsufficient() {
        return annualIncome < 30000 && loanAmount > 50000;
    }
    
    private boolean isLoanTermInvalid() {
        return loanTerm > 30 || loanTerm < 1;
    }
    
    private boolean isCreditScoreTooLow() {
        return applicant.getCreditScore() < 600 && loanAmount > 10000;
    }
    
    private boolean hasBankruptcyRestriction() {
        return applicant.hasBankruptcy() && loanAmount > 5000;
    }
}
```

## **Lợi Ích**

- **✅ Code dễ đọc**: Tên phương thức giải thích ý nghĩa điều kiện
- **✅ Dễ bảo trì**: Thay đổi logic chỉ ở một nơi
- **✅ Tái sử dụng**: Có thể sử dụng lại các điều kiện ở nhiều nơi
- **✅ Dễ test**: Có thể test từng điều kiện độc lập
- **✅ Giảm trùng lặp**: Tránh lặp lại cùng điều kiện ở nhiều chỗ

## **Điểm Cần Lưu Ý**

- **Đặt tên có ý nghĩa**: Tên phương thức nên mô tả rõ mục đích của điều kiện
- **Không tách quá nhỏ**: Tránh tạo quá nhiều phương thức nhỏ không cần thiết
- **Giữ nguyên logic**: Đảm bảo logic không thay đổi sau khi tách

## **Khi Nào Không Nên Sử Dụng**

- Khi điều kiện đã đơn giản và rõ ràng
- Khi việc tách phương thức làm mất tính tự nhiên của code
- Khi performance là yếu tố quan trọng và cần tối ưu

## **Kỹ Thuật Liên Quan**

- **Extract Method**: Kỹ thuật cơ bản để tách code thành phương thức
- **Consolidate Conditional Expression**: Kết hợp các điều kiện liên quan
- **Replace Conditional with Polymorphism**: Thay thế điều kiện bằng đa hình

## **Kết Luận**

Tách biến điều kiện là kỹ thuật đơn giản nhưng hiệu quả để cải thiện khả năng đọc và bảo trì code. Bằng cách chuyển các điều kiện phức tạp thành các phương thức có tên mô tả rõ ràng, bạn giúp người đọc code hiểu được ý nghĩa của điều kiện mà không cần phải phân tích logic phức tạp.

**Nguồn:** [refactoring.guru/decompose-conditional](https://refactoring.guru/decompose-conditional)