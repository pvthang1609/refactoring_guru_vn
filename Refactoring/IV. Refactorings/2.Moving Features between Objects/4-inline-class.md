# **Hợp nhất Lớp (Inline Class)**

## **Định Nghĩa**
Hợp nhất lớp là kỹ thuật tái cấu trúc di chuyển tất cả các tính năng từ một lớp vào một lớp khác và xóa lớp cũ đi.

## **Khi Nào Sử Dụng**

### **Nên hợp nhất khi:**
- Một lớp không còn đủ trách nhiệm để tồn tại độc lập
- Lớp quá nhỏ và không đủ chức năng (Lazy Class)
- Sau khi di chuyển hết các tính năng quan trọng, lớp chỉ còn lại vỏ rỗng
- Cần đơn giản hóa kiến trúc hệ thống

### **Dấu hiệu nhận biết:**
- Lớp có rất ít phương thức và trường
- Lớp được ủy quyền quá nhiều cho lớp khác (Middle Man)
- Lớp không có lý do rõ ràng để tồn tại độc lập

## **Các Bước Thực Hiện**

1. **Xác định lớp đích**: Chọn lớp sẽ nhận tất cả các tính năng của lớp cần hợp nhất
2. **Di chuyển trường**: Di chuyển tất cả các trường từ lớp nguồn sang lớp đích
3. **Di chuyển phương thức**: Di chuyển tất cả các phương thức từ lớp nguồn sang lớp đích
4. **Cập nhật tham chiếu**: Thay thế tất cả các tham chiếu đến lớp nguồn bằng lớp đích
5. **Xóa lớp nguồn**: Xóa lớp đã được hợp nhất hoàn toàn

## **Ví Dụ Minh Họa**

### **❌ Trước khi hợp nhất:**
```java
class Person {
    private String name;
    private TelephoneNumber telephoneNumber;
    
    public String getName() {
        return name;
    }
    
    public String getTelephoneNumber() {
        return telephoneNumber.getTelephoneNumber();
    }
}

class TelephoneNumber {
    private String areaCode;
    private String number;
    
    public String getTelephoneNumber() {
        return "(" + areaCode + ") " + number;
    }
    
    public String getAreaCode() {
        return areaCode;
    }
    
    public void setAreaCode(String areaCode) {
        this.areaCode = areaCode;
    }
    
    public String getNumber() {
        return number;
    }
    
    public void setNumber(String number) {
        this.number = number;
    }
}
```

### **✅ Sau khi hợp nhất:**
```java
class Person {
    private String name;
    private String areaCode;
    private String number;
    
    public String getName() {
        return name;
    }
    
    public String getTelephoneNumber() {
        return "(" + areaCode + ") " + number;
    }
    
    public String getAreaCode() {
        return areaCode;
    }
    
    public void setAreaCode(String areaCode) {
        this.areaCode = areaCode;
    }
    
    public String getNumber() {
        return number;
    }
    
    public void setNumber(String number) {
        this.number = number;
    }
}
```

## **Lợi Ích**

- **✅ Giảm độ phức tạp**: Giảm số lượng lớp trong hệ thống
- **✅ Đơn giản hóa kiến trúc**: Loại bỏ các lớp trung gian không cần thiết
- **✅ Dễ bảo trì**: Giảm số lượng phụ thuộc giữa các lớp
- **✅ Cải thiện hiệu năng**: Giảm số lần ủy quyền không cần thiết

## **Điểm Cần Lưu Ý**

- **Đảm bảo không mất tính đóng gói**: Vẫn duy trì khả năng bảo vệ dữ liệu
- **Cập nhật tất cả các tham chiếu**: Kiểm tra kỹ tất cả các nơi sử dụng lớp cũ
- **Cân nhắc kỹ trước khi hợp nhất**: Đảm bảo lớp thực sự không cần thiết
- **Giữ lại lịch sử**: Sử dụng version control để có thể khôi phục nếu cần

## **Khi Không Nên Sử Dụng**

- Khi lớp có trách nhiệm rõ ràng và quan trọng
- Khi lớp được sử dụng ở nhiều nơi và việc hợp nhất sẽ tạo ra coupling không cần thiết
- Khi lớp đại diện cho một khái niệm quan trọng trong domain

## **Kỹ Thuật Liên Quan**

- **[Trích xuất Lớp (Extract Class)]()**: Ngược lại với hợp nhất lớp
- **[Di chuyển Phương thức (Move Method)](./1-move-method.md)**: Kỹ thuật cơ bản để di chuyển từng phương thức
- **[Di chuyển Trường (Move Field)](./2-move-field.md)**: Kỹ thuật cơ bản để di chuyển từng trường

## **Kết Luận**

Hợp nhất lớp là kỹ thuật quan trọng để đơn giản hóa hệ thống khi có quá nhiều lớp nhỏ không cần thiết. Tuy nhiên, cần cân nhắc kỹ lưỡng để tránh việc tạo ra các lớp quá lớn và vi phạm nguyên tắc Single Responsibility.

**Nguồn:** [refactoring.guru/inline-class](https://refactoring.guru/inline-class)