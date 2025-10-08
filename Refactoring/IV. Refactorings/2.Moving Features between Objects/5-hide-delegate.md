# **Ẩn Đối Tượng Ủy Quyền (Hide Delegate)**

## **Định Nghĩa**
Ẩn đối tượng ủy quyền là kỹ thuật tái cấu trúc tạo các phương thức ủy quyền trong lớp trung gian để che giấu sự phụ thuộc vào cấu trúc của các đối tượng được ủy quyền.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Client cần truy cập qua một chuỗi dài các đối tượng ủy quyền
- Muốn giảm sự phụ thuộc của client vào cấu trúc bên trong của đối tượng
- Cần đóng gói đường dẫn truy cập đến đối tượng mục tiêu

### **Dấu hiệu nhận biết:**
- Client gọi `a.getB().getC().doSomething()`
- Client phải biết về cấu trúc của chuỗi đối tượng ủy quyền
- Thay đổi trong cấu trúc ủy quyền yêu cầu thay đổi ở tất cả client

## **Các Bước Thực Hiện**

1. **Xác định chuỗi ủy quyền**: Tìm các chuỗi lời gọi phương thức dài trong client code
2. **Tạo phương thức ủy quyền**: Trong lớp đầu tiên của chuỗi, tạo phương thức ủy quyền đến phương thức mục tiêu
3. **Cập nhật client**: Thay thế chuỗi lời gọi bằng phương thức ủy quyền mới
4. **Lặp lại**: Tiếp tục cho các chuỗi ủy quyền khác nếu cần

## **Ví Dụ Minh Họa**

### **❌ Trước khi ẩn ủy quyền:**
```java
class Person {
    private Department department;
    
    public Department getDepartment() {
        return department;
    }
}

class Department {
    private Person manager;
    
    public Person getManager() {
        return manager;
    }
}

// Client code - phải biết cấu trúc ủy quyền
Person john = new Person();
Person manager = john.getDepartment().getManager();
```

### **✅ Sau khi ẩn ủy quyền:**
```java
class Person {
    private Department department;
    
    public Department getDepartment() {
        return department;
    }
    
    // Phương thức ủy quyền - ẩn cấu trúc bên trong
    public Person getManager() {
        return department.getManager();
    }
}

class Department {
    private Person manager;
    
    public Person getManager() {
        return manager;
    }
}

// Client code - không cần biết cấu trúc ủy quyền
Person john = new Person();
Person manager = john.getManager(); // Đơn giản và trực tiếp
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Chuỗi ủy quyền phức tạp:**
```java
class Company {
    private List<Department> departments;
    
    public List<Department> getDepartments() {
        return departments;
    }
}

class Department {
    private List<Team> teams;
    
    public List<Team> getTeams() {
        return teams;
    }
}

class Team {
    private List<Person> members;
    
    public List<Person> getMembers() {
        return members;
    }
}

// Client code phức tạp
Company company = new Company();
List<Person> allEmployees = new ArrayList<>();
for (Department dept : company.getDepartments()) {
    for (Team team : dept.getTeams()) {
        allEmployees.addAll(team.getMembers());
    }
}
```

### **✅ Sau khi ẩn ủy quyền:**
```java
class Company {
    private List<Department> departments;
    
    public List<Person> getAllEmployees() {
        List<Person> allEmployees = new ArrayList<>();
        for (Department dept : departments) {
            allEmployees.addAll(dept.getAllEmployees());
        }
        return allEmployees;
    }
}

class Department {
    private List<Team> teams;
    
    public List<Person> getAllEmployees() {
        List<Person> allEmployees = new ArrayList<>();
        for (Team team : teams) {
            allEmployees.addAll(team.getMembers());
        }
        return allEmployees;
    }
}

class Team {
    private List<Person> members;
    
    public List<Person> getMembers() {
        return members;
    }
}

// Client code đơn giản
Company company = new Company();
List<Person> allEmployees = company.getAllEmployees();
```

## **Lợi Ích**

- **✅ Giảm coupling**: Client không phụ thuộc vào cấu trúc bên trong của đối tượng
- **✅ Dễ bảo trì**: Thay đổi cấu trúc ủy quyền chỉ ảnh hưởng đến một lớp
- **✅ Code rõ ràng hơn**: Client code trở nên đơn giản và dễ hiểu
- **✅ Đóng gói tốt hơn**: Ẩn chi tiết triển khai khỏi client

## **Điểm Cần Lưu Ý**

- **Không lạm dụng**: Chỉ tạo phương thức ủy quyền khi thực sự cần thiết
- **Đặt tên rõ ràng**: Tên phương thức ủy quyền nên phản ánh mục đích của nó
- **Cân nhắc hiệu năng**: Trong một số trường hợp, ủy quyền có thể ảnh hưởng đến hiệu năng

## **Kỹ Thuật Liên Quan**

- **Loại bỏ Người Trung Gian (Remove Middle Man)**: Ngược lại với ẩn đối tượng ủy quyền
- **Di chuyển Phương thức (Move Method)**: Có thể sử dụng kết hợp để di chuyển phương thức
- **Trích xuất Phương thức (Extract Method)**: Để tạo các phương thức ủy quyền

## **Kết Luận**

Ẩn đối tượng ủy quyền là kỹ thuật quan trọng để giảm sự phụ thuộc giữa các lớp và cải thiện tính đóng gói. Bằng cách tạo các phương thức ủy quyền, chúng ta che giấu được cấu trúc phức tạp bên trong và cung cấp interface đơn giản, dễ sử dụng cho client.

**Nguồn:** [refactoring.guru/hide-delegate](https://refactoring.guru/hide-delegate)