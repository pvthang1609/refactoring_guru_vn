Chào bạn,
# **Mùi Code: Lớp Dữ Liệu (Data Class)**

## **Định Nghĩa**
Một lớp chỉ chứa các trường dữ liệu và getter/setter, không có hành vi nào khác.

## **Dấu Hiệu Nhận Biết**
- Chỉ có các trường private với public getter/setter
- Không có phương thức nghiệp vụ
- Hành vi xử lý dữ liệu nằm ở lớp khác

## **Vấn Đề**
- Vi phạm nguyên tắc đóng gói (encapsulation)
- Logic nghiệp vụ bị phân tán khắp nơi
- Khó kiểm soát tính toàn vẹn dữ liệu

## **Giải Pháp**
**Di chuyển phương thức (Move Method)**
- Chuyển các phương thức xử lý dữ liệu vào trong lớp
- Đóng gói hành vi cùng với dữ liệu

## **Ví Dụ**
```java
// ❌ Lớp dữ liệu thuần túy
class Customer {
    private String name;
    private List<Order> orders;
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public List<Order> getOrders() { return orders; }
    public void setOrders(List<Order> orders) { this.orders = orders; }
}

// ✅ Thêm hành vi vào lớp
class Customer {
    private String name;
    private List<Order> orders;
    
    public String getName() { return name; }
    public List<Order> getOrders() { return orders; }
    
    public double getTotalSpent() {
        return orders.stream()
            .mapToDouble(Order::getAmount)
            .sum();
    }
    
    public boolean isVIP() {
        return getTotalSpent() > 1000;
    }
}
```

## **Kỹ Thuật Tái Cấu Trùng**
- 🔧 Di chuyển phương thức (Move Method)
- 🔧 Đóng gói trường (Encapsulate Field)
- 🔧 Ẩn trường delegate (Hide Delegate)

## **Kết Luận**
Lớp dữ liệu không phải là xấu, nhưng cần được bổ sung hành vi phù hợp. Đóng gói dữ liệu cùng với hành vi giúp code dễ bảo trì và tuân thủ OOP.
