# **Di Chuyển Trường (Move Field)**

## **Định Nghĩa**
Di chuyển trường là kỹ thuật tái cấu trúc di chuyển một trường (field) từ lớp hiện tại sang lớp khác nơi nó phù hợp hơn.

## **Khi Nào Sử Dụng**

### **Nên di chuyển khi:**
- Một trường được sử dụng nhiều hơn bởi một lớp khác so với lớp hiện tại của nó
- Trường không phù hợp với trách nhiệm chính của lớp hiện tại
- Cần giảm sự phụ thuộc giữa các lớp
- Muốn tăng tính gắn kết của lớp

### **Dấu hiệu nhận biết:**
- Một lớp thường xuyên truy cập trường của lớp khác thông qua getter/setter
- Có mối quan hệ dữ liệu chặt chẽ giữa hai lớp
- Trường ít được sử dụng trong lớp hiện tại nhưng thường xuyên được sử dụng bởi lớp khác

## **Các Bước Thực Hiện**

1. **Đóng gói trường**: Đảm bảo trường được đóng gói (private với getter/setter)
2. **Tạo trường mới**: Tạo trường mới trong lớp đích
3. **Cập nhật getter/setter**: Sửa getter/setter để sử dụng trường mới
4. **Cập nhật tham chiếu**: Cập nhật tất cả các tham chiếu đến trường cũ
5. **Xóa trường cũ**: Xóa trường khỏi lớp cũ

## **Ví Dụ Minh Họa**

### **❌ Trước khi di chuyển:**
```java
class Customer {
    private String name;
    private double discountRate; // Trường này thuộc về CustomerType
    
    public double applyDiscount(double amount) {
        return amount - (amount * discountRate);
    }
    
    public double getDiscountRate() {
        return discountRate;
    }
    
    public void setDiscountRate(double discountRate) {
        this.discountRate = discountRate;
    }
}

// Lớp này thường xuyên sử dụng discountRate
class Order {
    private Customer customer;
    private double total;
    
    public double getFinalPrice() {
        return total - (total * customer.getDiscountRate());
    }
}
```

### **✅ Sau khi di chuyển:**
```java
class Customer {
    private String name;
    private CustomerType type; // Thay thế discountRate bằng CustomerType
    
    public double applyDiscount(double amount) {
        return type.applyDiscount(amount);
    }
    
    public CustomerType getType() {
        return type;
    }
}

class CustomerType {
    private double discountRate; // Trường được di chuyển đến đây
    
    public double applyDiscount(double amount) {
        return amount - (amount * discountRate);
    }
    
    public double getDiscountRate() {
        return discountRate;
    }
    
    public void setDiscountRate(double discountRate) {
        this.discountRate = discountRate;
    }
}

class Order {
    private Customer customer;
    private double total;
    
    public double getFinalPrice() {
        return customer.getType().applyDiscount(total);
    }
}
```

## **Lợi Ích**

- **✅ Tăng tính gắn kết**: Dữ liệu được đặt trong lớp có trách nhiệm quản lý nó
- **✅ Giảm coupling**: Giảm sự phụ thuộc trực tiếp giữa các lớp
- **✅ Dễ bảo trì**: Thay đổi trong cấu trúc dữ liệu chỉ ảnh hưởng đến lớp chứa nó
- **✅ Tái sử dụng tốt hơn**: Dữ liệu có thể được chia sẻ và sử dụng một cách hợp lý hơn

## **Điểm Cần Lưu Ý**

- **Đảm bảo đóng gói**: Luôn sử dụng getter/setter thay vì truy cập trực tiếp vào trường
- **Cập nhật tất cả tham chiếu**: Kiểm tra kỹ tất cả các vị trí sử dụng trường
- **Cân nhắc di chuyển phương thức**: Nếu cần, di chuyển cả các phương thức liên quan đến trường
- **Kiểm tra tính nhất quán**: Đảm bảo dữ liệu vẫn nhất quán sau khi di chuyển

## **Kỹ Thuật Liên Quan**

- **[Di chuyển Phương thức (Move Method)](./1-move-method.md)**: Khi cần di chuyển cả phương thức làm việc với trường
- **Trích xuất Lớp (Extract Class)**: Khi một nhóm trường và phương thức nên được tách thành lớp mới
- **Đóng gói Trường (Encapsulate Field)**: Bước chuẩn bị quan trọng trước khi di chuyển trường

## **Kết Luận**

Di chuyển trường giúp cải thiện thiết kế hướng đối tượng bằng cách đặt dữ liệu vào đúng lớp có trách nhiệm quản lý nó. Kỹ thuật này đặc biệt hữu ích khi phát hiện ra sự phân bố trách nhiệm không hợp lý trong hệ thống hiện tại.