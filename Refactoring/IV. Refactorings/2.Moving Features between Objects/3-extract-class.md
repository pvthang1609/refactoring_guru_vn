# **Trích Xuất Lớp (Extract Class)**

## **Định Nghĩa**
Trích xuất lớp là kỹ thuật tái cấu trúc tách một nhóm các trường và phương thức liên quan từ một lớp hiện tại thành một lớp mới.

## **Khi Nào Sử Dụng**

### **Nên trích xuất khi:**
- Một lớp đang đảm nhận quá nhiều trách nhiệm
- Có thể nhóm một tập hợp các trường và phương thức lại với nhau
- Một phần của lớp có thể thay đổi độc lập với các phần khác
- Cần tái sử dụng một phần chức năng của lớp trong các ngữ cảnh khác

### **Dấu hiệu nhận biết:**
- Một lớp có quá nhiều trường và phương thức
- Có các nhóm trường và phương thức hoạt động độc lập với nhau
- Một số phương thức chỉ sử dụng một tập hợp con các trường của lớp

## **Các Bước Thực Hiện**

1. **Xác định trách nhiệm**: Phân tích và xác định nhóm trường và phương thức cần tách
2. **Tạo lớp mới**: Tạo lớp mới với tên phản ánh trách nhiệm của nó
3. **Di chuyển trường**: Di chuyển các trường liên quan sang lớp mới
4. **Di chuyển phương thức**: Di chuyển các phương thức liên quan sang lớp mới
5. **Cập nhật tham chiếu**: Thay thế các tham chiếu đến trường và phương thức cũ
6. **Tối ưu giao diện**: Điều chỉnh interface của lớp mới cho phù hợp

## **Ví Dụ Minh Họa**

### **❌ Trước khi trích xuất:**
```java
class Person {
    private String name;
    private String officeAreaCode;
    private String officeNumber;
    private String street;
    private String city;
    private String country;
    private String zipCode;
    
    public String getTelephoneNumber() {
        return "(" + officeAreaCode + ") " + officeNumber;
    }
    
    public String getFullAddress() {
        return street + ", " + city + ", " + country + " " + zipCode;
    }
    
    public String getName() {
        return name;
    }
    
    // Các phương thức khác...
}
```

### **✅ Sau khi trích xuất:**
```java
class Person {
    private String name;
    private TelephoneNumber telephoneNumber;
    private Address address;
    
    public String getTelephoneNumber() {
        return telephoneNumber.getTelephoneNumber();
    }
    
    public String getFullAddress() {
        return address.getFullAddress();
    }
    
    public String getName() {
        return name;
    }
}

class TelephoneNumber {
    private String areaCode;
    private String number;
    
    public String getTelephoneNumber() {
        return "(" + areaCode + ") " + number;
    }
    
    public String getAreaCode() {
        return areaCode;
    }
    
    public void setAreaCode(String areaCode) {
        this.areaCode = areaCode;
    }
    
    public String getNumber() {
        return number;
    }
    
    public void setNumber(String number) {
        this.number = number;
    }
}

class Address {
    private String street;
    private String city;
    private String country;
    private String zipCode;
    
    public String getFullAddress() {
        return street + ", " + city + ", " + country + " " + zipCode;
    }
    
    // Các getter và setter khác...
}
```

## **Lợi Ích**

- **✅ Tuân thủ SRP**: Mỗi lớp có một trách nhiệm duy nhất
- **✅ Giảm độ phức tạp**: Các lớp nhỏ hơn, dễ hiểu hơn
- **✅ Tăng tính tái sử dụng**: Các lớp mới có thể được sử dụng độc lập
- **✅ Dễ bảo trì**: Thay đổi ít ảnh hưởng đến toàn hệ thống
- **✅ Cải thiện khả năng testing**: Có thể test từng thành phần độc lập

## **Điểm Cần Lưu Ý**

- **Chọn tên phù hợp**: Tên lớp mới nên phản ánh rõ trách nhiệm của nó
- **Đảm bảo tính nhất quán**: Cập nhật tất cả các tham chiếu đến các thành phần đã di chuyển
- **Xem xét quan hệ giữa các lớp**: Quyết định quan hệ (composition, aggregation) phù hợp giữa lớp cũ và lớp mới
- **Không tách quá nhỏ**: Tránh tạo ra quá nhiều lớp nhỏ không cần thiết

## **Kỹ Thuật Liên Quan**

- **Trích xuất Lớp con (Extract Subclass)**: Khi cần tách theo quan hệ kế thừa
- **Trích xuất Interface**: Khi chỉ cần tách giao diện
- **[Hợp nhất Lớp (Inline Class)](./4-inline-class.md)**: Ngược lại với trích xuất lớp

## **Kết Luận**

Trích xuất lớp là kỹ thuật mạnh mẽ để giải quyết vấn đề lớp quá lớn và vi phạm nguyên tắc Single Responsibility. Bằng cách tách các trách nhiệm khác nhau vào các lớp chuyên biệt, chúng ta tạo ra hệ thống linh hoạt, dễ bảo trì và mở rộng hơn.