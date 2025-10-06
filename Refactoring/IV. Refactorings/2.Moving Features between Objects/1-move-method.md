# **Di Chuyển Phương Thức (Move Method)**

## **Định Nghĩa**
Di chuyển phương thức là kỹ thuật tái cấu trúc di chuyển một phương thức từ lớp hiện tại sang lớp khác nơi nó phù hợp hơn.

## **Khi Nào Sử Dụng**

### **Nên di chuyển khi:**
- Phương thức được sử dụng nhiều hơn bởi một lớp khác so với lớp hiện tại
- Phương thức không phù hợp với trách nhiệm chính của lớp hiện tại
- Cần giảm sự phụ thuộc giữa các lớp
- Muốn tăng tính gắn kết của lớp

### **Dấu hiệu nhận biết:**
- Một lớp có quá nhiều phương thức không liên quan chặt chẽ đến trách nhiệm chính
- Phương thức thường xuyên sử dụng dữ liệu của lớp khác
- Có mối quan hệ "ghen tị tính năng" (Feature Envy) giữa các lớp

## **Các Bước Thực Hiện**

1. **Kiểm tra phụ thuộc**: Xác định tất cả các thành phần mà phương thức hiện tại sử dụng
2. **Tạo phương thức mới**: Trong lớp đích, tạo phương thức mới với cùng chữ ký
3. **Copy code**: Sao chép code từ phương thức gốc sang phương thức mới
4. **Điều chỉnh tham chiếu**: Cập nhật các tham chiếu đến thuộc tính và phương thức của lớp cũ
5. **Xóa phương thức gốc**: Xóa phương thức khỏi lớp cũ
6. **Cập nhật lời gọi**: Thay thế tất cả lời gọi đến phương thức cũ bằng lời gọi đến phương thức mới

## **Ví Dụ Minh Họa**

### **❌ Trước khi di chuyển:**
```java
class BankAccount {
    private String accountNumber;
    private double balance;
    private AccountType type;
    
    // Phương thức này thuộc về AccountType hơn là BankAccount
    public double calculateOverdraftFee() {
        if (type.isPremium()) {
            return balance * 0.025;
        } else {
            return balance * 0.05;
        }
    }
}

class AccountType {
    private boolean premium;
    
    public boolean isPremium() {
        return premium;
    }
}
```

### **✅ Sau khi di chuyển:**
```java
class BankAccount {
    private String accountNumber;
    private double balance;
    private AccountType type;
    
    public double calculateOverdraftFee() {
        return type.calculateOverdraftFee(balance);
    }
}

class AccountType {
    private boolean premium;
    
    public boolean isPremium() {
        return premium;
    }
    
    public double calculateOverdraftFee(double balance) {
        if (isPremium()) {
            return balance * 0.025;
        } else {
            return balance * 0.05;
        }
    }
}
```

## **Lợi Ích**

- **✅ Tăng tính gắn kết**: Mỗi lớp chỉ chứa các phương thức liên quan đến trách nhiệm của nó
- **✅ Giảm coupling**: Giảm sự phụ thuộc không cần thiết giữa các lớp
- **✅ Dễ bảo trì**: Thay đổi trong logic chỉ ảnh hưởng đến lớp chứa nó
- **✅ Tái sử dụng tốt hơn**: Phương thức ở đúng vị trí có thể được sử dụng bởi nhiều thành phần khác

## **Điểm Cần Lưu Ý**

- **Không di chuyển phương thức** nếu nó tạo ra vòng phụ thuộc giữa các lớp
- **Cân nhắc việc di chuyển** các trường dữ liệu liên quan cùng với phương thức
- **Đảm bảo tính nhất quán** của dữ liệu sau khi di chuyển
- **Kiểm tra kỹ** tất cả các vị trí gọi phương thức trước và sau khi di chuyển

## **Kỹ Thuật Liên Quan**

- **Di chuyển Trường (Move Field)**: Khi cần di chuyển cả dữ liệu và phương thức
- **Trích xuất Lớp (Extract Class)**: Khi một nhóm phương thức và trường nên được tách thành lớp mới
- **Hợp nhất Lớp (Inline Class)**: Ngược lại với trích xuất lớp

## **Kết Luận**

Di chuyển phương thức là kỹ thuật cơ bản nhưng mạnh mẽ để cải thiện thiết kế hướng đối tượng. Bằng cách đặt mỗi phương thức vào đúng lớp có trách nhiệm phù hợp, chúng ta tạo ra hệ thống dễ bảo trì, mở rộng và ít lỗi hơn.