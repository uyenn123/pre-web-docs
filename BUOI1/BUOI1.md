# BUỔI 1: NHẬP MÔN CSDL
## I. CSDL là gì ?
- **Cơ sở dữ liệu** là một bộ tập hợp có tổ chức các dữ liệu liên quan tới nhau.
- “Dữ liệu” trong CSDL có thể bao gồm các đối tượng: chữ số, văn bản, đồ họa, video,…
- Một CSDL được thiết kế, xây dựng và sử dụng cho một số mục đích cụ thể. Nó được sử dụng bởi một tập người dùng và ứng dụng cụ thể ngay từ khi mới thiết kế.

```
CSDL SinhVien
 ├── MaSinhVien
 ├── Lop
 ├── MonHoc
 └── Diem
 ```

## II. Hệ quản trị CSDL
- Là một hệ thống phần mềm cho phép tạo lập CSDL và điều khiển mọi truy nhập đến CSDL đó.

Các đặc tính quan trọng của một hệ quản trị CSDL:
-	Cho phép người dùng tạo mới CSDL, thông qua ngôn ngữ định nghĩa dữ liệu. (DDLs – Data Definition Languages)
-	Cho phép người dùng truy vấn cơ sở dữ liệu, thông qua ngôn ngữ thao tác dữ liệu. (DMLs – Data Manipulation Languages)
-	Hỗ trợ lưu trữ số lượng lớn dữ liệu, thường lên tới hàng Gigabytes hoặc nhiều hơn, trong một thời gian dài. Duy trì tính bảo mật và tính toàn vẹn trong quá trình xử lý.
-	Kiểm soát truy nhập dữ liệu từ nhiều người dùng tại cùng một thời điểm.

Ví dụ một số hệ quản trị CSDL phổ biến:  MS SQL, MySQL,….

## IV. Câu lệnh tạo database, table trong MS SQL Server
### 1. Cách tạo Database trong SQL Server bằng T-SQL
T-SQL là phần mở rộng của SQL được sử dụng trong Microsoft SQL Server. Nếu bạn đang dùng SQL Server Management Studio, bạn có thể tạo database bằng cửa sổ truy vấn.
#### a.  Tạo database
```
CREATE DATABASE ten_database;
GO
```
Trong đó:
- CREATE DATABASE là câu lệnh dùng để tạo cơ sở dữ liệu mới.
- ten_database là tên database bạn muốn tạo.
- Tên database nên ngắn gọn, dễ hiểu, không dấu và hạn chế ký tự đặc biệt.
- Trong SQL Server, `GO` không phải là câu lệnh SQL chuẩn. Đây là lệnh phân tách batch, thường được dùng trong SSMS để kết thúc một nhóm lệnh.
  
Sau đó, bạn nhấn **Execute** hoặc **phím F5** để chạy lệnh.
Ví dụ:
```
CREATE DATABASE TestDB; 
GO
```

Câu lệnh trên sẽ tạo một database mới có tên là TestDB.
#### b. Sử dụng database
Sau khi tạo database, bạn có thể dùng lệnh USE để chuyển sang database đó:
```
USE TestDB;
GO
```
Từ thời điểm này, các lệnh tạo bảng, thêm dữ liệu hoặc truy vấn dữ liệu sẽ được thực hiện trong database TestDB.
#### c. Kiểm tra database đã tạo thành công
Bạn có thể kiểm tra bằng lệnh:
```
SELECT name
FROM sys.databases
WHERE name = ‘TestDB’;
```
Nếu kết quả trả về có TestDB, database đã được tạo thành công.

Ngoài ra, bạn có thể kiểm tra trong giao diện SSMS bằng cách vào Object Explorer > Databases, sau đó nhấn Refresh.

### 2. Câu lệnh tạo table trong MS SQL Server
```
CREATE TABLE TenBang (
    TenCot1 KieuDuLieu,
    TenCot2 KieuDuLieu,
    TenCot3 KieuDuLieu,
    ...
    TenCotN KieuDuLieu,
    PRIMARY KEY( mot hoac nhieu cot )
);
GO
```

**Ví dụ về lệnh CREATE TABLE**
Ví dụ về việc tạo bảng NHANVIEN với ID như khóa chính và NOT NULL là ràng buộc để đảm bảo các trường không thể NULL khi tạo các bản ghi trong bảng này.
```
CREATE TABLE NHANVIEN(
   ID   INT              NOT NULL,
   TEN VARCHAR (255)     NOT NULL,
   TUOI  INT             NOT NULL,
   DIACHI  CHAR (255) ,
   LUONG   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```
**Các điểm quan trọng cần nhớ về câu lệnh SQL CREATE TABLE**
- Câu lệnh CREATE TABLE được sử dụng để tạo bảng mới trong cơ sở dữ liệu.
- Câu lệnh này định nghĩa cấu trúc của bảng bao gồm tên và kiểu dữ liệu của các cột.
- Có thể sử dụng lệnh DESC table_name; để hiển thị cấu trúc của bảng đã tạo
- Chúng ta cũng có thể thêm ràng buộc vào bảng như NOT NULL, UNIQUE, CHECK và DEFAULT.
- Nếu cố gắng tạo một bảng đã tồn tại, MySQL sẽ báo lỗi. Để tránh lỗi này, bạn có thể sử dụng cú pháp CREATE TABLE IF NOT EXISTS.