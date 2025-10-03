# **Hợp nhất Biến tạm (Inline Temp)**

## **Định Nghĩa**
Kỹ thuật thay thế các tham chiếu đến biến tạm bằng biểu thức gốc của nó.

## **Khi Nào Sử Dụng**
- Khi biến tạm không giúp code dễ đọc hơn
- Khi biến tạm cản trở các kỹ thuật tái cấu trúc khác
- Khi biến chỉ được sử dụng một lần và không mang lại giá trị giải thích

## **Các Bước Thực Hiện**
1. Tìm tất cả các vị trí sử dụng biến tạm
2. Thay thế từng tham chiếu bằng biểu thức gốc
3. Xóa khai báo và gán giá trị của biến tạm

## **Ví Dụ**
```java
// ❌ Trước khi hợp nhất
double basePrice = order.getQuantity() * order.getItemPrice();
return basePrice > 1000;

// ✅ Sau khi hợp nhất
return order.getQuantity() * order.getItemPrice() > 1000;
```

## **Lợi Ích**
- ✅ Giảm số lượng biến không cần thiết
- ✅ Code ngắn gọn và trực tiếp hơn
- ✅ Tạo điều kiện cho các kỹ thuật tái cấu trúc khác

## **Khi Không Nên Sử Dụng**
- Khi biến có tên giải thích rõ ý nghĩa của biểu thức
- Khi biểu thức quá phức tạp và cần biến để dễ đọc
- Khi biến được sử dụng nhiều lần trong code

## **Kỹ Thuật Liên Quan**
- Trích xuất Biến (Extract Variable)
- Thay thế Biến tạm bằng Truy vấn (Replace Temp with Query)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
