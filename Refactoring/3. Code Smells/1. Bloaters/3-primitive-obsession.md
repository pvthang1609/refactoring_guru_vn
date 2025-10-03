# **Mùi Code: Ám Ảnh Nguyên Thủy (Primitive Obsession)**

## **Định Nghĩa**
Sử dụng quá nhiều kiểu dữ liệu nguyên thủy (int, string, float) thay vì tạo các đối tượng chuyên biệt cho các khái niệm trong domain.

## **Dấu Hiệu Nhận Biết**
- Sử dụng string cho số điện thoại, email, địa chỉ
- Dùng số nguyên cho tiền tệ, tỷ lệ phần trăm
- Dùng mảng cho các nhóm dữ liệu có cấu trúc

## **Vấn Đề**
- Không thể đảm bảo tính hợp lệ của dữ liệu
- Logic validation bị phân tán khắp nơi
- Khó biểu đạt ý nghĩa thực sự của dữ liệu

## **Giải Pháp**
**Thay thế kiểu nguyên thủy bằng đối tượng (Replace Primitive with Object)**
- Tạo lớp chuyên biệt cho từng khái niệm domain
- Đóng gói logic validation trong lớp

## **Ví Dụ**
```java
// ❌ Ám ảnh nguyên thủy
class Customer {
    private String phoneNumber; // "0123-456-789"
    private String email;       // "user@example.com"
    private double balance;     // 1000.0 (tiền tệ nào?)
}

// ✅ Sử dụng đối tượng chuyên biệt
class Customer {
    private PhoneNumber phoneNumber;
    private Email email;
    private Money balance;
}

class PhoneNumber {
    private String value;
    public PhoneNumber(String value) {
        if (!isValid(value)) throw new IllegalArgumentException();
        this.value = value;
    }
}

class Money {
    private double amount;
    private Currency currency;
    
    public Money(double amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }
}
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Thay thế kiểu nguyên thủy bằng đối tượng
- 🔧 Thay thế mã kiểu bằng lớp con
- 🔧 Giới thiệu tham số đối tượng

## **Kết Luận**
Ám ảnh nguyên thủy làm giảm khả năng biểu đạt của code. Sử dụng đối tượng chuyên biệt giúp code rõ ràng hơn và đảm bảo tính toàn vẹn dữ liệu.
