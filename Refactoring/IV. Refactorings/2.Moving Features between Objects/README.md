# **Di Chuyển Tính Năng Giữa Các Đối Tượng**

## **Giới Thiệu**
Đây là nhóm kỹ thuật tái cấu trúc giúp di chuyển các tính năng (phương thức, trường) giữa các đối tượng để cải thiện sự phân bố trách nhiệm và tăng tính gắn kết.

## **Các Kỹ Thuật Chính**

### **1. Di Chuyển Phương Thức (Move Method)**
**Mục đích:** Di chuyển một phương thức từ lớp hiện tại sang lớp khác phù hợp hơn

**Khi sử dụng:**
- Phương thức được sử dụng nhiều hơn bởi một lớp khác
- Phương thức không phù hợp với trách nhiệm của lớp hiện tại

**Ví dụ:**
```java
// ❌ Trước khi di chuyển
class Customer {
    private String name;
    
    public double getDiscountRate() {
        // Logic tính tỷ lệ giảm giá
        return isVIP ? 0.1 : 0.02;
    }
}

// ✅ Sau khi di chuyển
class Customer {
    private String name;
    private CustomerType type;
    
    public double getDiscountRate() {
        return type.getDiscountRate();
    }
}

class CustomerType {
    public double getDiscountRate() {
        return isVIP ? 0.1 : 0.02;
    }
}
```

### **2. Di Chuyển Trường (Move Field)**
**Mục đích:** Di chuyển một trường từ lớp hiện tại sang lớp khác

**Khi sử dụng:**
- Một lớp khác sử dụng trường này nhiều hơn
- Trường không thuộc về trách nhiệm của lớp hiện tại

### **3. Trích Xuất Lớp (Extract Class)**
**Mục đích:** Tách một nhóm các trường và phương thức liên quan thành lớp mới

**Khi sử dụng:**
- Một lớp có quá nhiều trách nhiệm
- Có thể nhóm một số trường và phương thức lại với nhau

### **4. Hợp Nhất Lớp (Inline Class)**
**Mục đích:** Di chuyển tất cả các tính năng của một lớp vào một lớp khác và xóa lớp cũ

**Khi sử dụng:**
- Lớp không còn đủ trách nhiệm để tồn tại độc lập
- Cần đơn giản hóa kiến trúc hệ thống

### **5. Ẩn Đối Tượng Ủy Quyền (Hide Delegate)**
**Mục đích:** Ẩn chi tiết về mối quan hệ ủy quyền giữa các đối tượng

### **6. Loại Bỏ Người Trung Gian (Remove Middle Man)**
**Mục đích:** Cho phép client gọi trực tiếp đến đối tượng ủy quyền

## **Lợi Ích**

✅ **Phân bố trách nhiệm rõ ràng** - Mỗi lớp có một nhiệm vụ cụ thể  
✅ **Tăng tính gắn kết** - Các thành phần liên quan được nhóm lại với nhau  
✅ **Giảm sự phụ thuộc** - Giảm coupling giữa các lớp  
✅ **Dễ bảo trì** - Thay đổi ít ảnh hưởng đến toàn hệ thống  

## **Các Bước Thực Hiện**

1. **Phân tích phụ thuộc** - Xác định các lớp và phương thức liên quan
2. **Tạo lớp mới** (nếu cần) - Cho các tính năng được trích xuất
3. **Di chuyển từng bước** - Di chuyển trường và phương thức một cách tuần tự
4. **Cập nhật tham chiếu** - Sửa tất cả các vị trí sử dụng tính năng đã di chuyển
5. **Kiểm thử** - Đảm bảo không có chức năng nào bị ảnh hưởng

## **Kỹ Thuật Liên Quan**

- **Trích xuất Phương thức (Extract Method)**
- **Trích xuất Lớp con (Extract Subclass)**
- **Thay thế Ủy quyền bằng Kế thừa (Replace Delegation with Inheritance)**

## **Kết Luận**

Việc di chuyển tính năng giữa các đối tượng giúp duy trì nguyên tắc Single Responsibility và tạo ra kiến trúc phần mềm linh hoạt, dễ bảo trì. Các kỹ thuật này đặc biệt hữu ích khi hệ thống phát triển và yêu cầu thay đổi theo thời gian.


**Nguồn:** [refactoring.guru/refactoring/techniques/moving-features-between-objects](https://refactoring.guru/refactoring/techniques/moving-features-between-objects)