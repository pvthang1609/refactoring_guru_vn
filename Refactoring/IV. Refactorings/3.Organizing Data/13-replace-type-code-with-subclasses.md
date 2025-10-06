# **Thay Thế Mã Kiểu Bằng Lớp Con (Replace Type Code with Subclasses)**

## **Vấn Đề**
Bạn có một mã kiểu ảnh hưởng trực tiếp đến hành vi của chương trình (các giá trị của kiểu này kích hoạt code khác nhau trong các điều kiện).

## **Giải Pháp**
Tạo các lớp con cho từng giá trị của mã kiểu. Sau đó trích xuất các hành vi liên quan từ lớp gốc sang các lớp con này. Thay thế code điều khiển luồng bằng đa hình.

## **Tại Sao Cần Tái Cấu Trúc**
Kỹ thuật tái cấu trúc này là một bước tiến tới việc thay thế các điều kiện bằng đa hình.

Một trong những lý do phổ biến nhất cho việc tái cấu trúc này là nhu cầu hỗ trợ một giá trị mới của mã kiểu, điều này yêu cầu bạn phải tìm và sao chép tất cả các điều kiện và sau đó thêm một điều kiện mới vào chúng. Việc này được đơn giản hóa rất nhiều bằng cách tạo một lớp con và di chuyển hành vi thích hợp đến đó.

## **Lợi Ích**
- Cải thiện tổ chức code. Code xử lý các kiểu khác nhau của một đối tượng cụ thể được phân phối trên nhiều lớp, thay vì bị mắc kẹt trong một lớp.
- Dễ dàng thêm các kiểu mới mà không thay đổi code hiện có (xem Nguyên tắc Mở/Đóng).
- Đơn giản hóa code bằng cách loại bỏ các điều kiện và sự trùng lặp.

## **Khi Nào Không Sử Dụng**
Nếu bạn có một mã kiểu đơn giản không thay đổi và không ảnh hưởng đến hành vi, bạn có thể thay thế nó bằng enum hoặc sử dụng kỹ thuật Thay thế Mã Kiểu bằng Lớp.

Nếu các giá trị mã kiểu thay đổi hoặc nếu bạn cần thêm các giá trị mới thường xuyên, và bạn không thể tạo lớp con cho chúng (do hệ thống phân cấp lớp hiện có hoặc vì các lý do khác), hãy sử dụng kỹ thuật Thay thế Mã Kiểu bằng State/Strategy.

## **Cách Tái Cấu Trúc**
1. Sử dụng Tự Đóng gói Trường để tạo getter cho trường mã kiểu.
2. Tạo constructor của lớp cha là private. Tạo một phương thức factory tĩnh với các tham số giống như constructor của lớp cha.
3. Tạo một lớp con duy nhất cho mỗi giá trị mã kiểu. Ghi đè getter của mã kiểu trong lớp con để trả về giá trị thích hợp.
4. Xóa trường mã kiểu khỏi lớp cha. Tạo getter của nó là abstract.
5. Bây giờ bạn đã có các lớp con, bạn có thể bắt đầu di chuyển các trường và phương thức từ lớp cha sang các lớp con thích hợp (sử dụng Đẩy xuống Trường và Đẩy xuống Phương thức).
6. Khi mọi thứ có thể di chuyển đã được di chuyển, sử dụng Thay thế Điều kiện bằng Đa hình để loại bỏ các điều kiện sử dụng mã kiểu.

## **Ví Dụ**

### **❌ Trước khi tái cấu trúc:**
```java
class Employee {
    public static final int ENGINEER = 0;
    public static final int SALESMAN = 1;
    public static final int MANAGER = 2;

    private int type;
    private int monthlySalary;
    private int commission;
    private int bonus;

    public Employee(int type) {
        this.type = type;
    }

    public int getType() {
        return type;
    }

    public int payAmount() {
        switch (type) {
            case ENGINEER:
                return monthlySalary;
            case SALESMAN:
                return monthlySalary + commission;
            case MANAGER:
                return monthlySalary + bonus;
            default:
                throw new RuntimeException("Invalid employee type");
        }
    }
}
```

### **✅ Sau khi tái cấu trúc:**
```java
abstract class Employee {
    private int monthlySalary;

    public Employee(int monthlySalary) {
        this.monthlySalary = monthlySalary;
    }

    public abstract int getType();
    public abstract int payAmount();

    protected int getMonthlySalary() {
        return monthlySalary;
    }
}

class Engineer extends Employee {
    public Engineer(int monthlySalary) {
        super(monthlySalary);
    }

    @Override
    public int getType() {
        return ENGINEER;
    }

    @Override
    public int payAmount() {
        return getMonthlySalary();
    }
}

class Salesman extends Employee {
    private int commission;

    public Salesman(int monthlySalary, int commission) {
        super(monthlySalary);
        this.commission = commission;
    }

    @Override
    public int getType() {
        return SALESMAN;
    }

    @Override
    public int payAmount() {
        return getMonthlySalary() + commission;
    }
}

class Manager extends Employee {
    private int bonus;

    public Manager(int monthlySalary, int bonus) {
        super(monthlySalary);
        this.bonus = bonus;
    }

    @Override
    public int getType() {
        return MANAGER;
    }

    @Override
    public int payAmount() {
        return getMonthlySalary() + bonus;
    }
}
```

Bây giờ, chúng ta có thể sử dụng một phương thức factory để tạo lớp con nhân viên thích hợp dựa trên mã kiểu.

```java
class EmployeeFactory {
    public static Employee createEmployee(int type, int monthlySalary, int commission, int bonus) {
        switch (type) {
            case ENGINEER:
                return new Engineer(monthlySalary);
            case SALESMAN:
                return new Salesman(monthlySalary, commission);
            case MANAGER:
                return new Manager(monthlySalary, bonus);
            default:
                throw new IllegalArgumentException("Invalid employee type");
        }
    }
}
```

Việc tái cấu trúc này cho phép chúng ta loại bỏ các điều kiện và sử dụng đa hình để xử lý các hành vi khác nhau cho từng loại nhân viên.