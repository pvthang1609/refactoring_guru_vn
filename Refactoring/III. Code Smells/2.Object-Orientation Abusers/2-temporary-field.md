# **Mùi Code: Trường Tạm Thời (Temporary Field)**

## **Dấu hiệu Nhận biết**
Một trường (field) trong lớp chỉ được sử dụng trong những trường hợp cụ thể, và để `null` hoặc không có giá trị trong các trường hợp khác.

## **Nguyên nhân**
Các biến instance (trường) chỉ được sử dụng trong một số tình huống đặc biệt, chẳng hạn như khi đối tượng ở một trạng thái nhất định hoặc khi một phương thức cụ thể được gọi.

## **Vấn đề**
- **Khó hiểu và khó bảo trì**: Người đọc code khó hiểu tại sao một trường lại tồn tại nếu nó không luôn được sử dụng
- **Tăng độ phức tạp không cần thiết**: Lớp chứa nhiều trường hơn mức cần thiết
- **Dễ gây lỗi**: Có thể vô tình sử dụng trường khi nó chưa được khởi tạo
- **Vi phạm nguyên tắc Đóng gói**: Trạng thái của đối tượng không nhất quán

## **Giải pháp**

### **Di chuyển Trường (Move Field)**
- Di chuyển trường tạm thời và các phương thức liên quan đến một lớp mới
- Tạo một lớp chuyên biệt cho các trường và hành vi liên quan

### **Trích xuất Lớp (Extract Class)**
- Nhóm các trường tạm thời liên quan với nhau vào một lớp riêng
- Đảm bảo lớp mới có mục đích rõ ràng và trạng thái nhất quán

### **Giới thiệu Đối tượng Null (Introduce Null Object)**
- Thay thế các điều kiện kiểm tra `null` bằng một đối tượng có hành vi mặc định

## **Kỹ thuật Tái cấu trúc**
- 🔧 **Trích xuất Lớp** (Extract Class)
- 🔧 **Di chuyển Trường** (Move Field)
- 🔧 **Giới thiệu Đối tượng Null** (Introduce Null Object)
- 🔧 **Thay thế Kiểu mã bằng Lớp con** (Replace Type Code with Subclasses)
- 🔧 **Thay thế Kiểu mã bằng Trạng thái/Chiến lược** (Replace Type Code with State/Strategy)

## **Lợi ích**
- ✅ **Giảm độ phức tạp**: Loại bỏ các trường không cần thiết
- ✅ **Cải thiện tính rõ ràng**: Mỗi lớp có trách nhiệm cụ thể và rõ ràng
- ✅ **Giảm lỗi**: Tránh tình trạng sử dụng trường khi chưa được khởi tạo
- ✅ **Tái sử dụng tốt hơn**: Các lớp nhỏ, chuyên biệt dễ tái sử dụng hơn

## **Khi nào Chấp nhận?**
Có thể chấp nhận "Temporary Field" trong các trường hợp:
- 🟡 **Tối ưu hóa hiệu năng**: Khi cần lưu trữ kết quả trung gian cho tính toán phức tạp
- 🟡 **Xử lý trạng thái tạm thời**: Trong các mẫu thiết kế như Memento hoặc Command
- 🟡 **Tích hợp với framework**: Khi framework yêu cầu các trường tạm thời

## **Ví dụ Minh họa**

```java
// ❌ Vấn đề: Trường tạm thời
class Customer {
    private String name;
    private String email;
    private double discountRate; // Chỉ được sử dụng trong một số trường hợp
    private boolean isPremium;   // Xác định khi nào discountRate được sử dụng
    
    public double calculatePrice(double price) {
        if (isPremium) {
            return price * (1 - discountRate);
        }
        return price;
    }
}

// ✅ Giải pháp: Trích xuất lớp
class Customer {
    private String name;
    private String email;
    private CustomerType type; // Thay thế trường tạm thời bằng đối tượng chuyên biệt
    
    public double calculatePrice(double price) {
        return type.applyDiscount(price);
    }
}

class CustomerType {
    private double discountRate;
    
    public double applyDiscount(double price) {
        return price * (1 - discountRate);
    }
}
```

## **Các bước Thực hiện**
1. **Xác định** các trường chỉ được sử dụng trong một số điều kiện
2. **Nhóm** các trường và phương thức liên quan với nhau
3. **Tạo lớp mới** và di chuyển các trường tạm thời vào lớp đó
4. **Đảm bảo** lớp mới có giao diện rõ ràng và trách nhiệm duy nhất
5. **Cập nhật** các tham chiếu đến trường tạm thời để sử dụng đối tượng mới

## **Kết luận**
"Temporary Field" là dấu hiệu cho thấy **một lớp đang cố gắng thực hiện quá nhiều trách nhiệm**. Bằng cách:

- 🎯 **Trích xuất các lớp chuyên biệt** cho từng nhóm trường liên quan
- 🎯 **Đảm bảo mỗi trường luôn có ý nghĩa** trong mọi ngữ cảnh sử dụng
- 🎯 **Sử dụng composition thay vì thêm trường tạm**

Việc loại bỏ mùi code này giúp hệ thống **dễ hiểu hơn, ít lỗi hơn và dễ bảo trì hơn**.

**Nguồn:** [refactoring.guru/smells/temporary-field](https://refactoring.guru/smells/temporary-field)
