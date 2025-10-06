# **Loại bỏ Gán Giá trị cho Tham số (Remove Assignments to Parameters)**

## **Định Nghĩa**
Kỹ thuật ngăn chặn việc gán giá trị mới cho các tham số của phương thức, thay vào đó sử dụng biến tạm.

## **Khi Nào Sử Dụng**
- Khi có mã gán giá trị mới cho tham số đầu vào
- Khi muốn tránh nhầm lẫn về việc tham số có bị thay đổi hay không
- Khi cần làm rõ những gì được sử dụng như dữ liệu đầu vào và đầu ra

## **Các Bước Thực Hiện**
1. Tạo biến tạm mới và gán giá trị của tham số vào đó
2. Thay thế tất cả các tham chiếu đến tham số bằng biến tạm
3. Thay thế các phép gán cho tham số bằng phép gán cho biến tạm

## **Ví Dụ**
```java
// ❌ Trước khi loại bỏ
public int discount(int inputVal, int quantity) {
    if (inputVal > 50) {
        inputVal -= 2;  // Gán giá trị mới cho tham số
    }
    // ... logic khác sử dụng inputVal
    return inputVal;
}

// ✅ Sau khi loại bỏ
public int discount(int inputVal, int quantity) {
    int result = inputVal;  // Sử dụng biến tạm
    if (inputVal > 50) {
        result -= 2;       // Thay đổi biến tạm thay vì tham số
    }
    // ... logic khác sử dụng result
    return result;
}
```

## **Lợi Ích**
- ✅ Tránh nhầm lẫn về việc tham số có bị thay đổi hay không
- ✅ Code rõ ràng hơn, dễ đọc và bảo trì
- ✅ Tránh các lỗi khó phát hiện do thay đổi tham số
- ✅ Phù hợp với nguyên tắc "tham số chỉ nên là đầu vào"

## **Khi Không Nên Sử Dụng**
- Khi sử dụng các ngôn ngữ chỉ truyền tham chiếu (pass-by-reference)
- Khi việc gán giá trị cho tham số là rõ ràng và được mong đợi
- Trong các phương thức setter hoặc constructor

## **Kỹ Thuật Liên Quan**
- Tách Biến tạm (Split Temporary Variable)
- Trích xuất Phương thức (Extract Method)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
