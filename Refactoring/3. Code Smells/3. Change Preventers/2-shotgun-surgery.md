# **Mùi Code: Phẫu Thuật Shotgun (Shotgun Surgery)**

## **Định Nghĩa**
Khi một thay đổi nhỏ buộc phải sửa đổi nhiều lớp khác nhau.

## **Dấu Hiệu Nhận Biết**
- Một thay đổi đơn lẻ yêu cầu sửa nhiều file/lớp
- Các lớp có liên kết chặt chẽ với nhau
- Code bị trùng lặp ở nhiều nơi

## **Nguyên Nhân**
Các trách nhiệm liên quan bị phân tán across nhiều lớp thay vì tập trung vào một chỗ.

## **Giải Pháp**
**Hợp nhất lớp (Inline Class)**
- Di chuyển phương thức (Move Method)
- Di chuyển trường (Move Field)
- Tập trung các hành vi liên quan vào một lớp

## **Ví Dụ**
```java
// ❌ Code bị phân tán
class Order {
    void calculateTotal() { ... }
}

class Discount {
    void applyDiscount(Order order) { ... }
}

class Tax {
    void calculateTax(Order order) { ... }
}

// ✅ Tập trung logic liên quan
class Order {
    void calculateTotal() {
        // Bao gồm discount và tax
    }
}
```

## **Lợi Ích**
- Giảm effort cho thay đổi
- Tập trung logic liên quan
- Dễ dàng bảo trì và mở rộng
