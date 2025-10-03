# **Mùi Code: Thay Đổi Phân Kỳ (Divergent Change)**

## **Định Nghĩa**
Khi một lớp thường xuyên bị sửa đổi vì nhiều lý do khác nhau, mỗi lý do lại thuộc về một nhóm chức năng khác biệt.

## **Dấu Hiệu Nhận Biết**
- Một lớp bị thay đổi vì nhiều lý do không liên quan
- Mỗi khi yêu cầu thay đổi, bạn phải sửa cùng một lớp
- Lớp có nhiều trách nhiệm khác nhau

## **Nguyên Nhân**
Lớp vi phạm nguyên tắc **Single Responsibility Principle** - một lớp đảm nhận quá nhiều trách nhiệm.

## **Giải Pháp**
**Trích xuất lớp (Extract Class)**
- Tách các chức năng không liên quan thành các lớp riêng biệt
- Mỗi lớp chỉ nên có một lý do để thay đổi

## **Ví Dụ**
```java
// ❌ Lớp có nhiều trách nhiệm
class Report {
    void calculateData() { ... }
    void formatHTML() { ... }
    void saveToFile() { ... }
}

// ✅ Tách thành các lớp chuyên biệt
class DataCalculator { ... }
class HTMLFormatter { ... }
class FileSaver { ... }
```

## **Lợi Ích**
- Giảm số lần sửa đổi cho mỗi lớp
- Code dễ bảo trì và mở rộng hơn
- Tuân thủ nguyên tắc Single Responsibility

**Nguồn:** [refactoring.guru/smells/divergent-change](https://refactoring.guru/smells/divergent-change)
