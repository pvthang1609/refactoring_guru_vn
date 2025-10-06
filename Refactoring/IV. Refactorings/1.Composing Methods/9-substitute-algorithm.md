# **Thay thế Thuật toán (Substitute Algorithm)**

## **Định Nghĩa**
Kỹ thuật thay thế toàn bộ thuật toán hiện tại bằng một thuật toán mới đơn giản và rõ ràng hơn.

## **Khi Nào Sử Dụng**
- Khi tìm thấy thuật toán đơn giản và rõ ràng hơn để thực hiện cùng công việc
- Khi thuật toán hiện tại quá phức tạp và khó hiểu
- Khi muốn cải thiện hiệu năng hoặc độ chính xác

## **Các Bước Thực Hiện**
1. Phân tích và hiểu rõ thuật toán hiện tại
2. Tìm hoặc phát triển thuật toán mới đơn giản hơn
3. Thay thế thuật toán cũ bằng thuật toán mới
4. Kiểm tra kết quả đầu ra có giống nhau không

## **Ví Dụ**
```java
// ❌ Thuật toán phức tạp
String findPerson(String[] people) {
    for (int i = 0; i < people.length; i++) {
        if (people[i].equals("Don")) {
            return "Don";
        }
        if (people[i].equals("John")) {
            return "John";
        }
        if (people[i].equals("Kent")) {
            return "Kent";
        }
    }
    return "";
}

// ✅ Thuật toán đơn giản hơn
String findPerson(String[] people) {
    List<String> candidates = Arrays.asList("Don", "John", "Kent");
    for (String person : people) {
        if (candidates.contains(person)) {
            return person;
        }
    }
    return "";
}
```

## **Lợi Ích**
- ✅ Code rõ ràng và dễ hiểu hơn
- ✅ Dễ bảo trì và mở rộng
- ✅ Có thể cải thiện hiệu năng
- ✅ Giảm độ phức tạp của code

## **Khi Không Nên Sử Dụng**
- Khi thuật toán mới không rõ ràng hơn thuật toán cũ
- Khi thuật toán cũ đã được tối ưu cho trường hợp cụ thể
- Khi không chắc chắn về tính chính xác của thuật toán mới

## **Kỹ Thuật Liên Quan**
- Trích xuất Phương thức (Extract Method)
- Tái cấu trúc các phương thức phức tạp khác

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
