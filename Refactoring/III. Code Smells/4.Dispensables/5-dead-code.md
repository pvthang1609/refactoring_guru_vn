# **Mùi Code: Code Chết (Dead Code)**

## **Định Nghĩa**
Code không bao giờ được sử dụng hoặc không thể thực thi được.

## **Dấu Hiệu Nhận Biết**
- Biến, phương thức hoặc lớp không được gọi đến
- Code sau câu lệnh return không thể thực thi
- Code bị comment out lâu ngày
- Phương thức luôn trả về một giá trị cố định

## **Nguyên Nhân**
- Tính năng bị loại bỏ nhưng code chưa xóa
- Refactoring không hoàn chỉnh
- Code dự phòng nhưng không dùng đến

## **Vấn Đề**
- Tăng độ phức tạp codebase không cần thiết
- Gây nhầm lẫn cho developer mới
- Tốn thời gian đọc và hiểu
- Ảnh hưởng đến hiệu năng biên dịch

## **Giải Pháp**
**Xóa bỏ hoàn toàn**
- Sử dụng IDE để phát hiện code không dùng
- Xóa biến, phương thức, lớp không được gọi
- Dùng version control để lưu lại nếu cần

## **Ví Dụ**
```java
// ❌ Code chết
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    // Phương thức không bao giờ được gọi
    public int multiply(int a, int b) {
        return a * b;
    }
    
    // Code không thể thực thi
    public int divide(int a, int b) {
        int result = a / b;
        System.out.println("Calculating..."); // Không bao giờ chạy
        return result;
        System.out.println("Done"); // KHÔNG THỂ THỰC THI
    }
}

// ✅ Sau khi dọn dẹp
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

## **Phòng Ngừa**
- Sử dụng static analysis tools
- Review code định kỳ
- Xóa code ngay khi không cần
- Dùng coverage tools để phát hiện

## **Kết Luận**
Code chết làm codebase phình to và khó bảo trì. Hãy dọn dẹp thường xuyên để giữ code sạch sẽ và hiệu quả.

**Nguồn:** [refactoring.guru/smells/dead-code](https://refactoring.guru/smells/dead-code)
