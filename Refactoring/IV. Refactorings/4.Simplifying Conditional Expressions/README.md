# **Đơn Giản Hóa Biểu Thức Điều Kiện (Simplifying Conditional Expressions)**

## **Giới Thiệu**
Nhóm kỹ thuật tái cấu trúc giúp đơn giản hóa các biểu thức điều kiện phức tạp, làm cho code dễ đọc, dễ hiểu và dễ bảo trì hơn.

## **Các Kỹ Thuật Chính**

### **1. Tách Biến Điều Kiện (Decompose Conditional)**
**Mục đích:** Tách các điều kiện phức tạp thành các phương thức riêng biệt có tên mô tả rõ ràng.

**Khi nào sử dụng:**
- Khi có các điều kiện phức tạp với nhiều toán tử logic
- Khi điều kiện khó hiểu và cần giải thích bằng comment

```java
// ❌ Trước
if (date.after(SUMMER_START) && date.before(SUMMER_END)) {
    charge = quantity * summerRate;
} else {
    charge = quantity * winterRate + winterServiceCharge;
}

// ✅ Sau
if (isSummer(date)) {
    charge = summerCharge(quantity);
} else {
    charge = winterCharge(quantity);
}

private boolean isSummer(Date date) {
    return date.after(SUMMER_START) && date.before(SUMMER_END);
}
```

### **2. Hợp Nhất Biểu Thức Điều Kiện (Consolidate Conditional Expression)**
**Mục đích:** Kết hợp các điều kiện liên quan thành một biểu thức duy nhất.

**Khi nào sử dụng:**
- Khi có nhiều điều kiện kiểm tra cùng một điều kiện
- Khi kết quả của tất cả các điều kiện là như nhau

```java
// ❌ Trước
if (seniority < 2) return 0;
if (monthsDisabled > 12) return 0;
if (isPartTime) return 0;

// ✅ Sau
if (isNotEligibleForDisability()) return 0;

private boolean isNotEligibleForDisability() {
    return seniority < 2 || monthsDisabled > 12 || isPartTime;
}
```

### **3. Hợp Nhất Điều Kiện Trùng Lặp (Consolidate Duplicate Conditional Fragments)**
**Mục đích:** Di chuyển code giống nhau trong tất cả các nhánh điều kiện ra bên ngoài.

```java
// ❌ Trước
if (isSpecialDeal()) {
    total = price * 0.95;
    send();
} else {
    total = price * 0.98;
    send();
}

// ✅ Sau
if (isSpecialDeal()) {
    total = price * 0.95;
} else {
    total = price * 0.98;
}
send();
```

### **4. Loại Bỏ Điều Kiện Luồng Điều Khiển (Remove Control Flag)**
**Mục đích:** Thay thế các biến cờ điều khiển bằng break, continue hoặc return.

```java
// ❌ Trước
boolean found = false;
for (Person p : people) {
    if (!found) {
        if (p.getName().equals("John")) {
            sendAlert();
            found = true;
        }
    }
}

// ✅ Sau
for (Person p : people) {
    if (p.getName().equals("John")) {
        sendAlert();
        break;
    }
}
```

### **5. Thay Thế Điều Kiện Bằng Đa Hình (Replace Conditional with Polymorphism)**
**Mục đích:** Thay thế các câu lệnh điều kiện phức tạp bằng đa hình.

```java
// ❌ Trước
class Bird {
    // ...
    double getSpeed() {
        switch (type) {
            case EUROPEAN:
                return getBaseSpeed();
            case AFRICAN:
                return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
            case NORWEGIAN_BLUE:
                return (isNailed) ? 0 : getBaseSpeed(voltage);
        }
        throw new RuntimeException("Should be unreachable");
    }
}

// ✅ Sau
abstract class Bird {
    abstract double getSpeed();
}

class EuropeanBird extends Bird {
    double getSpeed() {
        return getBaseSpeed();
    }
}
```

### **6. Giới Thiệu Đối Tượng Null (Introduce Null Object)**
**Mục đích:** Thay thế kiểm tra null bằng một đối tượng có hành vi mặc định.

```java
// ❌ Trước
if (customer == null) {
    plan = BillingPlan.basic();
} else {
    plan = customer.getPlan();
}

// ✅ Sau
class NullCustomer extends Customer {
    public Plan getPlan() {
        return BillingPlan.basic();
    }
}
```

### **7. Giới Thiệu Khẳng Định (Introduce Assertion)**
**Mục đích:** Thêm các khẳng định để làm rõ các giả định trong code.

```java
// ✅ Sử dụng assertion
private double getExpenseLimit() {
    Assert.isTrue(expenseLimit != NULL_EXPENSE || primaryProject != null);
    return (expenseLimit != NULL_EXPENSE) ? expenseLimit : primaryProject.getMemberExpenseLimit();
}
```

## **Lợi Ích**

✅ **Code dễ đọc** - Điều kiện được đặt tên rõ ràng  
✅ **Dễ bảo trì** - Thay đổi logic chỉ ở một nơi  
✅ **Giảm trùng lặp** - Loại bỏ code trùng lặp trong các nhánh điều kiện  
✅ **Tăng hiệu suất** - Loại bỏ các biến cờ và kiểm tra không cần thiết  
✅ **Dễ test** - Có thể test từng điều kiện độc lập  

## **Khi Nào Sử Dụng**

- Khi code có nhiều câu lệnh if-else hoặc switch-case phức tạp
- Khi có các điều kiện lồng nhau khó theo dõi
- Khi cần thêm comment để giải thích điều kiện
- Khi có code trùng lặp trong các nhánh điều kiện

## **Kết Luận**

Các kỹ thuật đơn giản hóa biểu thức điều kiện giúp code trở nên rõ ràng, dễ bảo trì và ít lỗi hơn. Bằng cách áp dụng các kỹ thuật này, bạn có thể biến các điều kiện phức tạp thành code tự giải thích, giúp người đọc dễ dàng hiểu được logic mà không cần phải phân tích quá nhiều.