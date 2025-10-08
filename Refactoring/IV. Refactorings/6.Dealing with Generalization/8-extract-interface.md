# **Trích Xuất Interface (Extract Interface)**

## **Định Nghĩa**
Trích xuất interface là kỹ thuật tái cấu trúc tạo một interface mới từ một tập các phương thức trong một lớp, cho phép nhiều lớp khác nhau có thể triển khai cùng một interface mà không cần chia sẻ chung hệ thống phân cấp kế thừa.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Một số lớp có chung behavior nhưng không chia sẻ chung lớp cha
- Client code chỉ phụ thuộc vào một phần interface của lớp
- Muốn tách biệt interface khỏi implementation
- Cần hỗ trợ multiple implementations cho cùng một contract

### **Không nên sử dụng khi:**
- Chỉ có một implementation của behavior
- Các lớp đã có chung hệ thống phân cấp phù hợp
- Interface quá nhỏ hoặc không có ý nghĩa

## **Các Bước Thực Hiện**

1. **Xác định các phương thức cần trích xuất**: Tìm các phương thức mà client code sử dụng
2. **Tạo interface mới**: Định nghĩa interface với các phương thức đã chọn
3. **Triển khai interface**: Cho lớp hiện có triển khai interface mới
4. **Thay thế kiểu tham chiếu**: Thay thế kiểu cụ thể bằng interface trong client code
5. **Kiểm thử**: Chạy test để đảm bảo không có lỗi

## **Ví Dụ Minh Họa**

### **❌ Trước khi trích xuất interface:**
```java
class EmailService {
    private String smtpServer;
    private int port;
    
    public EmailService(String smtpServer, int port) {
        this.smtpServer = smtpServer;
        this.port = port;
    }
    
    public void sendEmail(String to, String subject, String body) {
        // Logic gửi email thông qua SMTP
        System.out.println("Sending email via SMTP to: " + to);
    }
    
    public void connect() {
        // Logic kết nối đến SMTP server
    }
    
    public void disconnect() {
        // Logic ngắt kết nối
    }
    
    // Các phương thức cụ thể khác của EmailService
}

class SmsService {
    private String apiKey;
    private String gatewayUrl;
    
    public SmsService(String apiKey, String gatewayUrl) {
        this.apiKey = apiKey;
        this.gatewayUrl = gatewayUrl;
    }
    
    public void sendSms(String phoneNumber, String message) {
        // Logic gửi SMS thông qua gateway
        System.out.println("Sending SMS to: " + phoneNumber);
    }
    
    public void authenticate() {
        // Logic xác thực với SMS gateway
    }
    
    // Các phương thức cụ thể khác của SmsService
}

class PushNotificationService {
    private String firebaseKey;
    
    public PushNotificationService(String firebaseKey) {
        this.firebaseKey = firebaseKey;
    }
    
    public void sendPush(String deviceToken, String title, String message) {
        // Logic gửi push notification
        System.out.println("Sending push to device: " + deviceToken);
    }
    
    public void initialize() {
        // Logic khởi tạo push service
    }
}

// Client code phụ thuộc trực tiếp vào các lớp cụ thể
class NotificationManager {
    private EmailService emailService;
    private SmsService smsService;
    private PushNotificationService pushService;
    
    public NotificationManager(EmailService emailService, 
                              SmsService smsService,
                              PushNotificationService pushService) {
        this.emailService = emailService;
        this.smsService = smsService;
        this.pushService = pushService;
    }
    
    public void notifyUser(String userId, String message) {
        // Phải biết cụ thể từng service
        emailService.sendEmail(userId + "@company.com", "Notification", message);
        smsService.sendSms(getUserPhone(userId), message);
        pushService.sendPush(getUserDeviceToken(userId), "Notification", message);
    }
    
    private String getUserPhone(String userId) {
        // Logic lấy số điện thoại
        return "+1234567890";
    }
    
    private String getUserDeviceToken(String userId) {
        // Logic lấy device token
        return "device_token_123";
    }
}
```

### **✅ Sau khi trích xuất interface:**
```java
// Interface chung cho tất cả các dịch vụ thông báo
interface NotificationService {
    void send(String recipient, String title, String message);
    String getServiceType();
}

class EmailService implements NotificationService {
    private String smtpServer;
    private int port;
    
    public EmailService(String smtpServer, int port) {
        this.smtpServer = smtpServer;
        this.port = port;
    }
    
    @Override
    public void send(String recipient, String title, String message) {
        // Logic gửi email thông qua SMTP
        System.out.println("Sending email to: " + recipient + " with title: " + title);
    }
    
    @Override
    public String getServiceType() {
        return "EMAIL";
    }
    
    // Các phương thức cụ thể khác
    public void connect() {
        // Logic kết nối đến SMTP server
    }
}

class SmsService implements NotificationService {
    private String apiKey;
    private String gatewayUrl;
    
    public SmsService(String apiKey, String gatewayUrl) {
        this.apiKey = apiKey;
        this.gatewayUrl = gatewayUrl;
    }
    
    @Override
    public void send(String recipient, String title, String message) {
        // Logic gửi SMS thông qua gateway
        System.out.println("Sending SMS to: " + recipient + " with message: " + message);
    }
    
    @Override
    public String getServiceType() {
        return "SMS";
    }
    
    // Các phương thức cụ thể khác
    public void authenticate() {
        // Logic xác thực với SMS gateway
    }
}

class PushNotificationService implements NotificationService {
    private String firebaseKey;
    
    public PushNotificationService(String firebaseKey) {
        this.firebaseKey = firebaseKey;
    }
    
    @Override
    public void send(String recipient, String title, String message) {
        // Logic gửi push notification
        System.out.println("Sending push to device: " + recipient + " with title: " + title);
    }
    
    @Override
    public String getServiceType() {
        return "PUSH";
    }
    
    // Các phương thức cụ thể khác
    public void initialize() {
        // Logic khởi tạo push service
    }
}

// Client code chỉ phụ thuộc vào interface
class NotificationManager {
    private List<NotificationService> notificationServices;
    
    public NotificationManager(List<NotificationService> services) {
        this.notificationServices = new ArrayList<>(services);
    }
    
    public void addNotificationService(NotificationService service) {
        notificationServices.add(service);
    }
    
    public void notifyUser(String userId, String message) {
        for (NotificationService service : notificationServices) {
            String recipient = getRecipientForService(userId, service.getServiceType());
            service.send(recipient, "Notification", message);
        }
    }
    
    public void notifyWithPreferences(String userId, String message, Set<String> preferredServices) {
        for (NotificationService service : notificationServices) {
            if (preferredServices.contains(service.getServiceType())) {
                String recipient = getRecipientForService(userId, service.getServiceType());
                service.send(recipient, "Notification", message);
            }
        }
    }
    
    private String getRecipientForService(String userId, String serviceType) {
        switch (serviceType) {
            case "EMAIL":
                return userId + "@company.com";
            case "SMS":
                return getUserPhone(userId);
            case "PUSH":
                return getUserDeviceToken(userId);
            default:
                throw new IllegalArgumentException("Unknown service type: " + serviceType);
        }
    }
    
    private String getUserPhone(String userId) {
        return "+1234567890";
    }
    
    private String getUserDeviceToken(String userId) {
        return "device_token_123";
    }
}

// Có thể dễ dàng thêm dịch vụ mới
class SlackNotificationService implements NotificationService {
    private String webhookUrl;
    
    public SlackNotificationService(String webhookUrl) {
        this.webhookUrl = webhookUrl;
    }
    
    @Override
    public void send(String recipient, String title, String message) {
        // Logic gửi thông báo Slack
        System.out.println("Sending Slack message to channel: " + recipient);
    }
    
    @Override
    public String getServiceType() {
        return "SLACK";
    }
}

// Sử dụng
public class Application {
    public static void main(String[] args) {
        List<NotificationService> services = Arrays.asList(
            new EmailService("smtp.company.com", 587),
            new SmsService("api-key-123", "https://sms-gateway.com"),
            new PushNotificationService("firebase-key-456"),
            new SlackNotificationService("https://hooks.slack.com/services/...")
        );
        
        NotificationManager manager = new NotificationManager(services);
        
        // Gửi thông báo qua tất cả dịch vụ
        manager.notifyUser("user123", "Your order has been shipped!");
        
        // Gửi thông báo chỉ qua các dịch vụ được chọn
        Set<String> preferredServices = new HashSet<>(Arrays.asList("EMAIL", "SLACK"));
        manager.notifyWithPreferences("user123", "Meeting reminder", preferredServices);
    }
}
```

## **Lợi Ích**

- **✅ Giảm sự phụ thuộc**: Client code chỉ phụ thuộc vào interface, không phụ thuộc vào implementation cụ thể
- **✅ Linh hoạt hơn**: Dễ dàng thêm các implementation mới
- **✅ Dễ test**: Có thể tạo mock implementations cho testing
- **✅ Tách biệt concerns**: Interface và implementation được tách biệt rõ ràng
- **✅ Hỗ trợ multiple implementations**: Nhiều lớp có thể triển khai cùng interface

## **Điểm Cần Lưu ý**

- **Đặt tên interface có ý nghĩa**: Tên interface nên thể hiện capability hoặc role
- **Giữ interface nhỏ gọn**: Interface nên tập trung vào một responsibility
- **Sử dụng nguyên lý ISP**: Tách interface lớn thành nhiều interface nhỏ
- **Cân nhắc default methods**: Trong Java 8+, có thể sử dụng default methods cho behavior chung

## **Khi Nào Không Nên Sử Dụng**

- Khi chỉ có một implementation và không có kế hoạch thay đổi
- Khi các lớp đã có hệ thống phân cấp kế thừa phù hợp
- Khi interface quá nhỏ và không có giá trị
- Khi làm code trở nên phức tạp không cần thiết

## **Kỹ Thuật Liên Quan**

- **Extract Superclass**: Trích xuất lớp cha từ các lớp có chung tính năng
- **Extract Subclass**: Trích xuất lớp con từ lớp hiện có
- **Introduce Parameter Object**: Giới thiệu đối tượng tham số
- **Replace Inheritance with Delegation**: Thay thế kế thừa bằng ủy quyền

## **Kết Luận**

Trích xuất interface là kỹ thuật mạnh mẽ để tạo ra sự linh hoạt và giảm sự phụ thuộc trong hệ thống. Bằng cách định nghĩa một contract chung mà nhiều lớp có thể triển khai, chúng ta tạo ra code dễ bảo trì, dễ test và dễ mở rộng. Tuy nhiên, cần cân nhắc sử dụng hợp lý để không tạo ra quá nhiều interface không cần thiết và đảm bảo mỗi interface có ý nghĩa rõ ràng.

**Nguồn:** [refactoring.guru/extract-interface](https://refactoring.guru/extract-interface)