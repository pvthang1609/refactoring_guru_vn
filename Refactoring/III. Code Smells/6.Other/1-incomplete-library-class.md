# **Mùi Code: Lớp Thư Viện Không Hoàn Chỉnh (Incomplete Library Class)**

## **Định Nghĩa**
Khi một lớp trong thư viện bên ngoài không cung cấp đủ các phương thức bạn cần, nhưng bạn không thể sửa đổi trực tiếp thư viện đó.

## **Dấu Hiệu Nhận Biết**
- Cần thêm phương thức vào lớp thư viện
- Không thể sửa đổi trực tiếp mã nguồn thư viện
- Phải viết code "vá" xung quanh lớp thư viện

## **Giải Pháp**
**Giới thiệu Foreign Method (Introduce Foreign Method)**
- Tạo phương thức mới trong lớp của bạn để xử lý chức năng thiếu

**Giới thiệu Local Extension (Introduce Local Extension)**
- Tạo lớp con hoặc wrapper class để mở rộng chức năng

## **Ví Dụ**
```java
// ❌ Thiếu phương thức trong thư viện
Date startDate = new Date();
// ... sau 1 tháng
Date endDate = new Date();
// Không có phương thức addMonth() trong Date

// ✅ Giới thiệu Foreign Method
class DateUtils {
    public static Date addMonth(Date date, int months) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        cal.add(Calendar.MONTH, months);
        return cal.getTime();
    }
}

Date newDate = DateUtils.addMonth(startDate, 1);
```

## **Kỹ Thuật Tái Cấu Trúc**
- 🔧 Giới thiệu Foreign Method
- 🔧 Giới thiệu Local Extension

## **Kết Luận**
Khi lớp thư viện không đáp ứng đủ nhu cầu, hãy mở rộng nó thông qua Foreign Method hoặc Local Extension thay vì sửa trực tiếp thư viện.

**Nguồn:** [refactoring.guru/smells/long-method](https://refactoring.guru/smells/long-method)
