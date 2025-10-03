# **Mùi Code: Câu lệnh Switch (Switch Statements)**

## **Dấu hiệu Nhận biết**
Có các câu lệnh `switch` hoặc `if-else` dài, phức tạp xuất hiện lặp lại trong codebase, đặc biệt khi chúng xử lý cùng một tập hợp các điều kiện.

## **Nguyên nhân**
Lập trình viên thêm các trường hợp mới vào câu lệnh `switch` hiện có thay vì thiết kế các cấu trúc linh hoạt hơn, dẫn đến các khối code lớn, khó bảo trì.

## **Vấn đề**
- **Vi phạm Nguyên tắc Mở/Đóng (Open/Closed Principle)**: Mỗi khi thêm trường hợp mới, phải sửa đổi code hiện có
- **Trùng lặp code**: Cùng một cấu trúc `switch` có thể xuất hiện ở nhiều nơi
- **Khó mở rộng**: Thêm logic mới đòi hỏi phải tìm và sửa tất cả các câu lệnh `switch` liên quan
- **Độ phức tạp cao**: Các khối `switch` lớn khó đọc, khó hiểu và dễ gây lỗi

## **Giải pháp**

### **Thay thế bằng Đa hình (Replace with Polymorphism)**
- Tạo hệ thống phân cấp các lớp và di chuyển hành vi vào các lớp con
- Sử dụng phương thức ghi đè (override) thay vì kiểm tra điều kiện

### **Áp dụng Mẫu Thiết kế State/Strategy**
- **State Pattern**: Khi behavior thay đổi dựa trên trạng thái
- **Strategy Pattern**: Khi có nhiều thuật toán hoặc chiến lược khác nhau

### **Sử dụng Factory Method**
- Đóng gói việc tạo đối tượng và để đối tượng xử lý behavior cụ thể

## **Kỹ thuật Tái cấu trúc**
- 🔧 **Thay thế Kiểu mã bằng Lớp con** (Replace Type Code with Subclasses)
- 🔧 **Thay thế Kiểu mã bằng State/Strategy** (Replace Type Code with State/Strategy)
- 🔧 **Thay thế Điều kiện bằng Đa hình** (Replace Conditional with Polymorphism)
- 🔧 **Trích xuất Phương thức** (Extract Method)
- 🔧 **Di chuyển Phương thức** (Move Method)

## **Lợi ích**
- ✅ **Tuân thủ OCP**: Có thể thêm behavior mới mà không sửa đổi code hiện có
- ✅ **Giảm trùng lặp**: Loại bỏ các cấu trúc `switch` lặp lại
- ✅ **Code rõ ràng hơn**: Mỗi lớp có trách nhiệm cụ thể
- ✅ **Dễ mở rộng**: Thêm lớp mới dễ dàng hơn thêm case mới

## **Khi nào Chấp nhận?**
Có thể chấp nhận "Switch Statements" trong các trường hợp:
- 🟡 **Simple dispatchers**: Khi `switch` chỉ đơn giản định tuyến đến các phương thức khác
- 🟡 **Performance-critical code**: Trong một số trường hợp tối ưu hiệu năng
- 🟡 **Framework/API boundaries**: Khi tích hợp với code bên ngoài

## **Ví dụ Minh họa**

```java
// ❌ Vấn đề: Switch statement phức tạp
class Bird {
    private String type;
    
    public double getSpeed() {
        switch (type) {
            case "EUROPEAN":
                return getBaseSpeed();
            case "AFRICAN":
                return getBaseSpeed() - getLoadFactor();
            case "NORWEGIAN_BLUE":
                return (isNailed) ? 0 : getBaseSpeed();
            default:
                return 0;
        }
    }
}

// ✅ Giải pháp: Thay thế bằng đa hình
abstract class Bird {
    public abstract double getSpeed();
}

class EuropeanBird extends Bird {
    public double getSpeed() {
        return getBaseSpeed();
    }
}

class AfricanBird extends Bird {
    public double getSpeed() {
        return getBaseSpeed() - getLoadFactor();
    }
}

class NorwegianBlueBird extends Bird {
    public double getSpeed() {
        return (isNailed) ? 0 : getBaseSpeed();
    }
}
```

## **Các bước Thực hiện**
1. **Xác định** câu lệnh `switch` và các biến điều kiện
2. **Tạo lớp cơ sở** hoặc interface định nghĩa phương thức chung
3. **Tạo lớp con** cho mỗi trường hợp trong `switch`
4. **Di chuyển logic** từ mỗi case vào phương thức tương ứng trong lớp con
5. **Thay thế** câu lệnh `switch` bằng việc gọi phương thức đa hình

## **Mẫu thiết kế áp dụng**
### **State Pattern**
```java
// Sử dụng khi behavior phụ thuộc vào trạng thái
interface State {
    void handle(Context context);
}

class ConcreteStateA implements State {
    public void handle(Context context) {
        // Behavior cho state A
        context.setState(new ConcreteStateB());
    }
}
```

### **Strategy Pattern**
```java
// Sử dụng khi có nhiều thuật toán/thuộc tính khác nhau
interface SortingStrategy {
    void sort(int[] data);
}

class QuickSort implements SortingStrategy {
    public void sort(int[] data) {
        // Implement quick sort
    }
}
```

## **Kết luận**
"Switch Statements" là dấu hiệu cho thấy **code đang vi phạm nguyên tắc thiết kế hướng đối tượng**. Bằng cách:

- 🎯 **Ưu tiên composition over inheritance**
- 🎯 **Áp dụng đa hình thay vì kiểm tra điều kiện**
- 🎯 **Sử dụng các mẫu thiết kế phù hợp như State, Strategy**

Việc loại bỏ mùi code này giúp hệ thống **linh hoạt hơn, dễ bảo trì và mở rộng** trong dài hạn.
