# **Mùi Code: Người Trung Gian (Middle Man)**

## **Định Nghĩa**
Khi một lớp thực hiện quá nhiều phương thức chỉ để ủy quyền (delegate) cho lớp khác.

## **Dấu Hiệu Nhận Biết**
- Lớp có nhiều phương thức chỉ gọi phương thức của lớp khác
- Lớp không có logic nghiệp vụ riêng
- Hơn một nửa phương thức trong lớp là ủy quyền

## **Vấn Đề**
- Tạo ra lớp trung gian không cần thiết
- Tăng độ phức tạp của hệ thống
- Giảm hiệu suất do nhiều lần gọi phương thức

## **Giải Pháp**
**Loại bỏ người trung gian (Remove Middle Man)**
- Client gọi trực tiếp lớp thực hiện
- Sử dụng ủy quyền chỉ khi thực sự cần thiết

## **Ví Dụ**
```java
// ❌ Quá nhiều người trung gian
class Department {
    private Manager manager = new Manager();
    
    public String getManagerName() { return manager.getName(); }
    public double getManagerSalary() { return manager.getSalary(); }
    public void setManagerSalary(double s) { manager.setSalary(s); }
}

// ✅ Gọi trực tiếp
class Department {
    private Manager manager = new Manager();
    
    public Manager getManager() { return manager; }
}

// Client code
department.getManager().getName();
```

## **Khi Nào Thì Chấp Nhận?**
- Khi cần abstract hoặc bảo vệ quyền truy cập
- Trong các mẫu thiết kế như Proxy, Decorator
- Khi cần thêm logic trước/sau khi ủy quyền

## **Kết Luận**
Người trung gian không phải lúc nào cũng xấu, nhưng khi một lớp chỉ toàn ủy quyền, hãy cho client gọi trực tiếp đến lớp thực hiện.
