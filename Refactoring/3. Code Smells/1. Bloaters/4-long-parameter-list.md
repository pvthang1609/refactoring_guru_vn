# **Mùi Code: Danh Sách Tham Số Dài (Long Parameter List)**

## **Định Nghĩa**
Một phương thức có quá nhiều tham số, làm giảm khả năng đọc hiểu và khó sử dụng.

## **Dấu Hiệu Nhận Biết**
- Phương thức có từ 4-5 tham số trở lên
- Khó nhớ thứ tự và mục đích của các tham số
- Thường xuyên phải tra cứu tài liệu khi sử dụng phương thức

## **Vấn Đề**
- Khó đọc và hiểu phương thức
- Dễ nhầm lẫn thứ tự tham số khi gọi
- Khó bảo trì và mở rộng

## **Giải Pháp**
**Giới thiệu Đối tượng Tham số (Introduce Parameter Object)**
- Nhóm các tham số liên quan vào một đối tượng

**Giữ toàn bộ Đối tượng (Preserve Whole Object)**
- Truyền đối tượng thay vì các thuộc tính riêng lẻ

**Thay thế Tham số bằng Phương thức (Replace Parameter with Method Call)**

## **Ví Dụ**
```java
// ❌ Danh sách tham số dài
public void createUser(String firstName, String lastName, 
                      String email, String phone, 
                      String address, String city, 
                      String country, String zipCode) {
    // ...
}

// ✅ Sử dụng đối tượng tham số
public void createUser(UserData userData) {
    // ...
}

class UserData {
    private String firstName;
    private String lastName;
    private String email;
    private String phone;
    private Address address;
}
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Giới thiệu Đối tượng Tham số
- 🔧 Giữ toàn bộ Đối tượng
- 🔧 Thay thế Tham số bằng Phương thức

## **Kết Luận**
Danh sách tham số dài làm giảm khả năng đọc hiểu code. Nhóm các tham số liên quan vào đối tượng giúp code sạch sẽ và dễ bảo trì hơn.
