# **Mùi Code: Hệ Thống Kế Thừa Song Song (Parallel Inheritance Hierarchies)**

## **Định Nghĩa**
Khi mỗi lần tạo một lớp con cho một lớp, bạn buộc phải tạo một lớp con tương ứng cho một lớp khác.

## **Dấu Hiệu Nhận Biết**
- Hệ thống phân cấp lớp phát triển song song
- Tạo lớp con trong hierarchy này yêu cầu tạo lớp con trong hierarchy kia
- Các lớp trong hai hierarchy phụ thuộc chặt chẽ

## **Nguyên Nhân**
Hai hệ thống phân cấp kế thừa bị ràng buộc với nhau, thường do thiết kế ban đầu không tốt.

## **Giải Pháp**
**Hợp nhất hệ thống phân cấp**
- Di chuyển phương thức (Move Method)
- Di chuyển trường (Move Field)
- Loại bỏ một trong các hierarchy

## **Ví Dụ**
```java
// ❌ Hệ thống song song
class Vehicle { ... }
class Car extends Vehicle { ... }
class Truck extends Vehicle { ... }

class Driver { ... }
class CarDriver extends Driver { ... }
class TruckDriver extends Driver { ... }

// ✅ Hợp nhất hierarchy
class Vehicle {
    Driver driver;
    // ...
}
```

## **Lợi Ích**
- Giảm sự phức tạp không cần thiết
- Dễ dàng mở rộng và bảo trì
- Giảm số lượng lớp
