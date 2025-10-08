# **Trích Xuất Lớp Con (Extract Subclass)**

## **Định Nghĩa**
Trích xuất lớp con là kỹ thuật tái cấu trúc tạo một lớp con mới từ một lớp hiện có, khi một phần các tính năng của lớp cha chỉ được sử dụng trong một số trường hợp cụ thể.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một lớp có các tính năng chỉ được sử dụng trong một số trường hợp cụ thể
- Có các trường, phương thức hoặc điều kiện chỉ liên quan đến một phần đối tượng
- Muốn mô hình hóa các biến thể khác nhau của một khái niệm
- Cần tách biệt các mối quan tâm trong một lớp

### **Không nên sử dụng khi:**
- Tất cả các tính năng của lớp được sử dụng đồng đều trong mọi trường hợp
- Sự khác biệt giữa các trường hợp quá nhỏ để biện minh cho việc tạo lớp con
- Có thể sử dụng composition thay vì inheritance

## **Các Bước Thực Hiện**

1. **Xác định các tính năng cụ thể**: Tìm các trường và phương thức chỉ được sử dụng trong một số trường hợp
2. **Tạo lớp con mới**: Tạo lớp con kế thừa từ lớp cha
3. **Di chuyển tính năng cụ thể**: Di chuyển các trường và phương thức cụ thể xuống lớp con
4. **Thay thế điều kiện**: Thay thế các câu lệnh điều kiện bằng đa hình
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi trích xuất lớp con:**
```java
class Job {
    private String title;
    private double baseSalary;
    private int type; // 1 = Full-time, 2 = Part-time, 3 = Contract
    private double bonus; // Chỉ dùng cho Full-time
    private double hourlyRate; // Chỉ dùng cho Part-time
    private int contractDuration; // Chỉ dùng cho Contract
    
    public Job(String title, double baseSalary, int type) {
        this.title = title;
        this.baseSalary = baseSalary;
        this.type = type;
    }
    
    public double calculatePay() {
        switch (type) {
            case 1: // Full-time
                return baseSalary + bonus;
            case 2: // Part-time
                return hourlyRate * baseSalary; // baseSalary thực tế là số giờ
            case 3: // Contract
                return baseSalary * contractDuration;
            default:
                return baseSalary;
        }
    }
    
    public int getVacationDays() {
        switch (type) {
            case 1: // Full-time
                return 20;
            case 2: // Part-time
                return 10;
            case 3: // Contract
                return 0;
            default:
                return 0;
        }
    }
    
    // Các setter/getter cụ thể cho từng type
    public void setBonus(double bonus) {
        if (type != 1) {
            throw new IllegalStateException("Bonus only available for full-time jobs");
        }
        this.bonus = bonus;
    }
    
    public void setHourlyRate(double hourlyRate) {
        if (type != 2) {
            throw new IllegalStateException("Hourly rate only available for part-time jobs");
        }
        this.hourlyRate = hourlyRate;
    }
    
    public void setContractDuration(int duration) {
        if (type != 3) {
            throw new IllegalStateException("Contract duration only available for contract jobs");
        }
        this.contractDuration = duration;
    }
}
```

### **✅ Sau khi trích xuất lớp con:**
```java
abstract class Job {
    private String title;
    private double baseSalary;
    
    public Job(String title, double baseSalary) {
        this.title = title;
        this.baseSalary = baseSalary;
    }
    
    public abstract double calculatePay();
    public abstract int getVacationDays();
    
    // Các phương thức chung
    public String getTitle() { return title; }
    public double getBaseSalary() { return baseSalary; }
}

class FullTimeJob extends Job {
    private double bonus;
    
    public FullTimeJob(String title, double baseSalary) {
        super(title, baseSalary);
    }
    
    @Override
    public double calculatePay() {
        return getBaseSalary() + bonus;
    }
    
    @Override
    public int getVacationDays() {
        return 20;
    }
    
    public void setBonus(double bonus) {
        this.bonus = bonus;
    }
    
    public double getBonus() { return bonus; }
}

class PartTimeJob extends Job {
    private double hourlyRate;
    
    public PartTimeJob(String title, double hourlyRate) {
        super(title, 0); // baseSalary không dùng cho part-time
        this.hourlyRate = hourlyRate;
    }
    
    @Override
    public double calculatePay() {
        // Giả sử baseSalary lưu số giờ làm việc
        return hourlyRate * getBaseSalary();
    }
    
    @Override
    public int getVacationDays() {
        return 10;
    }
    
    public void setHourlyRate(double hourlyRate) {
        this.hourlyRate = hourlyRate;
    }
    
    public double getHourlyRate() { return hourlyRate; }
}

class ContractJob extends Job {
    private int contractDuration; // tháng
    
    public ContractJob(String title, double monthlyRate, int contractDuration) {
        super(title, monthlyRate);
        this.contractDuration = contractDuration;
    }
    
    @Override
    public double calculatePay() {
        return getBaseSalary() * contractDuration;
    }
    
    @Override
    public int getVacationDays() {
        return 0;
    }
    
    public void setContractDuration(int duration) {
        this.contractDuration = duration;
    }
    
    public int getContractDuration() { return contractDuration; }
}

// Factory để tạo đối tượng phù hợp
class JobFactory {
    public static Job createJob(String type, String title, double rate, int duration) {
        switch (type.toLowerCase()) {
            case "fulltime":
                return new FullTimeJob(title, rate);
            case "parttime":
                return new PartTimeJob(title, rate);
            case "contract":
                return new ContractJob(title, rate, duration);
            default:
                throw new IllegalArgumentException("Unknown job type: " + type);
        }
    }
}
```

## **Lợi Ích**

- **✅ Code rõ ràng hơn**: Mỗi lớp con chỉ chứa logic cụ thể của nó
- **✅ Loại bỏ điều kiện**: Thay thế các câu lệnh switch/case bằng đa hình
- **✅ Dễ mở rộng**: Thêm loại job mới dễ dàng bằng cách tạo lớp con mới
- **✅ Đảm bảo tính đúng đắn**: Không thể set bonus cho contract job
- **✅ Dễ test**: Có thể test từng loại job độc lập

## **Điểm Cần Lưu ý**

- **Không lạm dụng**: Chỉ tạo lớp con khi có sự khác biệt đáng kể về behavior
- **Cân nhắc composition**: Đôi khi sử dụng strategy pattern hoặc composition tốt hơn
- **Xem xét số lượng lớp con**: Quá nhiều lớp con có thể làm hệ thống phức tạp
- **Sử dụng factory**: Dùng factory pattern để che giấu việc tạo đối tượng cụ thể

## **Khi Nào Không Nên Sử Dụng**

- Khi sự khác biệt chỉ là về dữ liệu, không phải behavior
- Khi có quá nhiều biến thể và hệ thống phân cấp trở nên phức tạp
- Khi có thể sử dụng enum hoặc composition đơn giản hơn
- Khi các biến thể thay đổi thường xuyên và động

## **Kỹ Thuật Liên Quan**

- **Extract Superclass**: Trích xuất lớp cha từ các lớp có chung tính năng
- **Replace Conditional with Polymorphism**: Thay thế điều kiện bằng đa hình
- **Replace Type Code with Subclasses**: Thay thế mã type bằng lớp con
- **Introduce Null Object**: Giới thiệu đối tượng null

## **Kết Luận**

Trích xuất lớp con là kỹ thuật mạnh mẽ để xử lý các biến thể khác nhau của một khái niệm. Bằng cách tách các behavior cụ thể vào các lớp con riêng biệt, code trở nên rõ ràng, dễ bảo trì và tuân thủ tốt hơn các nguyên tắc OOP. Tuy nhiên, cần cân nhắc kỹ để không tạo ra hệ thống phân cấp quá phức tạp và xem xét các giải pháp thay thế như composition khi phù hợp.

**Nguồn:** [refactoring.guru/extract-subclass](https://refactoring.guru/extract-subclass)