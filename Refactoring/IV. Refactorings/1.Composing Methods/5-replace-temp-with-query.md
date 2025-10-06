# **Thay thế Biến tạm bằng Truy vấn (Replace Temp with Query)**

## **Định Nghĩa**
Kỹ thuật thay thế các biến tạm bằng các lời gọi phương thức (truy vấn) để tính toán giá trị.

## **Khi Nào Sử Dụng**
- Khi biến tạm được sử dụng để lưu kết quả của một biểu thức
- Khi kết quả tính toán không thay đổi trong phạm vi hiện tại
- Khi muốn loại bỏ biến tạm để code sạch hơn

## **Các Bước Thực Hiện**
1. Xác định biến tạm được gán giá trị từ một biểu thức
2. Khai báo phương thức mới và chuyển biểu thức vào đó
3. Thay thế tất cả các tham chiếu đến biến tạm bằng lời gọi phương thức mới
4. Xóa khai báo biến tạm

## **Ví Dụ**
```java
// ❌ Trước khi thay thế
double calculateTotal() {
    double basePrice = quantity * itemPrice;
    if (basePrice > 1000) {
        return basePrice * 0.95;
    } else {
        return basePrice * 0.98;
    }
}

// ✅ Sau khi thay thế
double calculateTotal() {
    if (basePrice() > 1000) {
        return basePrice() * 0.95;
    } else {
        return basePrice() * 0.98;
    }
}

private double basePrice() {
    return quantity * itemPrice;
}
```

## **Lợi Ích**
- ✅ Giảm số lượng biến tạm trong code
- ✅ Code dễ đọc và bảo trì hơn
- ✅ Tái sử dụng logic tính toán ở nhiều nơi
- ✅ Dễ dàng kiểm thử các phương thức nhỏ

## **Khi Không Nên Sử Dụng**
- Khi biểu thức có tác dụng phụ (side effects)
- Khi hiệu năng là vấn đề quan trọng và biểu thức tính toán phức tạp
- Khi biến tạm làm code dễ hiểu hơn

## **Kỹ Thuật Liên Quan**
- Trích xuất Phương thức (Extract Method)
- Trích xuất Biến (Extract Variable)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
