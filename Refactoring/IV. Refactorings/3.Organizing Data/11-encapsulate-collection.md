# **Đóng Gói Bộ Sưu Tập (Encapsulate Collection)**

## **Định Nghĩa**
Đóng gói bộ sưu tập là kỹ thuật tái cấu trúc đảm bảo rằng các collection (danh sách, tập hợp, mảng) chỉ được truy cập và sửa đổi thông qua các phương thức đặc biệt của lớp chứa nó, thay vì cho phép truy cập trực tiếp.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một lớp có trường là collection được truy cập trực tiếp từ bên ngoài
- Cần kiểm soát cách thêm, xóa hoặc sửa đổi các phần tử trong collection
- Muốn đảm bảo tính toàn vẹn dữ liệu của collection

### **Dấu hiệu nhận biết:**
- Getter trả về trực tiếp collection, cho phép client sửa đổi nội dung
- Không có phương thức để thêm/xóa phần tử một cách an toàn
- Logic nghiệp vụ liên quan đến collection bị phân tán ở client code

## **Các Bước Thực Hiện**

1. **Thêm phương thức quản lý**: Thêm phương thức để thêm, xóa phần tử vào collection
2. **Khởi tạo collection**: Đảm bảo collection được khởi tạo
3. **Thay đổi getter**: Sửa getter để trả về read-only view của collection
4. **Cập nhật client**: Thay thế các thao tác trực tiếp trên collection bằng phương thức mới

## **Ví Dụ Minh Họa**

### **❌ Trước khi đóng gói:**
```java
class Course {
    private String name;
    
    public Course(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
}

class Student {
    private String name;
    private List<Course> courses = new ArrayList<>(); // Collection public thông qua getter
    
    public Student(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    // Getter trả về trực tiếp collection - NGUY HIỂM!
    public List<Course> getCourses() {
        return courses;
    }
}

// Client code có thể sửa đổi collection tùy ý
Student student = new Student("John");
List<Course> courses = student.getCourses();
courses.add(new Course("Math"));     // Thêm trực tiếp
courses.add(new Course("Physics"));
courses.clear();                     // Xóa toàn bộ - KHÔNG KIỂM SOÁT ĐƯỢC!
```

### **✅ Sau khi đóng gói:**
```java
class Student {
    private String name;
    private List<Course> courses = new ArrayList<>();
    
    public Student(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }
    
    // Getter trả về read-only view
    public List<Course> getCourses() {
        return Collections.unmodifiableList(courses);
    }
    
    // Phương thức để thêm course với validation
    public void addCourse(Course course) {
        if (course == null) {
            throw new IllegalArgumentException("Course không được null");
        }
        if (courses.contains(course)) {
            throw new IllegalStateException("Course đã tồn tại");
        }
        courses.add(course);
    }
    
    // Phương thức để xóa course
    public void removeCourse(Course course) {
        if (course == null) return;
        courses.remove(course);
    }
    
    // Phương thức tiện ích
    public boolean isEnrolledIn(Course course) {
        return courses.contains(course);
    }
    
    public int getCourseCount() {
        return courses.size();
    }
}

// Client code sử dụng an toàn
Student student = new Student("John");
student.addCourse(new Course("Math"));      // Thông qua phương thức được kiểm soát
student.addCourse(new Course("Physics"));

// Không thể sửa đổi trực tiếp collection
List<Course> courses = student.getCourses();
// courses.add(new Course("Chemistry"));    // Sẽ ném exception - KHÔNG CHO PHÉP!
// courses.clear();                         // Sẽ ném exception - KHÔNG CHO PHÉP!

if (student.isEnrolledIn(mathCourse)) {
    student.removeCourse(mathCourse);
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với Set và Map:**
```java
class Order {
    private String orderId;
    private Map<Product, Integer> lineItems = new HashMap<>();
    
    public Order(String orderId) {
        this.orderId = orderId;
    }
    
    public String getOrderId() {
        return orderId;
    }
    
    // Trả về read-only view của line items
    public Map<Product, Integer> getLineItems() {
        return Collections.unmodifiableMap(lineItems);
    }
    
    // Thêm sản phẩm với số lượng
    public void addProduct(Product product, int quantity) {
        if (product == null) {
            throw new IllegalArgumentException("Product không được null");
        }
        if (quantity <= 0) {
            throw new IllegalArgumentException("Số lượng phải lớn hơn 0");
        }
        
        lineItems.merge(product, quantity, Integer::sum);
    }
    
    // Cập nhật số lượng sản phẩm
    public void updateProductQuantity(Product product, int newQuantity) {
        if (!lineItems.containsKey(product)) {
            throw new IllegalArgumentException("Product không tồn tại trong order");
        }
        if (newQuantity <= 0) {
            removeProduct(product);
        } else {
            lineItems.put(product, newQuantity);
        }
    }
    
    // Xóa sản phẩm
    public void removeProduct(Product product) {
        lineItems.remove(product);
    }
    
    // Tính tổng tiền
    public double getTotalAmount() {
        return lineItems.entrySet().stream()
            .mapToDouble(entry -> entry.getKey().getPrice() * entry.getValue())
            .sum();
    }
    
    // Kiểm tra xem order có sản phẩm nào không
    public boolean isEmpty() {
        return lineItems.isEmpty();
    }
}
```

## **Lợi Ích**

- **✅ Kiểm soát truy cập**: Ngăn chặn sửa đổi collection từ bên ngoài
- **✅ Đảm bảo tính toàn vẹn**: Có thể thêm validation khi thêm/xóa phần tử
- **✅ Tính đóng gói**: Ẩn implementation chi tiết của collection
- **✅ Linh hoạt**: Dễ dàng thay đổi loại collection mà không ảnh hưởng client
- **✅ Thống kê và logging**: Có thể thêm logging khi collection thay đổi

## **Điểm Cần Lưu Ý**

- **Trả về read-only view**: Luôn sử dụng `Collections.unmodifiableXXX()`
- **Khởi tạo collection**: Đảm bảo collection luôn được khởi tạo
- **Phương thức tiện ích**: Cung cấp đầy đủ phương thức cho các thao tác phổ biến
- **Performance**: Với collection lớn, cân nhắc trả về copy thay vì read-only view

## **Khi Nào Không Nên Sử Dụng**

- Trong các lớp DTO đơn thuần chỉ để chứa dữ liệu
- Khi performance là yếu tố quan trọng và cần truy cập trực tiếp
- Trong các internal class hoặc private implementation

## **Kỹ Thuật Liên Quan**

- **Encapsulate Field**: Đóng gói trường đơn lẻ
- **Replace Array with Object**: Thay thế mảng bằng đối tượng
- **Introduce Parameter Object**: Nhóm các tham số liên quan

## **Kết Luận**

Đóng gói bộ sưu tập là kỹ thuật quan trọng để bảo vệ tính toàn vẹn dữ liệu và đảm bảo tính đóng gói trong các lớp có chứa collection. Bằng cách cung cấp các phương thức được kiểm soát để thao tác với collection, bạn ngăn chặn được các sửa đổi không mong muốn và tạo ra code an toàn, dễ bảo trì hơn.

**Nguồn:** [refactoring.guru/encapsulate-collection](https://refactoring.guru/encapsulate-collection)