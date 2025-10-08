# **Thay Thế Mã Lỗi Bằng Ngoại Lệ (Replace Error Code with Exception)**

## **Định Nghĩa**
Thay thế mã lỗi bằng ngoại lệ là kỹ thuật tái cấu trúc thay thế việc trả về mã lỗi đặc biệt (như -1, null, false) bằng cách ném ra ngoại lệ, giúp xử lý lỗi rõ ràng và tách biệt logic chính với logic xử lý lỗi.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Phương thức trả về các giá trị đặc biệt để biểu thị lỗi
- Mã lỗi được kiểm tra và xử lý ở nhiều nơi trong code
- Muốn tách biệt rõ ràng giữa logic nghiệp vụ và xử lý lỗi
- Cần cung cấp thông tin chi tiết về lỗi cho người gọi

### **Không nên sử dụng khi:**
- Lỗi là một phần bình thường của luồng nghiệp vụ (không phải ngoại lệ)
- Mã lỗi đơn giản và được xử lý ngay tại chỗ
- Trong môi trường hiệu năng cao mà ngoại lệ quá tốn kém

## **Các Bước Thực Hiện**

1. **Xác định mã lỗi**: Tìm các giá trị đặc biệt được dùng để biểu thị lỗi
2. **Tạo ngoại lệ**: Tạo lớp ngoại lệ mới hoặc sử dụng ngoại lệ có sẵn
3. **Thay thế mã lỗi**: Thay thế việc trả về mã lỗi bằng ném ngoại lệ
4. **Cập nhật lời gọi**: Thay thế kiểm tra mã lỗi bằng khối try-catch
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế bằng ngoại lệ:**
```java
class BankAccount {
    private double balance;
    private String accountNumber;
    
    // Trả về mã lỗi thay vì ngoại lệ
    public int withdraw(double amount) {
        if (amount <= 0) {
            return -1; // Mã lỗi: số tiền không hợp lệ
        }
        if (amount > balance) {
            return -2; // Mã lỗi: số dư không đủ
        }
        if (!isAccountActive()) {
            return -3; // Mã lỗi: tài khoản không hoạt động
        }
        
        balance -= amount;
        return 0; // Thành công
    }
    
    // Phương thức khác cũng dùng mã lỗi
    public int transfer(BankAccount target, double amount) {
        int result = withdraw(amount);
        if (result != 0) {
            return result; // Truyền mã lỗi lên trên
        }
        
        return target.deposit(amount);
    }
    
    public int deposit(double amount) {
        if (amount <= 0) {
            return -1;
        }
        balance += amount;
        return 0;
    }
    
    private boolean isAccountActive() {
        // Kiểm tra trạng thái tài khoản
        return true;
    }
}

// Client code - phải kiểm tra mã lỗi ở mọi nơi
BankAccount account = new BankAccount();
int result = account.withdraw(1000);

if (result == -1) {
    System.out.println("Số tiền không hợp lệ");
} else if (result == -2) {
    System.out.println("Số dư không đủ");
} else if (result == -3) {
    System.out.println("Tài khoản không hoạt động");
} else if (result == 0) {
    System.out.println("Rút tiền thành công");
} else {
    System.out.println("Lỗi không xác định: " + result);
}

// Dễ quên kiểm tra mã lỗi
int transferResult = account.transfer(anotherAccount, 500);
// Nếu quên kiểm tra transferResult, lỗi sẽ bị bỏ qua
```

### **✅ Sau khi thay thế bằng ngoại lệ:**
```java
// Custom exceptions cho từng loại lỗi
class InvalidAmountException extends RuntimeException {
    public InvalidAmountException(String message) {
        super(message);
    }
}

class InsufficientFundsException extends RuntimeException {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

class InactiveAccountException extends RuntimeException {
    public InactiveAccountException(String message) {
        super(message);
    }
}

class BankAccount {
    private double balance;
    private String accountNumber;
    
    // Sử dụng ngoại lệ thay vì mã lỗi
    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new InvalidAmountException("Số tiền rút phải lớn hơn 0: " + amount);
        }
        if (amount > balance) {
            throw new InsufficientFundsException(
                String.format("Số dư %.2f không đủ để rút %.2f", balance, amount)
            );
        }
        if (!isAccountActive()) {
            throw new InactiveAccountException("Tài khoản không hoạt động: " + accountNumber);
        }
        
        balance -= amount;
        // Không cần trả về mã lỗi - thành công là mặc định
    }
    
    // Phương thức transfer trở nên đơn giản hơn
    public void transfer(BankAccount target, double amount) {
        withdraw(amount); // Có thể ném ngoại lệ
        target.deposit(amount);
    }
    
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new InvalidAmountException("Số tiền gửi phải lớn hơn 0: " + amount);
        }
        balance += amount;
    }
    
    private boolean isAccountActive() {
        return true;
    }
}

// Client code - xử lý lỗi rõ ràng với try-catch
BankAccount account = new BankAccount();

try {
    account.withdraw(1000);
    System.out.println("Rút tiền thành công");
    
    // Có thể thực hiện nhiều operation trong cùng try block
    account.transfer(anotherAccount, 500);
    System.out.println("Chuyển tiền thành công");
    
} catch (InvalidAmountException e) {
    System.out.println("Lỗi số tiền: " + e.getMessage());
    // Có thể log, hiển thị thông báo cho user, etc.
} catch (InsufficientFundsException e) {
    System.out.println("Lỗi số dư: " + e.getMessage());
    // Gợi ý user nạp thêm tiền
} catch (InactiveAccountException e) {
    System.out.println("Lỗi tài khoản: " + e.getMessage());
    // Hướng dẫn user kích hoạt tài khoản
} catch (Exception e) {
    System.out.println("Lỗi không xác định: " + e.getMessage());
    // Xử lý các lỗi khác
}

// Hoặc có thể để ngoại lệ lan truyền lên trên
public void processPayment(BankAccount account, double amount) {
    // Không cần try-catch ở đây - để caller xử lý
    account.withdraw(amount);
    // Logic xử lý thanh toán...
}
```

## **Lợi Ích**

- **✅ Code rõ ràng hơn**: Tách biệt logic chính và xử lý lỗi
- **✅ Không thể bỏ qua lỗi**: Ngoại lệ phải được xử lý hoặc lan truyền
- **✅ Thông tin lỗi phong phú**: Ngoại lệ có thể chứa message, stack trace, cause
- **✅ Hỗ trợ tốt từ IDE**: Auto-completion và checking cho exception handling
- **✅ Phân cấp lỗi linh hoạt**: Có thể tạo hierarchy của exceptions

## **Điểm Cần Lưu ý**

- **Chọn loại ngoại lệ phù hợp**: Checked vs Unchecked exceptions
- **Không lạm dụng ngoại lệ**: Không dùng cho flow control thông thường
- **Performance**: Ném ngoại lệ tốn kém hơn trả về mã lỗi
- **Thiết kế exception hierarchy**: Tạo exceptions có ý nghĩa cho domain

## **Khi Nào Không Nên Sử Dụng**

- Khi lỗi là phổ biến và là một phần của luồng nghiệp vụ bình thường
- Trong critical code mà performance là ưu tiên hàng đầu
- Khi working với legacy code hoặc API yêu cầu mã lỗi
- Khi lỗi đơn giản và được xử lý ngay tại chỗ

## **Kỹ Thuật Liên Quan**

- **Introduce Null Object**: Giới thiệu đối tượng null
- **Replace Exception with Test**: Thay thế ngoại lệ bằng kiểm tra
- **Extract Method**: Tách phương thức để giảm độ phức tạp
- **Introduce Assertion**: Giới thiệu khẳng định

## **Kết Luận**

Thay thế mã lỗi bằng ngoại lệ là kỹ thuật quan trọng để viết code rõ ràng và robust. Bằng cách sử dụng ngoại lệ, chúng ta tách biệt logic nghiệp vụ khỏi xử lý lỗi, làm cho code dễ đọc và bảo trì hơn. Ngoại lệ cũng cung cấp thông tin chi tiết về lỗi và bắt buộc phải được xử lý, tránh tình trạng bỏ qua lỗi. Tuy nhiên, cần sử dụng một cách thận trọng và chỉ cho các tình huống thực sự ngoại lệ, không phải cho luồng điều khiển thông thường.

**Nguồn:** [refactoring.guru/replace-error-code-with-exception](https://refactoring.guru/replace-error-code-with-exception)