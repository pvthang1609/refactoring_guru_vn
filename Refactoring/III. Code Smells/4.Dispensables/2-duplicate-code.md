# **Mùi Code: Code Trùng Lặp (Duplicate Code)**

## **Định Nghĩa**
Cùng một đoạn code xuất hiện ở nhiều nơi trong codebase.

## **Nguyên Nhân**
- Copy-paste code mà không tái sử dụng
- Thiếu tổ chức và abstraction
- Phát triển nhanh, ít quan tâm đến chất lượng code

## **Vấn Đề**
- Khó bảo trì: sửa một chỗ phải sửa nhiều chỗ
- Dễ gây lỗi: có thể bỏ sót vị trí cần sửa
- Code phình to, khó đọc hiểu

## **Giải Pháp**
**Trích xuất phương thức (Extract Method)**
- Tách code trùng lặp thành phương thức riêng
- Gọi phương thức đó ở các nơi cần sử dụng

**Trích xuất lớp (Extract Class)**
- Nhóm các phương thức trùng lặp vào lớp chung
- Sử dụng inheritance hoặc composition

## **Ví Dụ**
```java
// ❌ Code trùng lặp
class Order {
    void printInvoice() {
        // ... code tính toán
        // ... code định dạng hóa đơn
    }
    
    void printReceipt() {
        // ... code tính toán (giống trên)
        // ... code định dạng biên lai
    }
}

// ✅ Sau khi tái cấu trúc
class Order {
    void printInvoice() {
        calculateTotal();
        formatInvoice();
    }
    
    void printReceipt() {
        calculateTotal();
        formatReceipt();
    }
    
    private void calculateTotal() {
        // code tính toán chung
    }
}
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Trích xuất phương thức (Extract Method)
- 🔧 Trích xuất lớp (Extract Class)
- 🔧 Hợp nhất phương thức (Consolidate Conditional Expression)

## **Kết Luận**
Code trùng lặp là mùi code phổ biến nhất và dễ nhận biết nhất. Loại bỏ nó giúp code dễ bảo trì, ít lỗi và ngắn gọn hơn.

**Nguồn:** [refactoring.guru/smells/duplicate-code](https://refactoring.guru/smells/duplicate-code)
