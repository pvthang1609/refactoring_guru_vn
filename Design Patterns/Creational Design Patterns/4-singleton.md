# Singleton

**Singleton** là một **Creational Design Pattern** đảm bảo rằng một lớp chỉ có một instance duy nhất và cung cấp một điểm truy cập toàn cục đến instance đó.

![Singleton Pattern](https://refactoring.guru/images/patterns/content/singleton/singleton-en.png)

## 🎯 Vấn Đề

**Singleton** pattern giải quyết hai vấn đề cùng một lúc, vi phạm **Single Responsibility Principle**:

1. **Đảm bảo một lớp chỉ có một instance duy nhất**. Tại sao lại có bất kỳ ai muốn kiểm soát số lượng instance một lớp có? Lý do phổ biến nhất là để kiểm soát quyền truy cập vào một số tài nguyên dùng chung, chẳng hạn như cơ sở dữ liệu hoặc tệp.

    Hãy nghĩ về việc bạn sẽ làm thế nào để thực hiện điều này. Làm thế nào để bạn ngăn chặn ai đó tạo ra nhiều hơn một instance của một lớp? Constructor đầu tiên xuất hiện trong tâm trí là constructor mặc định, được công khai. Nhưng bạn có thể tạo một constructor `private` và viết một static creation method gọi constructor private và lưu trữ instance trong một static field. Tất cả các lệnh gọi tiếp theo đến method này trả về instance được lưu trong cache.

2. **Cung cấp một điểm truy cập toàn cục đến instance đó**. Hãy nhớ lại những biến toàn cục mà bạn đã sử dụng để lưu trữ một số đối tượng thiết yếu. Mặc dù chúng rất tiện lợi, nhưng chúng cũng rất không an toàn vì bất kỳ code nào cũng có thể ghi đè lên nội dung của những biến đó và làm hỏng ứng dụng.

    Cũng giống như một biến toàn cục, **Singleton** pattern cho phép bạn truy cập một số đối tượng từ bất kỳ đâu trong chương trình. Tuy nhiên, nó cũng bảo vệ instance đó khỏi bị code khác ghi đè.

## 💡 Giải Pháp

Tất cả các implementations của **Singleton** đều có hai bước chung:

1. **Tạo constructor `private`** để ngăn chặn các toán tử `new` trực tiếp với lớp Singleton.
2. **Tạo một static creation method** hoạt động như một constructor. Method này gọi constructor private để tạo một đối tượng và lưu nó trong một static field. Tất cả các lệnh gọi sau này đến method này đều trả về đối tượng được lưu trong cache.

![Solution](https://refactoring.guru/images/patterns/diagrams/singleton/structure-en.png)
*Singleton đảm bảo chỉ có một instance của lớp tồn tại*

Nếu code của bạn có quyền truy cập vào lớp Singleton, thì nó sẽ có thể gọi static method của Singleton. Do đó, bất cứ khi nào method đó được gọi, cùng một đối tượng luôn được trả về.

## 🏗️ Cấu Trúc

![Structure](https://refactoring.guru/images/patterns/diagrams/singleton/structure.png)

1. **Singleton** lớp khai báo static method `getInstance` trả về cùng một instance của lớp riêng của nó. Constructor của Singleton phải được ẩn khỏi client code. Gọi method `getInstance` phải là cách duy nhất để lấy đối tượng Singleton.

## 📝 Mã Giả

Trong ví dụ này, lớp cơ sở dữ liệu kết nối đóng vai trò là **Singleton**. Lớp này không có constructor public, vì vậy cách duy nhất để lấy instance của nó là gọi method `getInstance`. Method này lưu trữ instance đầu tiên được tạo và trả về nó trong tất cả các lần gọi tiếp theo.

```pseudocode
// Lớp Database định nghĩa method `getInstance` cho phép client
// truy cập cùng một instance của kết nối cơ sở dữ liệu trên toàn cầu
class Database is
    // Trường để lưu trữ instance Singleton phải được khai báo static
    private static field instance: Database

    // Constructor của Singleton phải luôn là private
    // để ngăn chặn các hoạt động xây dựng trực tiếp thông qua toán tử `new`
    private constructor Database() is
        // Mã code khởi tạo, chẳng hạn như kết nối thực tế đến
        // máy chủ cơ sở dữ liệu
        // ...

    // Method creation method tĩnh đóng vai trò là constructor
    // và gọi constructor private để tạo đối tượng và lưu nó
    // trong static field
    public static method getInstance() is
        if (Database.instance == null) then
            // Đảm bảo instance chưa được khởi tạo bởi
            // một thread khác trong khi chờ khóa
            acquireThreadLock() and then
                if (Database.instance == null) then
                    Database.instance = new Database()
        return Database.instance

    // Cuối cùng, bất kỳ Singleton nào cũng nên xác định một số
    // logic nghiệp vụ có thể được thực thi trên instance của nó
    public method query(sql) is
        // Tất cả các truy vấn cơ sở dữ liệu của ứng dụng sẽ
        // đi qua method này. Do đó, bạn có thể đặt logic
        // điều phối hoặc giới hạn tốc độ ở đây
        // ...

class Application is
    method main() is
        Database foo = Database.getInstance()
        foo.query("SELECT ...")
        // ...
        Database bar = Database.getInstance()
        bar.query("SELECT ...")
        // Biến `bar` sẽ chứa cùng một đối tượng như biến `foo`
```

## 🎯 Tính Áp Dụng

**Sử dụng Singleton pattern khi:**

- ✅ **Bạn cần chính xác một instance của một lớp** có sẵn và dễ tiếp cận từ mọi nơi trong ứng dụng
- ✅ **Instance đó cần được kiểm soát chặt chẽ** thông qua việc kế thừa
- ✅ **Bạn muốn tiết kiệm tài nguyên hệ thống** bằng cách tái sử dụng cùng một instance

## 📋 Các Bước Triển Khai

1. **Thêm một trường static private** trong lớp để lưu trữ instance Singleton
2. **Khai báo một public static creation method** để truy cập instance Singleton
3. **Thực hiện "lazy initialization"** bên trong static method - nó nên tạo một đối tượng mới khi được gọi lần đầu và đặt nó vào trường static
4. **Tạo constructor của lớp thành private** - static method của lớp vẫn sẽ có thể gọi constructor, nhưng các đối tượng khác thì không
5. **Xem lại client code** và thay thế tất cả các lệnh gọi trực tiếp đến constructor Singleton bằng lệnh gọi đến static creation method của nó

## ✅ Ưu Điểm và ❌ Nhược Điểm

### ✅ Ưu điểm:
- **🎯 Đảm bảo một lớp chỉ có một instance**
- **🌍 Cung cấp điểm truy cập toàn cục** đến instance đó
- **💾 Instance Singleton chỉ được khởi tạo khi được yêu cầu lần đầu tiên**

### ❌ Nhược điểm:
- **⚡ Vi phạm Single Responsibility Principle** - pattern giải quyết hai vấn đề cùng lúc
- **🐜 Có thể che giấu thiết kế kém** - các thành phần biết quá nhiều về nhau
- **🔒 Yêu cầu xử lý đặc biệt trong môi trường đa luồng**
- **🧪 Khó unit test** client code của Singleton

## 🔗 Mối Quan Hệ Với Các Mẫu Khác

- **Facade** lớp thường có thể được chuyển đổi thành **Singleton** vì một đối tượng facade duy nhất thường là đủ
- **Flyweight** sẽ cho thấy sự tương đồng với **Singleton** nếu bạn bằng cách nào đó giảm số lượng đối tượng được tạo xuống chỉ còn một
- **Abstract Factories, Builders, và Prototypes** thường có thể được triển khai dưới dạng **Singletons**
