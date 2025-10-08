# Builder

**Builder** là một **Creational Design Pattern** cho phép bạn xây dựng các đối tượng phức tạp theo từng bước. Pattern này cho phép bạn tạo ra các loại và biểu diễn khác nhau của một đối tượng bằng cùng một code xây dựng.

![Builder Pattern](https://refactoring.guru/images/patterns/content/builder/builder-en.png)

## 🎯 Vấn Đề

Hãy tưởng tượng một đối tượng phức tạp cần khởi tạo tỉ mỉ với nhiều trường và đối tượng lồng nhau. Code khởi tạo như vậy thường bị chôn vùi bên trong một constructor khổng lồ với rất nhiều tham số.

Ví dụ, hãy nghĩ về cách tạo một đối tượng `House`. Để xây một ngôi nhà đơn giản, bạn cần xây bốn bức tường và một sàn nhà, lắp cửa ra vào, lắp một cặp cửa sổ và xây mái nhà. Nhưng nếu bạn muốn một ngôi nhà lớn hơn, có sân vườn, hệ thống sưởi, hệ thống dây điện và đường ống nước, thì sao?

Giải pháp đơn giản nhất là mở rộng lớp `House` cơ sở và tạo một tập hợp các lớp con để bao phủ tất cả các kết hợp của các tham số. Nhưng cuối cùng bạn sẽ có rất nhiều lớp con. Bất kỳ tham số mới nào như kiểu phòng ngủ, có nhà để xe không, v.v., sẽ làm cho hệ thống phân cấp này phát triển theo cấp số nhân.

Có một cách tiếp cận khác: bạn có thể tạo một constructor khổng lồ trong lớp `House` cơ sở với tất cả các tham số có thể cần thiết để kiểm soát đối tượng house. Cách tiếp cận này thực sự không cần các lớp con, nhưng nó tạo ra một vấn đề khác.

![Problem](https://refactoring.guru/images/patterns/diagrams/builder/problem1.png)
*Constructor với quá nhiều tham số*

Hầu hết các tham số sẽ không được sử dụng, làm cho **constructor calls** rất xấu. Ví dụ, chỉ một phần nhỏ ngôi nhà có hồ bơi, do đó các tham số liên quan đến hồ bơi sẽ vô dụng 99% thời gian.

## 💡 Giải Pháp

**Builder** pattern đề xuất rằng bạn trích xuất code xây dựng đối tượng ra khỏi class của chính nó và chuyển nó sang các đối tượng riêng biệt gọi là **builders**.

![Builder Solution](https://refactoring.guru/images/patterns/diagrams/builder/solution1.png)
*Builder pattern cho phép bạn xây dựng các đối tượng phức tạp theo từng bước*

Pattern này tổ chức quá trình xây dựng đối tượng thành một tập hợp các bước (`buildWalls`, `buildDoor`, v.v.). Để tạo một đối tượng, bạn thực hiện một chuỗi các bước này trên một đối tượng builder. Điều quan trọng là bạn không cần phải gọi tất cả các bước. Bạn chỉ cần gọi những bước cần thiết để cấu hình một đối tượng cụ thể.

Một số bước xây dựng có thể yêu cầu triển khai khác nhau khi bạn cần xây dựng các biểu diễn khác nhau của sản phẩm. Trong trường hợp này, bạn có thể tạo một số lớp builder khác nhau triển khai cùng một tập hợp các bước xây dựng, nhưng theo những cách khác nhau.

### Director

Bạn có thể trích xuất thêm một chuỗi các lệnh gọi đến các bước builder thành một lớp **Director**. Lớp `Director` xác định thứ tự thực hiện các bước xây dựng, trong khi `Builder` cung cấp triển khai cho các bước đó.

![Director](https://refactoring.guru/images/patterns/diagrams/builder/structure.png)
*Director ẩn các chi tiết xây dựng khỏi client code*

Lớp `Director` không bắt buộc phải có trong pattern. Bạn có thể gọi trực tiếp các bước xây dựng theo một thứ tự cụ thể từ client code. Tuy nhiên, `Director` có thể là một nơi tốt để đặt các routine xây dựng khác nhau để bạn có thể tái sử dụng chúng trong suốt chương trình.

## 🏗️ Cấu Trúc

1. **Builder Interface** khai báo các bước xây dựng chung cho tất cả các loại builder.

2. **Concrete Builders** cung cấp các triển khai khác nhau cho các bước xây dựng. Concrete builders có thể tạo ra các sản phẩm không tuân theo interface chung.

3. **Products** là các đối tượng kết quả. Các sản phẩm được xây dựng bởi các builder khác nhau không nhất thiết phải thuộc cùng một hệ thống phân cấp class hoặc interface.

4. **Director** lớp xác định thứ tự gọi các bước xây dựng, vì vậy bạn có thể tạo và tái sử dụng các cấu hình cụ thể của sản phẩm.

5. **Client** phải liên kết một trong các đối tượng builder với director. Director sử dụng builder đó cho tất cả các công việc xây dựng tiếp theo.

## 📝 Mã Giả

Ví dụ này minh họa cách **Builder** pattern có thể được sử dụng để xây dựng các ngôi nhà với các thành phần khác nhau.

```pseudocode
// Builder interface xác định tất cả các bước xây dựng có thể
interface HouseBuilder is
    method buildWalls()
    method buildDoors()
    method buildWindows()
    method buildRoof()
    method buildGarage()
    method getResult(): House

// Concrete Builder triển khai các bước xây dựng cụ thể
class WoodHouseBuilder implements HouseBuilder is
    private field house: House

    constructor WoodHouseBuilder() is
        this.reset()

    method reset() is
        this.house = new House()

    method buildWalls() is
        // Xây tường gỗ

    method buildDoors() is
        // Lắp cửa gỗ

    method buildWindows() is
        // Lắp cửa sổ gỗ

    method buildRoof() is
        // Xây mái nhà gỗ

    method buildGarage() is
        // Xây nhà để xe gỗ

    method getResult(): House is
        result = this.house
        this.reset()
        return result

// Director chịu trách nhiệm thực thi các bước xây dựng theo trình tự
class HouseDirector is
    private field builder: HouseBuilder

    method setBuilder(builder: HouseBuilder) is
        this.builder = builder

    method buildSimpleHouse() is
        builder.buildWalls()
        builder.buildDoors()
        builder.buildWindows()
        builder.buildRoof()

    method buildLuxuryHouse() is
        builder.buildWalls()
        builder.buildDoors()
        builder.buildWindows()
        builder.buildRoof()
        builder.buildGarage()
        // Có thể thêm các tính năng khác

// Product
class House is
    // Các thành phần của ngôi nhà

// Client code
class Client is
    method main() is
        director = new HouseDirector()
        builder = new WoodHouseBuilder()
        
        director.setBuilder(builder)
        
        // Xây nhà đơn giản
        director.buildSimpleHouse()
        simpleHouse = builder.getResult()
        
        // Xây nhà sang trọng
        director.buildLuxuryHouse()
        luxuryHouse = builder.getResult()
```

## 🎯 Tính Áp Dụng

**Sử dụng Builder pattern khi:**

- ✅ **Bạn muốn tránh "telescoping constructor"** (constructor với quá nhiều tham số)
- ✅ **Code cần tạo các biểu diễn khác nhau của một đối tượng phức tạp**
- ✅ **Bạn muốn composite trees hoặc các đối tượng phức tạp khác được xây dựng**

## 📋 Các Bước Triển Khai

1. **Xác định các bước xây dựng** phổ biến cho tất cả các loại builder
2. **Khai báo Builder interface** với các bước xây dựng
3. **Tạo Concrete Builder classes** cho từng biến thể sản phẩm
4. **Xem xét tạo Director class** để đóng gói các routine xây dựng khác nhau
5. **Client code tạo cả builder và director objects** và liên kết chúng với nhau

## ✅ Ưu Điểm và ❌ Nhược Điểm

### ✅ Ưu điểm:
- **🎯 Xây dựng đối tượng theo từng bước**, trì hoãn các bước hoặc chạy đệ quy
- **🔄 Tái sử dụng code xây dựng** khi xây dựng various representations của sản phẩm
- **📏 Nguyên tắc Single Responsibility**: tách biệt code xây dựng phức tạp khỏi business logic

### ❌ Nhược điểm:
- **⚡ Độ phức tạp tổng thể tăng** do cần thêm nhiều lớp mới
- **📐 Yêu cầu tạo Builder interface** cho mỗi loại product khác nhau

## 🔗 Mối Quan Hệ Với Các Mẫu Khác

- **Builder** tập trung vào việc xây dựng các đối tượng phức tạp theo từng bước, trong khi **Abstract Factory** chuyên tạo ra các họ đối tượng liên quan
- **Builder** trả về sản phẩm như bước cuối cùng, nhưng trong **Abstract Factory** sản phẩm được trả về ngay lập tức
- **Prototype** không yêu cầu các lớp con, trong khi **Builder** tạo các đối tượng phức tạp bằng cách sử dụng các bước được xác định trước
