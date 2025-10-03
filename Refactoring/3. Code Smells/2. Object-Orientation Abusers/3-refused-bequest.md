# **Mùi Code: Từ Chối Kế Thừa (Refused Bequest)**

## **Dấu hiệu Nhận biết**
Một lớp con chỉ sử dụng một phần các phương thức và thuộc tính được kế thừa từ lớp cha. Các thành phần không được sử dụng bị "từ chối".

## **Nguyên nhân**
Khi thiết kế hệ thống phân cấp kế thừa, đôi khi lớp cha chứa nhiều hành vi và dữ liệu hơn mức cần thiết cho lớp con. Lớp con chỉ sử dụng một số ít các thành phần được kế thừa, bỏ qua phần còn lại.

## **Vấn đề**
- **Vi phạm nguyên lý Thay thế Liskov (LSP)**: Lớp con không thể thay thế hoàn toàn cho lớp cha
- **Quan hệ "is-a" không rõ ràng**: Nếu lớp con không sử dụng hết các thành phần của lớp cha, mối quan hệ kế thừa có thể không phù hợp
- **Khó bảo trì**: Các thành phần không sử dụng vẫn tồn tại trong lớp con, gây khó hiểu và tiềm ẩn lỗi

## **Giải pháp**

### **Tái cấu trúc Hệ thống Phân cấp**
- **Thay thế Kế thừa bằng Ủy quyền (Replace Inheritance with Delegation)**: 
  - Chuyển từ quan hệ kế thừa sang sử dụng composition
  - Lớp con sẽ chứa một tham chiếu đến lớp cha và chỉ ủy quyền cho các phương thức thực sự cần thiết

- **Trích xuất Siêu lớp (Extract Superclass)**:
  - Tạo một lớp cha mới chỉ chứa các thành phần thực sự được chia sẻ
  - Để lại lớp cha cũ cho các lớp con cần tất cả chức năng

- **Trích xuất Lớp con (Extract Subclass)**:
  - Tách các phần không liên quan trong lớp cha thành các lớp con riêng biệt

## **Kỹ thuật Tái cấu trúc**
- 🔧 **Thay thế Kế thừa bằng Ủy quyền** (Replace Inheritance with Delegation)
- 🔧 **Trích xuất Siêu lớp** (Extract Superclass)
- 🔧 **Trích xuất Lớp con** (Extract Subclass)
- 🔧 **Kéo lên Trường** (Pull Up Field)
- 🔧 **Kéo lên Phương thức** (Pull Up Method)

## **Lợi ích**
- ✅ **Tuân thủ nguyên lý LSP**: Lớp con có thể thay thế hoàn toàn cho lớp cha
- ✅ **Quan hệ "is-a" rõ ràng hơn**
- ✅ **Giảm sự phụ thuộc không cần thiết**
- ✅ **Code dễ hiểu và bảo trì hơn**

## **Khi nào Chấp nhận?**
Có thể chấp nhận "Refused Bequest" trong các trường hợp:
- 🟡 **Lớp cha không thể sửa đổi**: Khi bạn không có quyền truy cập vào mã nguồn lớp cha
- 🟡 **Lợi ích không đáng kể**: Khi chi phí tái cấu trúc vượt quá lợi ích
- 🟡 **Kế thừa có chủ đích**: Khi cố ý kế thừa chỉ để sử dụng một phần chức năng

## **Ví dụ Minh họa**

```java
// ❌ Vấn đề: Lớp con chỉ sử dụng một phần kế thừa
class Vehicle {
    void startEngine() { /* ... */ }
    void fly() { /* ... */ } // Car không cần phương thức này!
}

class Car extends Vehicle {
    // Car chỉ sử dụng startEngine(), không dùng fly()
}

// ✅ Giải pháp: Thay thế bằng ủy quyền
class Car {
    private Vehicle vehicle = new Vehicle();
    
    void startEngine() {
        vehicle.startEngine();
    }
    // Không có phương thức fly() - đã loại bỏ sự phụ thuộc không cần thiết
}
```

## **Kết luận**
"Refused Bequest" là dấu hiệu cho thấy **quan hệ kế thừa không phù hợp**. Khi lớp con từ chối sử dụng hầu hết các thành phần kế thừa, hãy xem xét:

- 🎯 **Chuyển sang sử dụng composition** thay vì inheritance
- 🎯 **Tái cấu trúc hệ thống phân cấp** để đảm bảo quan hệ "is-a" thực sự
- 🎯 **Đảm bảo lớp con tuân thủ nguyên lý thay thế Liskov**

Việc sửa chữa mùi code này giúp hệ thống linh hoạt hơn và dễ bảo trì về lâu dài.

