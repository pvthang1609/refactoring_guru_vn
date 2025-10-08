# **Thay Thế Ngoại Lệ Bằng Kiểm Tra (Replace Exception with Test)**

## **Định Nghĩa**
Thay thế ngoại lệ bằng kiểm tra là kỹ thuật tái cấu trúc thay thế việc bắt và xử lý ngoại lệ bằng cách kiểm tra điều kiện trước khi thực hiện hành động, khi ngoại lệ được sử dụng để kiểm soát luồng xử lý thông thường thay vì xử lý tình huống bất thường.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Ngoại lệ được sử dụng để kiểm soát luồng xử lý thông thường (flow control)
- Tình huống "lỗi" thực sự là một phần bình thường của nghiệp vụ
- Việc kiểm tra điều kiện đơn giản và rẻ hơn so với xử lý ngoại lệ
- Muốn code dễ đọc và hiệu suất tốt hơn

### **Không nên sử dụng khi:**
- Tình huống thực sự là ngoại lệ và hiếm khi xảy ra
- Việc kiểm tra điều kiện phức tạp hoặc tốn kém
- Cần đảm bảo tính nguyên tử của thao tác
- Lỗi cần được truyền lên nhiều cấp để xử lý

## **Các Bước Thực Hiện**

1. **Xác định ngoại lệ được dùng cho flow control**: Tìm các khối try-catch xử lý tình huống thông thường
2. **Tạo điều kiện kiểm tra**: Tạo điều kiện kiểm tra trước khi thực hiện hành động
3. **Thay thế try-catch**: Thay thế khối try-catch bằng kiểm tra điều kiện
4. **Xử lý cả hai trường hợp**: Xử lý cả trường hợp thành công và thất bại
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi thay thế (dùng ngoại lệ cho flow control):**
```java
class ConfigManager {
    private Map<String, String> config = new HashMap<>();
    
    // Phương thức ném ngoại lệ khi key không tồn tại
    public String getConfigValue(String key) {
        if (!config.containsKey(key)) {
            throw new ConfigNotFoundException("Config key not found: " + key);
        }
        return config.get(key);
    }
    
    // Sử dụng ngoại lệ để kiểm soát luồng
    public String getConfigValueWithDefault(String key, String defaultValue) {
        try {
            return getConfigValue(key);
        } catch (ConfigNotFoundException e) {
            return defaultValue; // Dùng ngoại lệ cho flow control - KHÔNG TỐT
        }
    }
    
    // Cách khác cũng dùng ngoại lệ
    public boolean hasConfig(String key) {
        try {
            getConfigValue(key);
            return true;
        } catch (ConfigNotFoundException e) {
            return false;
        }
    }
}

class DataProcessor {
    private ConfigManager config;
    
    public void process() {
        // Sử dụng ngoại lệ cho flow control thông thường
        try {
            String batchSize = config.getConfigValue("batch.size");
            int size = Integer.parseInt(batchSize);
            processBatch(size);
        } catch (ConfigNotFoundException e) {
            // Đây là tình huống bình thường - dùng default value
            processBatch(100); // default batch size
        } catch (NumberFormatException e) {
            System.out.println("Invalid batch size format");
        }
    }
    
    // Method khác cũng dùng ngoại lệ
    public void initialize() {
        try {
            String timeout = config.getConfigValue("connection.timeout");
            // thiết lập timeout
        } catch (ConfigNotFoundException e) {
            // Không có config - dùng giá trị mặc định
            // Nhưng đây là tình huống phổ biến, không nên dùng exception
        }
    }
    
    private void processBatch(int size) {
        // Xử lý batch
    }
}

// Custom exception
class ConfigNotFoundException extends RuntimeException {
    public ConfigNotFoundException(String message) {
        super(message);
    }
}
```

### **✅ Sau khi thay thế (dùng kiểm tra thay vì ngoại lệ):**
```java
class ConfigManager {
    private Map<String, String> config = new HashMap<>();
    
    // Phương thức trả về Optional thay vì ném ngoại lệ
    public Optional<String> getConfigValue(String key) {
        return Optional.ofNullable(config.get(key));
    }
    
    // Phương thức trả về giá trị với default
    public String getConfigValue(String key, String defaultValue) {
        return config.getOrDefault(key, defaultValue);
    }
    
    // Kiểm tra trực tiếp
    public boolean hasConfig(String key) {
        return config.containsKey(key);
    }
    
    // Phương thức đặc biệt cho các kiểu dữ liệu cụ thể
    public Optional<Integer> getIntConfig(String key) {
        return getConfigValue(key)
            .map(value -> {
                try {
                    return Integer.parseInt(value);
                } catch (NumberFormatException e) {
                    return null; // Không parse được
                }
            });
    }
    
    public int getIntConfig(String key, int defaultValue) {
        return getIntConfig(key).orElse(defaultValue);
    }
}

class DataProcessor {
    private ConfigManager config;
    
    public void process() {
        // Sử dụng kiểm tra thay vì ngoại lệ
        int batchSize = config.getIntConfig("batch.size", 100);
        processBatch(batchSize);
        
        // Hoặc với Optional
        config.getIntConfig("batch.size").ifPresentOrElse(
            size -> processBatch(size),
            () -> processBatch(100) // default
        );
    }
    
    public void initialize() {
        // Sử dụng giá trị mặc định trực tiếp
        int timeout = config.getIntConfig("connection.timeout", 30);
        setupConnection(timeout);
        
        // Kiểm tra điều kiện trước khi sử dụng
        if (config.hasConfig("feature.flag")) {
            String flag = config.getConfigValue("feature.flag");
            enableFeature(flag);
        }
    }
    
    // Method xử lý config phức tạp
    public void loadConfiguration() {
        // Kiểm tra tất cả config cần thiết
        List<String> requiredConfigs = Arrays.asList("db.url", "db.username", "db.password");
        boolean allConfigsPresent = requiredConfigs.stream()
            .allMatch(config::hasConfig);
            
        if (!allConfigsPresent) {
            // Đây mới thực sự là exceptional case
            throw new IllegalStateException("Missing required database configuration");
        }
        
        // Config hợp lệ, tiếp tục xử lý
        String dbUrl = config.getConfigValue("db.url");
        String username = config.getConfigValue("db.username");
        String password = config.getConfigValue("db.password");
        connectToDatabase(dbUrl, username, password);
    }
    
    private void processBatch(int size) {
        // Xử lý batch
    }
    
    private void setupConnection(int timeout) {
        // Thiết lập connection
    }
    
    private void enableFeature(String flag) {
        // Bật feature
    }
    
    private void connectToDatabase(String url, String user, String password) {
        // Kết nối database
    }
}
```

## **Lợi Ích**

- **✅ Hiệu suất tốt hơn**: Tránh chi phí xử lý ngoại lệ
- **✅ Code rõ ràng hơn**: Thể hiện rõ luồng xử lý thông thường
- **✅ Dễ đọc và bảo trì**: Logic nghiệp vụ không bị che khuất bởi xử lý ngoại lệ
- **✅ Phân biệt rõ ràng**: Phân biệt được tình huống thông thường và ngoại lệ thực sự
- **✅ Tận dụng ngôn ngữ**: Sử dụng các tính năng như Optional (Java), null checking, etc.

## **Điểm Cần Lưu ý**

- **Phân biệt tình huống**: Chỉ áp dụng cho tình huống thực sự là "bình thường"
- **Không lạm dụng**: Vẫn dùng ngoại lệ cho các tình huống thực sự bất thường
- **Cân nhắc hiệu suất**: Chỉ khi việc kiểm tra thực sự rẻ hơn xử lý ngoại lệ
- **Giữ tính rõ ràng**: Đảm bảo code vẫn dễ đọc sau khi thay đổi

## **Khi Nào Không Nên Sử Dụng**

- Khi tình huống thực sự hiếm và bất thường (true exceptions)
- Khi việc kiểm tra điều kiện phức tạp hoặc tốn kém
- Khi cần đảm bảo tính nguyên tử (atomicity) của thao tác
- Khi lỗi cần được truyền qua nhiều lớp để xử lý

## **Kỹ Thuật Liên Quan**

- **Introduce Null Object**: Giới thiệu đối tượng null
- **Replace Error Code with Exception**: Thay thế mã lỗi bằng ngoại lệ
- **Introduce Special Case**: Giới thiệu trường hợp đặc biệt
- **Use Optional**: Sử dụng Optional để biểu diễn giá trị có thể không tồn tại

## **Kết Luận**

Thay thế ngoại lệ bằng kiểm tra là kỹ thuật quan trọng để phân biệt rõ ràng giữa luồng xử lý thông thường và tình huống ngoại lệ thực sự. Bằng cách sử dụng kiểm tra điều kiện cho các tình huống phổ biến và dự kiến, code trở nên hiệu quả hơn, dễ đọc hơn và thể hiện rõ ý định của người viết. Tuy nhiên, cần phân biệt rõ ràng khi nào một tình huống là "bình thường" (nên dùng kiểm tra) và khi nào là "ngoại lệ" (nên dùng exception).

**Nguồn:** [refactoring.guru/replace-exception-with-test](https://refactoring.guru/replace-exception-with-test)