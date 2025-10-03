# **Mùi Code: Lớp Quá Lớn (Large Class)**

## **Định Nghĩa**
Một lớp có quá nhiều trường, phương thức và dòng code, đảm nhận quá nhiều trách nhiệm.

## **Dấu Hiệu Nhận Biết**
- Lớp có hàng trăm dòng code
- Chứa quá nhiều trường và phương thức
- Khó đặt tên mô tả chính xác cho lớp
- Có nhóm trường/phương thức không liên quan đến nhau

## **Nguyên Nhân**
- Thêm tính năng mới vào lớp hiện có mà không tái cấu trúc
- Lớp cố gắng xử lý quá nhiều nhiệm vụ khác nhau
- Thiếu phân tách trách nhiệm rõ ràng

## **Giải Pháp**
**Trích xuất lớp (Extract Class)**
- Tách nhóm trường và phương thức liên quan thành lớp mới
- Tạo lớp con cho các nhiệm vụ chuyên biệt

**Trích xuất lớp con (Extract Subclass)**
**Trích xuất interface (Extract Interface)**

## **Ví Dụ**
```java
// ❌ Lớp quá lớn
class Report {
    private String title;
    private String content;
    private String author;
    private Date createdDate;
    // ... nhiều trường khác
    
    public void generatePDF() { ... }
    public void generateHTML() { ... }
    public void generateExcel() { ... }
    public void print() { ... }
    public void saveToDatabase() { ... }
    public void sendEmail() { ... }
    // ... nhiều phương thức khác
}

// ✅ Tách thành các lớp nhỏ
class Report {
    private String title;
    private String content;
    private ReportGenerator generator;
    private ReportExporter exporter;
}

class ReportGenerator { ... }
class ReportExporter { ... }
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Trích xuất lớp (Extract Class)
- 🔧 Trích xuất lớp con (Extract Subclass)
- 🔧 Trích xuất interface (Extract Interface)

## **Lợi Ích**
- ✅ Dễ bảo trì và mở rộng
- ✅ Tái sử dụng code tốt hơn
- ✅ Giảm sự phụ thuộc giữa các thành phần
- ✅ Dễ kiểm thử hơn

## **Kết Luận**
Lớp quá lớn vi phạm nguyên tắc Single Responsibility. Tách lớp lớn thành các lớp nhỏ, chuyên biệt giúp hệ thống linh hoạt và dễ quản lý hơn.
