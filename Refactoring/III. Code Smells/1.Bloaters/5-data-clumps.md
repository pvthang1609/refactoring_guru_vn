# **Mùi Code: Cụm Dữ Liệu (Data Clumps)**

## **Định Nghĩa**
Khi một nhóm các biến (trường, tham số) luôn xuất hiện cùng nhau trong nhiều ngữ cảnh.

## **Dấu Hiệu Nhận Biết**
- Các trường dữ liệu luôn xuất hiện cùng nhau trong nhiều lớp
- Các tham số luôn được truyền cùng nhau trong nhiều phương thức
- Thiếu một khái niệm chung để nhóm các dữ liệu này

## **Vấn Đề**
- Code trùng lặp khi xử lý cùng một nhóm dữ liệu
- Khó bảo trì khi cần thay đổi cấu trúc dữ liệu
- Khó biểu đạt đúng ý nghĩa của dữ liệu

## **Giải Pháp**
**Trích xuất lớp (Extract Class)**
- Nhóm các trường liên quan thành một lớp mới

**Giới thiệu đối tượng tham số (Introduce Parameter Object)**
- Thay thế danh sách tham số bằng một đối tượng

## **Ví Dụ**
```java
// ❌ Cụm dữ liệu bị phân tán
class Customer {
    private String street;
    private String city;
    private String country;
    private String zipCode;
}

class Order {
    public void shipOrder(String street, String city, 
                         String country, String zipCode) {
        // ...
    }
}

// ✅ Nhóm thành lớp chuyên biệt
class Address {
    private String street;
    private String city;
    private String country;
    private String zipCode;
}

class Customer {
    private Address address;
}

class Order {
    public void shipOrder(Address address) {
        // ...
    }
}
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Trích xuất lớp (Extract Class)
- 🔧 Giới thiệu đối tượng tham số (Introduce Parameter Object)

## **Kết Luận**
Cụm dữ liệu cho thấy cần một khái niệm chung để nhóm các dữ liệu liên quan. Tạo lớp chuyên biệt giúp code rõ ràng và dễ bảo trì hơn.

**Nguồn:** [refactoring.guru/smells/data-clumps](https://refactoring.guru/smells/data-clumps)
