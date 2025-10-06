# **Nhân Bản Dữ Liệu Quan Sát (Duplicate Observed Data)**

## **Định Nghĩa**
Nhân bản dữ liệu quan sát là kỹ thuật tái cấu trúc tách dữ liệu khỏi các thành phần GUI và đặt nó vào lớp domain, đồng thời thiết lập cơ chế Observer để đồng bộ hóa dữ liệu giữa hai lớp.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Dữ liệu nghiệp vụ bị mắc kẹt trong các thành phần GUI
- Cần sử dụng cùng dữ liệu trong nhiều cửa sổ hoặc view khác nhau
- Muốn tách biệt logic nghiệp vụ khỏi giao diện người dùng
- Cần test logic nghiệp vụ mà không phụ thuộc vào GUI

### **Dấu hiệu nhận biết:**
- Dữ liệu được lưu trữ trực tiếp trong các control của GUI (text field, combo box)
- Không thể sử dụng dữ liệu nghiệp vụ khi không có GUI
- Logic nghiệp vụ bị trộn lẫn với code xử lý sự kiện GUI

## **Các Bước Thực Hiện**

1. **Tạo lớp domain**: Tạo lớp chứa dữ liệu và logic nghiệp vụ
2. **Triển khai Observer**: Thêm cơ chế quan sát để theo dõi thay đổi
3. **Kết nối GUI với domain**: Liên kết các thành phần GUI với dữ liệu domain
4. **Chuyển logic nghiệp vụ**: Di chuyển logic từ GUI sang lớp domain

## **Ví Dụ Minh Họa**

### **❌ Trước khi tái cấu trúc:**
```java
// Dữ liệu và logic bị mắc kẹt trong GUI
class PersonWindow extends JFrame {
    private JTextField nameField = new JTextField();
    private JTextField ageField = new JTextField();
    
    public PersonWindow() {
        // Khởi tạo GUI...
        addSaveButtonListener();
    }
    
    private void addSaveButtonListener() {
        JButton saveButton = new JButton("Save");
        saveButton.addActionListener(e -> savePerson());
    }
    
    private void savePerson() {
        // Logic nghiệp vụ bên trong GUI
        String name = nameField.getText();
        String ageText = ageField.getText();
        
        // Validation trong GUI
        if (name.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Tên không được để trống");
            return;
        }
        
        int age;
        try {
            age = Integer.parseInt(ageText);
            if (age < 0 || age > 150) {
                throw new NumberFormatException();
            }
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Tuổi không hợp lệ");
            return;
        }
        
        // Lưu dữ liệu - không thể tái sử dụng logic này
        saveToDatabase(name, age);
    }
    
    private void saveToDatabase(String name, int age) {
        // Logic lưu dữ liệu
    }
}
```

### **✅ Sau khi tái cấu trúc:**
```java
// Lớp domain chứa dữ liệu và logic nghiệp vụ
class Person {
    private String name;
    private int age;
    private List<PersonListener> listeners = new ArrayList<>();
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Getter và setter với notification
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Tên không được để trống");
        }
        this.name = name;
        notifyListeners();
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("Tuổi không hợp lệ");
        }
        this.age = age;
        notifyListeners();
    }
    
    // Observer pattern
    public void addListener(PersonListener listener) {
        listeners.add(listener);
    }
    
    public void removeListener(PersonListener listener) {
        listeners.remove(listener);
    }
    
    private void notifyListeners() {
        for (PersonListener listener : listeners) {
            listener.personChanged(this);
        }
    }
    
    // Logic nghiệp vụ
    public void save() {
        // Logic lưu dữ liệu
        saveToDatabase();
    }
    
    private void saveToDatabase() {
        // Logic lưu vào database
    }
}

// Interface cho observer
interface PersonListener {
    void personChanged(Person person);
}

// Lớp GUI chỉ chịu trách nhiệm hiển thị
class PersonWindow extends JFrame implements PersonListener {
    private JTextField nameField = new JTextField();
    private JTextField ageField = new JTextField();
    private Person person;
    
    public PersonWindow(Person person) {
        this.person = person;
        this.person.addListener(this);
        
        initializeGUI();
        updateFieldsFromPerson();
    }
    
    private void initializeGUI() {
        // Khởi tạo GUI components
        addSaveButtonListener();
        addFieldListeners();
    }
    
    private void addSaveButtonListener() {
        JButton saveButton = new JButton("Save");
        saveButton.addActionListener(e -> savePerson());
    }
    
    private void addFieldListeners() {
        nameField.addActionListener(e -> updatePersonFromFields());
        ageField.addActionListener(e -> updatePersonFromFields());
    }
    
    private void updatePersonFromFields() {
        try {
            person.setName(nameField.getText());
            person.setAge(Integer.parseInt(ageField.getText()));
        } catch (IllegalArgumentException ex) {
            JOptionPane.showMessageDialog(this, ex.getMessage());
        }
    }
    
    private void updateFieldsFromPerson() {
        nameField.setText(person.getName());
        ageField.setText(String.valueOf(person.getAge()));
    }
    
    private void savePerson() {
        try {
            person.save();
            JOptionPane.showMessageDialog(this, "Lưu thành công");
        } catch (Exception ex) {
            JOptionPane.showMessageDialog(this, "Lỗi khi lưu: " + ex.getMessage());
        }
    }
    
    // Implement PersonListener
    @Override
    public void personChanged(Person person) {
        updateFieldsFromPerson();
    }
}
```

## **Ví Dụ Phức Tạp Hơn**

### **✅ Với nhiều trường dữ liệu:**
```java
// Lớp domain phức tạp hơn
class Order {
    private String orderId;
    private Customer customer;
    private List<OrderItem> items;
    private OrderStatus status;
    private List<OrderListener> listeners = new ArrayList<>();
    
    public Order(String orderId, Customer customer) {
        this.orderId = orderId;
        this.customer = customer;
        this.items = new ArrayList<>();
        this.status = OrderStatus.PENDING;
    }
    
    // Business logic
    public void addItem(Product product, int quantity) {
        if (status != OrderStatus.PENDING) {
            throw new IllegalStateException("Không thể thêm item vào đơn hàng đã xác nhận");
        }
        
        OrderItem item = new OrderItem(product, quantity);
        items.add(item);
        notifyListeners();
    }
    
    public void confirm() {
        if (items.isEmpty()) {
            throw new IllegalStateException("Không thể xác nhận đơn hàng trống");
        }
        this.status = OrderStatus.CONFIRMED;
        notifyListeners();
    }
    
    public double getTotalAmount() {
        return items.stream()
            .mapToDouble(OrderItem::getSubtotal)
            .sum();
    }
    
    // Observer pattern
    public void addListener(OrderListener listener) {
        listeners.add(listener);
    }
    
    private void notifyListeners() {
        for (OrderListener listener : listeners) {
            listener.orderChanged(this);
        }
    }
    
    // Getters
    public String getOrderId() { return orderId; }
    public Customer getCustomer() { return customer; }
    public List<OrderItem> getItems() { return Collections.unmodifiableList(items); }
    public OrderStatus getStatus() { return status; }
}

// GUI class
class OrderWindow extends JFrame implements OrderListener {
    private Order order;
    private JTable itemsTable;
    private JLabel totalLabel;
    private JComboBox<OrderStatus> statusCombo;
    
    public OrderWindow(Order order) {
        this.order = order;
        this.order.addListener(this);
        
        initializeGUI();
        updateFromOrder();
    }
    
    private void initializeGUI() {
        // Khởi tạo các thành phần GUI
        setupItemsTable();
        setupStatusCombo();
        setupActionButtons();
    }
    
    private void updateFromOrder() {
        // Cập nhật GUI từ đối tượng Order
        updateItemsTable();
        updateTotalLabel();
        updateStatusCombo();
    }
    
    @Override
    public void orderChanged(Order order) {
        updateFromOrder();
    }
}
```

## **Lợi Ích**

- **✅ Tách biệt concerns**: Logic nghiệp vụ tách rời khỏi GUI
- **✅ Tái sử dụng**: Có thể sử dụng lớp domain trong nhiều GUI khác nhau
- **✅ Dễ test**: Có thể test logic nghiệp vụ mà không cần GUI
- **✅ Đồng bộ hóa**: Dễ dàng đồng bộ nhiều view với cùng dữ liệu
- **✅ Bảo trì dễ dàng**: Thay đổi GUI không ảnh hưởng đến logic nghiệp vụ

## **Điểm Cần Lưu Ý**

- **Độ phức tạp**: Tăng độ phức tạp do thêm Observer pattern
- **Performance**: Cần cân nhắc khi có quá nhiều listener
- **Memory management**: Đảm bảo remove listener khi không cần thiết

## **Khi Nào Không Nên Sử Dụng**

- Ứng dụng nhỏ, đơn giản không cần tách biệt GUI và logic
- Khi dữ liệu chỉ được sử dụng trong một GUI duy nhất
- Khi overhead của Observer pattern quá lớn so với lợi ích

## **Kỹ Thuật Liên Quan**

- **Observer Pattern**: Cơ sở cho kỹ thuật này
- **MVC Pattern**: Kiến trúc tổng quát hơn
- **Separate Domain from Presentation**: Nguyên tắc tổng quát

## **Kết Luận**

Nhân bản dữ liệu quan sát là kỹ thuật quan trọng để tách biệt logic nghiệp vụ khỏi giao diện người dùng. Bằng cách di chuyển dữ liệu và logic sang lớp domain và sử dụng Observer pattern để đồng bộ hóa, bạn tạo ra ứng dụng linh hoạt, dễ bảo trì và dễ test hơn.