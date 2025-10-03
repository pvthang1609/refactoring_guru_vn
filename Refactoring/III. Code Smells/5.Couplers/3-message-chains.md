# **Mùi Code: Chuỗi Thông Điệp (Message Chains)**

## **Định Nghĩa**
Khi bạn thấy một chuỗi dài các lời gọi phương thức nối tiếp nhau (ví dụ: `a.getB().getC().getD().doSomething()`).

## **Dấu Hiệu Nhận Biết**
- Chuỗi lời gọi phương thức dài và phức tạp
- Client code phụ thuộc vào cấu trúc của nhiều lớp trung gian
- Khó thay đổi vì ảnh hưởng đến toàn bộ chuỗi

## **Vấn Đề**
- Tạo ra sự phụ thuộc chặt chẽ giữa client và cấu trúc đối tượng
- Khó bảo trì khi cấu trúc lớp trung gian thay đổi
- Vi phạm nguyên tắc Demeter (Law of Demeter)

## **Giải Pháp**
**Ẩn đối tượng ủy quyền (Hide Delegate)**
- Tạo phương thức trung gian để ẩn chuỗi gọi
- Trích xuất phương thức (Extract Method)

## **Ví Dụ**
```java
// ❌ Chuỗi thông điệp dài
String managerName = employee.getDepartment().getManager().getName();

// ✅ Ẩn chuỗi gọi
class Employee {
    public String getManagerName() {
        return department.getManager().getName();
    }
}

String managerName = employee.getManagerName();
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Ẩn đối tượng ủy quyền (Hide Delegate)
- 🔧 Trích xuất phương thức (Extract Method)

## **Kết Luận**
Chuỗi thông điệp tạo ra sự phụ thuộc không cần thiết. Bằng cách ẩn các delegate, bạn giảm coupling và làm code dễ bảo trì hơn.

**Nguồn:** [refactoring.guru/smells/message-chains](https://refactoring.guru/smells/message-chains)
