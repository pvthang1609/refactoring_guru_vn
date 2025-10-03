# **Mùi Code: Comment (Comments)**

## **Định Nghĩa**
Khi comment được sử dụng để giải thích code phức tạp thay vì làm cho code tự giải thích.

## **Khi Nào Comment Trở Thành Mùi Code?**
- Comment giải thích "code làm gì" thay vì "tại sao"
- Comment cho code phức tạp, khó hiểu
- Comment trở nên lỗi thời, không còn đúng với code hiện tại

## **Giải Pháp**
**Làm code tự giải thích**
- Đổi tên phương thức và biến cho rõ nghĩa
- Trích xuất phương thức (Extract Method)
- Giới thiệu hằng số giải thích (Introduce Explaining Variable)

## **Ví Dụ**
```java
// ❌ Comment giải thích code phức tạp
// Kiểm tra nếu nhân viên đủ điều kiện thưởng
if (employee.age > 25 && employee.yearsOfService > 3 && employee.lastYearPerformance > 8.5) {
    giveBonus();
}

// ✅ Code tự giải thích
boolean isEligibleForBonus = employee.isEligibleForBonus();
if (isEligibleForBonus) {
    giveBonus();
}

// Trong class Employee:
public boolean isEligibleForBonus() {
    return age > MIN_BONUS_AGE && 
           yearsOfService > MIN_SERVICE_YEARS && 
           lastYearPerformance > MIN_PERFORMANCE_SCORE;
}
```

## **Comment Tốt vs Comment Xấu**
**✅ Comment tốt:**
- Giải thích "tại sao" thay vì "cái gì"
- Cảnh báo về hậu quả không rõ ràng
- API documentation

**❌ Comment xấu:**
- Lặp lại điều code đã thể hiện
- Giải thích code phức tạp
- Đã lỗi thời so với code

## **Kết Luận**
Comment không phải lúc nào cũng xấu, nhưng thường là dấu hiệu của code không rõ ràng. Ưu tiên làm code tự giải thích thay vì dựa vào comment.

**Nguồn:** [refactoring.guru/smells/comments](https://refactoring.guru/smells/comments)
