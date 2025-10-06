# **Thay Thế Số Ma Thuật Bằng Hằng Số Có Tên (Replace Magic Number with Symbolic Constant)**

## **Định Nghĩa**
Thay thế số ma thuật bằng hằng số có tên là kỹ thuật tái cấu trúc thay thế các con số xuất hiện trực tiếp trong code bằng các hằng số có tên mô tả rõ ràng ý nghĩa của chúng.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các con số xuất hiện trực tiếp trong code mà không có giải thích
- Cùng một con số được sử dụng ở nhiều nơi
- Con số có ý nghĩa đặc biệt trong nghiệp vụ

### **Dấu hiệu nhận biết:**
- Các con số khó hiểu xuất hiện trong công thức tính toán
- Có comment giải thích ý nghĩa của con số
- Khó nhớ ý nghĩa của con số khi đọc code

## **Các Bước Thực Hiện**

1. **Xác định số ma thuật**: Tìm các con số xuất hiện trực tiếp trong code
2. **Khai báo hằng số**: Tạo hằng số với tên mô tả rõ ý nghĩa
3. **Thay thế**: Thay thế tất cả các xuất hiện của số ma thuật bằng hằng số
4. **Kiểm tra**: Đảm bảo không bỏ sót vị trí nào

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế:**
```java
class Geometry {
    public double calculateCircleArea(double radius) {
        return 3.14159 * radius * radius; // 3.14159 là gì?
    }
    
    public double calculateCircleCircumference(double radius) {
        return 2 * 3.14159 * radius; // Số 2 và 3.14159 khó hiểu
    }
    
    public boolean isAdult(int age) {
        return age >= 18; // 18 có ý nghĩa gì?
    }
    
    public double calculateTax(double income) {
        if (income <= 18200) { // 18200 là gì?
            return 0;
        } else if (income <= 45000) { // 45000 là gì?
            return (income - 18200) * 0.19; // 0.19 là gì?
        } else {
            return 5092 + (income - 45000) * 0.325; // 5092, 0.325 là gì?
        }
    }
}
```

### **✅ Sau khi thay thế:**
```java
class Geometry {
    private static final double PI = 3.14159;
    private static final int CIRCLE_RADIUS_MULTIPLIER = 2;
    
    public double calculateCircleArea(double radius) {
        return PI * radius * radius;
    }
    
    public double calculateCircleCircumference(double radius) {
        return CIRCLE_RADIUS_MULTIPLIER * PI * radius;
    }
}

class PersonUtils {
    private static final int ADULT_AGE_THRESHOLD = 18;
    
    public boolean isAdult(int age) {
        return age >= ADULT_AGE_THRESHOLD;
    }
}

class TaxCalculator {
    // Các ngưỡng thuế
    private static final double TAX_FREE_THRESHOLD = 18200.0;
    private static final double FIRST_TAX_BRACKET_MAX = 45000.0;
    private static final double SECOND_TAX_BRACKET_BASE = 5092.0;
    
    // Các tỷ lệ thuế
    private static final double FIRST_TAX_RATE = 0.19;
    private static final double SECOND_TAX_RATE = 0.325;
    
    public double calculateTax(double income) {
        if (income <= TAX_FREE_THRESHOLD) {
            return 0;
        } else if (income <= FIRST_TAX_BRACKET_MAX) {
            return (income - TAX_FREE_THRESHOLD) * FIRST_TAX_RATE;
        } else {
            return SECOND_TAX_BRACKET_BASE + 
                   (income - FIRST_TAX_BRACKET_MAX) * SECOND_TAX_RATE;
        }
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Code với nhiều số ma thuật:**
```java
class LoanCalculator {
    public double calculateMonthlyPayment(double principal, int years) {
        double annualRate = 0.05; // 0.05 là gì?
        int paymentsPerYear = 12; // 12 là gì?
        
        double monthlyRate = annualRate / 12; // Tại sao chia 12?
        int totalPayments = years * 12; // Tại sao nhân 12?
        
        return principal * monthlyRate * 
               Math.pow(1 + monthlyRate, totalPayments) / 
               (Math.pow(1 + monthlyRate, totalPayments) - 1);
    }
    
    public boolean isLoanApproved(double income, double loanAmount) {
        return income >= 30000 && // 30000 là gì?
               loanAmount <= income * 4; // 4 là gì?
    }
}
```

### **✅ Sau khi thay thế:**
```java
class LoanCalculator {
    // Các hằng số cho tính toán lãi suất
    private static final double ANNUAL_INTEREST_RATE = 0.05;
    private static final int MONTHS_IN_YEAR = 12;
    private static final int PAYMENTS_PER_YEAR = 12;
    
    // Các hằng số cho điều kiện phê duyệt khoản vay
    private static final double MINIMUM_ANNUAL_INCOME = 30000.0;
    private static final double LOAN_TO_INCOME_RATIO = 4.0;
    
    public double calculateMonthlyPayment(double principal, int years) {
        double monthlyRate = ANNUAL_INTEREST_RATE / MONTHS_IN_YEAR;
        int totalPayments = years * PAYMENTS_PER_YEAR;
        
        return principal * monthlyRate * 
               Math.pow(1 + monthlyRate, totalPayments) / 
               (Math.pow(1 + monthlyRate, totalPayments) - 1);
    }
    
    public boolean isLoanApproved(double income, double loanAmount) {
        return income >= MINIMUM_ANNUAL_INCOME && 
               loanAmount <= income * LOAN_TO_INCOME_RATIO;
    }
}
```

## **Lợi Ích**

- **✅ Code dễ đọc**: Tên hằng số giải thích ý nghĩa của con số
- **✅ Dễ bảo trì**: Thay đổi giá trị chỉ ở một nơi
- **✅ Giảm lỗi**: Tránh nhầm lẫn khi sử dụng cùng giá trị ở nhiều nơi
- **✅ Tái sử dụng**: Có thể sử dụng hằng số ở nhiều nơi trong code

## **Điểm Cần Lưu ý**

- **Đặt tên có ý nghĩa**: Tên hằng số nên mô tả rõ mục đích sử dụng
- **Nhóm theo chức năng**: Tổ chức các hằng số liên quan thành nhóm
- **Không lạm dụng**: Với các số đơn giản như 0, 1, 100 có thể không cần thay thế
- **Sử dụng enum khi phù hợp**: Khi có tập hợp các giá trị liên quan

## **Khi Nào Không Nên Sử Dụng**

- Khi con số rất rõ ràng và dễ hiểu (0, 1, 100%)
- Khi con số chỉ xuất hiện một lần và không có khả năng thay đổi
- Khi việc thêm hằng số làm code phức tạp hơn

## **Kỹ Thuật Liên Quan**

- **Introduce Parameter Object**: Khi có nhiều tham số liên quan
- **Replace Type Code with Class**: Khi cần thay thế mã kiểu bằng lớp
- **Extract Method**: Khi cần tách logic chứa số ma thuật thành phương thức riêng

## **Kết Luận**

Thay thế số ma thuật bằng hằng số có tên là kỹ thuật đơn giản nhưng hiệu quả để cải thiện khả năng đọc và bảo trì code. Bằng cách đặt tên có ý nghĩa cho các con số, bạn giúp người đọc code hiểu được ý nghĩa thực sự của chúng mà không cần phải đoán hoặc tìm kiếm tài liệu.