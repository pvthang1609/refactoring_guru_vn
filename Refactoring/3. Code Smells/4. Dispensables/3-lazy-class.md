# **Mùi Code: Lớp Lười Nhác (Lazy Class)**

## **Định Nghĩa**
Một lớp thực hiện quá ít chức năng, không đủ để biện minh cho sự tồn tại của nó.

## **Dấu Hiệu Nhận Biết**
- Lớp có rất ít phương thức và thuộc tính
- Lớp được tạo cho một mục đích cụ thể nhưng hiện không còn cần thiết
- Chi phí bảo trì lớp vượt quá giá trị nó mang lại

## **Nguyên Nhân**
- Lớp được tạo cho tính năng có thể đã bị loại bỏ
- Quá nhiều lớp được tạo trong quá trình thiết kế
- Tách lớp không cần thiết

## **Giải Pháp**
**Hợp nhất lớp (Inline Class)**
- Di chuyển các phương thức và trường sang lớp khác
- Xóa lớp không cần thiết

## **Ví Dụ**
```java
// ❌ Lớp lười nhác
class AddressValidator {
    public boolean isValid(String address) {
        return address != null && !address.trim().isEmpty();
    }
}

class User {
    private String address;
    private AddressValidator validator = new AddressValidator();
    
    public boolean validateAddress() {
        return validator.isValid(address);
    }
}

// ✅ Hợp nhất lớp
class User {
    private String address;
    
    public boolean validateAddress() {
        return address != null && !address.trim().isEmpty();
    }
}
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Hợp nhất lớp (Inline Class)
- 🔧 Hợp nhất hệ thống phân cấp (Collapse Hierarchy)

## **Kết Luận**
Lớp lười nhác làm tăng độ phức tạp không cần thiết. Hợp nhất hoặc loại bỏ chúng giúp codebase gọn gàng và dễ bảo trì hơn.
