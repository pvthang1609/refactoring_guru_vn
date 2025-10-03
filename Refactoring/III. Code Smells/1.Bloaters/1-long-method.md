# **Mùi Code: Phương Thức Quá Dài (Long Method)**

## **Định Nghĩa**
Một phương thức chứa quá nhiều dòng code, làm giảm khả năng đọc hiểu và bảo trì.

## **Dấu Hiệu Nhận Biết**
- Phương thức dài hàng chục hoặc hàng trăm dòng
- Có nhiều cấp độ thụt lề (nested logic)
- Khó đặt tên mô tả chính xác cho phương thức
- Cần comment để giải thích các phần trong phương thức

## **Nguyên Nhân**
- Thêm tính năng mới vào phương thức hiện có mà không tái cấu trúc
- Ngại tách phương thức nhỏ hơn
- Thiếu kỷ luật trong việc giữ phương thức ngắn gọn

## **Giải Pháp**
**Trích xuất phương thức (Extract Method)**
- Tách các đoạn code thành phương thức riêng với tên mô tả rõ ràng
- Mỗi phương thức chỉ thực hiện một nhiệm vụ duy nhất

**Thay thế biến tạm bằng truy vấn (Replace Temp with Query)**
**Giới thiệu đối tượng tham số (Introduce Parameter Object)**

## **Ví Dụ**
```java
// ❌ Phương thức quá dài
public void printOwing() {
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

// ✅ Tách thành phương thức nhỏ
public void printOwing() {
    printBanner();
    double outstanding = calculateOutstanding();
    printDetails(outstanding);
}

private double calculateOutstanding() {
    double outstanding = 0.0;
    for (Order order : orders) {
        outstanding += order.getAmount();
    }
    return outstanding;
}

private void printDetails(double outstanding) {
    System.out.println("Tên: " + name);
    System.out.println("Số tiền: " + outstanding);
}
```

## **Khi Nào Có Thể Chấp Nhận?**
- Phương thức chỉ chứa một cấu trúc điều khiển đơn giản
- Code đã đủ rõ ràng và dễ hiểu
- Không có logic phức tạp cần tách riêng

## **Lợi Ích**
- ✅ Dễ đọc và hiểu code hơn
- ✅ Tái sử dụng code tốt hơn
- ✅ Dễ dàng kiểm thử từng phần nhỏ
- ✅ Giảm khả năng xuất hiện lỗi

## **Kết Luận**
Phương thức dài là một trong những mùi code phổ biến nhất. Giữ phương thức ngắn gọn (thường dưới 10-15 dòng) giúp code dễ bảo trì và mở rộng hơn.

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
