# Prototype

**Prototype** là một **Creational Design Pattern** cho phép bạn sao chép các đối tượng hiện có mà không làm cho code của bạn phụ thuộc vào các lớp của chúng.

![Prototype Pattern](https://refactoring.guru/images/patterns/content/prototype/prototype-en.png)

## 🎯 Vấn Đề

Giả sử bạn có một đối tượng và bạn muốn tạo ra một bản sao chính xác của nó. Làm thế nào để bạn làm điều đó? Đầu tiên, bạn phải tạo một đối tượng mới của cùng lớp. Sau đó, bạn phải duyệt qua tất cả các trường của đối tượng gốc và sao chép các giá trị của chúng sang đối tượng mới.

Tuy nhiên, có một số vấn đề với cách tiếp cận này. Không phải tất cả các đối tượng đều có thể được sao chép theo cách đó vì một số trường có thể là private và không thể truy cập được từ bên ngoài đối tượng.

![Problem](https://refactoring.guru/images/patterns/diagrams/prototype/problem.png)
*Một đối tượng phức tạp có thể có nhiều trường khác nhau*

Có một vấn đề khác với cách tiếp cận sao chép trực tiếp. Vì bạn phải biết lớp của đối tượng để tạo ra một bản sao, code của bạn trở nên phụ thuộc vào lớp đó. Nếu bạn muốn tránh sự phụ thuộc này, bạn cần một cách tiếp cận khác.

## 💡 Giải Pháp

**Prototype** pattern ủy quyền quá trình sao chép cho chính các đối tượng được sao chép. Pattern này khai báo một interface chung cho tất cả các đối tượng hỗ trợ sao chép. Interface này thường chứa một phương thức `clone` duy nhất.

Các lớp triển khai phương thức `clone` chịu trách nhiệm sao chép dữ liệu của chính đối tượng. Một đối tượng hỗ trợ sao chép được gọi là **prototype**.

![Solution](https://refactoring.guru/images/patterns/diagrams/prototype/solution.png)
*Pre-configured prototypes thay thế subclassing*

Khi các đối tượng của bạn có hàng chục trường và hàng trăm cấu hình khác nhau, việc sao chép chúng có thể được coi như một giải pháp thay thế cho việc tạo lớp con. Bạn có thể tạo một tập hợp các đối tượng, được cấu hình theo các cách khác nhau và khi bạn cần một đối tượng giống như đối tượng bạn đã cấu hình, bạn chỉ cần sao chép một prototype thay vì xây dựng một đối tượng mới từ đầu.

## 🏗️ Cấu Trúc

### Cơ Bản

![Basic Structure](https://refactoring.guru/images/patterns/diagrams/prototype/structure.png)

1. **Prototype Interface** khai báo phương thức `clone`. Trong hầu hết các trường hợp, nó là một phương thức duy nhất.

2. **Concrete Prototype** triển khai phương thức `clone`. Ngoài việc sao chép dữ liệu của chính đối tượng, phương thức này cũng có thể xử lý một số chi tiết cụ thể của việc sao chép liên quan đến các trường đối tượng lồng nhau, v.v.

3. **Client** có thể tạo ra một bản sao của bất kỳ đối tượng nào triển khai prototype interface.

### Registry Prototype (Nâng Cao)

![Registry Structure](https://refactoring.guru/images/patterns/diagrams/prototype/structure-prototype-cache.png)

Bạn có thể tạo một **prototype registry** để lưu trữ một tập hợp các prototype thường được sử dụng. Registry này có thể được triển khai như một factory mới hoặc được thêm vào factory prototype cơ sở. Factory này có các phương thức để tìm kiếm và trả về các prototype dựa trên các tiêu chí tìm kiếm khác nhau.

## 📝 Mã Giả

Ví dụ này minh họa cách **Prototype** pattern có thể được sử dụng để sao chép các đối tượng hình học phức tạp mà không làm cho code phụ thuộc vào lớp của chúng.

```pseudocode
// Prototype interface khai báo phương thức clone
interface Shape is
    method clone(): Shape
    method getArea(): float

// Concrete Prototypes triển khai phương thức clone
class Circle implements Shape is
    private field radius: float
    private field x: int
    private field y: int
    private field color: string

    constructor Circle(radius, x, y, color) is
        this.radius = radius
        this.x = x
        this.y = y
        this.color = color

    method clone(): Shape is
        return new Circle(this.radius, this.x, this.y, this.color)

    method getArea(): float is
        return 3.14 * this.radius * this.radius

class Rectangle implements Shape is
    private field width: float
    private field height: float
    private field color: string

    constructor Rectangle(width, height, color) is
        this.width = width
        this.height = height
        this.color = color

    method clone(): Shape is
        return new Rectangle(this.width, this.height, this.color)

    method getArea(): float is
        return this.width * this.height

// Prototype Registry lưu trữ các prototype thường dùng
class ShapeRegistry is
    private static field prototypes: Map<string, Shape>

    static method initialize() is
        prototypes = new Map<string, Shape>()
        prototypes.put("default_circle", new Circle(10, 0, 0, "red"))
        prototypes.put("default_rectangle", new Rectangle(20, 30, "blue"))

    static function getPrototype(type: string): Shape is
        if (prototypes.containsKey(type)) then
            return prototypes.get(type).clone()
        else
            throw new Exception("Prototype not found")

// Client code
class Application is
    method main() is
        // Khởi tạo registry
        ShapeRegistry.initialize()

        // Tạo các đối tượng từ prototypes
        circle1 = ShapeRegistry.getPrototype("default_circle")
        circle2 = ShapeRegistry.getPrototype("default_circle")
        
        rectangle1 = ShapeRegistry.getPrototype("default_rectangle")
        
        // Kiểm tra xem các đối tượng có phải là bản sao không
        print("Circle 1 area: " + circle1.getArea())
        print("Circle 2 area: " + circle2.getArea())
        print("Rectangle 1 area: " + rectangle1.getArea())
        
        // Các đối tượng là độc lập
        print("Are circles the same object? " + (circle1 == circle2))
```

## 🎯 Tính Áp Dụng

**Sử dụng Prototype pattern khi:**

- ✅ **Code của bạn không nên phụ thuộc vào các lớp cụ thể** của các đối tượng bạn cần sao chép
- ✅ **Bạn muốn giảm số lượng lớp con** chỉ để tạo các đối tượng với các cấu hình khác nhau
- ✅ **Bạn có các đối tượng tốn kém để tạo** và bạn muốn sao chép các đối tượng hiện có thay vì tạo mới

## 📋 Các Bước Triển Khai

1. **Tạo prototype interface** với phương thức `clone`
2. **Triển khai phương thức `clone`** trong các concrete classes
3. **Xác định phương pháp sao chép** - shallow copy hay deep copy
4. **Tạo prototype registry** (tùy chọn) để lưu trữ các prototype thường dùng
5. **Thay thế các lệnh gọi constructor trực tiếp** bằng lệnh gọi đến phương thức `clone`

## ✅ Ưu Điểm và ❌ Nhược Điểm

### ✅ Ưu điểm:
- **🎯 Sao chép đối tượng mà không cần coupling** với lớp cụ thể của chúng
- **🔄 Loại bỏ code khởi tạo lặp đi lặp lại** trong việc xây dựng đối tượng phức tạp
- **🚀 Tạo các đối tượng phức tạp** dễ dàng hơn so với việc xây dựng từ đầu
- **📐 Thay thế cho việc kế thừa** khi xử lý các cấu hình đối tượng phức tạp

### ❌ Nhược điểm:
- **⚡ Sao chép các đối tượng có tham chiếu vòng** có thể phức tạp
- **🔍 Khó triển khai `clone`** khi các lớp hiện có không có interface phù hợp
- **💻 Yêu cầu hiểu biết về shallow copy vs deep copy**

## 🔗 Mối Quan Hệ Với Các Mẫu Khác

- **Abstract Factory** và **Prototype** đều có thể được sử dụng thay thế cho nhau trong nhiều trường hợp, nhưng **Prototype** dựa trên kế thừa trong khi **Abstract Factory** dựa trên composition
- **Prototype** không sử dụng inheritance nhưng yêu cầu operation khởi tạo phức tạp. **Factory Method** dựa trên inheritance nhưng không yêu cầu operation khởi tạo
- **Composite** và **Decorator** thường được hưởng lợi từ **Prototype** vì chúng có cấu trúc phân cấp phức tạp
- **Prototype** dựa trên việc sao chép đối tượng trong khi **Memento** dựa trên việc lưu trữ và khôi phục trạng thái
