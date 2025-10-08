# **Giới Thiệu Khẳng Định (Introduce Assertion)**

## **Định Nghĩa**
Giới thiệu khẳng định là kỹ thuật chèn các câu lệassertion vào code để kiểm tra các điều kiện luôn phải đúng tại một thời điểm cụ thể.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có các giả định (assumptions) trong code cần được kiểm tra rõ ràng
- Cần xác nhận điều kiện tiên quyết (preconditions) hoặc điều kiện hậu quyết (postconditions)
- Muốn ghi lại và kiểm tra các bất biến (invariants) trong code

### **Không nên sử dụng khi:**
- Kiểm tra điều kiện lỗi có thể xảy ra trong quá trình chạy thông thường
- Xử lý các tình huống ngoại lệ cần xử lý error handling
- Kiểm tra đầu vào từ người dùng

## **Các Bước Thực Hiện**

1. **Xác định điều kiện**: Tìm các giả định trong code cần được kiểm tra
2. **Thêm assertion**: Chèn câu lệnh assertion để kiểm tra điều kiện
3. **Xác định rõ ý nghĩa**: Đảm bảo assertion truyền đạt rõ mục đích
4. **Không thay thế xử lý lỗi**: Giữ assertion tách biệt với error handling

## **Ví Dụ Minh Họa**

### **❌ Trước khi giới thiệu assertion:**
```java
class Employee {
    private double expenseLimit;
    private Project primaryProject;
    
    public double getExpenseLimit() {
        // Giả định: employee phải có expense limit HOẶC primary project
        if (expenseLimit != 0) {
            return expenseLimit;
        } else {
            return primaryProject.getMemberExpenseLimit();
        }
    }
}
```

### **✅ Sau khi giới thiệu assertion:**
```java
class Employee {
    private double expenseLimit;
    private Project primaryProject;
    
    public double getExpenseLimit() {
        // Khẳng định điều kiện tiên quyết
        Assert.isTrue(expenseLimit != 0 || primaryProject != null, 
                     "Employee must have either expense limit or primary project");
        
        if (expenseLimit != 0) {
            return expenseLimit;
        } else {
            return primaryProject.getMemberExpenseLimit();
        }
    }
}
```

## **Lợi Ích**

- **✅ Ghi lại giả định**: Làm rõ các giả định trong code
- **✅ Phát hiện lỗi sớm**: Tìm thấy bug sớm trong quá trình phát triển
- **✅ Tự kiểm chứng**: Code trở nên tự kiểm chứng các điều kiện
- **✅ Cải thiện khả năng bảo trì**: Dễ hiểu hơn về các điều kiện cần thiết

## **Điểm Cần Lưu Ý**

- **Không thay thế validation**: Assertion không thay thế kiểm tra đầu vào
- **Có thể tắt**: Assertion có thể bị vô hiệu hóa trong production
- **Không có hiệu ứng phụ**: Assertion không được thay đổi trạng thái chương trình

## **Khi Nào Không Nên Sử Dụng**

- Kiểm tra đầu vào từ người dùng
- Xử lý các tình huống lỗi có thể phục hồi
- Thay thế cho exception handling
- Khi assertion có hiệu ứng phụ

## **Kết Luận**

Giới thiệu khẳng định là kỹ thuật hiệu quả để làm rõ và kiểm tra các giả định trong code. Nó giúp code trở nên rõ ràng hơn, phát hiện lỗi sớm và dễ bảo trì. Tuy nhiên, cần phân biệt rõ giữa assertion (cho điều kiện không bao giờ được vi phạm) và error handling (cho lỗi có thể xảy ra).

**Nguồn:** [refactoring.guru/introduce-assertion](https://refactoring.guru/introduce-assertion)