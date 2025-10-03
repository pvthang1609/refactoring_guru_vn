# **Mùi Code: Tính Tổng Quát Hóa Suy Đoán (Speculative Generality)**

## **Định Nghĩa**
Khi code được thiết kế quá tổng quát cho các nhu cầu trong tương lai có thể không bao giờ xảy ra.

## **Dấu Hiệu Nhận Biết**
- Lớp, phương thức, tham số hoặc trường không được sử dụng
- Hệ thống phân cấp phức tạp không cần thiết
- Code "phòng hờ" cho tính năng chưa bao giờ được triển khai

## **Vấn Đề**
- Code phức tạp hơn mức cần thiết
- Khó hiểu và bảo trì
- Tốn thời gian phát triển các tính năng không dùng đến

## **Giải Pháp**
**Đơn giản hóa code**
- Hợp nhất lớp (Inline Class)
- Hợp nhất phương thức (Inline Method)
- Gỡ bỏ tham số không sử dụng (Remove Parameter)
- Thu gọn hệ thống phân cấp (Collapse Hierarchy)

## **Ví Dụ**
```java
// ❌ Tính tổng quát hóa suy đoán
interface Animal { void eat(); void sleep(); void fly(); } // fly() không cần cho mọi animal

class Dog implements Animal {
    public void eat() { ... }
    public void sleep() { ... }
    public void fly() { } // Phương thức không bao giờ dùng
}

// ✅ Đơn giản hóa
interface Animal { void eat(); void sleep(); }

class Dog implements Animal {
    public void eat() { ... }
    public void sleep() { ... }
}
```

## **Khi Nào Thì Chấp Nhận?**
Chỉ nên giữ lại tính tổng quát khi:
- Có kế hoạch cụ thể sử dụng trong tương lai gần
- Đang xây dựng thư viện/thư viện framework
- Yêu cầu rõ ràng từ kiến trúc hệ thống

## **Kết Luận**
Tránh "suy đoán" về nhu cầu tương lai. Chỉ triển khai những gì thực sự cần thiết, code sẽ đơn giản và dễ bảo trì hơn.

**Nguồn:** [refactoring.guru/smells/speculative-generality](https://refactoring.guru/smells/speculative-generality)
