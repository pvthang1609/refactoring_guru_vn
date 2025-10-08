# **Thay Thế Ủy Quyền Bằng Kế Thừa (Replace Delegation with Inheritance)**

## **Định Nghĩa**
Thay thế ủy quyền bằng kế thừa là kỹ thuật tái cấu trúc thay thế quan hệ ủy quyền (delegation) giữa hai lớp bằng quan hệ kế thừa, khi lớp con thực sự cần kế thừa toàn bộ hoặc hầu hết các tính năng của lớp cha và quan hệ "is-a" là phù hợp.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Lớp hiện tại (lớp ủy quyền) sử dụng hầu hết các phương thức của lớp được ủy quyền
- Quan hệ giữa hai lớp thực sự là quan hệ "is-a" (là một)
- Không có vấn đề với việc kế thừa (như fragile base class)
- Muốn giảm số lượng code trung gian (boilerplate code) do ủy quyền tạo ra

### **Không nên sử dụng khi:**
- Lớp hiện tại chỉ sử dụng một phần nhỏ các phương thức của lớp được ủy quyền
- Quan hệ giữa hai lớp là "has-a" (có một) thay vì "is-a"
- Lớp được ủy quyền có thể thay đổi thường xuyên và không ổn định
- Cần sự linh hoạt của composition (có thể thay đổi đối tượng được ủy quyền tại runtime)

## **Các Bước Thực Hiện**

1. **Tạo quan hệ kế thừa**: Cho lớp hiện tại kế thừa từ lớp được ủy quyền
2. **Thay thế trường ủy quyền**: Xóa trường tham chiếu đến đối tượng được ủy quyền
3. **Thay thế các phương thức ủy quyền**: Thay thế các phương thức ủy quyền bằng việc kế thừa trực tiếp
4. **Điều chỉnh constructor**: Điều chỉnh constructor để gọi constructor của lớp cha
5. **Xóa code ủy quyền**: Xóa các phương thức ủy quyền không cần thiết
6. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế (sử dụng ủy quyền không cần thiết):**
```java
// Lớp cơ sở
class Collection {
    protected List<String> items = new ArrayList<>();
    
    public void add(String item) {
        items.add(item);
    }
    
    public void remove(String item) {
        items.remove(item);
    }
    
    public boolean contains(String item) {
        return items.contains(item);
    }
    
    public int size() {
        return items.size();
    }
    
    public void clear() {
        items.clear();
    }
    
    public List<String> getAll() {
        return new ArrayList<>(items);
    }
}

// Lớp Stack sử dụng ủy quyền - nhưng thực sự Stack là một Collection
class Stack {
    private Collection collection; // Ủy quyền
    
    public Stack() {
        this.collection = new Collection();
    }
    
    // Các phương thức ủy quyền - boilerplate code
    public void push(String item) {
        collection.add(item);
    }
    
    public String pop() {
        if (collection.size() == 0) {
            throw new IllegalStateException("Stack is empty");
        }
        List<String> items = collection.getAll();
        String lastItem = items.get(items.size() - 1);
        collection.remove(lastItem);
        return lastItem;
    }
    
    public String peek() {
        if (collection.size() == 0) {
            throw new IllegalStateException("Stack is empty");
        }
        List<String> items = collection.getAll();
        return items.get(items.size() - 1);
    }
    
    public boolean isEmpty() {
        return collection.size() == 0;
    }
    
    public int size() {
        return collection.size();
    }
    
    // Vấn đề: Quá nhiều code trung gian để ủy quyền
    // Stack thực sự là một Collection đặc biệt
}
```

### **✅ Sau khi thay thế (sử dụng kế thừa phù hợp):**
```java
// Lớp cơ sở
class Collection {
    protected List<String> items = new ArrayList<>();
    
    public void add(String item) {
        items.add(item);
    }
    
    public void remove(String item) {
        items.remove(item);
    }
    
    public boolean contains(String item) {
        return items.contains(item);
    }
    
    public int size() {
        return items.size();
    }
    
    public void clear() {
        items.clear();
    }
    
    public List<String> getAll() {
        return new ArrayList<>(items);
    }
}

// Stack kế thừa từ Collection - vì Stack thực sự là một Collection
class Stack extends Collection {
    
    public void push(String item) {
        // Sử dụng trực tiếp phương thức từ lớp cha
        add(item);
    }
    
    public String pop() {
        if (size() == 0) {
            throw new IllegalStateException("Stack is empty");
        }
        // Truy cập trực tiếp items từ lớp cha
        String lastItem = items.get(items.size() - 1);
        remove(lastItem);
        return lastItem;
    }
    
    public String peek() {
        if (size() == 0) {
            throw new IllegalStateException("Stack is empty");
        }
        return items.get(items.size() - 1);
    }
    
    public boolean isEmpty() {
        return size() == 0;
    }
    
    // Không cần phương thức size() - kế thừa từ lớp cha
    
    // Có thể override phương thức nếu cần behavior khác
    @Override
    public void add(String item) {
        // Stack có thể thêm logic đặc biệt khi thêm phần tử
        System.out.println("Adding to stack: " + item);
        super.add(item);
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Ủy quyền không cần thiết:**
```java
class Vehicle {
    private String make;
    private String model;
    private int year;
    
    public Vehicle(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    public void start() {
        System.out.println("Vehicle starting");
    }
    
    public void stop() {
        System.out.println("Vehicle stopping");
    }
    
    public String getInfo() {
        return String.format("%s %s (%d)", make, model, year);
    }
    
    // Getters
    public String getMake() { return make; }
    public String getModel() { return model; }
    public int getYear() { return year; }
}

// Car sử dụng ủy quyền - nhưng thực sự Car là một Vehicle
class Car {
    private Vehicle vehicle; // Ủy quyền
    private int numberOfDoors;
    
    public Car(String make, String model, int year, int numberOfDoors) {
        this.vehicle = new Vehicle(make, model, year);
        this.numberOfDoors = numberOfDoors;
    }
    
    // Rất nhiều phương thức ủy quyền
    public void start() {
        vehicle.start();
    }
    
    public void stop() {
        vehicle.stop();
    }
    
    public String getMake() {
        return vehicle.getMake();
    }
    
    public String getModel() {
        return vehicle.getModel();
    }
    
    public int getYear() {
        return vehicle.getYear();
    }
    
    public String getVehicleInfo() {
        return vehicle.getInfo();
    }
    
    // Phương thức riêng của Car
    public int getNumberOfDoors() {
        return numberOfDoors;
    }
    
    public String getCarInfo() {
        return getVehicleInfo() + " - " + numberOfDoors + " doors";
    }
}
```

### **✅ Kế thừa phù hợp:**
```java
class Vehicle {
    protected String make;
    protected String model;
    protected int year;
    
    public Vehicle(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    public void start() {
        System.out.println("Vehicle starting");
    }
    
    public void stop() {
        System.out.println("Vehicle stopping");
    }
    
    public String getInfo() {
        return String.format("%s %s (%d)", make, model, year);
    }
    
    // Getters
    public String getMake() { return make; }
    public String getModel() { return model; }
    public int getYear() { return year; }
}

// Car kế thừa từ Vehicle - vì Car thực sự là một Vehicle
class Car extends Vehicle {
    private int numberOfDoors;
    
    public Car(String make, String model, int year, int numberOfDoors) {
        // Gọi constructor của lớp cha
        super(make, model, year);
        this.numberOfDoors = numberOfDoors;
    }
    
    // Không cần các phương thức ủy quyền
    // Đã kế thừa: start(), stop(), getInfo(), getMake(), getModel(), getYear()
    
    // Có thể override phương thức nếu cần
    @Override
    public void start() {
        System.out.println("Car starting with key ignition");
        super.start();
    }
    
    @Override
    public String getInfo() {
        // Mở rộng thông tin từ lớp cha
        return super.getInfo() + String.format(" - %d doors", numberOfDoors);
    }
    
    // Phương thức riêng của Car
    public int getNumberOfDoors() {
        return numberOfDoors;
    }
    
    // Không cần getCarInfo() nữa vì getInfo() đã cung cấp đủ thông tin
}

// Có thể dễ dàng mở rộng với các loại Vehicle khác
class Motorcycle extends Vehicle {
    private boolean hasSideCar;
    
    public Motorcycle(String make, String model, int year, boolean hasSideCar) {
        super(make, model, year);
        this.hasSideCar = hasSideCar;
    }
    
    @Override
    public void start() {
        System.out.println("Motorcycle starting with kick start");
    }
    
    @Override
    public String getInfo() {
        String info = super.getInfo();
        if (hasSideCar) {
            info += " with side car";
        }
        return info;
    }
    
    public boolean hasSideCar() {
        return hasSideCar;
    }
}
```

## **Lợi Ích**

- **✅ Giảm code trung gian**: Loại bỏ các phương thức ủy quyền (boilerplate code)
- **✅ Code rõ ràng hơn**: Thể hiện đúng mối quan hệ "is-a"
- **✅ Tận dụng tính kế thừa**: Có thể sử dụng đa hình và các tính năng khác của kế thừa
- **✅ Truy cập trực tiếp**: Có thể truy cập các protected members của lớp cha
- **✅ Dễ bảo trì**: Thay đổi trong lớp cha tự động áp dụng cho lớp con

## **Điểm Cần Lưu ý**

- **Đảm bảo quan hệ "is-a"**: Chỉ sử dụng khi lớp con thực sự là một dạng của lớp cha
- **Cân nhắc vấn đề kế thừa**: Kế thừa có thể dẫn đến coupling chặt và fragile base class
- **Sử dụng protected thay vì private**: Cho phép lớp con truy cập các thành phần cần thiết
- **Xem xét nguyên tắc LSP**: Lớp con phải thay thế được lớp cha mà không làm thay đổi behavior

## **Khi Nào Không Nên Sử Dụng**

- Khi lớp hiện tại chỉ sử dụng một phần nhỏ các phương thức của lớp được ủy quyền
- Khi cần linh hoạt thay đổi behavior tại runtime (composition cho phép điều này)
- Khi lớp được ủy quyền không ổn định và thường xuyên thay đổi
- Khi muốn tránh các vấn đề của kế thừa như fragile base class

## **Kỹ Thuật Liên Quan**

- **Replace Inheritance with Delegation**: Thay thế kế thừa bằng ủy quyền (ngược lại)
- **Extract Superclass**: Trích xuất lớp cha từ các lớp có chung tính năng
- **Pull Up Method**: Kéo phương thức lên lớp cha
- **Pull Up Field**: Kéo trường lên lớp cha

## **Kết Luận**

Thay thế ủy quyền bằng kế thừa là kỹ thuật hữu ích khi quan hệ giữa hai lớp thực sự là "is-a" và việc ủy quyền tạo ra quá nhiều code trung gian không cần thiết. Bằng cách chuyển sang kế thừa, code trở nên gọn gàng hơn và thể hiện đúng mối quan hệ giữa các lớp. Tuy nhiên, cần thận trọng vì kế thừa có những nhược điểm riêng và composition thường được ưu tiên hơn trong thiết kế hiện đại. Luôn đảm bảo rằng quan hệ "is-a" thực sự tồn tại trước khi áp dụng kỹ thuật này.

**Nguồn:** [refactoring.guru/replace-delegation-with-inheritance](https://refactoring.guru/replace-delegation-with-inheritance)