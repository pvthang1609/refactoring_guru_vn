# **Trích xuất Biến (Extract Variable)**

## **Định Nghĩa**
Kỹ thuật gán một biểu thức phức tạp cho một biến có tên mô tả rõ ràng.

## **Khi Nào Sử Dụng**
- Khi có biểu thức phức tạp khó hiểu
- Khi cần giải thích ý nghĩa của một phần trong biểu thức
- Khi có code trùng lặp trong cùng một biểu thức

## **Các Bước Thực Hiện**
1. Chọn một biểu thức con từ biểu thức phức tạp
2. Khai báo biến mới và gán biểu thức con vào đó
3. Thay thế biểu thức con bằng biến mới
4. Lặp lại cho các biểu thức con khác nếu cần

## **Ví Dụ**
```java
// ❌ Trước khi trích xuất
if (order.quantity * order.itemPrice > 1000) {
    return order.quantity * order.itemPrice * 0.9;
}
return order.quantity * order.itemPrice * 0.95;

// ✅ Sau khi trích xuất
double basePrice = order.quantity * order.itemPrice;
double discountFactor = basePrice > 1000 ? 0.9 : 0.95;
return basePrice * discountFactor;
```

## **Lợi Ích**
- ✅ Code dễ đọc và hiểu hơn
- ✅ Dễ debug và kiểm tra giá trị trung gian
- ✅ Giảm trùng lặp trong biểu thức
- ✅ Biến có tên giải thích ý nghĩa của biểu thức

## **Khi Không Nên Sử Dụng**
- Khi biểu thức đã đủ đơn giản và rõ ràng
- Khi việc thêm biến làm code phức tạp hơn

## **Kỹ Thuật Liên Quan**
- Trích xuất Phương thức (Extract Method)
- Hợp nhất Biến (Inline Temp)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
