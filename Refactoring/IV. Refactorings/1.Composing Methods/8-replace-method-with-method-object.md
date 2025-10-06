# **Thay thế Phương thức bằng Đối tượng Phương thức (Replace Method with Method Object)**

## **Định Nghĩa**
Kỹ thuật chuyển đổi một phương thức phức tạp thành một đối tượng riêng biệt, cho phép xử lý các biến cục bộ thành các trường của đối tượng.

## **Khi Nào Sử Dụng**
- Khi phương thức quá dài và phức tạp, chứa nhiều biến tạm
- Khi không thể áp dụng Extract Method do có quá nhiều biến cục bộ
- Khi cần tách biệt logic phức tạp để dễ quản lý và test

## **Các Bước Thực Hiện**
1. Tạo lớp mới với tên mô tả mục đích của phương thức
2. Trong lớp mới, tạo trường cho đối tượng gốc và từng tham số của phương thức
3. Tạo constructor nhận các tham số này
4. Tạo phương thức `compute()` và chuyển toàn bộ code của phương thức gốc vào
5. Thay thế phương thức gốc bằng việc tạo đối tượng mới và gọi `compute()`

## **Ví Dụ**
```java
// ❌ Trước khi thay thế
class Order {
    public double price() {
        double primaryBasePrice;
        double secondaryBasePrice;
        double tertiaryBasePrice;
        // ... nhiều tính toán phức tạp với nhiều biến tạm
        return finalPrice;
    }
}

// ✅ Sau khi thay thế
class Order {
    public double price() {
        return new PriceCalculator(this).compute();
    }
}

class PriceCalculator {
    private Order order;
    private double primaryBasePrice;
    private double secondaryBasePrice;
    private double tertiaryBasePrice;
    
    public PriceCalculator(Order order) {
        this.order = order;
    }
    
    public double compute() {
        // ... logic tính toán phức tạp
        return finalPrice;
    }
}
```

## **Lợi Ích**
- ✅ Tách biệt logic phức tạp thành đối tượng độc lập
- ✅ Dễ dàng áp dụng các kỹ thuật tái cấu trúc khác trên đối tượng mới
- ✅ Code trở nên rõ ràng và dễ quản lý hơn
- ✅ Dễ dàng viết unit test cho logic phức tạp

## **Khi Không Nên Sử Dụng**
- Khi phương thức đủ đơn giản
- Khi số lượng biến cục bộ ít
- Khi có thể sử dụng Extract Method thay thế

## **Kỹ Thuật Liên Quan**
- Trích xuất Phương thức (Extract Method)
- Trích xuất Lớp (Extract Class)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
