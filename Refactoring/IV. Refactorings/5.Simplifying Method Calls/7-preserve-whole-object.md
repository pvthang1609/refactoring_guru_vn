# **Bảo Toàn Toàn Bộ Đối Tượng (Preserve Whole Object)**

## **Định Nghĩa**
Bảo toàn toàn bộ đối tượng là kỹ thuật tái cấu trúc thay thế việc truyền nhiều giá trị riêng lẻ từ một đối tượng bằng cách truyền toàn bộ đối tượng đó, giúp code gọn gàng và giảm kết nối chặt chẽ.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một phương thức cần nhiều giá trị từ cùng một đối tượng
- Có sự phụ thuộc chặt chẽ giữa phương thức được gọi và đối tượng tham số
- Muốn giảm số lượng tham số trong phương thức
- Cần tránh vi phạm nguyên tắc "Law of Demeter"

### **Không nên sử dụng khi:**
- Phương thức chỉ cần một vài giá trị từ đối tượng
- Truyền toàn bộ đối tượng tạo ra sự phụ thuộc không cần thiết
- Đối tượng quá lớn và phương thức chỉ cần một phần nhỏ

## **Các Bước Thực Hiện**

1. **Xác định phương thức**: Tìm phương thức đang nhận nhiều tham số từ cùng một đối tượng
2. **Thay thế tham số**: Thay thế các tham số riêng lẻ bằng đối tượng gốc
3. **Cập nhật lời gọi**: Sửa tất cả lời gọi phương thức để truyền đối tượng thay vì các giá trị riêng lẻ
4. **Xóa tham số cũ**: Xóa các tham số không còn cần thiết
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi bảo toàn đối tượng:**
```java
class TemperatureRange {
    private int low;
    private int high;
    
    public int getLow() { return low; }
    public int getHigh() { return high; }
    public boolean includes(int temperature) {
        return temperature >= low && temperature <= high;
    }
}

class Room {
    // Phương thức nhận nhiều giá trị riêng lẻ từ cùng một đối tượng
    public boolean withinPlan(HeatingPlan plan) {
        int low = daysTempRange().getLow();
        int high = daysTempRange().getHigh();
        return plan.withinRange(low, high);
    }
    
    private TemperatureRange daysTempRange() {
        return new TemperatureRange(18, 26);
    }
}

class HeatingPlan {
    private TemperatureRange range;
    
    // Phương thức nhận các giá trị riêng lẻ
    public boolean withinRange(int low, int high) {
        return (low >= range.getLow() && high <= range.getHigh());
    }
}

// Client code phải trích xuất nhiều giá trị
Room room = new Room();
HeatingPlan plan = new HeatingPlan();
TemperatureRange range = room.daysTempRange();
boolean withinPlan = plan.withinRange(range.getLow(), range.getHigh());
```

### **✅ Sau khi bảo toàn đối tượng:**
```java
class TemperatureRange {
    private int low;
    private int high;
    
    public int getLow() { return low; }
    public int getHigh() { return high; }
    public boolean includes(int temperature) {
        return temperature >= low && temperature <= high;
    }
    
    // Thêm phương thức so sánh với range khác
    public boolean includesRange(TemperatureRange other) {
        return other.getLow() >= low && other.getHigh() <= high;
    }
}

class Room {
    // Phương thức truyền toàn bộ đối tượng
    public boolean withinPlan(HeatingPlan plan) {
        return plan.withinRange(daysTempRange());
    }
    
    private TemperatureRange daysTempRange() {
        return new TemperatureRange(18, 26);
    }
}

class HeatingPlan {
    private TemperatureRange range;
    
    // Phương thức nhận toàn bộ đối tượng
    public boolean withinRange(TemperatureRange otherRange) {
        return range.includesRange(otherRange);
    }
    
    // Hoặc cách khác: ủy quyền cho đối tượng TemperatureRange
    public boolean withinRangeAlternative(TemperatureRange otherRange) {
        return otherRange.getLow() >= range.getLow() 
            && otherRange.getHigh() <= range.getHigh();
    }
}

// Client code trở nên đơn giản
Room room = new Room();
HeatingPlan plan = new HeatingPlan();
boolean withinPlan = plan.withinRange(room.daysTempRange());
```

## **Lợi Ích**

- **✅ Giảm số lượng tham số**: Phương thức có ít tham số hơn
- **✅ Code gọn gàng hơn**: Loại bỏ việc trích xuất giá trị trung gian
- **✅ Giảm kết nối chặt**: Phương thức ít phụ thuộc vào cấu trúc của đối tượng tham số
- **✅ Dễ mở rộng**: Khi đối tượng thêm thuộc tính mới, không cần thay đổi signature
- **✅ Tăng tính đóng gói**: Giữ cho dữ liệu và hành vi cùng nhau

## **Điểm Cần Lưu ý**

- **Không lạm dụng**: Chỉ áp dụng khi phương thức thực sự cần nhiều thuộc tính từ đối tượng
- **Cân nhắc phụ thuộc**: Truyền toàn bộ đối tượng có thể tạo phụ thuộc không mong muốn
- **Xem xét nguyên tắc Demeter**: Đôi khi tốt hơn nên ủy quyền cho đối tượng
- **Hiệu suất**: Với đối tượng lớn, chỉ truyền khi cần thiết

## **Khi Nào Không Nên Sử Dụng**

- Khi phương thức chỉ cần một hoặc hai thuộc tính từ đối tượng
- Khi đối tượng quá lớn và phương thức chỉ cần một phần nhỏ
- Khi việc truyền toàn bộ đối tượng tạo ra vòng phụ thuộc
- Khi phương thức thuộc về một lớp khác và không nên biết đến đối tượng

## **Kỹ Thuật Liên Quan**

- **Introduce Parameter Object**: Nhóm các tham số không liên quan thành object
- **Replace Parameter with Method Call**: Thay thế tham số bằng lời gọi phương thức
- **Extract Method**: Tách phương thức để giảm độ phức tạp

## **Kết Luận**

Bảo toàn toàn bộ đối tượng là kỹ thuật hiệu quả khi một phương thức cần nhiều giá trị từ cùng một đối tượng. Bằng cách truyền toàn bộ đối tượng thay vì các giá trị riêng lẻ, code trở nên gọn gàng, dễ bảo trì và ít phụ thuộc hơn. Tuy nhiên, cần cân nhắc kỹ để tránh tạo ra các phụ thuộc không cần thiết hoặc vi phạm nguyên tắc thiết kế hướng đối tượng.

**Nguồn:** [refactoring.guru/preserve-whole-object](https://refactoring.guru/preserve-whole-object)