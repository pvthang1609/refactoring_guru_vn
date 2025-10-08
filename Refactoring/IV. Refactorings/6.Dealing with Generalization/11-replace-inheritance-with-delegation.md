# **Thay Thế Kế Thừa Bằng Ủy Quyền (Replace Inheritance with Delegation)**

## **Định Nghĩa**
Thay thế kế thừa bằng ủy quyền là kỹ thuật tái cấu trúc thay thế quan hệ kế thừa giữa hai lớp bằng quan hệ ủy quyền (delegation), khi lớp con không thực sự cần kế thừa tất cả các tính năng của lớp cha hoặc khi kế thừa không còn phù hợp.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Lớp con chỉ sử dụng một phần tính năng của lớp cha
- Lớp con không thực sự là một dạng đặc biệt của lớp cha (vi phạm nguyên tắc LSP)
- Muốn giảm sự kết hợp chặt chẽ (tight coupling) giữa các lớp
- Cần linh hoạt hơn trong việc thay đổi behavior tại runtime
- Muốn tránh vấn đề "fragile base class" (lớp cơ sở dễ vỡ)

### **Không nên sử dụng khi:**
- Lớp con thực sự là một dạng đặc biệt của lớp cha và cần kế thừa toàn bộ tính năng
- Quan hệ kế thừa phản ánh đúng mối quan hệ "is-a" trong thực tế
- Việc ủy quyền tạo ra quá nhiều code trung gian không cần thiết

## **Các Bước Thực Hiện**

1. **Tạo trường tham chiếu đến lớp cha**: Trong lớp con, tạo một trường để lưu đối tượng của lớp cha
2. **Thay đổi kế thừa thành ủy quyền**: Thay vì kế thừa, lớp con sẽ ủy quyền các yêu cầu đến đối tượng lớp cha
3. **Điều chỉnh constructor**: Khởi tạo đối tượng lớp cha trong constructor của lớp con
4. **Cập nhật các phương thức**: Thay thế các lời gọi đến phương thức kế thừa bằng lời gọi ủy quyền
5. **Xóa quan hệ kế thừa**: Xóa quan hệ kế thừa giữa hai lớp

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế (sử dụng kế thừa không phù hợp):**
```java
// Lớp Stack kế thừa từ ArrayList - không phù hợp vì Stack không phải là ArrayList
class Stack<E> extends ArrayList<E> {
    public void push(E element) {
        add(element);
    }
    
    public E pop() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return remove(size() - 1);
    }
    
    public E peek() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return get(size() - 1);
    }
    
    // Vấn đề: Stack kế thừa tất cả các phương thức của ArrayList
    // như add(int index, E element), remove(int index),...
    // mà những phương thức này không phù hợp với ngữ nghĩa của Stack
}

// Sử dụng
Stack<String> stack = new Stack<>();
stack.push("first");
stack.push("second");

// Có thể sử dụng các phương thức không phù hợp từ ArrayList
stack.add(0, "inserted"); // Phá vỡ ngữ nghĩa stack - phần tử mới không ở đỉnh
stack.remove(0); // Xóa phần tử không ở đỉnh

// Điều này vi phạm nguyên tắc Liskov Substitution Principle (LSP)
// Stack không thể thay thế ArrayList trong mọi trường hợp
```

### **✅ Sau khi thay thế (sử dụng ủy quyền):**
```java
// Stack sử dụng ủy quyền thay vì kế thừa từ ArrayList
class Stack<E> {
    // Ủy quyền thay vì kế thừa
    private List<E> list = new ArrayList<>();
    
    public void push(E element) {
        list.add(element);
    }
    
    public E pop() {
        if (list.isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return list.remove(list.size() - 1);
    }
    
    public E peek() {
        if (list.isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return list.get(list.size() - 1);
    }
    
    public boolean isEmpty() {
        return list.isEmpty();
    }
    
    public int size() {
        return list.size();
    }
    
    // Chỉ cung cấp các phương thức phù hợp với Stack
    // Không có các phương thức như add(int index, E element) hay remove(int index)
    
    // Có thể thêm các phương thức tiện ích
    public void clear() {
        list.clear();
    }
    
    @Override
    public String toString() {
        return "Stack: " + list.toString();
    }
}

// Sử dụng
Stack<String> stack = new Stack<>();
stack.push("first");
stack.push("second");
stack.push("third");

System.out.println(stack.peek()); // "third"
System.out.println(stack.pop());  // "third"
System.out.println(stack.pop());  // "second"

// Không thể sử dụng các phương thức không phù hợp
// stack.add(0, "inserted"); // LỖI BIÊN DỊCH - không có phương thức này
// stack.remove(0);          // LỖI BIÊN DỊCH - không có phương thức này
```

## **Ví Dụ Phức Tạp Hơn**

### **❌ Kế thừa không phù hợp:**
```java
class Engine {
    private int horsepower;
    private String fuelType;
    
    public Engine(int horsepower, String fuelType) {
        this.horsepower = horsepower;
        this.fuelType = fuelType;
    }
    
    public void start() {
        System.out.println("Engine starting with " + horsepower + " HP");
    }
    
    public void stop() {
        System.out.println("Engine stopping");
    }
    
    public void maintenance() {
        System.out.println("Performing engine maintenance");
    }
}

// Car kế thừa từ Engine - không phù hợp vì Car không phải là Engine
class Car extends Engine {
    private String model;
    private String color;
    
    public Car(String model, String color, int horsepower, String fuelType) {
        super(horsepower, fuelType);
        this.model = model;
        this.color = color;
    }
    
    public void drive() {
        start(); // Sử dụng phương thức từ Engine
        System.out.println("Car is driving");
    }
    
    public void park() {
        stop(); // Sử dụng phương thức từ Engine
        System.out.println("Car is parked");
    }
    
    // Vấn đề: Car kế thừa các phương thức không phù hợp từ Engine
    // như maintenance() - bảo trì động cơ chỉ là một phần của bảo trì xe
}

// Sử dụng
Car car = new Car("Toyota", "Red", 150, "Gasoline");
car.drive();
car.maintenance(); // Có sẵn từ Engine, nhưng không hoàn toàn phù hợp
```

### **✅ Ủy quyền phù hợp:**
```java
class Engine {
    private int horsepower;
    private String fuelType;
    
    public Engine(int horsepower, String fuelType) {
        this.horsepower = horsepower;
        this.fuelType = fuelType;
    }
    
    public void start() {
        System.out.println("Engine starting with " + horsepower + " HP");
    }
    
    public void stop() {
        System.out.println("Engine stopping");
    }
    
    public void performMaintenance() {
        System.out.println("Performing engine maintenance");
    }
    
    public int getHorsepower() { return horsepower; }
    public String getFuelType() { return fuelType; }
}

class Car {
    private String model;
    private String color;
    private Engine engine; // Ủy quyền thay vì kế thừa
    
    public Car(String model, String color, int horsepower, String fuelType) {
        this.model = model;
        this.color = color;
        this.engine = new Engine(horsepower, fuelType);
    }
    
    // Ủy quyền các phương thức liên quan đến động cơ
    public void drive() {
        engine.start();
        System.out.println("Car is driving");
    }
    
    public void park() {
        engine.stop();
        System.out.println("Car is parked");
    }
    
    public void performEngineMaintenance() {
        engine.performMaintenance();
    }
    
    // Các phương thức riêng của Car
    public void wash() {
        System.out.println("Washing the car");
    }
    
    public void changeTires() {
        System.out.println("Changing car tires");
    }
    
    // Có thể ủy quyền thông tin từ Engine nếu cần
    public int getHorsepower() {
        return engine.getHorsepower();
    }
    
    public String getFuelType() {
        return engine.getFuelType();
    }
    
    // Getter cho các thuộc tính riêng
    public String getModel() { return model; }
    public String getColor() { return color; }
}

// Sử dụng
Car car = new Car("Toyota", "Red", 150, "Gasoline");
car.drive();                    // Ủy quyền đến engine.start()
car.performEngineMaintenance(); // Ủy quyền đến engine.performMaintenance()
car.wash();                     // Phương thức riêng của Car

System.out.println("Car horsepower: " + car.getHorsepower()); // Ủy quyền

// Linh hoạt hơn: có thể thay đổi Engine tại runtime
class ElectricEngine {
    public void start() {
        System.out.println("Electric engine starting silently");
    }
    
    public void stop() {
        System.out.println("Electric engine stopping");
    }
    
    public void performMaintenance() {
        System.out.println("Performing electric engine maintenance");
    }
    
    public int getPower() { return 200; } // kW thay vì HP
}

class ElectricCar extends Car {
    private ElectricEngine electricEngine;
    
    public ElectricCar(String model, String color) {
        super(model, color, 0, "Electric"); // Gọi constructor cha
        this.electricEngine = new ElectricEngine();
    }
    
    @Override
    public void drive() {
        electricEngine.start();
        System.out.println("Electric car is driving silently");
    }
    
    @Override
    public void performEngineMaintenance() {
        electricEngine.performMaintenance();
    }
    
    @Override
    public int getHorsepower() {
        // Chuyển đổi kW sang HP
        return (int) (electricEngine.getPower() * 1.34);
    }
}
```

## **Lợi Ích**

- **✅ Giảm kết hợp chặt chẽ**: Các lớp độc lập hơn với nhau
- **✅ Linh hoạt hơn**: Có thể thay đổi behavior tại runtime
- **✅ Tuân thủ nguyên tắc LSP**: Không vi phạm nguyên tắc thay thế của Liskov
- **✅ Kiểm soát tốt hơn**: Chỉ ủy quyền các phương thức cần thiết
- **✅ Tránh fragile base class**: Thay đổi trong lớp cha không ảnh hưởng đến lớp con
- **✅ Hỗ trợ composition over inheritance**: Tuân thủ nguyên tắc thiết kế quan trọng

## **Điểm Cần Lưu ý**

- **Không lạm dụng**: Vẫn sử dụng kế thừa khi có quan hệ "is-a" thực sự
- **Cân nhắc performance**: Ủy quyền có thể thêm một chút overhead
- **Sử dụng interface**: Có thể ủy quyền cho interface để linh hoạt hơn
- **Document rõ ràng**: Ghi chú rõ mối quan hệ ủy quyền trong code

## **Khi Nào Không Nên Sử Dụng**

- Khi lớp con thực sự là một dạng đặc biệt của lớp cha (quan hệ "is-a")
- Khi cần truy cập đến các protected members của lớp cha
- Khi việc ủy quyền tạo ra quá nhiều code wrapper không cần thiết
- Khi performance là yếu tố quan trọng và ủy quyền tạo overhead đáng kể

## **Kỹ Thuật Liên Quan**

- **Replace Delegation with Inheritance**: Thay thế ủy quyền bằng kế thừa (ngược lại)
- **Extract Class**: Trích xuất lớp mới từ một lớp hiện có
- **Introduce Local Extension**: Giới thiệu phần mở rộng cục bộ
- **Hide Delegate**: Ẩn đối tượng được ủy quyền

## **Kết Luận**

Thay thế kế thừa bằng ủy quyền là kỹ thuật quan trọng để thiết kế các hệ thống linh hoạt và dễ bảo trì. Bằng cách sử dụng composition thay vì inheritance trong những trường hợp không phù hợp, chúng ta giảm được sự kết hợp chặt chẽ giữa các lớp, tăng tính linh hoạt và tuân thủ tốt hơn các nguyên tắc thiết kế hướng đối tượng. Tuy nhiên, cần cân nhắc kỹ lưỡng để lựa chọn đúng kỹ thuật cho từng tình huống cụ thể, vì kế thừa vẫn có giá trị khi mối quan hệ "is-a" thực sự tồn tại.

**Nguồn:** [refactoring.guru/replace-inheritance-with-delegation](https://refactoring.guru/replace-inheritance-with-delegation)