# **Đơn Giản Hóa Lời Gọi Phương Thức (Simplifying Method Calls)**

## **Định Nghĩa**
Đơn giản hóa lời gọi phương thức là nhóm kỹ thuật tái cấu trúc nhằm làm cho giao diện phương thức trở nên đơn giản và dễ sử dụng hơn.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Giao diện phương thức quá phức tạp, khó hiểu
- Có quá nhiều tham số trong phương thức
- Phương thức có quá nhiều trách nhiệm
- Quan hệ phụ thuộc giữa các đối tượng quá phức tạp

## **Các Kỹ Thuật Chính**

### **1. Đổi Tên Phương Thức (Rename Method)**
```java
// ❌ Trước
class Customer {
    public double getInv() { ... }
}

// ✅ Sau
class Customer {
    public double getInvoiceAmount() { ... }
}
```

### **2. Thêm/ Xóa Tham Số (Add/Remove Parameter)**
```java
// ❌ Trước - quá nhiều tham số
public void createUser(String name, String email, String phone, 
                      String address, Date birthday, int type) {}

// ✅ Sau - nhóm thành object
public void createUser(UserInfo userInfo) {}
```

### **3. Tách Truy Vấn Khỏi Bộ Điều Chỉnh (Separate Query from Modifier)**
```java
// ❌ Trước - vừa truy vấn vừa thay đổi trạng thái
public boolean sendInvoiceIfNeeded() {
    if (shouldSendInvoice) {
        sendInvoice();
        return true;
    }
    return false;
}

// ✅ Sau - tách thành 2 phương thức
public boolean shouldSendInvoice() {
    return shouldSendInvoice;
}

public void sendInvoice() {
    // gửi invoice
}
```

### **4. Tham Số Hóa Phương Thức (Parameterize Method)**
```java
// ❌ Trước - phương thức trùng lặp
public void raiseSalary5Percent() { ... }
public void raiseSalary10Percent() { ... }

// ✅ Sau - tham số hóa
public void raiseSalary(double percentage) { ... }
```

## **Lợi Ích**

- **✅ Code dễ đọc hơn**: Tên phương thức rõ ràng, dễ hiểu
- **✅ Giảm phụ thuộc**: Giảm kết hợp chặt chẽ giữa các đối tượng
- **✅ Dễ bảo trì**: Thay đổi ít ảnh hưởng đến code khác
- **✅ Tái sử dụng tốt hơn**: Phương thức linh hoạt hơn

## **Điểm Cần Lưu Ý**

- **Giữ nguyên hành vi**: Đảm bảo không thay đổi chức năng khi tái cấu trúc
- **Cập nhật tất cả lời gọi**: Sửa tất cả nơi gọi phương thức đã thay đổi
- **Kiểm thử kỹ lưỡng**: Test kỹ sau mỗi thay đổi

## **Khi Nào Không Nên Sử Dụng**

- Khi giao diện đã đủ đơn giản và rõ ràng
- Khi thay đổi sẽ phá vỡ nhiều code hiện có
- Khi không có đủ test coverage để đảm bảo an toàn

## **Kết Luận**

Đơn giản hóa lời gọi phương thức giúp code trở nên dễ đọc, dễ bảo trì và linh hoạt hơn. Bằng cách cải thiện giao diện phương thức, chúng ta làm cho hệ thống dễ hiểu và ít lỗi hơn.

**Nguồn:** [refactoring.guru/refactoring/techniques/simplifying-method-calls](https://refactoring.guru/refactoring/techniques/simplifying-method-calls)