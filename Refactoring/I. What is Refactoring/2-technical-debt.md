## Nợ kỹ thuật (Technical Debt)

Mọi người đều cố gắng hết sức để viết mã nguồn xuất sắc ngay từ đầu. Có lẽ không có lập trình viên nào cố tình viết mã không sạch sẽ gây bất lợi cho dự án. Nhưng đến thời điểm nào thì mã sạch lại trở thành không sạch?

Phép ẩn dụ về **"nợ kỹ thuật"** đối với mã nguồn không sạch ban đầu được đề xuất bởi Ward Cunningham. Nếu bạn vay tiền từ ngân hàng, điều này cho phép bạn mua sắm nhanh hơn. Bạn phải trả thêm tiền để đẩy nhanh quá trình - bạn không chỉ trả nợ gốc mà còn trả thêm lãi cho khoản vay.

Không cần phải nói, bạn thậm chí có thể gánh một khoản lãi lớn đến mức số tiền lãi vượt quá tổng thu nhập của bạn, khiến việc trả nợ toàn bộ trở nên bất khả thi. Điều tương tự cũng có thể xảy ra với mã nguồn. Bạn có thể tạm thời tăng tốc mà không cần viết các bài kiểm thử (test) cho các tính năng mới, nhưng điều này sẽ dần dần làm chậm tiến độ của bạn mỗi ngày cho đến khi bạn trả hết nợ bằng cách viết các bài kiểm thử đó.

### Các nguyên nhân của nợ kỹ thuật

* **Áp lực kinh doanh:** Đôi khi, hoàn cảnh kinh doanh có thể buộc bạn phải tung ra các tính năng trước khi chúng hoàn thiện. Trong trường- hợp này, các bản vá và giải pháp tạm bợ (kludges) sẽ xuất hiện trong mã để che giấu các phần chưa hoàn thành của dự án.

* **Thiếu hiểu biết về hậu quả của nợ kỹ thuật:** Đôi khi, nhà tuyển dụng của bạn có thể không hiểu rằng nợ kỹ thuật có "lãi" vì nó làm chậm tốc độ phát triển khi nợ tích tụ. Điều này có thể gây khó khăn cho việc dành thời gian của nhóm để tái cấu trúc vì ban quản lý không thấy được giá trị của nó.

* **Không giải quyết được sự gắn kết chặt chẽ của các thành phần:** Đây là khi dự án giống như một khối thống nhất (monolith) thay vì là sản phẩm của các mô-đun riêng lẻ. Trong trường hợp này, bất kỳ thay đổi nào đối với một phần của dự án sẽ ảnh hưởng đến các phần khác. Việc phát triển theo nhóm trở nên khó khăn hơn vì khó có thể cô lập công việc của từng thành viên.

* **Thiếu các bài kiểm thử (tests):** Việc thiếu phản hồi ngay lập tức khuyến khích các giải pháp thay thế nhanh chóng nhưng rủi ro hoặc các giải pháp tạm bợ. Trong trường hợp xấu nhất, những thay đổi này được triển khai và đưa thẳng vào môi trường sản phẩm (production) mà không qua bất kỳ thử nghiệm nào trước đó. Hậu quả có thể là thảm họa. Ví dụ, một bản sửa lỗi nhanh có vẻ vô hại có thể gửi một email thử nghiệm kỳ lạ đến hàng nghìn khách hàng hoặc tệ hơn nữa là xóa hoặc làm hỏng toàn bộ cơ sở dữ liệu.

* **Thiếu tài liệu:** Điều này làm chậm quá trình giới thiệu người mới vào dự án và có thể làm đình trệ quá trình phát triển nếu những người chủ chốt rời khỏi dự án.

* **Thiếu tương tác giữa các thành viên trong nhóm:** Nếu cơ sở kiến thức không được phân phối trong toàn công ty, mọi người cuối cùng sẽ làm việc với sự hiểu biết lỗi thời về các quy trình và thông tin về dự án. Tình hình này có thể trở nên tồi tệ hơn khi các lập trình viên cấp dưới được người hướng dẫn đào tạo không đúng cách.

* **Phát triển đồng thời dài hạn trên nhiều nhánh:** Điều này có thể dẫn đến việc tích lũy nợ kỹ thuật, sau đó sẽ tăng lên khi các thay đổi được hợp nhất (merge). Càng có nhiều thay đổi được thực hiện một cách độc lập, tổng nợ kỹ thuật càng lớn.

* **Trì hoãn tái cấu trúc:** Các yêu cầu của dự án liên tục thay đổi và tại một thời điểm nào đó, có thể trở nên rõ ràng rằng các phần của mã đã lỗi thời, trở nên cồng kềnh và phải được thiết kế lại để đáp ứng các yêu cầu mới. Mặt khác, các lập trình viên của dự án đang viết mã mới mỗi ngày làm việc với các phần đã lỗi thời. Do đó, việc trì hoãn tái cấu trúc càng lâu, mã phụ thuộc sẽ càng phải được làm lại nhiều hơn trong tương lai.

* **Thiếu giám sát tuân thủ:** Điều này xảy ra khi mọi người làm việc trong dự án đều viết mã theo cách họ thấy phù hợp (tức là theo cách họ đã viết cho dự án trước đó).

* **Năng lực yếu kém:** Đây là khi nhà phát triển đơn giản là không biết cách viết mã nguồn tốt.

**Nguồn:** [refactoring.guru/refactoring/technical-debt](https://refactoring.guru/refactoring/technical-debt)
