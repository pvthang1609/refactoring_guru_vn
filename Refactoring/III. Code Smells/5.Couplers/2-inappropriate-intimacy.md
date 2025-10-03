# **Mùi Code: Quá Thân Mật (Inappropriate Intimacy)**

## **Định Nghĩa**
Khi hai lớp biết quá nhiều về chi tiết nội bộ của nhau, tạo ra sự phụ thuộc chặt chẽ không cần thiết.

## **Dấu Hiệu Nhận Biết**
- Một lớp truy cập trực tiếp vào private fields/methods của lớp khác
- Hai lớp thường xuyên thay đổi cùng nhau
- Quan hệ hai chiều không cần thiết

## **Vấn Đề**
- Thay đổi trong một lớp ảnh hưởng trực tiếp đến lớp kia
- Khó tái sử dụng từng lớp độc lập
- Vi phạm nguyên tắc đóng gói

## **Giải Pháp**
**Giảm sự phụ thuộc**
- Di chuyển phương thức (Move Method)
- Di chuyển trường (Move Field)
- Thay đổi tham chiếu hai chiều thành một chiều
- Trích xuất lớp (Extract Class)

## **Ví Dụ**
```java
// ❌ Quá thân mật
class Person {
    private String name;
    private Department department;
    
    public Department getDepartment() { return department; }
}

class Department {
    private String name;
    private Person manager;
    
    public String getManagerName() {
        return manager.name; // Truy cập trực tiếp private field
    }
}

// ✅ Giảm sự thân mật
class Person {
    private String name;
    private Department department;
    
    public String getName() { return name; }
    public Department getDepartment() { return department; }
}

class Department {
    private String name;
    private Person manager;
    
    public String getManagerName() {
        return manager.getName(); // Sử dụng public method
    }
}
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Di chuyển phương thức (Move Method)
- 🔧 Di chuyển trường (Move Field)
- 🔧 Trích xuất lớp (Extract Class)
- 🔧 Thay thế tham chiếu hai chiều bằng một chiều

## **Kết Luận**
Quan hệ quá thân mật giữa các lớp làm giảm tính module hóa. Duy trì ranh giới rõ ràng giữa các lớp giúp hệ thống linh hoạt và dễ bảo trì hơn.

**Nguồn:** [refactoring.guru/smells/inappropriate-intimacy](https://refactoring.guru/smells/inappropriate-intimacy)
