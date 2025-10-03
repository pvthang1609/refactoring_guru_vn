## Danh mục tái cấu trúc mã (Catalog of Refactoring)

Danh mục này được chia thành hai phần chính: **Mùi mã (Code Smells)** — các dấu hiệu nhận biết mã cần được tái cấu trúc, và **Các kỹ thuật tái cấu trúc (Refactoring Techniques)** — các phương pháp cụ thể để loại bỏ những mùi mã đó.

### I. Mùi mã (Code Smells)

Mùi Mã là các dấu hiệu cho thấy có vấn đề trong cấu trúc mã của bạn.

#### 1. Mã phình to (Bloaters)

Là những đoạn mã, phương thức và lớp đã phát triển đến mức quá lớn, gây khó khăn trong việc xử lý. Chúng thường tích tụ theo thời gian khi chương trình phát triển.

* **Phương thức dài** (Long Method)
* **Lớp lớn** (Large Class)
* **Ám ảnh kiểu dữ liệu nguyên thủy** (Primitive Obsession)
* **Danh sách tham số dài** (Long Parameter List)
* **Cụm dữ liệu** (Data Clumps)

#### 2. Lạm dụng lập trình hướng đối tượng (Object-Orientation Abusers)

Là những mùi mã cho thấy việc áp dụng các nguyên tắc lập trình hướng đối tượng chưa đầy đủ hoặc không chính xác.

* **Các lớp thay thế với giao diện khác nhau** (Alternative Classes with Different Interfaces)
* **Từ chối kế thừa** (Refused Bequest)
* **Các câu lệnh `switch` hoặc `if/else` dài** (Switch Statements)
* **Trường tạm thời** (Temporary Field)

#### 3. Ngăn chặn thay đổi (Change Preventers)

Những mùi này có nghĩa là khi bạn cần thay đổi một thứ ở một nơi, bạn phải thực hiện nhiều thay đổi ở những nơi khác. Kết quả là việc phát triển chương trình trở nên phức tạp và tốn kém hơn nhiều.

* **Thay đổi phân kỳ** (Divergent Change): Một lớp bị thay đổi theo nhiều cách khác nhau.
* **Phân cấp kế thừa song song** (Parallel Inheritance Hierarchies)
* **Phẫu thuật bằng súng săn** (Shotgun Surgery): Phải thay đổi nhiều lớp khi thực hiện một thay đổi nhỏ.

#### 4. Phần có thể bỏ qua (Dispensables)

Là những thứ vô nghĩa và không cần thiết, sự vắng mặt của chúng sẽ làm cho mã sạch hơn, hiệu quả hơn và dễ hiểu hơn.

* **Chú thích** (Comments) – *Khi được sử dụng quá mức hoặc để giải thích mã tồi.*
* **Mã trùng lặp** (Duplicate Code)
* **Lớp dữ liệu** (Data Class)
* **Mã chết** (Dead Code)
* **Lớp lười biếng** (Lazy Class)
* **Tính tổng quát đầu cơ** (Speculative Generality)

#### 5. Bộ Nối / Khớp Nối (Couplers)

Tất cả các mùi mã trong nhóm này góp phần vào sự khớp nối (coupling) quá mức giữa các lớp hoặc cho thấy điều gì xảy ra nếu khớp nối được thay thế bằng việc ủy quyền (delegation) quá mức.

* **Feature Envy** (Ghen tị Tính năng)
* **Inappropriate Intimacy** (Thân mật Không thích hợp)
* **Incomplete Library Class** (Lớp Thư viện Không đầy đủ)
* **Message Chains** (Chuỗi Thông điệp)
* **Middle Man** (Người Trung gian)

---

### II. Các Kỹ thuật Tái cấu trúc (Refactoring Techniques)

#### 1. Composing Methods (Soạn thảo Phương thức)

Phần lớn việc tái cấu trúc tập trung vào việc soạn thảo các phương thức một cách chính xác. Các phương thức quá dài thường là gốc rễ của mọi vấn đề. Các kỹ thuật trong nhóm này giúp sắp xếp hợp lý các phương thức, loại bỏ mã trùng lặp và mở đường cho những cải tiến trong tương lai.

* **Extract Method** (Trích xuất Phương thức)
* **Inline Method** (Nội tuyến Phương thức)
* **Extract Variable** (Trích xuất Biến)
* **Inline Temp** (Nội tuyến Biến Tạm)
* **Replace Temp with Query** (Thay thế Biến Tạm bằng Truy vấn)
* **Split Temporary Variable** (Tách Biến Tạm thời)
* **Remove Assignments to Parameters** (Loại bỏ Gán cho Tham số)
* **Replace Method with Method Object** (Thay thế Phương thức bằng Đối tượng Phương thức)
* **Substitute Algorithm** (Thay thế Thuật toán)

#### 2. Moving Features between Objects (Di chuyển Tính năng giữa các Đối tượng)

Các kỹ thuật này cho thấy cách di chuyển chức năng giữa các lớp một cách an toàn, tạo các lớp mới và ẩn chi tiết triển khai khỏi sự truy cập công khai.

* **Move Method** (Di chuyển Phương thức)
* **Move Field** (Di chuyển Trường)
* **Extract Class** (Trích xuất Lớp)
* **Inline Class** (Nội tuyến Lớp)
* **Hide Delegate** (Ẩn Người ủy quyền)
* **Remove Middle Man** (Loại bỏ Người Trung gian)
* **Introduce Foreign Method** (Giới thiệu Phương thức Ngoài)
* **Introduce Local Extension** (Giới thiệu Phần mở rộng Cục bộ)

#### 3. Organizing Data (Tổ chức Dữ liệu)

Các kỹ thuật này giúp xử lý dữ liệu, thay thế các kiểu nguyên thủy bằng chức năng lớp phong phú hơn. Một kết quả quan trọng khác là việc gỡ rối các mối liên kết lớp, làm cho các lớp dễ di chuyển và tái sử dụng hơn.

* **Self Encapsulate Field** (Tự Đóng gói Trường)
* **Replace Data Value with Object** (Thay thế Giá trị Dữ liệu bằng Đối tượng)
* **Change Value to Reference** (Thay đổi Giá trị thành Tham chiếu)
* **Change Reference to Value** (Thay đổi Tham chiếu thành Giá trị)
* **Replace Array with Object** (Thay thế Mảng bằng Đối tượng)
* **Duplicate Observed Data** (Nhân đôi Dữ liệu Được Quan sát)
* **Change Unidirectional Association to Bidirectional** (Thay đổi Liên kết Một chiều thành Hai chiều)
* **Change Bidirectional Association to Unidirectional** (Thay đổi Liên kết Hai chiều thành Một chiều)
* **Replace Magic Number with Symbolic Constant** (Thay thế Số "Ma thuật" bằng Hằng số Ký hiệu)
* **Encapsulate Field** (Đóng gói Trường)
* **Encapsulate Collection** (Đóng gói Bộ sưu tập)
* **Replace Type Code with Class** (Thay thế Mã Kiểu bằng Lớp)
* **Replace Type Code with Subclasses** (Thay thế Mã Kiểu bằng Lớp con)
* **Replace Type Code with State/Strategy** (Thay thế Mã Kiểu bằng State/Strategy)
* **Replace Subclass with Fields** (Thay thế Lớp con bằng Trường)

#### 4. Simplifying Conditional Expressions (Đơn giản hóa Biểu thức Điều kiện)

Các câu lệnh điều kiện có xu hướng trở nên phức tạp hơn theo thời gian và có nhiều kỹ thuật để chống lại điều này.

* **Decompose Conditional** (Phân tách Điều kiện)
* **Consolidate Conditional Expression** (Củng cố Biểu thức Điều kiện)
* **Consolidate Duplicate Conditional Fragments** (Củng cố các Đoạn Điều kiện Trùng lặp)
* **Remove Control Flag** (Loại bỏ Cờ Kiểm soát)
* **Replace Nested Conditional with Guard Clauses** (Thay thế Điều kiện Lồng nhau bằng Mệnh đề Bảo vệ)
* **Replace Conditional with Polymorphism** (Thay thế Điều kiện bằng Đa hình)
* **Introduce Null Object** (Giới thiệu Đối tượng Null)
* **Introduce Assertion** (Giới thiệu Khẳng định)

#### 5. Simplifying Method Calls (Đơn giản hóa Lời gọi Phương thức)

Các kỹ thuật này làm cho lời gọi phương thức đơn giản và dễ hiểu hơn. Điều này, đến lượt nó, giúp đơn giản hóa giao diện tương tác giữa các lớp.

* **Rename Method** (Đổi tên Phương thức)
* **Add Parameter** (Thêm Tham số)
* **Remove Parameter** (Loại bỏ Tham số)
* **Separate Query from Modifier** (Tách Truy vấn khỏi Bộ điều chỉnh)
* **Parameterize Method** (Tham số hóa Phương thức)
* **Replace Parameter with Explicit Methods** (Thay thế Tham số bằng Phương thức Tường minh)
* **Preserve Whole Object** (Bảo toàn Toàn bộ Đối tượng)
* **Replace Parameter with Method Call** (Thay thế Tham số bằng Lời gọi Phương thức)
* **Introduce Parameter Object** (Giới thiệu Đối tượng Tham số)
* **Remove Setting Method** (Loại bỏ Phương thức Gán)
* **Hide Method** (Ẩn Phương thức)
* **Replace Constructor with Factory Method** (Thay thế Hàm tạo bằng Phương thức Factory)
* **Replace Error Code with Exception** (Thay thế Mã Lỗi bằng Ngoại lệ)
* **Replace Exception with Test** (Thay thế Ngoại lệ bằng Kiểm thử)

#### 6. Dealing with Generalization (Xử lý Tính Tổng quát)

Nhóm các kỹ thuật liên quan đến việc di chuyển chức năng dọc theo hệ thống phân cấp kế thừa của lớp, tạo các lớp và giao diện mới, và thay thế kế thừa bằng ủy quyền (delegation) và ngược lại.

* **Pull Up Field** (Kéo Trường lên)
* **Pull Up Method** (Kéo Phương thức lên)
* **Pull Up Constructor Body** (Kéo Thân Hàm tạo lên)
* **Push Down Method** (Đẩy Phương thức xuống)
* **Push Down Field** (Đẩy Trường xuống)
* **Extract Subclass** (Trích xuất Lớp con)
* **Extract Superclass** (Trích xuất Lớp Cha)
* **Extract Interface** (Trích xuất Giao diện)
* **Collapse Hierarchy** (Thu gọn Phân cấp)
* **Form Template Method** (Hình thành Phương thức Mẫu)
* **Replace Inheritance with Delegation** (Thay thế Kế thừa bằng Ủy quyền)
* **Replace Delegation with Inheritance** (Thay thế Ủy quyền bằng Kế thừa)

---
*Nguồn: Refactoring.Guru*