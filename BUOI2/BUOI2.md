# Buoi 2: CƠ BẢN VỀ THIẾT KẾ CƠ SỞ DỮ LIỆU
## I.	Lý thuyết cơ bản về thiết kế cơ sở dữ liệu:
Quá trình thiết kế một CSDL:
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
## II. Lược đồ quan hệ E-R (The Entity - Relationship Model)
- là một công cụ đồ họa được sử dụng để mô hình hóa cấu trúc của một cơ sở dữ liệu.
-	**Mô hình thực thể liên kết (E-R)** gồm 3 khái niệm cơ bản: **thực thể, tập quan hệ và thuộc tính.**
### 1. Thực thể
-	**Thực thể** là một đối tượng trong thế giới thực và có thể phân biệt được với các đối tượng khác. Thực thể có thể cụ thể (một người, một quyển sách,..) hoặc cũng có thể trừu tượng(một khoản vay ngân hàng, một khái niệm,..)
- Tập thực thể: Một tập hợp tất cả các thực thể được gọi là một tập hợp thực thể.
-	Thực thể được biểu diễn bởi một tập các **thuộc tính** (là các thuộc tính mô tả hoặc các đặc tính của thực thể)
- **Khóa chính** là một thuộc tính có thể xác định duy nhất một thực thể trong một tập thực thể.
 ![](image/15.png)
Như trong bảng trên, mỗi học sinh có một số đăng ký duy nhất, bất kì học sinh nào cũng có thể được xác định dựa trên đăng kí số. Enrollment_number là khóa chính trong bảng.
- Tập thực thể yếu:
     -	Các tập thực thể không có đủ thuộc tính để thiết lập khóa chính được gọi là tập thực thể yếu
- Tập thực thể mạnh:
     -	Các tập thực thể có đủ thuộc tính để thiết lập khóa chính được gọi là tập thực thể mạnh.
-   Ký hiệu: Hình chữ nhật.

 ![](image/16.png)
### 2. Thuộc tính 
- Mỗi tập thực thể có một tập các tính chất đặc trưng, mỗi tính chất đặc trưng này gọi là thuộc tính của tập thực thể. Ứng với mỗi thuộc tính có một tập các giá trị cho thuộc tính đó gọi là miền giá trị.
-   Một thuộc tính của một thực thể là một hàm ánh xạ từ một tập thực thể vào một miền giá trị
-   Ký hiệu: Hình bầu dục (hoặc hình elip).

**Thuộc tính bao gồm các loại như sau:**
- Thuộc tính đơn:
   - Không thể tách nhỏ ra được
- Thuộc tính phức hợp:
   -  Có thể tách ra thành các thành phần nhỏ hơn

**Các loại giá trị của thuộc tính:**
- **Đơn trị**: các thuộc tính có giá trị duy nhất cho một thực thể 
    - VD: số CMND, …
- **Thuộc tính đa trị:**
    - Được minh họa bằng 2 hình elip lồng nhau, thuộc tính này có thể có nhiều hơn 1 giá trị cho ít nhất một trường hợp thực thể của nó. Thuộc tính này có thể có giới hạn và giới hạn được chỉ định cho bất kì giá trị thực thể riêng lẻ nào.
    - Ví dụ: thuộc tính số điện thoại của một cá nhân có thể có một hoặc nhiều giá trị, một người có thể có một hoặc nhiều số điện thoại. 
     ![](image/17.png)
- Thuộc tính phức hợp:
    - Có thể chứa 2 hoặc nhiều thuộc tính, các thuộc tính này đại diện cho các thuộc tính cơ bản có ý nghĩa độc lập với nhau. 
    - Ví dụ: thuộc tính địa chỉ thường là thuộc tính phức hợp, bao gồm các thuộc tính như đường phố, khu vực,..
     ![](image/18.png)
- Thuộc tính dẫn xuất:
    - Thuộc tính dẫn xuất là thuộc tính mà giá trị của nó hoàn toàn phụ thuộc vào một thuộc tính khác và được biểu thị bằng dấu nét đứt.
    - Ví dụ: thuộc tính tuổi của một người, đối với một người cụ thể, tuổi của một người được xác định từ ngày hiện tại và ngày sinh của người đó.
     ![](image/19.png)

Mỗi thực thể đều được phân biệt bởi thuộc tính khóa

### 3. Mối quan hệ:
#### a. Khái niệm:
- Mối quan hệ là một liên kết hoặc liên kết tồn tại giữa một hoặc nhiều thực thể. 
- Ví dụ, thuộc về, sở hữu, mua,…

- **Tập mối quan hệ:** Tập hợp các mối quan hệ tương tự giữa 2 hoặc nhiều tập thực thể được gọi là tập mối quan hệ hay tập quan hệ. Ví dụ: nhân viên làm việc trong một bộ phân cụ thể. Tập hợp tất cả các quan hệ làm việc trong tồn tại giữa các nhân viên và bộ phân được gọi là tập hợp mối quan hệ làm việc trong.
#### b. Phân loại:
Có 3 loại:
- Mối quan hệ tự thân:
    - Mối quan hệ giữa các thực thể của cùng một tập thực thể được  gọi là mối quan hệ tự thân.
    - Ví dụ: một người quản lý và thành viên nhóm của anh ta. Cả 2 đều thuộc tập thực thể nhân viên.
     ![](image/8.png)
- Mối quan hệ nhị phân:
    - Mối quan hệ tồn tại giữa các thực thể của 2 tập thực thể khác nhau được gọi là mối quan hệ nhị phân.
    - Ví dụ: Một nhân viên thuộc một bộ phân. Mối quan hệ tồn tại giữa 2 thực thể thuộc về 2 tập thực thể khác nhau. Thực thể nhân viên thuộc về thực thể nhân viên. Thực thể bộ phận thuộc về thực thể bộ phân.
     ![](image/9.png)
- Mối quan hệ bậc 3:
    - Các mối quan hệ tồn tại giữa 3 thực thể của các tập thực thể khác nhau được gọi là mối quan hệ bậc 3.
    - Ví dụ: Một nhân viên làm việc trong một bộ phận tài khoản của chi nhánh khu vực. 
     ![](image/10.png)
#### c. Các mối quan hệ cũng có thể được phân loại theo bản đồ ánh xạ. Các bản đồ ánh xạ khác nhau là như sau:
![](image/3.png)
- 1-1:
    - Tồn tại khi một thực thể của một tập thực thể chỉ có thể được liên kết với một tập thực thể của 1 tập hợp khác.
    - Ví dụ: Mối quan hệ giữa chiếc xe và giấy phép sở hữu chiếc xe. Mỗi phương tiện đều có một đăng ký. Không có 2 phương tiện nào có thể có các chi tiết đăng kí giống nhau. Một xe – Một đăng kí.
     ![](image/11.png)
- 1-N:
    - Tồn tại khi một thực thể của một tập thực có thể được liên kết với nhiều thực thể của một tập thực thể khác.
    - Ví dụ: Mối quan hệ giữa khác hàng và phương tiện di chuyển. Một khách hàng có thể có nhiều hơn một phương tiện di chuyển. Một khách hàng – một hoặc nhiều phương tiện.
    ![](image/12.png)
- N-1:
    - Tồn tại khi nhiều thực thể của một tập hợp được liên kết với một thực thể của tập hợp khác bộ. Sự liên kết này được thực hiện bất kể thực hiện bất kể thực thể sau đã được liên kết với 1 hoặc nhiều thực thể của tập thực thể cũ
    - Ví dụ: mối quan hệ giữa một chiếc xe với nhà sản xuất. Mỗi phương tiện chỉ có 1 công ty sản xuất, nhưng một công ty có thể sản xuất nhiều loại phương tiện.
     ![](image/13.png)
- N-N:
    - Tồn tại khi bất kì số lương thực thể nào của một tập hợp có thể được liên kết với bất kì số thực thể của tập thực thể khác.
    - Ví dụ: mối quan hệ giữa khách hàng của ngân hàng và tài khoản của khách hàng. Một khách hàng có thể có nhiều tài khoản và một tài khoản có thể có nhiều khách hàng được liên kết với nó trong trường hợp đó là tài khoản chung. 
      ![](image/14.png)
![](image/2.png)

## III. Mô hình dữ liệu quan hệ
- Mô hình Dữ liệu Quan hệ (Relational Data Model – RDM) lần đầu tiên được Ted Codd của IBM phát triển vào những năm 1970
•	Cơ sở dữ liệu là một tập hợp các quan hệ có liên quan (bảng giá trị).
•	Mỗi quan hệ có một tên gọi riêng cho biết loại tuple (bộ dữ liệu) mà quan hệ có.
•	Mỗi quan hệ có một tập hợp các thuộc tính (tên cột) đại diện cho các tính chất hoặc các đặc trưng của từng thực thể.
•	Một bộ – tuple (hàng) biểu diễn một thực thể với các các giá trị tương ứng với từng thuộc tính.
•	Mỗi cột trong bảng còn được gọi là một trường (field)
 ![](image/20.png)
- Đặc điểm của mô hình cơ sở dữ liệu quan hệ:
    1.	Mỗi quan hệ trong cơ sở dữ liệu phải có một tên riêng biệt và duy nhất để phân biệt nó với các quan hệ khác trong cơ sở dữ liệu.
    2.	Một quan hệ không được có hai thuộc tính trùng tên. Mỗi thuộc tính phải có một tên riêng biệt.
    3.	Trong một quan hệ không được xuất hiện các bộ giá trị trùng lặp.
    4.	Các bộ (tuples) hay các thuộc tính (attributes) trong một quan hệ đều không nhất thiết phải tuân theo một thứ tự nhất định

## IV. Chuẩn hóa dữ liệu
Chuẩn hóa dữ liệu là quá trình biểu diễn cơ sở dữ liệu dưới dạng chuẩn. Đây là một kỹ thuật thiết kế bảng trong cơ sở dữ liệu, chia các bảng lớn thành các bảng nhỏ hơn và liên kết chúng bằng các mối quan hệ. 
### 1. Dạng chuẩn 1NF (First Normal Form)
- Một bảng cơ sở dữ liệu được gọi là ở dạng chuẩn hóa dữ liệu 1NF khi toàn bộ các miền giá trị của các cột trong bảng đều chỉ chứa các giá trị nguyên tử (nguyên tố) và mỗi cột chỉ chứa một giá trị từ miền.
Ví dụ về bảng lưu trữ tên và số điện thoại của khách hàng:

| Customer ID | First Name | Surname | Telephone Number                     |
| ----------: | ---------- | ------- | ------------------------------------ |
|         123 | Pooja      | Singh   | 555-861-2025, 192-122-1111           |
|         456 | San        | Zhang   | (555) 403-1659 Ext. 53; 182-929-2929 |
|         789 | John       | Doe     | 555-808-9633                         |

Bảng này đang vi phạm 1NF vì cột Telephone Number chứa nhiều giá trị (nhiều số điện thoại) nên các giá trị trong cột không phải là nguyên tố mà có thể được chia thành hai số. 

Chỉnh sửa để đưa về dạng chuẩn 1NF:

| Customer ID | First Name | Surname | Telephone Number       |
| ----------: | ---------- | ------- | ---------------------- |
|         123 | Pooja      | Singh   | 555-861-2025           |
|         123 | Pooja      | Singh   | 192-122-1111           |
|         456 | San        | Zhang   | 182-929-2929           |
|         456 | San        | Zhang   | (555) 403-1659 Ext. 53 |
|         789 | John       | Doe     | 555-808-9633           |

### 2 Dạng chuẩn 2NF (Second Normal Form)
Một quan hệ đủ tiêu chí là dạng chuẩn hóa dữ liệu 2NF nếu quan hệ đó:
- Là 1NF
- Các thuộc tính không khoá phải phụ thuộc hàm đầy đủ vào khoá chính
Ví dụ 1: Cho quan hệ R = (ABCD), khoá chính là AB và tập phụ thuộc hàm là F = {AB => C, AB => D} là quan hệ đạt chuẩn 2NF.

Ví dụ 2: Cho quan hệ R = (ABCD), khoá chính là AB và tập phụ thuộc hàm là F = {AB => C, AB => D, B => DC} là quan hệ không đạt chuẩn 2NF vì có B => DC là phụ thuộc hàm không đầy đủ vào khoá chính. Chúng ta sẽ đưa về dạng chuẩn 2NF như sau:
 - Tách R:
      - R1(B,D,C), khóa là B
      - R2(A,B), khóa là AB
### 3. Dạng chuẩn 3NF (Third Nomal Form)
Một quan hệ đủ tiêu chí là dạng chuẩn hóa dữ liệu 3NF nếu quan hệ đó:
- Là 2NF
- Các thuộc tính không khoá phải phụ thuộc trực tiếp vào khoá chính

Ví dụ 1: Cho quan hệ R = (ABCDGH), khoá chính là AB và tập phụ thuộc hàm F = {AB -> C, AB -> D, AB -> GH} là quan hệ đạt chuẩn 3NF.
      - Tách R:
            - R1(G,D,H), khóa là G
            - R2(A,B,C,G), khóa là AB
        
Ví dụ 2: Cho quan hệ R = (ABCDGH) , khoá là AB và tập phụ thuộc hàm F = {AB -> C, AB -> D, AB -> GH, G -> DH}. Đây là quan hệ không đạt chuẩn 3NF vì có G -> DH là phụ thuộc hàm gián tiếp vào khoá. Chúng ta sẽ đưa nó về dạng chuẩn 3NF như sau:

