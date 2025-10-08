# Abstract Factory

**Abstract Factory** là một **Creational Design Pattern** cho phép bạn tạo ra các họ đối tượng liên quan mà không cần chỉ định các lớp cụ thể của chúng.

![Abstract Factory Pattern](https://refactoring.guru/images/patterns/content/abstract-factory/abstract-factory-en.png)

## 🎯 Vấn Đề

Hãy tưởng tượng bạn đang tạo một cửa hàng đồ nội thất mô phỏng. Code của bạn bao gồm các classes đại diện cho:

1. **Một họ các sản phẩm liên quan**: `Chair` + `Sofa` + `CoffeeTable`
2. **Vài biến thể của họ này**: `Modern` + `Victorian` + `ArtDeco`

![Problem](https://refactoring.guru/images/patterns/diagrams/abstract-factory/problem-en.png)
*Các sản phẩm đồ nội thất theo các phong cách khác nhau*

Bạn cần tạo các đối tượng đồ nội thất riêng lẻ sao cho chúng phù hợp với các đối tượng khác trong cùng một họ. Nếu không, khách hàng có thể nhận được đồ nội thất không phù hợp về phong cách.

Hơn nữa, bạn không muốn thay đổi code hiện có khi thêm các sản phẩm mới hoặc họ sản phẩm mới. Các nhà cung cấp đồ nội thất thường xuyên cập nhật danh mục sản phẩm của họ, và bạn không muốn phải thay đổi code mỗi khi điều này xảy ra.

## 💡 Giải Pháp

**Abstract Factory** pattern đề xuất rằng bạn khai báo rõ ràng các interfaces cho từng sản phẩm riêng biệt của họ sản phẩm (ví dụ: `Chair`, `Sofa`, `CoffeeTable`). Sau đó, bạn có thể tạo các factory classes triển khai interface trả về tất cả các sản phẩm theo một phong cách cụ thể.

![Solution](https://refactoring.guru/images/patterns/diagrams/abstract-factory/solution.png)
*Mỗi factory class tương ứng với một biến thể cụ thể của sản phẩm*

Client code có thể gọi các factory và product classes thông qua các interfaces trừu tượng. Điều này cho phép bạn thay đổi loại factory được chuyển đến client code, cũng như biến thể sản phẩm mà client code nhận được, mà không làm hỏng chính client code đó.

![Abstract Factory Structure](https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure.png)
*Cấu trúc của Abstract Factory pattern*

Để minh họa:
- Đối với Modern style: `ModernChair` + `ModernSofa` + `ModernCoffeeTable`
- Đối với Victorian style: `VictorianChair` + `VictorianSofa` + `VictorianCoffeeTable` 

Client code làm việc với cả factories và products thông qua các interfaces trừu tượng. Điều này cho phép client code làm việc với bất kỳ loại factory cụ thể nào và bất kỳ biến thể sản phẩm nào.

## 🏗️ Cấu Trúc

1. **Abstract Products** khai báo interfaces cho một tập hợp các thành phần riêng biệt nhưng liên quan đến nhau tạo nên một họ sản phẩm.
   - `Chair`, `Sofa`, `CoffeeTable`

2. **Concrete Products** là các triển khai cụ thể của abstract products, được nhóm theo các biến thể.
   - `ModernChair`, `ModernSofa`, `ModernCoffeeTable`
   - `VictorianChair`, `VictorianSofa`, `VictorianCoffeeTable`

3. **Abstract Factory** interface khai báo một tập hợp các methods để tạo ra mỗi abstract product.
   - `createChair()`, `createSofa()`, `createCoffeeTable()`

4. **Concrete Factories** triển khai các phương thức tạo của abstract factory. Mỗi concrete factory tương ứng với một biến thể cụ thể của sản phẩm.
   - `ModernFurnitureFactory`
   - `VictorianFurnitureFactory`

5. **Client** làm việc với factories và products chỉ thông qua các abstract types. Điều này cho phép client code làm việc với bất kỳ factory và product variant nào.

## 📝 Mã Giả

Ví dụ này minh họa cách **Abstract Factory** pattern có thể được sử dụng để tạo các thành phần UI đa nền tảng mà không cần ghép cứng client code với các lớp cụ thể.

```pseudocode
// Abstract Factory interface khai báo các phương thức tạo
// cho từng loại product riêng biệt
interface GUIFactory is
    method createButton(): Button
    method createCheckbox(): Checkbox

// Concrete Factories triển khai các phương thức tạo
// để tạo các Concrete Products cụ thể
class WinFactory implements GUIFactory is
    method createButton(): Button is
        return new WinButton()
    method createCheckbox(): Checkbox is
        return new WinCheckbox()

class MacFactory implements GUIFactory is
    method createButton(): Button is
        return new MacButton()
    method createCheckbox(): Checkbox is
        return new MacCheckbox()

// Mỗi product family nên có base interface
// Tất cả các variants của product phải triển khai interface này
interface Button is
    method paint()

interface Checkbox is
    method paint()

// Concrete Products triển khai abstract products
// và được tạo bởi các Concrete Factories tương ứng
class WinButton implements Button is
    method paint() is
        // Render a button in Windows style

class MacButton implements Button is
    method paint() is
        // Render a button in macOS style

class WinCheckbox implements Checkbox is
    method paint() is
        // Render a checkbox in Windows style

class MacCheckbox implements Checkbox is
    method paint() is
        // Render a checkbox in macOS style

// Client code làm việc với factories và products
// thông qua abstract types
class Application is
    private factory: GUIFactory
    private button: Button
    
    constructor Application(factory: GUIFactory) is
        this.factory = factory
    
    method createUI() is
        this.button = factory.createButton()
    
    method paint() is
        button.paint()

// Ứng dụng chọn loại factory tùy thuộc vào cấu hình
// hoặc môi trường hiện tại
class ApplicationConfigurator is
    method main() is
        config = readApplicationConfigFile()
        
        if (config.OS == "Windows") then
            factory = new WinFactory()
        else if (config.OS == "Mac") then
            factory = new MacFactory()
        else
            throw new Exception("Error! Unknown operating system.")
        
        app = new Application(factory)
        app.createUI()
        app.paint()
```

## 🎯 Tính Áp Dụng

**Sử dụng Abstract Factory khi:**

- ✅ **Code cần làm việc với các họ đối tượng liên quan khác nhau** nhưng không phụ thuộc vào các lớp cụ thể của chúng
- ✅ **Bạn muốn cung cấp một thư viện classes** và chỉ muốn tiết lộ interfaces của chúng, không phải implementations
- ✅ **Hệ thống cần được cấu hình với một trong nhiều họ sản phẩm**
- ✅ **Bạn muốn đảm bảo các sản phẩm từ cùng một họ được sử dụng cùng nhau**

## 📋 Các Bước Triển Khai

1. **Xác định các họ sản phẩm** và ánh xạ chúng thành các abstract products
2. **Khai báo Abstract Factory interface** với các phương thức tạo cho tất cả abstract products
3. **Tạo Concrete Factory classes** cho mỗi biến thể của sản phẩm
4. **Triển khai tập hợp Concrete Product classes** cho mỗi biến thể
5. **Triển khai initialization code** để tạo và sử dụng appropriate factory

## ✅ Ưu Điểm và ❌ Nhược Điểm

### ✅ Ưu điểm:
- **🎯 Đảm bảo tính tương thích** giữa các sản phẩm từ cùng một họ
- **🛡️ Tránh tight coupling** giữa client code và concrete products
- **📦 Nguyên tắc Single Responsibility**: tập trung code tạo sản phẩm vào một chỗ
- **🔓 Nguyên tắc Open/Closed**: dễ dàng giới thiệu các biến thể mới mà không phá vỡ code hiện có

### ❌ Nhược điểm:
- **⚡ Code có thể trở nên phức tạp** hơn do cần nhiều interfaces và classes mới
- **📐 Over-engineering** cho các họ sản phẩm đơn giản hoặc ít thay đổi

## 🔗 Mối Quan Hệ Với Các Mẫu Khác

- **Abstract Factory** thường dựa trên tập hợp các **Factory Methods**, nhưng cũng có thể sử dụng **Prototype**
- **Abstract Factory** có thể đóng vai trò thay thế cho **Facade** khi chỉ ẩn cách tạo các đối tượng con khỏi client code
- **Builder** tập trung vào việc xây dựng các đối tượng phức tạp từng bước, trong khi **Abstract Factory** tạo ra các họ đối tượng liên quan
- **Abstract Factory** có thể được triển khai bằng **Singleton**
