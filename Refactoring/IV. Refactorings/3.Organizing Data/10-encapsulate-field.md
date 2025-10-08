# **Đóng Gói Trường (Encapsulate Field)**

## **Định Nghĩa**
Đóng gói trường là kỹ thuật tái cấu trúc chuyển đổi một trường public thành private và cung cấp các phương thức truy cập (getter/setter) để kiểm soát việc đọc/ghi trường đó.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có trường public mà bất kỳ lớp nào cũng có thể truy cập và sửa đổi
- Muốn kiểm soát cách thức truy cập và thay đổi dữ liệu
- Cần thêm validation, logging hoặc logic khác khi đọc/ghi trường

### **Không nên sử dụng khi:**
- Trường đã là private và chỉ được truy cập qua methods
- Trong các lớp đơn giản chỉ chứa dữ liệu (data classes)

## **Các Bước Thực Hiện**

1. **Tạo getter và setter**: Tạo phương thức getter và setter cho trường
2. **Tìm tất cả tham chiếu**: Tìm tất cả các vị trí truy cập trực tiếp đến trường từ bên ngoài lớp
3. **Thay thế bằng getter/setter**: Thay thế các tham chiếu trực tiếp bằng lời gọi getter/setter
4. **Đánh dấu trường là private**: Đảm bảo trường chỉ có thể truy cập qua getter/setter

## **Ví Dụ Minh Họa**

### **❌ Trước khi đóng gói:**
```java
class Person {
    public String name;        // Trường public - bất kỳ đâu cũng truy cập được
    public int age;           // Trường public - nguy hiểm!
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

// Client code có thể truy cập và sửa đổi trực tiếp
Person person = new Person("John", 25);
person.name = "Alice";        // Cho phép sửa đổi trực tiếp
person.age = -5;              // Tuổi âm - không hợp lệ nhưng không thể ngăn chặn
```

### **✅ Sau khi đóng gói:**
```java
class Person {
    private String name;      // Trường private - an toàn
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        setAge(age);         // Sử dụng setter để validation
    }
    
    // Getter cho name
    public String getName() {
        return name;
    }
    
    // Setter cho name với validation
    public void setName(String name) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Tên không được để trống");
        }
        this.name = name;
    }
    
    // Getter cho age
    public int getAge() {
        return age;
    }
    
    // Setter cho age với validation
    public void setAge(int age) {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("Tuổi phải từ 0 đến 150");
        }
        this.age = age;
    }
}

// Client code phải sử dụng methods
Person person = new Person("John", 25);
person.setName("Alice");     // Phải sử dụng setter
person.setAge(30);           // Validation sẽ chặn giá trị không hợp lệ

// person.setAge(-5);        // Sẽ ném exception - không cho phép tuổi âm
```

## **Lợi Ích**

- **✅ Kiểm soát truy cập**: Có thể thêm validation, logging, hoặc logic khác
- **✅ Tính đóng gói**: Ẩn đi implementation chi tiết của lớp
- **✅ Linh hoạt**: Dễ dàng thay đổi cách lưu trữ dữ liệu mà không ảnh hưởng client
- **✅ Dễ bảo trì**: Tập trung logic kiểm tra ở một nơi

## **Điểm Cần Lưu Ý**

- **Không lạm dụng**: Với các trường đơn giản, đôi khi getter/setter cơ bản là đủ
- **Cân nhắc tính bất biến**: Với các đối tượng immutable, có thể chỉ cần getter
- **Đặt tên phù hợp**: Tuân thủ quy ước đặt tên getter/setter (getXxx, setXxx)

## **Khi Nào Không Nên Sử Dụng**

- Trong các lớp data transfer object (DTO) đơn thuần
- Khi performance là yếu tố quan trọng và cần truy cập trực tiếp
- Trong các inner class hoặc private nested class nhỏ

## **Kỹ Thuật Liên Quan**

- **Self Encapsulate Field**: Tự đóng gói trường ngay trong cùng lớp
- **Replace Data Value with Object**: Khi cần nâng cấp kiểu dữ liệu
- **Extract Class**: Khi một lớp có quá nhiều trách nhiệm

## **Kết Luận**

Đóng gói trường là kỹ thuật cơ bản nhưng quan trọng trong lập trình hướng đối tượng. Bằng cách chuyển các trường public thành private và cung cấp methods truy cập, bạn bảo vệ được tính toàn vẹn dữ liệu và tạo ra code linh hoạt, dễ bảo trì hơn.

**Nguồn:** [refactoring.guru/encapsulate-field](https://refactoring.guru/encapsulate-field)