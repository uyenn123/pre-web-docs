# Buoi 2: CƠ BẢN VỀ THIẾT KẾ CƠ SỞ DỮ LIỆU
## I.	Lý thuyết cơ bản về thiết kế cơ sở dữ liệu:
Gồm 6 bước:
### 1.	Phân tích yêu cầu:
-	Dữ liệu nào được lưu trữ trong CSDL
-	Ứng dụng nào sẽ được sử dụng trên CSDL 
-	Các thao tác nào được sử dụng thường xuyên và các yêu cầu về hiệu năng của hệ thống
->	Quá trình này liên quan đến những trao đổi của các nhóm người dùng và nhóm nghiên cứu môi trường hiện tại. Tìm hiểu các ứng dụng đang có xem có cần thay thế hoặc bổ trợ cho hệ CSDL không
### 2.	Thiết kế CSDL mức khái niệm:
-	Những thông tin có được từ bước phân tích yêu cầu sẽ được dùng để phát triển một mô tả mức tổng quát dữ liệu được lưu trong CSDL, cùng với các ràng buộc cần thiết trên dữ liệu này.
### 3.	Thiết kế CSDL mức logic:
-	Một hệ quản trị CSDL sẽ được chọn để cài đặt CSDL và chuyển thiết kế CSDL mức khái niệm thành một lược đồ CSDL với mô hình dữ liệu của hệ quản trị CSDL đã chọn
### 4.	Cải tiến lược đồ:
-	Các lược đồ được phát triển ở bước 3 sẽ được phân tích các vấn đề tiềm ẩn. Tại đây, CSDL sẽ được chuẩn hóa, dựa trên lý thuyết toán học.
### 5.	Thiết kế CSDL mức vật lý:
-	Khối lượng công việc tiềm ẩn và các phương pháp truy nhập được mô phỏng để xác nhận các điểm yếu tiềm ẩn trong CSDL mức khái niệm. Quá trình này thường là nguyên nhân tạo ra các tệp chỉ mục hoặc/và các quan hệ nhóm. Trong trường hợp đặc biệt, toàn bộ mô hình khái niệm sẽ được xây dựng lại
### 6.	Thiết kế an toàn bảo mật:
-	Xác định các nhóm người dùng và phân tích vai trò của họ để định nghĩa các phương pháp truy nhập dữ liệu.

Trong quá trình phát triển, thường có bước cuối cùng (bước thứ 7), gọi là pha điều chỉnh(tuning phase), trong đó CSDL sẽ được thực hiện (mặc dù nó có thể chỉ được chạy mô phỏng) và sẽ được cải tiến, chỉnh sửa để đáp ứng nhu cầu thực thi trong môi trường mong đợi

![](image/1.png)
