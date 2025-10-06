# **Tách Biến tạm (Split Temporary Variable)**

## **Định Nghĩa**
Kỹ thuật tách một biến tạm được gán giá trị nhiều lần thành nhiều biến riêng biệt, mỗi biến chỉ chịu trách nhiệm cho một mục đích duy nhất.

## **Khi Nào Sử Dụng**
- Khi một biến tạm được gán giá trị nhiều lần trong cùng một phương thức
- Khi biến tạm được sử dụng cho nhiều mục đích khác nhau
- Khi biến có tên không rõ ràng, không phản ánh đúng mục đích sử dụng

## **Các Bước Thực Hiện**
1. Tìm biến tạm được gán giá trị nhiều hơn một lần
2. Khai báo biến mới cho mỗi lần gán giá trị sau lần đầu tiên
3. Thay thế tất cả các tham chiếu đến biến cũ bằng biến mới tương ứng
4. Kiểm tra và đặt tên phù hợp cho từng biến mới

## **Ví Dụ**
```java
// ❌ Trước khi tách biến
double temp = 2 * (height + width);
System.out.println("Chu vi: " + temp);
temp = height * width;
System.out.println("Diện tích: " + temp);

// ✅ Sau khi tách biến
double perimeter = 2 * (height + width);
System.out.println("Chu vi: " + perimeter);
double area = height * width;
System.out.println("Diện tích: " + area);
```

## **Lợi Ích**
- ✅ Code dễ đọc và hiểu hơn
- ✅ Mỗi biến có một mục đích rõ ràng
- ✅ Dễ dàng debug và theo dõi giá trị
- ✅ Tránh được lỗi do sử dụng nhầm biến

## **Khi Không Nên Sử Dụng**
- Khi biến tạm chỉ được gán giá trị một lần
- Khi các lần gán giá trị thực sự có cùng mục đích
- Khi việc tách biến làm code phức tạp hơn

## **Kỹ Thuật Liên Quan**
- Trích xuất Biến (Extract Variable)
- Đổi tên Biến (Rename Variable)

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
