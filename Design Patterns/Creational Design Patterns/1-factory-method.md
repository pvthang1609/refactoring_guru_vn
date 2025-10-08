# Factory Method

**Factory Method** là một **Creational Design Pattern** cung cấp một interface để tạo các object trong superclass, nhưng cho phép các subclass thay đổi kiểu object sẽ được tạo.

![Factory Method Pattern](https://refactoring.guru/images/patterns/content/factory-method/factory-method-en.png)

## 🎯 Vấn Đề

Hãy tưởng tượng bạn đang tạo một ứng dụng quản lý logistics. Phiên bản đầu tiên của bạn chỉ xử lý vận chuyển bằng xe tải, vì vậy phần lớn code của bạn nằm trong class `Truck`.

Sau một thời gian, ứng dụng của bạn trở nên rất phổ biến. Mỗi ngày bạn nhận được hàng chục yêu cầu từ các công ty vận tải đường biển để tích hợp vận chuyển bằng đường biển vào ứng dụng.

![Problem](https://refactoring.guru/images/patterns/diagrams/factory-method/problem1-en.png)
*Thật tuyệt, nhưng code của bạn thì sao? Hiện tại, phần lớn code của bạn đã kết hợp chặt chẽ với class `Truck`. Để thêm class `Ship` vào ứng dụng, bạn sẽ phải thay đổi toàn bộ codebase.*

Hơn nữa, nếu sau này bạn quyết định thêm một loại hình vận chuyển khác vào ứng dụng, bạn có thể sẽ phải thực hiện lại tất cả những thay đổi đó một lần nữa.

Kết quả là, bạn sẽ có một code khủng khiếp với rất nhiều câu lệnh điều kiện thay đổi behavior của ứng dụng tùy thuộc vào class phương tiện.

## 💡 Giải Pháp

**Factory Method** pattern đề xuất rằng thay vì gọi trực tiếp toán tử `new` để tạo object, bạn nên gọi một **factory method** đặc biệt. Đừng lo lắng: các object vẫn được tạo thông qua toán tử `new`, nhưng nó được gọi từ bên trong factory method.

![Solution](https://refactoring.guru/images/patterns/diagrams/factory-method/solution1.png)
*Các subclass có thể thay đổi loại product sẽ được tạo bởi factory method.*

Nghe có vẻ hơi vô nghĩa: chúng ta chỉ di chuyển lệnh gọi constructor từ một phần của chương trình sang một phần khác. Tuy nhiên, hãy xem xét điều này: giờ đây bạn có thể ghi đè factory method trong một subclass và thay đổi class của product sẽ được tạo.

Tuy nhiên, có một hạn chế nhỏ: các subclass chỉ có thể trả về các products có kiểu khác nhau nếu các products này có một base class chung hoặc một interface chung. Ngoài ra, factory method trong base class phải có return type được khai báo là interface chung này.

![Solution](https://refactoring.guru/images/patterns/diagrams/factory-method/solution2-en.png)
*Tất cả các products phải tuân theo cùng một interface.*

Ví dụ, cả class `Truck` và `Ship` đều phải implement interface `Transport`, interface này khai báo một method có tên là `deliver()`. Mỗi class implement method này theo một cách khác nhau: xe tải vận chuyển hàng hóa bằng đường bộ, còn tàu thủy vận chuyển hàng hóa bằng đường biển. Factory method trong class `RoadLogistics` trả về một object `Truck`, trong khi factory method trong class `SeaLogistics` trả về một object `Ship`.

![Solution](https://refactoring.guru/images/patterns/diagrams/factory-method/solution3-en.png)
*Miễn là tất cả các class products implement một interface chung, bạn có thể chuyển các objects của chúng đến client code mà không làm hỏng nó.*

Phần code sử dụng factory method (thường được gọi là client code) không thấy sự khác biệt giữa các products thực tế được trả về bởi các subclass khác nhau. Client coi tất cả các products là một `Transport` trừu tượng. Client biết rằng tất cả các objects vận tải đều phải có method `deliver()`, nhưng chính xác nó hoạt động như thế nào thì không quan trọng đối với client.

## 🏗️ Cấu Trúc

![Structure](https://refactoring.guru/images/patterns/diagrams/factory-method/structure.png)

1. **Product** khai báo interface, interface này là chung cho tất cả các objects mà creator và các subclass của nó có thể tạo.

2. **Concrete Products** là các implementations khác nhau của product interface.

3. **Creator** khai báo factory method trả về các product objects. Return type của method này phải khớp với product interface.
   
   Bạn có thể khai báo factory method là `abstract` để buộc tất cả các subclass implement phiên bản của riêng chúng. Hoặc, factory method cơ sở có thể trả về một product type mặc định.
   
   Lưu ý, mặc dù tên gọi là "factory method", nhưng nó không phải là nhiệm vụ duy nhất của Creator. Thông thường, class creator chứa một số core business logic dựa trên các product objects mà factory method trả về.

4. **Concrete Creators** ghi đè factory method cơ sở để trả về một product type khác.

Lưu ý rằng factory method không *phải lúc nào* cũng *phải* tạo một instance mới. Nó cũng có thể trả về một object hiện có từ bộ nhớ cache, một object pool hoặc một nguồn khác.

## 📝 Mã Giả

Ví dụ này minh họa cách sử dụng **Factory Method** để tạo các UI objects đa nền tảng mà không cần ghép cứng client code với các UI classes cụ thể.

![Example](https://refactoring.guru/images/patterns/diagrams/factory-method/example.png)
*Các concrete creators trả về các product types khác nhau tùy thuộc vào môi trường.*

```pseudocode
// Creator class cũng có thể cung cấp một implementation mặc định
// của factory method.
class Dialog is
    // Creator sử dụng factory method bên trong business logic
    // của nó. Subclass có thể thay đổi logic này trực tiếp bằng cách
    // ghi đè factory method.
    method render() is
        // Gọi factory method để tạo một product object.
        Button okButton = createButton()
        // Bây giờ, sử dụng product.
        okButton.onClick(closeDialog)
        okButton.render()

    // Factory method được khai báo là abstract để buộc các
    // subclass implement logic của riêng chúng.
    abstract method createButton()

// Concrete Creators ghi đè factory method để
// thay đổi loại product kết quả.
class WindowsDialog extends Dialog is
    method createButton() is
        return new WindowsButton()

class WebDialog extends Dialog is
    method createButton() is
        return new HTMLButton()

// Product interface khai báo các behaviors của tất cả
// Concrete Products.
interface Button is
    method render()
    method onClick(f)

// Concrete Products cung cấp các implementations khác nhau
// của product interface.
class WindowsButton implements Button is
    method render(a, b) is
        // Hiển thị một button theo phong cách Windows.
    method onClick(f) is
        // Gắn sự kiện click theo phong cách Windows.

class HTMLButton implements Button is
    method render(a, b) is
        // Trả về một HTML representation của một button.
    method onClick(f) is
        // Gắn sự kiện click của trình duyệt web.

// Client code hoạt động với một instance của một Concrete Creator,
// mặc dù thông qua base interface của nó. Miễn là client tiếp tục
// làm việc với creator thông qua base interface, bạn có thể
// chuyển nó cho bất kỳ subclass nào của creator.
class Application is
    field dialog: Dialog

    // Ứng dụng chọn loại creator tùy thuộc vào cấu hình
    // hoặc môi trường hiện tại.
    method initialize() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            dialog = new WindowsDialog()
        else if (config.OS == "Web") then
            dialog = new WebDialog()
        else
            throw new Exception("Error! Unknown operating system.")

    // Client code hiện tại sẽ hoạt động với bất kỳ instance
    // Dialog cụ thể nào.
    method main() is
        this.initialize()
        dialog.render()
```

## 🎯 Tính Áp Dụng

**Sử dụng Factory Method khi:**

- **Bạn không biết trước chính xác loại và dependencies của các objects** mà code của bạn nên làm việc cùng.
  
  Factory Method tách biệt code xây dựng product khỏi code thực sự sử dụng product. Do đó, việc mở rộng code xây dựng product một cách độc lập với phần còn lại của code là dễ dàng hơn.
  
  Ví dụ, để thêm một product type mới vào ứng dụng, bạn chỉ cần tạo một creator subclass mới và ghi đè factory method trong đó.

- **Bạn muốn cung cấp cho người dùng thư viện hoặc framework của mình** một cách để mở rộng các internal components của nó.
  
  Inheritance có lẽ là cách đơn giản nhất để mở rộng behavior mặc định của một thư viện hoặc framework. Nhưng một framework nên nhận ra các components tùy chỉnh thông qua một base class hoặc interface duy nhất, thay vì các subclass cụ thể của chúng.

- **Bạn muốn tiết kiệm tài nguyên hệ thống** bằng cách sử dụng lại các objects hiện có thay vì tạo lại chúng mỗi lần.
  
  Bạn thường gặp yêu cầu này khi làm việc với các objects lớn, nặng tài nguyên như kết nối cơ sở dữ liệu, kết nối mạng và tệp tin.

## 📋 Các Bước Triển Khai

1. Làm cho tất cả các products implement cùng một interface. Interface này nên khai báo các methods có ý nghĩa trong mọi product.

2. Thêm một factory method trống bên trong creator class. Return type của method phải khớp với product interface chung.

3. Trong code của creator class, tìm tất cả các tham chiếu đến product constructors. Thay thế lần lượt từng tham chiếu đó bằng một lời gọi đến factory method, đồng thời trích xuất code tạo product sang factory method.

4. Bây giờ, tạo một tập hợp các creator subclasses cho từng product type được liệt kê trong factory method. Ghi đè factory method trong các subclasses và trích xuất các phần phù hợp của code tạo từ method cơ sở.

5. Nếu có quá nhiều product types và không có ý nghĩa gì khi tạo một subclass cho mỗi loại, bạn có thể sử dụng lại parameter điều khiển từ base class trong các subclasses.

6. Nếu sau khi tất cả các bước di chuyển, factory method cơ sở trống rỗng, bạn có thể biến nó thành abstract. Nếu không, bạn có thể biến nó thành một method mặc định trả về một product mặc định.

## ✅ Ưu Điểm và ❌ Nhược Điểm

### ✅ Ưu điểm:
- **Bạn tránh được tight coupling** giữa creator và các concrete products.
- **Single Responsibility Principle**: Bạn có thể di chuyển code tạo product vào một địa điểm duy nhất trong chương trình, giúp code dễ dàng hỗ trợ hơn.
- **Open/Closed Principle**: Bạn có thể giới thiệu các product types mới vào chương trình mà không làm hỏng client code hiện có.

### ❌ Nhược điểm:
- Code có thể trở nên phức tạp hơn vì bạn cần giới thiệu nhiều subclasses mới để implement pattern.

## 🔗 Mối Quan Hệ Với Các Mẫu Khác

- Nhiều designs bắt đầu bằng cách sử dụng **Factory Method** (ít phức tạp hơn và có thể tùy chỉnh thông qua các subclasses) và phát triển thành **Abstract Factory**, **Prototype**, hoặc **Builder** (linh hoạt hơn nhưng phức tạp hơn).

- **Abstract Factory** thường dựa trên một tập hợp các **Factory Methods**, nhưng bạn cũng có thể sử dụng **Prototype** để soạn các methods này.

- Bạn có thể sử dụng **Factory Method** cùng với **Iterator** để cho phép các collection subclasses trả về các iterators types khác nhau tương thích với các collections.

- **Prototype** không dựa trên inheritance, vì vậy nó không có nhược điểm của inheritance. Mặt khác, *Prototype* yêu cầu một initialization operation phức tạp cho object được nhân bản. **Factory Method** dựa trên inheritance nhưng không yêu cầu bước initialization.

- **Factory Method** là một specialization của **Template Method**. Đồng thời, một *Factory Method* có thể đóng vai trò như một bước trong một *Template Method* lớn.
