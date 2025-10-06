# **Tự Đóng Gói Trường (Self Encapsulate Field)**

## **Định Nghĩa**
Tự đóng gói trường là kỹ thuật tái cấu trúc tạo getter và setter cho một trường, và chỉ sử dụng chúng để truy cập trường ngay cả trong cùng một lớp.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Bạn cần kiểm soát nhiều hơn việc truy cập vào một trường
- Cần thêm logic khi đọc hoặc ghi giá trị cho trường (validation, logging)
- Làm việc với lớp cha và lớp con cần ghi đè cách truy cập trường
- Chuẩn bị cho việc thay đổi cấu trúc dữ liệu trong tương lai

### **Không nên sử dụng khi:**
- Trường chỉ được sử dụng đơn giản trong lớp hiện tại
- Việc thêm getter/setter làm code phức tạp hơn không cần thiết

## **Các Bước Thực Hiện**

1. **Tạo getter và setter**: Tạo phương thức getter và setter cho trường
2. **Tìm tất cả tham chiếu**: Tìm tất cả các vị trí truy cập trực tiếp đến trường trong cùng lớp
3. **Thay thế bằng getter/setter**: Thay thế các tham chiếu trực tiếp bằng lời gọi getter/setter
4. **Đánh dấu trường là private**: Đảm bảo trường chỉ có thể truy cập qua getter/setter

## **Ví Dụ Minh Họa**

### **❌ Trước khi tự đóng gói:**
```java
class Range {
    private int low;
    private int high;
    
    public boolean includes(int arg) {
        return arg >= low && arg <= high;
    }
    
    public void grow(int factor) {
        high = high * factor;
    }
}
```

### **✅ Sau khi tự đóng gói:**
```java
class Range {
    private int low;
    private int high;
    
    // Sử dụng getter/setter ngay trong cùng lớp
    public boolean includes(int arg) {
        return arg >= getLow() && arg <= getHigh();
    }
    
    public void grow(int factor) {
        setHigh(getHigh() * factor);
    }
    
    // Getter và setter
    public int getLow() {
        return low;
    }
    
    public int getHigh() {
        return high;
    }
    
    public void setLow(int low) {
        this.low = low;
    }
    
    public void setHigh(int high) {
        this.high = high;
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Trường hợp cần thêm logic:**
```java
class Account {
    private double balance;
    private double interestRate;
    
    public void applyInterest() {
        // Truy cập trực tiếp đến balance
        balance = balance + (balance * interestRate);
    }
    
    public void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
        } else {
            throw new IllegalArgumentException("Số dư không đủ");
        }
    }
}
```

### **✅ Sau khi tự đóng gói với logic bổ sung:**
```java
class Account {
    private double balance;
    private double interestRate;
    
    public void applyInterest() {
        // Sử dụng setter để có thể thêm logic sau này
        setBalance(getBalance() + (getBalance() * interestRate));
    }
    
    public void withdraw(double amount) {
        if (amount <= getBalance()) {
            setBalance(getBalance() - amount);
        } else {
            throw new IllegalArgumentException("Số dư không đủ");
        }
    }
    
    public double getBalance() {
        // Có thể thêm logging, caching, hoặc logic khác
        return balance;
    }
    
    public void setBalance(double balance) {
        // Có thể thêm validation
        if (balance < 0) {
            throw new IllegalArgumentException("Số dư không thể âm");
        }
        this.balance = balance;
    }
}
```

## **Lợi Ích**

- **✅ Kiểm soát truy cập**: Có thể thêm validation, logging, hoặc logic khác khi đọc/ghi trường
- **✅ Linh hoạt trong kế thừa**: Lớp con có thể ghi đè getter/setter để thay đổi hành vi
- **✅ Dễ dàng refactor**: Thay đổi cách lưu trữ dữ liệu mà không ảnh hưởng đến code sử dụng
- **✅ Chuẩn bị cho tính đa hình**: Có thể chuyển từ trường sang phương thức tính toán khi cần

## **Điểm Cần Lưu Ý**

- **Không lạm dụng**: Với các trường đơn giản, việc tự đóng gói có thể tạo ra code dư thừa
- **Cân nhắc hiệu năng**: Trong một số trường hợp, lời gọi phương thức có thể chậm hơn truy cập trực tiếp
- **Nhất quán**: Một khi đã tự đóng gói, luôn sử dụng getter/setter trong toàn bộ lớp

## **Khi Nào Không Nên Sử Dụng**

- Trong các lớp đơn giản chỉ chứa dữ liệu (Data Class)
- Khi trường chỉ được sử dụng vài lần và không cần logic đặc biệt
- Trong các lớp immutable object

## **Kỹ Thuật Liên Quan**

- **Đóng gói Trường (Encapsulate Field)**: Tương tự nhưng tập trung vào việc ẩn trường khỏi các lớp khác
- **Thay thế Trường bằng Đối tượng (Replace Data Value with Object)**: Khi cần nâng cấp kiểu dữ liệu đơn giản thành đối tượng
- **Thay thế Biến tạm bằng Truy vấn (Replace Temp with Query)**: Để loại bỏ biến tạm

## **Kết Luận**

Tự đóng gói trường là kỹ thuật quan trọng để chuẩn bị cho những thay đổi trong tương lai và tăng tính linh hoạt của code. Bằng cách luôn sử dụng getter/setter ngay cả trong nội bộ lớp, bạn tạo ra một điểm kiểm soát duy nhất cho việc truy cập dữ liệu, giúp dễ dàng thêm các tính năng như validation, logging, caching, hoặc thay đổi cách lưu trữ dữ liệu mà không ảnh hưởng đến toàn bộ codebase.