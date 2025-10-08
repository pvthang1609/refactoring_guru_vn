# **Định Dạng Phương Thức Mẫu (Form Template Method)**

## **Định Nghĩa**
Định dạng phương thức mẫu là kỹ thuật tái cấu trúc tạo ra một phương thức mẫu trong lớp cha bằng cách xác định khung của một thuật toán và để các lớp con triển khai các bước cụ thể mà không thay đổi cấu trúc tổng thể.

## **Khi Nào Sử Dụng**

### **Nên sử dụng khi:**
- Có nhiều lớp con triển khai cùng một thuật toán với các biến thể nhỏ
- Muốn tránh sự trùng lặp code trong khi vẫn cho phép tùy chỉnh
- Cần kiểm soát cấu trúc thuật toán ở mức lớp cha
- Muốn đảm bảo thứ tự các bước trong thuật toán được tuân thủ

### **Không nên sử dụng khi:**
- Các thuật toán quá khác biệt giữa các lớp con
- Chỉ có một implementation của thuật toán
- Các bước thuật toán không ổn định và thường xuyên thay đổi

## **Các Bước Thực Hiện**

1. **Xác định thuật toán chung**: Tìm các phương thức có cấu trúc tương tự trong các lớp con
2. **Tạo phương thức mẫu trong lớp cha**: Di chuyển thuật toán chung lên lớp cha
3. **Xác định các bước biến đổi**: Tìm các phần của thuật toán khác nhau giữa các lớp con
4. **Tạo phương thức trừu tượng hoặc hook**: Định nghĩa các phương thức cho các bước biến đổi
5. **Thay thế bằng lời gọi phương thức trừu tượng**: Thay thế code cụ thể bằng lời gọi đến các phương thức trừu tượng

## **Ví Dụ Minh Họa**

### **❌ Trước khi định dạng phương thức mẫu:**
```java
abstract class DataProcessor {
    // Các lớp con triển khai toàn bộ thuật toán với sự trùng lặp
}

class CSVDataProcessor extends DataProcessor {
    public void processData(String inputFile, String outputFile) {
        // Bước 1: Đọc dữ liệu
        System.out.println("Reading CSV data from: " + inputFile);
        String rawData = readCSVFile(inputFile);
        
        // Bước 2: Validate dữ liệu
        if (!isValidCSV(rawData)) {
            throw new IllegalArgumentException("Invalid CSV data");
        }
        
        // Bước 3: Parse dữ liệu
        List<Record> records = parseCSVData(rawData);
        
        // Bước 4: Transform dữ liệu
        List<Record> transformedRecords = transformCSVData(records);
        
        // Bước 5: Ghi dữ liệu
        writeCSVFile(transformedRecords, outputFile);
        System.out.println("CSV data processed successfully");
    }
    
    private String readCSVFile(String filename) {
        // Logic đọc file CSV
        return "csv,data,content";
    }
    
    private boolean isValidCSV(String data) {
        // Logic validate CSV
        return data != null && !data.isEmpty();
    }
    
    private List<Record> parseCSVData(String rawData) {
        // Logic parse CSV
        return Arrays.asList(new Record("csv data"));
    }
    
    private List<Record> transformCSVData(List<Record> records) {
        // Logic transform CSV
        return records.stream()
            .map(record -> new Record(record.getData().toUpperCase()))
            .collect(Collectors.toList());
    }
    
    private void writeCSVFile(List<Record> records, String filename) {
        // Logic ghi file CSV
        System.out.println("Writing CSV to: " + filename);
    }
}

class XMLDataProcessor extends DataProcessor {
    public void processData(String inputFile, String outputFile) {
        // Bước 1: Đọc dữ liệu
        System.out.println("Reading XML data from: " + inputFile);
        String rawData = readXMLFile(inputFile);
        
        // Bước 2: Validate dữ liệu
        if (!isValidXML(rawData)) {
            throw new IllegalArgumentException("Invalid XML data");
        }
        
        // Bước 3: Parse dữ liệu
        List<Record> records = parseXMLData(rawData);
        
        // Bước 4: Transform dữ liệu
        List<Record> transformedRecords = transformXMLData(records);
        
        // Bước 5: Ghi dữ liệu
        writeXMLFile(transformedRecords, outputFile);
        System.out.println("XML data processed successfully");
    }
    
    private String readXMLFile(String filename) {
        // Logic đọc file XML
        return "<data>xml content</data>";
    }
    
    private boolean isValidXML(String data) {
        // Logic validate XML
        return data != null && data.contains("<data>");
    }
    
    private List<Record> parseXMLData(String rawData) {
        // Logic parse XML
        return Arrays.asList(new Record("xml data"));
    }
    
    private List<Record> transformXMLData(List<Record> records) {
        // Logic transform XML
        return records.stream()
            .map(record -> new Record(record.getData().toLowerCase()))
            .collect(Collectors.toList());
    }
    
    private void writeXMLFile(List<Record> records, String filename) {
        // Logic ghi file XML
        System.out.println("Writing XML to: " + filename);
    }
}

class Record {
    private String data;
    
    public Record(String data) {
        this.data = data;
    }
    
    public String getData() {
        return data;
    }
}
```

### **✅ Sau khi định dạng phương thức mẫu:**
```java
abstract class DataProcessor {
    // Phương thức mẫu - định nghĩa khung thuật toán
    public final void processData(String inputFile, String outputFile) {
        // Bước 1: Đọc dữ liệu
        String rawData = readData(inputFile);
        
        // Bước 2: Validate dữ liệu
        if (!isValidData(rawData)) {
            throw new IllegalArgumentException("Invalid data format");
        }
        
        // Bước 3: Parse dữ liệu
        List<Record> records = parseData(rawData);
        
        // Bước 4: Transform dữ liệu
        List<Record> transformedRecords = transformData(records);
        
        // Bước 5: Ghi dữ liệu
        writeData(transformedRecords, outputFile);
        
        // Hook method - có thể ghi đè nếu cần
        onProcessingComplete(inputFile, outputFile);
    }
    
    // Các phương thức trừu tượng - lớp con phải triển khai
    protected abstract String readData(String inputFile);
    protected abstract boolean isValidData(String rawData);
    protected abstract List<Record> parseData(String rawData);
    protected abstract List<Record> transformData(List<Record> records);
    protected abstract void writeData(List<Record> records, String outputFile);
    
    // Hook method - cung cấp implementation mặc định
    protected void onProcessingComplete(String inputFile, String outputFile) {
        System.out.println("Data processing completed successfully");
    }
    
    // Có thể thêm các phương thức tiện ích chung
    protected final void log(String message) {
        System.out.println("[DataProcessor] " + message);
    }
}

class CSVDataProcessor extends DataProcessor {
    @Override
    protected String readData(String inputFile) {
        log("Reading CSV data from: " + inputFile);
        // Logic đọc file CSV cụ thể
        return "csv,data,content";
    }
    
    @Override
    protected boolean isValidData(String rawData) {
        // Logic validate CSV cụ thể
        return rawData != null && !rawData.isEmpty() && rawData.contains(",");
    }
    
    @Override
    protected List<Record> parseData(String rawData) {
        // Logic parse CSV cụ thể
        String[] fields = rawData.split(",");
        return Arrays.stream(fields)
            .map(Record::new)
            .collect(Collectors.toList());
    }
    
    @Override
    protected List<Record> transformData(List<Record> records) {
        // Logic transform CSV cụ thể
        return records.stream()
            .map(record -> new Record(record.getData().toUpperCase()))
            .collect(Collectors.toList());
    }
    
    @Override
    protected void writeData(List<Record> records, String outputFile) {
        log("Writing CSV data to: " + outputFile);
        // Logic ghi file CSV cụ thể
        String csvContent = records.stream()
            .map(Record::getData)
            .collect(Collectors.joining(","));
        System.out.println("CSV Content: " + csvContent);
    }
    
    @Override
    protected void onProcessingComplete(String inputFile, String outputFile) {
        log("CSV processing completed. Input: " + inputFile + ", Output: " + outputFile);
    }
}

class XMLDataProcessor extends DataProcessor {
    @Override
    protected String readData(String inputFile) {
        log("Reading XML data from: " + inputFile);
        // Logic đọc file XML cụ thể
        return "<data>xml content</data>";
    }
    
    @Override
    protected boolean isValidData(String rawData) {
        // Logic validate XML cụ thể
        return rawData != null && rawData.contains("<data>") && rawData.contains("</data>");
    }
    
    @Override
    protected List<Record> parseData(String rawData) {
        // Logic parse XML cụ thể
        // Giả sử extract content từ thẻ <data>
        String content = rawData.replace("<data>", "").replace("</data>", "");
        return Arrays.asList(new Record(content));
    }
    
    @Override
    protected List<Record> transformData(List<Record> records) {
        // Logic transform XML cụ thể
        return records.stream()
            .map(record -> new Record(record.getData().toLowerCase()))
            .collect(Collectors.toList());
    }
    
    @Override
    protected void writeData(List<Record> records, String outputFile) {
        log("Writing XML data to: " + outputFile);
        // Logic ghi file XML cụ thể
        String xmlContent = records.stream()
            .map(record -> "<record>" + record.getData() + "</record>")
            .collect(Collectors.joining(""));
        System.out.println("XML Content: <data>" + xmlContent + "</data>");
    }
}

class JSONDataProcessor extends DataProcessor {
    @Override
    protected String readData(String inputFile) {
        log("Reading JSON data from: " + inputFile);
        return "{\"data\": \"json content\"}";
    }
    
    @Override
    protected boolean isValidData(String rawData) {
        return rawData != null && rawData.startsWith("{") && rawData.endsWith("}");
    }
    
    @Override
    protected List<Record> parseData(String rawData) {
        // Logic parse JSON đơn giản
        String content = rawData.replace("{\"data\": \"", "").replace("\"}", "");
        return Arrays.asList(new Record(content));
    }
    
    @Override
    protected List<Record> transformData(List<Record> records) {
        // Thêm prefix cho JSON data
        return records.stream()
            .map(record -> new Record("JSON: " + record.getData()))
            .collect(Collectors.toList());
    }
    
    @Override
    protected void writeData(List<Record> records, String outputFile) {
        log("Writing JSON data to: " + outputFile);
        String jsonContent = records.stream()
            .map(record -> "\"" + record.getData() + "\"")
            .collect(Collectors.joining(", "));
        System.out.println("JSON Content: {" + jsonContent + "}");
    }
}

// Client code sử dụng
public class DataProcessingApp {
    public static void main(String[] args) {
        DataProcessor csvProcessor = new CSVDataProcessor();
        csvProcessor.processData("input.csv", "output.csv");
        
        System.out.println();
        
        DataProcessor xmlProcessor = new XMLDataProcessor();
        xmlProcessor.processData("input.xml", "output.xml");
        
        System.out.println();
        
        DataProcessor jsonProcessor = new JSONDataProcessor();
        jsonProcessor.processData("input.json", "output.json");
    }
}
```

## **Lợi Ích**

- **✅ Loại bỏ trùng lặp**: Code chung được đưa lên lớp cha
- **✅ Kiểm soát cấu trúc**: Lớp cha kiểm soát luồng thuật toán
- **✅ Dễ mở rộng**: Thêm định dạng mới dễ dàng bằng cách tạo lớp con
- **✅ Bảo vệ thuật toán**: Phương thức mẫu có thể được đánh dấu final để ngăn ghi đè
- **✅ Linh hoạt**: Có thể sử dụng hook methods để cung cấp điểm tùy chỉnh

## **Điểm Cần Lưu ý**

- **Sử dụng final cho phương thức mẫu**: Ngăn không cho lớp con thay đổi cấu trúc thuật toán
- **Cân bằng giữa abstract methods và hook methods**: Sử dụng hook methods cho các bước tùy chọn
- **Đảm bảo các bước độc lập**: Mỗi phương thức trừu tượng nên thực hiện một nhiệm vụ rõ ràng
- **Document rõ ràng**: Ghi chú rõ các phương thức nào cần được triển khai và các hook methods có sẵn

## **Khi Nào Không Nên Sử Dụng**

- Khi các thuật toán quá khác biệt và không có cấu trúc chung
- Khi có quá nhiều biến thể và hệ thống phân cấp trở nên phức tạp
- Khi sử dụng strategy pattern hoặc composition phù hợp hơn

## **Kỹ Thuật Liên Quan**

- **Template Method Pattern**: Pattern thiết kế tương ứng
- **Strategy Pattern**: Sử dụng composition thay vì inheritance cho các thuật toán
- **Pull Up Method**: Kéo phương thức lên lớp cha
- **Replace Conditional with Polymorphism**: Thay thế điều kiện bằng đa hình

## **Kết Luận**

Định dạng phương thức mẫu là kỹ thuật mạnh mẽ để quản lý các thuật toán có cấu trúc chung nhưng khác biệt trong các bước cụ thể. Bằng cách định nghĩa khung thuật toán trong lớp cha và để các lớp con triển khai các bước chi tiết, chúng ta tạo ra code có cấu trúc tốt, dễ bảo trì và dễ mở rộng. Kỹ thuật này đặc biệt hữu ích khi bạn muốn đảm bảo thứ tự thực hiện các bước trong thuật toán trong khi vẫn cho phép linh hoạt trong việc triển khai chi tiết.

**Nguồn:** [refactoring.guru/form-template-method](https://refactoring.guru/form-template-method)