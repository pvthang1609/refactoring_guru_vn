# **Giới Thiệu Local Extension (Introduce Local Extension)**

## **Định Nghĩa**
Giới thiệu Local Extension là kỹ thuật tái cấu trúc tạo một lớp mới mở rộng chức năng của lớp không thể sửa đổi (third-party class), bằng cách sử dụng inheritance hoặc wrapper.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Cần thêm nhiều phương thức cho lớp third-party
- Không thể sửa đổi trực tiếp lớp gốc
- Cần nhóm nhiều Foreign Method liên quan với nhau
- Muốn cung cấp interface thống nhất cho các chức năng mở rộng

### **Dấu hiệu nhận biết:**
- Có nhiều Foreign Method liên quan đến cùng một lớp
- Cần thêm chức năng phức tạp không thể giải quyết bằng một phương thức đơn lẻ
- Muốn tái sử dụng nhóm chức năng mở rộng ở nhiều nơi

## **Các Bước Thực Hiện**

1. **Tạo lớp extension**: Tạo lớp mới kế thừa hoặc chứa lớp cần mở rộng
2. **Tạo constructor**: Thêm constructor nhận đối tượng gốc (với wrapper)
3. **Chuyển Foreign Methods**: Di chuyển các Foreign Method vào lớp extension
4. **Ủy quyền phương thức gốc**: Với wrapper, ủy quyền các phương thức cần thiết
5. **Cập nhật client**: Thay thế sử dụng lớp gốc bằng lớp extension

## **Cách Tiếp Cận**

### **1. Subclass (Kế thừa)**
```java
// ❌ Nhiều Foreign Method rời rạc
class DateUtils {
    public static Date addDays(Date date, int days) { ... }
    public static Date addMonths(Date date, int months) { ... }
    public static boolean isWeekend(Date date) { ... }
}

// ✅ Sử dụng Subclass
class ExtendedDate extends Date {
    public ExtendedDate(long date) {
        super(date);
    }
    
    public ExtendedDate addDays(int days) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(this);
        cal.add(Calendar.DATE, days);
        return new ExtendedDate(cal.getTime().getTime());
    }
    
    public boolean isWeekend() {
        Calendar cal = Calendar.getInstance();
        cal.setTime(this);
        int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
        return dayOfWeek == Calendar.SATURDAY || dayOfWeek == Calendar.SUNDAY;
    }
}
```

### **2. Wrapper (Composition)**
```java
// ✅ Sử dụng Wrapper - linh hoạt hơn
class DateWrapper {
    private final Date date;
    
    public DateWrapper(Date date) {
        this.date = date;
    }
    
    // Ủy quyền phương thức gốc
    public long getTime() {
        return date.getTime();
    }
    
    public boolean after(Date when) {
        return date.after(when);
    }
    
    // Thêm phương thức mở rộng
    public DateWrapper addDays(int days) {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        cal.add(Calendar.DATE, days);
        return new DateWrapper(cal.getTime());
    }
    
    public boolean isWeekend() {
        Calendar cal = Calendar.getInstance();
        cal.setTime(date);
        int dayOfWeek = cal.get(Calendar.DAY_OF_WEEK);
        return dayOfWeek == Calendar.SATURDAY || dayOfWeek == Calendar.SUNDAY;
    }
    
    public String toCustomFormat() {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        return sdf.format(date);
    }
    
    // Chuyển đổi trở lại
    public Date toDate() {
        return new Date(date.getTime());
    }
}
```

## **Ví Dụ Thực Tế**

### **❌ Nhiều Foreign Method cho String:**
```java
class StringUtils {
    public static String toTitleCase(String str) { ... }
    public static boolean isNullOrEmpty(String str) { ... }
    public static String truncate(String str, int length) { ... }
    public static String removeAccents(String str) { ... }
}

// Sử dụng rời rạc
String name = StringUtils.toTitleCase("john doe");
if (!StringUtils.isNullOrEmpty(name)) {
    name = StringUtils.truncate(name, 20);
}
```

### **✅ Với Local Extension:**
```java
class ExtendedString {
    private final String value;
    
    public ExtendedString(String value) {
        this.value = value;
    }
    
    // Ủy quyền phương thức String quan trọng
    public int length() {
        return value.length();
    }
    
    public boolean isEmpty() {
        return value.isEmpty();
    }
    
    public char charAt(int index) {
        return value.charAt(index);
    }
    
    // Phương thức mở rộng
    public ExtendedString toTitleCase() {
        if (value == null || value.trim().isEmpty()) {
            return new ExtendedString(value);
        }
        String trimmed = value.trim().toLowerCase();
        String result = trimmed.substring(0, 1).toUpperCase() + 
                       trimmed.substring(1);
        return new ExtendedString(result);
    }
    
    public ExtendedString truncate(int maxLength) {
        if (value == null || value.length() <= maxLength) {
            return this;
        }
        return new ExtendedString(value.substring(0, maxLength) + "...");
    }
    
    public ExtendedString removeAccents() {
        if (value == null) return this;
        String normalized = Normalizer.normalize(value, Normalizer.Form.NFD);
        String result = normalized.replaceAll("[^\\p{ASCII}]", "");
        return new ExtendedString(result);
    }
    
    public boolean isNullOrEmpty() {
        return value == null || value.trim().isEmpty();
    }
    
    // Chuyển đổi trở lại
    @Override
    public String toString() {
        return value;
    }
}

// Sử dụng liên tục và rõ ràng
ExtendedString name = new ExtendedString("  jöhn döe  ")
    .toTitleCase()
    .removeAccents()
    .truncate(20);

if (!name.isNullOrEmpty()) {
    System.out.println(name);
}
```

## **Ví Dụ Với Collection**

### **✅ Mở rộng List functionality:**
```java
class ExtendedList<T> {
    private final List<T> list;
    
    public ExtendedList(List<T> list) {
        this.list = new ArrayList<>(list);
    }
    
    // Ủy quyền phương thức List cơ bản
    public int size() {
        return list.size();
    }
    
    public boolean isEmpty() {
        return list.isEmpty();
    }
    
    public T get(int index) {
        return list.get(index);
    }
    
    // Phương thức mở rộng
    public ExtendedList<T> filter(Predicate<T> predicate) {
        return new ExtendedList<>(
            list.stream().filter(predicate).collect(Collectors.toList())
        );
    }
    
    public <R> ExtendedList<R> map(Function<T, R> mapper) {
        return new ExtendedList<>(
            list.stream().map(mapper).collect(Collectors.toList())
        );
    }
    
    public T findFirst(Predicate<T> predicate) {
        return list.stream().filter(predicate).findFirst().orElse(null);
    }
    
    public boolean anyMatch(Predicate<T> predicate) {
        return list.stream().anyMatch(predicate);
    }
    
    public ExtendedList<T> distinct() {
        return new ExtendedList<>(
            list.stream().distinct().collect(Collectors.toList())
        );
    }
    
    // Chuyển đổi trở lại
    public List<T> toList() {
        return new ArrayList<>(list);
    }
}

// Sử dụng
List<String> names = Arrays.asList("John", "Jane", "Bob", "John");
ExtendedList<String> extendedNames = new ExtendedList<>(names);

String result = extendedNames
    .filter(name -> name.startsWith("J"))
    .distinct()
    .map(String::toUpperCase)
    .findFirst(name -> name.length() > 3);
```

## **Lợi Ích**

- **✅ Nhóm chức năng liên quan**: Tập hợp các Foreign Method vào một nơi
- **✅ Interface thống nhất**: Cung cấp API nhất quán cho client
- **✅ Dễ bảo trì**: Quản lý tập trung các chức năng mở rộng
- **✅ Tái sử dụng cao**: Có thể sử dụng ở nhiều nơi trong ứng dụng
- **✅ Linh hoạt**: Có thể chọn giữa inheritance và wrapper

## **So Sánh Subclass vs Wrapper**

| **Tiêu chí** | **Subclass** | **Wrapper** |
|-------------|--------------|-------------|
| **Tính kế thừa** | Có thể kế thừa | Không thể kế thừa |
| **Lớp final** | Không làm được | Làm được |
| **Ủy quyền** | Tự động | Phải viết thủ công |
| **Linh hoạt** | Thấp | Cao |
| **Độ phức tạp** | Thấp | Cao hơn |

## **Khi Nào Chọn Cách Nào**

- **Dùng Subclass khi**: Lớp gốc không phải final và bạn cần đa hình
- **Dùng Wrapper khi**: Lớp gốc là final, hoặc cần linh hoạt hơn

## **Kết Luận**

Giới thiệu Local Extension là giải pháp mạnh mẽ khi cần mở rộng đáng kể chức năng của lớp third-party. Nó cung cấp cách tiếp cận có cấu trúc để nhóm các chức năng liên quan và tạo ra interface thống nhất, dễ sử dụng cho client code.


**Nguồn:** [refactoring.guru/introduce-local-extension](https://refactoring.guru/introduce-local-extension)