# **Mùi Code: Ghen Tị Tính Năng (Feature Envy)**

## **Định Nghĩa**
Khi một phương thức sử dụng nhiều dữ liệu của lớp khác hơn là dữ liệu của chính lớp chứa nó.

## **Dấu Hiệu Nhận Biết**
- Phương thức gọi nhiều getter của đối tượng khác
- Phương thức có nhiều tham số từ cùng một đối tượng
- Code trong phương thức "trông có vẻ" thuộc về lớp khác

## **Vấn Đề**
- Vi phạm nguyên tắc đóng gói
- Tạo ra sự phụ thuộc không cần thiết giữa các lớp
- Khó bảo trì khi cấu trúc dữ liệu thay đổi

## **Giải Pháp**
**Di chuyển phương thức (Move Method)**
- Chuyển phương thức sang lớp mà nó thường xuyên truy cập
- Kết hợp với trích xuất phương thức nếu cần

## **Ví Dụ**
```java
// ❌ Phương thức ghen tị tính năng
class Customer {
    private String name;
    private Address address;
    
    public String getCustomerInfo() {
        // Phương thức này quan tâm quá nhiều đến Address
        return name + " lives at " + address.getStreet() + 
               ", " + address.getCity() + 
               ", " + address.getZipCode();
    }
}

// ✅ Di chuyển phương thức
class Address {
    public String getFullAddress() {
        return street + ", " + city + ", " + zipCode;
    }
}

class Customer {
    public String getCustomerInfo() {
        return name + " lives at " + address.getFullAddress();
    }
}
```

## **Khi Nào Thì Chấp Nhận?**
- Khi cần tránh phụ thuộc vòng
- Trong các phương thức static utility
- Khi thiết kế theo hướng procedural là hợp lý

## **Kết Luận**
"Feature Envy" cho thấy phương thức đang ở sai vị trí. Di chuyển nó đến lớp có dữ liệu mà nó cần sử dụng sẽ cải thiện đóng gói và giảm coupling.

**Nguồn:** [refactoring.guru/smells/feature-envy](https://refactoring.guru/smells/feature-envy)
