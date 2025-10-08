# **Thu Gọn Hệ Thống Phân Cấp (Collapse Hierarchy)**

## **Định Nghĩa**
Thu gọn hệ thống phân cấp là kỹ thuật tái cấu trúc hợp nhất lớp cha và lớp con khi sự phân biệt giữa chúng không còn cần thiết hoặc không còn ý nghĩa.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Lớp cha và lớp con không còn khác biệt đáng kể
- Hệ thống phân cấp trở nên quá phức tạp và không cần thiết
- Các lớp con không còn có hành vi hoặc dữ liệu đặc biệt
- Việc duy trì hệ thống phân cấp gây khó khăn cho việc bảo trì

### **Không nên sử dụng khi:**
- Hệ thống phân cấp vẫn còn ý nghĩa và có sự khác biệt rõ ràng
- Các lớp con có các phương thức hoặc trường riêng biệt quan trọng
- Việc thu gọn làm mất đi tính đa hình cần thiết

## **Các Bước Thực Hiện**

1. **Chọn lớp cần thu gọn**: Quyết định xem sẽ hợp nhất lớp con vào lớp cha hay ngược lại
2. **Di chuyển các phương thức và trường**: Di chuyển tất cả các phương thức và trường từ lớp này sang lớp kia
3. **Điều chỉnh các tham chiếu**: Cập nhật tất cả các tham chiếu đến lớp bị xóa để trỏ đến lớp được giữ lại
4. **Xóa lớp không cần thiết**: Xóa lớp đã được hợp nhất
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi thu gọn hệ thống phân cấp:**
```java
// Hệ thống phân cấp quá phức tạp và không cần thiết
abstract class Vehicle {
    protected String make;
    protected String model;
    protected int year;
    
    public Vehicle(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    public abstract void start();
    public abstract void stop();
    
    public String getInfo() {
        return String.format("%s %s (%d)", make, model, year);
    }
}

class Car extends Vehicle {
    private int numberOfDoors;
    
    public Car(String make, String model, int year, int numberOfDoors) {
        super(make, model, year);
        this.numberOfDoors = numberOfDoors;
    }
    
    @Override
    public void start() {
        System.out.println("Turning key to start car");
    }
    
    @Override
    public void stop() {
        System.out.println("Turning key to stop car");
    }
    
    public int getNumberOfDoors() {
        return numberOfDoors;
    }
}

class ElectricCar extends Car {
    private int batteryCapacity;
    
    public ElectricCar(String make, String model, int year, int numberOfDoors, int batteryCapacity) {
        super(make, model, year, numberOfDoors);
        this.batteryCapacity = batteryCapacity;
    }
    
    @Override
    public void start() {
        System.out.println("Pressing button to start electric car");
    }
    
    @Override
    public void stop() {
        System.out.println("Pressing button to stop electric car");
    }
    
    public int getBatteryCapacity() {
        return batteryCapacity;
    }
}

// Vấn đề: ElectricCar và Car quá giống nhau, hệ thống phân cấp không cần thiết
// Cả hai đều có số cửa, chỉ khác cách start/stop và battery capacity
```

### **✅ Sau khi thu gọn hệ thống phân cấp:**
```java
// Hợp nhất ElectricCar vào Car - hệ thống phân cấp đơn giản hơn
class Vehicle {
    protected String make;
    protected String model;
    protected int year;
    protected int numberOfDoors;
    protected VehicleType type;
    private Integer batteryCapacity; // Chỉ có với electric vehicles
    
    public Vehicle(String make, String model, int year, int numberOfDoors, VehicleType type) {
        this.make = make;
        this.model = model;
        this.year = year;
        this.numberOfDoors = numberOfDoors;
        this.type = type;
    }
    
    public Vehicle(String make, String model, int year, int numberOfDoors, 
                  VehicleType type, int batteryCapacity) {
        this(make, model, year, numberOfDoors, type);
        this.batteryCapacity = batteryCapacity;
    }
    
    public void start() {
        if (type == VehicleType.ELECTRIC) {
            System.out.println("Pressing button to start electric vehicle");
        } else {
            System.out.println("Turning key to start vehicle");
        }
    }
    
    public void stop() {
        if (type == VehicleType.ELECTRIC) {
            System.out.println("Pressing button to stop electric vehicle");
        } else {
            System.out.println("Turning key to stop vehicle");
        }
    }
    
    public String getInfo() {
        String info = String.format("%s %s (%d) - %d doors", make, model, year, numberOfDoors);
        if (type == VehicleType.ELECTRIC) {
            info += String.format(" - Battery: %dkWh", batteryCapacity);
        }
        return info;
    }
    
    // Các getter
    public String getMake() { return make; }
    public String getModel() { return model; }
    public int getYear() { return year; }
    public int getNumberOfDoors() { return numberOfDoors; }
    public VehicleType getType() { return type; }
    public Integer getBatteryCapacity() { return batteryCapacity; }
    
    // Setter cho battery capacity (chỉ với electric)
    public void setBatteryCapacity(int batteryCapacity) {
        if (type != VehicleType.ELECTRIC) {
            throw new IllegalStateException("Battery capacity only applicable for electric vehicles");
        }
        this.batteryCapacity = batteryCapacity;
    }
}

enum VehicleType {
    GASOLINE, DIESEL, ELECTRIC, HYBRID
}

// Factory để tạo vehicle
class VehicleFactory {
    public static Vehicle createCar(String make, String model, int year, int numberOfDoors) {
        return new Vehicle(make, model, year, numberOfDoors, VehicleType.GASOLINE);
    }
    
    public static Vehicle createElectricCar(String make, String model, int year, 
                                          int numberOfDoors, int batteryCapacity) {
        return new Vehicle(make, model, year, numberOfDoors, VehicleType.ELECTRIC, batteryCapacity);
    }
}

// Client code đơn giản hơn
public class TransportationSystem {
    public static void main(String[] args) {
        Vehicle regularCar = VehicleFactory.createCar("Toyota", "Camry", 2023, 4);
        Vehicle electricCar = VehicleFactory.createElectricCar("Tesla", "Model 3", 2023, 4, 75);
        
        System.out.println(regularCar.getInfo());
        regularCar.start();
        regularCar.stop();
        
        System.out.println(electricCar.getInfo());
        electricCar.start();
        electricCar.stop();
    }
}
```

## **Lợi Ích**

- **✅ Đơn giản hóa code**: Giảm số lượng lớp và độ phức tạp
- **✅ Dễ bảo trì**: Ít lớp hơn để quản lý và ít sự kế thừa hơn
- **✅ Giảm overhead**: Không cần duy trì hệ thống phân cấp không cần thiết
- **✅ Linh hoạt**: Một lớp duy nhất có thể xử lý nhiều trường hợp

## **Điểm Cần Lưu ý**

- **Không phá vỡ tính đa hình**: Nếu các lớp con có hành vi khác nhau, việc thu gọn có thể làm mất tính đa hình
- **Cân nhắc nguyên tắc đơn trách nhiệm**: Đảm bảo lớp hợp nhất không trở nên quá lớn và có quá nhiều trách nhiệm
- **Sử dụng enum hoặc flags**: Để phân biệt các loại khác nhau trong một lớp
- **Xem xét sử dụng composition**: Đôi khi composition có thể tốt hơn inheritance

## **Khi Nào Không Nên Sử Dụng**

- Khi các lớp con có hành vi khác biệt đáng kể
- Khi hệ thống phân cấp phản ánh đúng các khái niệm trong domain
- Khi việc thu gọn làm cho lớp trở nên quá phức tạp (God class)
- Khi cần tận dụng tính đa hình

## **Kỹ Thuật Liên Quan**

- **Extract Subclass**: Trích xuất lớp con (ngược lại)
- **Extract Superclass**: Trích xuất lớp cha
- **Inline Class**: Hợp nhất hai lớp không có quan hệ kế thừa
- **Replace Inheritance with Delegation**: Thay thế kế thừa bằng ủy quyền

## **Kết Luận**

Thu gọn hệ thống phân cấp là kỹ thuật hữu ích khi sự phân biệt giữa lớp cha và lớp con không còn cần thiết. Bằng cách hợp nhất chúng, chúng ta đơn giản hóa code và làm cho nó dễ bảo trì hơn. Tuy nhiên, cần thận trọng để không làm mất đi tính linh hoạt và khả năng mở rộng của hệ thống. Kỹ thuật này đặc biệt hữu ích khi hệ thống phân cấp đã trở nên quá phức tạp và không còn phản ánh đúng thực tế của domain.

**Nguồn:** [refactoring.guru/collapse-hierarchy](https://refactoring.guru/collapse-hierarchy)