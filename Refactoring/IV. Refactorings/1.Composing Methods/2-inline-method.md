# **Hợp nhất Phương thức (Inline Method)**

## **Định Nghĩa**
Kỹ thuật thay thế lời gọi phương thức bằng nội dung thực tế của phương thức đó.

## **Khi Nào Sử Dụng**
- Khi phương thức quá đơn giản, chỉ chứa 1-2 dòng code
- Khi phương thức không làm code dễ đọc hơn
- Khi có quá nhiều phương thức ủy quyền làm code khó theo dõi

## **Các Bước Thực Hiện**
1. Đảm bảo phương thức không được ghi đè trong lớp con
2. Tìm tất cả các lời gọi đến phương thức
3. Thay thế mỗi lời gọi bằng nội dung của phương thức
4. Xóa phương thức đã hợp nhất

## **Ví Dụ**
```java
// ❌ Trước khi hợp nhất
class PizzaOrder {
    private int quantity;
    private double itemPrice;
    
    public double getPrice() {
        return basePrice() * discountFactor();
    }
    
    private double basePrice() {
        return quantity * itemPrice;
    }
    
    private double discountFactor() {
        return getPrice() > 1000 ? 0.95 : 0.98;
    }
}

// ✅ Sau khi hợp nhất
class PizzaOrder {
    private int quantity;
    private double itemPrice;
    
    public double getPrice() {
        double basePrice = quantity * itemPrice;
        double discountFactor = basePrice > 1000 ? 0.95 : 0.98;
        return basePrice * discountFactor;
    }
}
```

## **Lợi Ích**
- ✅ Giảm số lượng phương thức không cần thiết
- ✅ Code trực tiếp và dễ theo dõi hơn
- ✅ Giảm độ phức tạp không cần thiết

## **Khi Không Nên Sử Dụng**
- Khi phương thức có logic phức tạp
- Khi phương thức được sử dụng ở nhiều nơi
- Khi phương thức có tên mô tả rõ ý nghĩa

## **Kỹ Thuật Liên Quan**
- Trích xuất Phương thức (Extract Method)
- Hợp nhất Lớp (Inline Class)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
