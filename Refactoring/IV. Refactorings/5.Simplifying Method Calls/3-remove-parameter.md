# **Xóa Tham Số (Remove Parameter)**

## **Định Nghĩa**
Xóa tham số là kỹ thuật tái cấu trúc loại bỏ tham số không cần thiết khỏi phương thức, giúp phương thức đơn giản và dễ sử dụng hơn.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Tham số không được sử dụng trong thân phương thức
- Tham số có thể được lấy từ nguồn khác (đối tượng hiện tại, tham số khác)
- Tham số trở nên dư thừa do thay đổi trong logic
- Phương thức có quá nhiều tham số gây khó sử dụng

### **Không nên sử dụng khi:**
- Tham số vẫn cần thiết cho logic của phương thức
- Tham số được sử dụng trong các điều kiện hoặc tính toán
- Xóa tham số sẽ phá vỡ tính đúng đắn của phương thức

## **Các Bước Thực Hiện**

1. **Xác định tham số dư thừa**: Tìm tham số không được sử dụng
2. **Kiểm tra phụ thuộc**: Đảm bảo tham số thực sự không cần thiết
3. **Xóa tham số**: Xóa tham số khỏi khai báo phương thức
4. **Cập nhật lời gọi**: Xóa tham số khỏi tất cả lời gọi phương thức
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi xóa tham số:**
```java
class UserService {
    private UserRepository userRepository;
    
    // Tham số 'isAdmin' không được sử dụng - dư thừa
    public User createUser(String username, String email, String password, boolean isAdmin) {
        // Tham số isAdmin không được sử dụng trong phương thức
        User user = new User(username, email, password);
        return userRepository.save(user);
    }
    
    // Tham số 'currentUserId' có thể lấy từ nguồn khác
    public void updateProfile(String username, String email, Long currentUserId) {
        User user = userRepository.findById(currentUserId);
        user.setUsername(username);
        user.setEmail(email);
        userRepository.save(user);
    }
}

// Client code - phải truyền tham số không cần thiết
UserService service = new UserService();
service.createUser("john", "john@email.com", "password123", false); // isAdmin không cần thiết
service.updateProfile("john_doe", "new_email@email.com", 123L); // currentUserId có thể lấy từ session
```

### **✅ Sau khi xóa tham số:**
```java
class UserService {
    private UserRepository userRepository;
    
    // Đã xóa tham số isAdmin không cần thiết
    public User createUser(String username, String email, String password) {
        User user = new User(username, email, password);
        return userRepository.save(user);
    }
    
    // currentUserId được lấy từ session/context thay vì tham số
    public void updateProfile(String username, String email) {
        Long currentUserId = getCurrentUserIdFromSession(); // Lấy từ nguồn phù hợp
        User user = userRepository.findById(currentUserId);
        user.setUsername(username);
        user.setEmail(email);
        userRepository.save(user);
    }
    
    private Long getCurrentUserIdFromSession() {
        // Logic lấy user ID từ session/context
        return SecurityContext.getCurrentUserId();
    }
}

// Client code - đơn giản hơn
UserService service = new UserService();
service.createUser("john", "john@email.com", "password123");
service.updateProfile("john_doe", "new_email@email.com");
```

## **Lợi Ích**

- **✅ Đơn giản hóa interface**: Phương thức dễ sử dụng hơn
- **✅ Giảm độ phức tạp**: Ít tham số cần quan tâm
- **✅ Code rõ ràng hơn**: Loại bỏ các yếu tố gây nhiễu
- **✅ Dễ bảo trì**: Ít code cần quản lý hơn

## **Điểm Cần Lưu ý**

- **Kiểm tra kỹ**: Đảm bảo tham số thực sự không được sử dụng
- **Cẩn thận với override**: Khi xóa tham số từ phương thức được override
- **Cập nhật tất cả lời gọi**: Đảm bảo không bỏ sót lời gọi nào
- **Xem xét tương thích ngược**: Với API public, có thể cần giữ lại

## **Khi Nào Không Nên Sử Dụng**

- Khi tham số được sử dụng trong các phương thức con hoặc callback
- Khi tham số cần thiết cho tính đúng đắn của phương thức
- Khi phương thức là phần của API public và không thể thay đổi
- Khi có quá nhiều phụ thuộc và việc cập nhật không khả thi

## **Kỹ Thuật Liên Quan**

- **Add Parameter**: Thêm tham số mới khi cần
- **Introduce Parameter Object**: Nhóm các tham số liên quan
- **Preserve Whole Object**: Truyền object thay vì từng thuộc tính
- **Replace Parameter with Method Call**: Thay thế tham số bằng lời gọi phương thức

## **Kết Luận**

Xóa tham số là kỹ thuật quan trọng để giữ cho phương thức đơn giản và dễ sử dụng. Bằng cách loại bỏ các tham số dư thừa, chúng ta làm cho code trở nên rõ ràng và dễ bảo trì hơn. Tuy nhiên, cần thận trọng để đảm bảo rằng tham số thực sự không cần thiết trước khi xóa, và luôn cập nhật tất cả các lời gọi đến phương thức.

**Nguồn:** [refactoring.guru/remove-parameter](https://refactoring.guru/remove-parameter)