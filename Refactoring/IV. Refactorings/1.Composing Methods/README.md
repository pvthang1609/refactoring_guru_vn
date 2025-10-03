# **Kỹ Thuật Tái Cấu Trúc: Tổ Chức Phương Thức**

## **Định Nghĩa**
Nhóm kỹ thuật giúp đơn giản hóa và tối ưu hóa các phương thức trong code.

## **Các Kỹ Thuật Chính**

### **1. Trích xuất Phương thức (Extract Method)**
**Mục đích:** Tách đoạn code thành phương thức mới
```java
// ❌ Trước
void printOwing() {
    printBanner();
    // Print details
    System.out.println("name: " + name);
    System.out.println("amount: " + getOutstanding());
}

// ✅ Sau
void printOwing() {
    printBanner();
    printDetails(getOutstanding());
}

void printDetails(double outstanding) {
    System.out.println("name: " + name);
    System.out.println("amount: " + outstanding);
}
```

### **2. Hợp nhất Phương thức (Inline Method)**
**Mục đích:** Thay thế lời gọi phương thức bằng nội dung của nó

### **3. Trích xuất Biến (Extract Variable)**
**Mục đích:** Tách biểu thức phức tạp thành biến có tên rõ ràng

### **4. Hợp nhất Biến (Inline Temp)**
**Mục đích:** Thay thế biến tạm bằng biểu thức gốc

### **5. Thay thế Biến tạm bằng Truy vấn (Replace Temp with Query)**
**Mục đích:** Loại bỏ biến tạm bằng phương thức

### **6. Tách Biến (Split Temporary Variable)**
**Mục đích:** Tách biến tạm được gán nhiều lần thành nhiều biến

### **7. Loại bỏ Biến tạm (Remove Assignments to Parameters)**
**Mục đích:** Ngăn gán giá trị mới cho tham số

### **8. Thay thế Phương thức bằng Đối tượng Phương thức (Replace Method with Method Object)**
**Mục đích:** Biến phương thức phức tạp thành đối tượng riêng

### **9. Thuật toán Thay thế (Substitute Algorithm)**
**Mục đích:** Thay thế thuật toán bằng phiên bản rõ ràng hơn

## **Lợi Ích**
- ✅ Code rõ ràng, dễ đọc hơn
- ✅ Tái sử dụng tốt hơn
- ✅ Dễ bảo trì và mở rộng
- ✅ Giảm độ phức tạp

## **Kết Luận**
Các kỹ thuật tổ chức phương thức giúp code trở nên rõ ràng, dễ bảo trì và linh hoạt hơn thông qua việc phân tách và tối ưu hóa các phương thức.

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
