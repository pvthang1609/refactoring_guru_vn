# **Trích xuất Phương thức (Extract Method)**

## **Định Nghĩa**
Kỹ thuật tách một đoạn code thành phương thức mới với tên mô tả rõ ràng mục đích của nó.

## **Khi Nào Sử Dụng**
- Khi có một đoạn code có thể được nhóm lại với nhau
- Khi code trở nên khó đọc và khó hiểu
- Khi có code trùng lặp ở nhiều nơi

## **Các Bước Thực Hiện**
1. Tạo phương thức mới và đặt tên mô tả những gì nó làm
2. Sao chép đoạn code liên quan sang phương thức mới
3. Tạo các tham số cần thiết để truyền dữ liệu
4. Thay thế đoạn code gốc bằng lời gọi đến phương thức mới

## **Ví Dụ**
```java
// ❌ Trước khi trích xuất
void printOwing() {
    printBanner();
    
    // Tính toán số tiền nợ
    double outstanding = 0.0;
    for (Order order : orders) {
        outstanding += order.getAmount();
    }
    
    // In chi tiết
    System.out.println("Tên: " + name);
    System.out.println("Số tiền: " + outstanding);
}

// ✅ Sau khi trích xuất
void printOwing() {
    printBanner();
    double outstanding = calculateOutstanding();
    printDetails(outstanding);
}

double calculateOutstanding() {
    double outstanding = 0.0;
    for (Order order : orders) {
        outstanding += order.getAmount();
    }
    return outstanding;
}

void printDetails(double outstanding) {
    System.out.println("Tên: " + name);
    System.out.println("Số tiền: " + outstanding);
}
```

## **Lợi Ích**
- ✅ Code dễ đọc và hiểu hơn
- ✅ Giảm trùng lặp code
- ✅ Tái sử dụng code tốt hơn
- ✅ Dễ dàng kiểm thử từng phần

## **Khi Không Nên Sử Dụng**
- Khi đoạn code chỉ có một dòng đơn giản
- Khi code đã đủ rõ ràng và dễ hiểu

## **Kỹ Thuật Liên Quan**
- Hợp nhất Phương thức (Inline Method)
- Thay thế Biến tạm bằng Truy vấn (Replace Temp with Query)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
