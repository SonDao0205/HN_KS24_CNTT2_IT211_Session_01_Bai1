Phần 1: Phân tích logic - Nguyên nhân gốc rễ
Vấn đề khiến client không thể phân tích dữ liệu nằm ở việc sử dụng phương thức products.toString().

Định dạng không chuẩn (Non-JSON format): Khi bạn gọi .toString() trên một List<Product>, Java sẽ trả về một chuỗi văn bản mô tả đối tượng (thường có dạng [ClassName@hashcode, ...]). Chuỗi này không tuân theo cú pháp của JSON (ví dụ: thiếu dấu ngoặc kép cho key, cấu trúc không đúng). Các trình phân tích cú pháp (Parser) ở phía client sẽ báo lỗi ngay lập tức vì không tìm thấy cấu trúc JSON hợp lệ.

Kiểu dữ liệu trả về (Return Type): Phương thức của bạn đang khai báo kiểu trả về là String. Khi đó, Spring Boot hiểu rằng bạn muốn gửi một chuỗi văn bản thuần túy (text/plain) thay vì một đối tượng cần được chuyển đổi sang JSON (application/json).

Content-Type không khớp: Mặc dù dùng @RestController, nhưng vì bạn chủ động ép kiểu về String, header Content-Type trong HTTP Response có thể bị gửi đi là text/plain, khiến client không nhận diện được đây là một API trả về dữ liệu cấu trúc.