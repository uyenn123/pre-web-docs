# BUỔI 4: SQL NÂNG CAO
## I. Tối ưu truy vấn
### 1. Sử dụng lập chỉ mục phù hợp
-	Hãy tưởng tượng chúng ta có một cuốn sách trong thư viện mà không có mục lục. Ta sẽ phải kiểm tra từng kệ, từng hàng cho đến khi tìm thấy. Chỉ mục trong cơ sở dữ liệu tương tự như mục lục. Giúp nhanh chóng định vị dữ liệu cần thiết mà không phải quét toàn bộ bảng.
#### Cách chỉ mục hoạt động:
-	Chỉ mục là các cấu trúc dữ liệu giúp cải thiện tốc độ truy xuất dữ liệu. Chúng hoạt động bằng cách tạo một bản sao đã được sắp xếp của các cột được lập chỉ mục, cho phép cơ sở dữ liệu nhanh chóng xác định các hàng khớp với truy vấn của chúng ta, tiết kiệm rất nhiều thời gian.
-	Có 3 loại chỉ mục chính:
        -	Chỉ mục **clustered** – Sắp xếp dữ liệu vật lý dựa trên giá trị cột và phù hợp nhất cho dữ liệu tuần tự hoặc đã sắp xếp không trùng lặp, như khóa chính.
        -	Chỉ mục **non-clustered** – Tạo 2 cấu trúc tách biệt, phù hợp cho các bảng ánh xạ hoặc bảng thuật ngữ.
        -	Chỉ mục toàn văn **(full-text)** – Dùng để tìm kiếm các trường văn bản lớn, như bài viết hoặc email, bằng cách lưu vị trí của các thuật ngữ trong văn bản.
- Dưới đây là một số thực hành tốt:
    - Lập chỉ mục cho các cột được truy vấn thường xuyên. Nếu chúng ta thường tìm trong bảng bằng customer_id hoặc item_id, việc lập chỉ mục cho các cột đó sẽ tác động lớn đến tốc độ. Xem bên dưới cách tạo một chỉ mục:
        ```
        CREATE INDEX index_customer_id ON customers (customer_id);
        ```
    - Tránh sử dụng chỉ mục không cần thiết. Mặc dù chỉ mục rất hữu ích để tăng tốc các truy vấn SELECT, chúng có thể làm chậm nhẹ các thao tác INSERT, UPDATE và DELETE. Lý do là chỉ mục phải được cập nhật mỗi khi bạn sửa đổi dữ liệu. Vì vậy, quá nhiều chỉ mục có thể làm chậm hệ thống do tăng chi phí xử lý cho các lần sửa đổi dữ liệu. 
    - Chọn đúng loại chỉ mục. Các cơ sở dữ liệu khác nhau cung cấp các loại chỉ mục khác nhau. Chúng ta nên chọn loại phù hợp nhất với dữ liệu và mẫu truy vấn của mình. Ví dụ, chỉ mục B-tree là lựa chọn tốt nếu chúng ta thường tìm các khoảng giá trị.
### 2. Tránh dùng SELECT *
- Đôi khi, chúng ta có xu hướng dùng `SELECT *`để lấy tất cả các cột, kể cả những cột không liên quan đến phân tích. Mặc dù có vẻ tiện, nhưng điều này dẫn đến các truy vấn rất kém hiệu quả có thể làm chậm hiệu năng. 
- Cơ sở dữ liệu phải đọc và truyền nhiều dữ liệu hơn mức cần thiết, đòi hỏi sử dụng bộ nhớ cao hơn vì máy chủ phải xử lý và lưu trữ nhiều thông tin thừa.
- Chúng ta chỉ nên chọn các cột cụ thể cần dùng. Giảm thiểu dữ liệu không cần thiết không chỉ giúp mã nguồn gọn gàng, dễ hiểu mà còn tối ưu hiệu năng.
- Vì vậy, thay vì viết:
    ```
    SELECT * 
    FROM products;
    ```
- Chúng ta nên viết:
    ```
    SELECT product_id, product_name, product_price 
    FROM products;
    ```
### 3. Tránh truy xuất dữ liệu dư thừa hoặc không cần thiết
-	Chúng ta vừa thảo luận rằng chỉ chọn các cột liên quan là thực hành tốt để tối ưu truy vấn SQL. Tuy nhiên, việc giới hạn số lượng hàng truy xuất cũng quan trọng không kém, không chỉ là cột. Truy vấn thường chậm lại khi số lượng hàng tăng. 
-	Chúng ta có thể dùng LIMIT để giảm số hàng trả về. Tính năng này giúp tránh việc vô tình lấy hàng nghìn hàng dữ liệu khi ta chỉ cần làm việc với vài hàng. 
- 	Hàm LIMIT đặc biệt hữu ích cho các truy vấn kiểm tra/đối chiếu hoặc xem thử đầu ra của một bước chuyển đổi đang thực hiện. Nó lý tưởng cho việc thử nghiệm và hiểu cách mã của chúng ta hoạt động. Tuy nhiên, có thể không phù hợp cho các mô hình dữ liệu tự động, nơi cần trả về toàn bộ tập dữ liệu. 
- Dưới đây là ví dụ về cách LIMIT hoạt động:
    - SELECT name 
    - FROM customers 
    - ORDER BY customer_group DESC 
    - LIMIT 100;
### 4. Sử dụng join hiệu quả
- Khi làm việc với cơ sở dữ liệu quan hệ, dữ liệu thường được tổ chức thành các bảng riêng biệt để tránh dư thừa và tăng hiệu quả. Điều này có nghĩa là chúng ta cần truy xuất dữ liệu từ nhiều nơi và ghép chúng lại để có đầy đủ thông tin cần thiết.  
- Join cho phép kết hợp các hàng từ hai hoặc nhiều bảng dựa trên một cột liên quan giữa chúng trong một truy vấn, giúp thực hiện các phân tích phức tạp hơn.
- Có các loại join khác nhau và chúng ta cần hiểu cách sử dụng. Dùng sai loại join có thể tạo bản ghi trùng lặp trong tập dữ liệu và làm chậm hiệu năng:
    - **Inner join** chỉ trả về các hàng có khớp ở cả hai bảng. Nếu một bản ghi tồn tại ở một bảng nhưng không có ở bảng kia, bản ghi đó sẽ bị loại khỏi kết quả.
     ![](image/1.png)
    ```
    SELECT o.order_id, c.name
    FROM orders o
    INNER JOIN customers c ON o.customer_id = c.customer_id;
    ```
    - **Outer join** trả về tất cả các hàng từ một bảng và các hàng khớp từ bảng còn lại. Nếu không có khớp, các cột từ bảng không có hàng khớp sẽ nhận giá trị NULL. 
     ![](image/2.png)
    ```
    SELECT o.order_id, c.name
    FROM orders o
    FULL OUTER JOIN customers c ON o.customer_id = c.customer_id;
    ```
    - Left join bao gồm tất cả các hàng từ bảng bên trái và các hàng khớp từ bảng bên phải. Nếu không tìm thấy khớp, các cột từ bảng bên phải sẽ nhận NULL. 
    - Tương tự, right join bao gồm tất cả các hàng từ bảng bên phải và các hàng khớp từ bảng bên trái, điền NULL tại nơi không có khớp.
    ![](image/3.png)
    ![](image/4.png)
    ```
    SELECT c.name, o.order_id
    FROM customers c
    LEFT JOIN orders o ON c.customer_id = o.customer_id;
    ```
- Mẹo cho join hiệu quả:
    - **Sắp xếp thứ tự join hợp lý**. Nên bắt đầu với các bảng trả về ít hàng nhất. Điều này giảm lượng dữ liệu cần xử lý ở các bước join tiếp theo.
    - **Dùng chỉ mục trên các cột join**. Một lần nữa, chỉ mục là đồng minh của chúng ta. Dùng chỉ mục giúp cơ sở dữ liệu nhanh chóng tìm các hàng khớp.
    - **Cân nhắc dùng subquery hoặc CTE (Common Table Expressions)** để đơn giản hóa các join phức tạp:
    ```
    WITH RecentOrders AS (
    SELECT customer_id, order_id
    FROM orders
    WHERE order_date >= DATE('now', '-30 days') 
    )
    SELECT c.customer_name, ro.order_id
    FROM customers c
    INNER JOIN RecentOrders ro ON c.customer_id = ro.customer_id;
    ```
### 5. Phân tích kế hoạch thực thi truy vấn
- Hầu hết cơ sở dữ liệu cung cấp các chức năng như EXPLAIN hoặc EXPLAIN PLAN để trực quan hóa quy trình này. Các kế hoạch này cung cấp phần phân tích chi tiết từng bước về cách cơ sở dữ liệu sẽ truy xuất dữ liệu. Chúng ta có thể dùng tính năng này để xác định nơi có nút thắt hiệu năng và đưa ra quyết định tối ưu truy vấn một cách có cơ sở.
- Hãy xem cách dùng EXPLAIN để xác định nút thắt. Chúng ta sẽ chạy đoạn mã sau:
    ```
    EXPLAIN SELECT f.title, a.actor_name
    FROM film f, film_actor fa,  actor a
    WHERE f.film_id = fa.film_id and fa.actor_id = a.id 
    ```
- Sau đó, chúng ta có thể xem xét kết quả:
   ![](image/5.png)
- Một số hướng dẫn chung để diễn giải kết quả:
    - Quét toàn bảng **(full table scan)**: Nếu kế hoạch hiển thị quét toàn bộ bảng, cơ sở dữ liệu sẽ quét mọi hàng trong bảng, có thể rất chậm. Điều này thường cho thấy thiếu chỉ mục hoặc mệnh đề WHERE kém hiệu quả.
    - Chiến lược join kém hiệu quả: Kế hoạch có thể cho thấy cơ sở dữ liệu đang dùng thuật toán join kém tối ưu.
    - Các vấn đề tiềm ẩn khác: **Explain plan** có thể làm nổi bật các vấn đề khác, như chi phí sắp xếp cao hoặc lạm dụng bảng tạm.
### 6. Tối ưu mệnh đề WHERE
- Mệnh đề **WHERE** rất quan trọng trong truy vấn SQL vì cho phép lọc dữ liệu theo điều kiện cụ thể, đảm bảo chỉ trả về các bản ghi liên quan. Nó cải thiện hiệu quả truy vấn bằng cách giảm lượng dữ liệu được xử lý, điều này rất quan trọng khi làm việc với tập dữ liệu lớn. 
- Vì vậy, một mệnh đề WHERE đúng đắn có thể là đồng minh mạnh mẽ khi tối ưu hiệu năng truy vấn SQL. Dưới đây là một số cách tận dụng mệnh đề này:
    - Thêm điều kiện lọc phù hợp càng sớm càng tốt. Đôi khi, có mệnh đề WHERE là tốt nhưng chưa đủ. Cần lưu ý vị trí đặt mệnh đề. Loại bỏ càng nhiều hàng càng sớm trong mệnh đề WHERE có thể giúp tối ưu truy vấn.
    - Tránh dùng hàm trên các cột trong mệnh đề WHERE. Khi áp dụng hàm lên một cột, cơ sở dữ liệu phải áp dụng hàm đó cho mọi hàng trước khi có thể lọc kết quả. Điều này ngăn cản việc sử dụng chỉ mục hiệu quả.
- Ví dụ, thay vì: 
    ```
    SELECT * 
    FROM employees WHERE 
    YEAR(hire_date) = 2020;
    ```
- Chúng ta nên dùng: 
    ```
    SELECT * 
    FROM employees 
    WHERE hire_date >= '2020-01-01' AND hire_date < '2021-01-01';
    ```
- Dùng toán tử phù hợp. Hãy chọn các toán tử hiệu quả nhất phù hợp nhu cầu. Ví dụ, = thường nhanh hơn LIKE, và sử dụng khoảng ngày cụ thể nhanh hơn dùng các hàm như `MONTH(order_date)`.
  
Vì vậy, ví dụ, thay vì thực hiện truy vấn này:
```
SELECT * 
FROM orders 
WHERE MONTH(order_date) = 12 AND YEAR(order_date) = 2023;
```
Chúng ta có thể thực hiện như sau: 
```
SELECT * 
FROM orders 
WHERE order_date >= '2023-12-01' AND order_date < '2024-01-01';
```
### 7. Tối ưu truy vấn con (subquery)
- Trong một số trường hợp, khi viết truy vấn, chúng ta muốn linh hoạt lọc, tổng hợp hoặc join dữ liệu ngay trong cùng một truy vấn thay vì chạy nhiều truy vấn riêng lẻ. 
- Trong những trường hợp đó, chúng ta có thể dùng subquery. Subquery trong SQL là các truy vấn lồng bên trong một truy vấn khác, thường nằm trong các câu lệnh `SELECT`, `INSERT`, `UPDATE` hoặc `DELETE`. 
- Subquery có thể mạnh và nhanh, nhưng cũng có thể gây vấn đề hiệu năng nếu không dùng cẩn thận. Nguyên tắc là nên giảm thiểu việc dùng subquery và tuân theo một số thực hành tốt:
    - Thay thế subquery bằng join khi có thể. Join nhìn chung nhanh và hiệu quả hơn subquery.
    - Thay vào đó, dùng `common table expressions (CTE)`.  CTE giúp tách mã của chúng ta thành vài truy vấn nhỏ thay vì một truy vấn lớn, dễ đọc hơn nhiều.
    ```
    WITH SalesCTE AS ( 
            SELECT salesperson_id, SUM(sales_amount) AS total_sales 
            FROM sales GROUP BY salesperson_id ) 
    SELECT salesperson_id, total_sales 
    FROM SalesCTE WHERE total_sales > 5000;
    ```
    - Dùng subquery không tương quan (uncorrelated). Subquery không tương quan độc lập với truy vấn ngoài và có thể được thực thi một lần, trong khi subquery tương quan sẽ chạy cho mỗi hàng của truy vấn ngoài.
### 8. Dùng EXISTS thay cho IN đối với subquery
- Khi làm việc với `subquery`, chúng ta thường cần kiểm tra một giá trị có tồn tại trong một tập kết quả không. Ta có thể làm điều này bằng IN hoặc EXISTS, nhưng EXISTS thường hiệu quả hơn, đặc biệt với tập dữ liệu lớn.
- Mệnh đề IN sẽ đọc toàn bộ kết quả của subquery vào bộ nhớ trước khi so sánh. Ngược lại, EXISTS dừng xử lý subquery ngay khi tìm thấy một khớp. 
- Ví dụ cách sử dụng mệnh đề này:
    ```
    SELECT * 
    FROM orders o
    WHERE EXISTS (SELECT 1 FROM customers c WHERE c.customer_id = o.customer_id AND c.country = 'USA');
    ``` 
### 9. Hạn chế sử dụng DISTINCT
- Hàm này hữu ích trong một số trường hợp nhưng có thể tốn tài nguyên, đặc biệt với tập dữ liệu lớn. Có vài lựa chọn thay thế cho DISTINCT:
    - Xác định và loại bỏ dữ liệu trùng lặp trong quy trình làm sạch dữ liệu. Điều này ngăn trùng lặp xâm nhập vào cơ sở dữ liệu ngay từ đầu.
    - Dùng GROUP BY thay cho DISTINCT khi có thể. GROUP BY có thể hiệu quả hơn, đặc biệt khi kết hợp với các hàm tổng hợp. 
Vì vậy, thay vì thực hiện:
```
SELECT DISTINCT city FROM customers;
```
Chúng ta có thể dùng:
```
SELECT city FROM customers GROUP BY city;
```
### 10. Tận dụng các tính năng đặc thù của hệ quản trị
- Khi làm việc với dữ liệu, chúng ta tương tác bằng SQL thông qua Hệ quản trị cơ sở dữ liệu (DBMS). DBMS xử lý các lệnh SQL, quản lý cơ sở dữ liệu và đảm bảo tính toàn vẹn, bảo mật dữ liệu. Các hệ thống cơ sở dữ liệu khác nhau cung cấp những tính năng độc đáo có thể giúp tối ưu truy vấn. 
- Database hint là các hướng dẫn đặc biệt chúng ta có thể thêm vào truy vấn để thực thi hiệu quả hơn. Chúng hữu ích nhưng cần sử dụng thận trọng. 
- Ví dụ, trong MySQL, hint `USE INDEX` có thể buộc sử dụng một chỉ mục cụ thể:
    ```
    SELECT * FROM employees USE INDEX (idx_salary) WHERE salary > 50000;
    ```
- Trong SQL Server, hint OPTION (LOOP JOIN) chỉ định phương thức join: 
    ```
    SELECT * 
    FROM orders 
    INNER JOIN customers ON orders.customer_id = customers.id OPTION (LOOP JOIN); 
    ```
- Các hint này ghi đè tối ưu hóa truy vấn mặc định, cải thiện hiệu năng trong các kịch bản cụ thể.
- Mặt khác, partitioning và sharding là hai kỹ thuật để phân phối dữ liệu trên đám mây. 
    - Với partitioning, chúng ta chia một bảng lớn thành nhiều bảng nhỏ, mỗi bảng có khóa phân vùng riêng. Khóa phân vùng thường dựa trên dấu thời gian tạo hàng hoặc giá trị số nguyên. Khi thực thi truy vấn trên bảng này, máy chủ sẽ tự động định tuyến đến bảng phân vùng phù hợp với truy vấn. 
    - Sharding khá giống, ngoại trừ việc thay vì tách một bảng lớn thành các bảng nhỏ, nó tách một cơ sở dữ liệu lớn thành các cơ sở dữ liệu nhỏ hơn. Mỗi cơ sở dữ liệu này nằm trên một máy chủ khác nhau. Thay vì khóa phân vùng, khóa sharding định tuyến truy vấn chạy trên cơ sở dữ liệu thích hợp. Sharding tăng tốc độ xử lý vì tải được chia đều cho các máy chủ khác nhau. 
### 11. Giám sát và tối ưu thống kê của cơ sở dữ liệu
- Giữ cho thống kê của cơ sở dữ liệu luôn cập nhật là quan trọng để bộ tối ưu truy vấn có thể đưa ra quyết định chính xác, sáng suốt về cách thực thi truy vấn hiệu quả nhất. 
- Thống kê mô tả phân bố dữ liệu trong một bảng (ví dụ: số lượng hàng, tần suất các giá trị và độ phân tán của giá trị qua các cột), và bộ tối ưu dựa vào thông tin này để ước lượng chi phí thực thi truy vấn. Nếu thống kê lỗi thời, bộ tối ưu có thể chọn kế hoạch thực thi kém hiệu quả, như dùng sai chỉ mục hoặc chọn quét toàn bảng thay vì quét theo chỉ mục hiệu quả hơn, dẫn đến hiệu năng truy vấn kém.
- Nhiều cơ sở dữ liệu hỗ trợ cập nhật tự động để duy trì thống kê chính xác. Chẳng hạn, trong SQL Server, cấu hình mặc định sẽ tự động cập nhật thống kê khi có lượng dữ liệu thay đổi đáng kể. 
- Tuy nhiên, chúng ta có thể cập nhật thủ công trong trường hợp cập nhật tự động chưa đủ hoặc cần can thiệp thủ công. Trong SQL Server, có thể dùng lệnh `UPDATE STATISTICS` để làm mới thống kê cho một bảng hoặc chỉ mục cụ thể, trong khi ở `PostgreSQL`, có thể chạy lệnh `ANALYZE` để cập nhật thống kê cho một hoặc nhiều bảng.
``` 
-- Update statistics for all tables in the current database
ANALYZE;
-- Update statistics for a specific table
ANALYZE my_table;
```
### 12. Sử dụng stored procedure
- **Stored procedure** là tập lệnh SQL được lưu trong cơ sở dữ liệu để chúng ta không phải viết đi viết lại cùng một SQL. Có thể hình dung nó như một kịch bản tái sử dụng. 
- Khi cần thực hiện một tác vụ nhất định, như cập nhật bản ghi hoặc tính toán giá trị, chúng ta chỉ cần gọi `stored procedure1`. Nó có thể nhận đầu vào, thực hiện công việc như truy vấn hoặc sửa đổi dữ liệu và thậm chí trả về kết quả. `Stored procedure` giúp tăng tốc vì SQL được biên dịch sẵn, khiến mã của bạn gọn gàng và dễ quản lý hơn. 
- Chúng ta có thể tạo stored procedure trong PostgreSQL như sau:
```
CREATE OR REPLACE PROCEDURE insert_employee(
    emp_id INT,
    emp_first_name VARCHAR,
    emp_last_name VARCHAR
)
LANGUAGE plpgsql
AS $
BEGIN
    -- Insert a new employee into the employees table
    INSERT INTO employees (employee_id, first_name, last_name)
    VALUES (emp_id, emp_first_name, emp_last_name);
END;
$;

-- call the procedure
CALL insert_employee(101, 'John', 'Doe');
```
### 13. Tránh sắp xếp và nhóm không cần thiết
- Là người làm dữ liệu, chúng ta thích dữ liệu được sắp xếp và nhóm để rút ra thông tin dễ dàng hơn. Ta thường dùng `ORDER BY` và `GROUP BY` trong các truy vấn SQL.
- Tuy nhiên, cả hai mệnh đề này đều tốn tài nguyên tính toán, đặc biệt khi xử lý tập dữ liệu lớn. Khi sắp xếp hoặc tổng hợp dữ liệu, bộ máy cơ sở dữ liệu thường phải quét toàn bộ dữ liệu rồi sắp xếp, xác định nhóm và/hoặc áp dụng các hàm tổng hợp, thường dùng các thuật toán tiêu tốn tài nguyên. 
- Để tối ưu truy vấn, chúng ta có thể làm theo một số gợi ý:
    - Giảm thiểu sắp xếp. Chỉ dùng `ORDER BY` khi cần thiết. Nếu việc sắp xếp không quan trọng, loại bỏ mệnh đề này có thể giúp giảm đáng kể thời gian xử lý. 
    - Dùng chỉ mục. Khi có thể, hãy đảm bảo các cột tham gia `ORDER BY` và `GROUP BY` được lập chỉ mục. 
    - Đẩy việc sắp xếp lên tầng ứng dụng. Nếu có thể, hãy xử lý sắp xếp ở tầng ứng dụng thay vì trong cơ sở dữ liệu. 
    - Tiền tổng hợp dữ liệu. Với các truy vấn phức tạp liên quan đến `GROUP BY`, chúng ta có thể tổng hợp dữ liệu từ sớm hoặc trong `materialized view`, để cơ sở dữ liệu không phải tính đi tính lại cùng các phép tổng hợp.
### 14.Dùng UNION ALL thay vì UNION
- Khi muốn kết hợp kết quả từ nhiều truy vấn thành một danh sách, chúng ta có thể dùng các mệnh đề `UNION` và `UNION ALL`. Cả hai đều kết hợp kết quả của hai hoặc nhiều câu lệnh `SELECT` khi chúng có cùng tên cột. Tuy nhiên, chúng không giống nhau và sự khác biệt khiến chúng phù hợp cho các trường hợp sử dụng khác nhau.
- Mệnh đề `UNION` loại bỏ các hàng trùng lặp, điều này đòi hỏi thời gian xử lý nhiều hơn. 
![](image/6.png)
- Ngược lại, `UNION ALL` kết hợp kết quả nhưng giữ tất cả các hàng, bao gồm cả trùng lặp. Vì vậy, nếu không cần loại bỏ trùng lặp, chúng ta nên dùng `UNION ALL` để có hiệu năng tốt hơn.
![](image/7.png)
```
-- Potentially slower
SELECT product_id FROM products WHERE category = 'Electronics'
UNION
SELECT product_id FROM products WHERE category = 'Books';

-- Potentially faster
SELECT product_id FROM products WHERE category = 'Electronics'
UNION ALL
SELECT product_id FROM products WHERE category = 'Books';
```
### 15. Chia nhỏ các truy vấn phức tạp
- Làm việc với các tập dữ liệu lớn đồng nghĩa với việc chúng ta sẽ thường xuyên gặp các truy vấn phức tạp khó hiểu và khó tối ưu. Chúng ta có thể xử lý chúng bằng cách chia nhỏ thành các truy vấn nhỏ, đơn giản hơn. Bằng cách này, ta dễ dàng xác định các nút thắt hiệu năng và áp dụng kỹ thuật tối ưu.
- Một trong những chiến lược được dùng thường xuyên để chia nhỏ truy vấn là `materialized view`. Đây là kết quả truy vấn được tính sẵn và lưu trữ, có thể truy cập nhanh thay vì phải tính lại mỗi lần tham chiếu. Khi dữ liệu nền thay đổi, `materialized view` phải được làm mới thủ công hoặc tự động.
- Ví dụ cách tạo và truy vấn một `materialized view`:
```
-- Create a materialized view
CREATE MATERIALIZED VIEW daily_sales AS
SELECT product_id, SUM(quantity) AS total_quantity
FROM order_items
GROUP BY product_id;

-- Query the materialized view
SELECT * FROM daily_sales;
```
## II. Sử dụng index
### 1. Khái niệm 
- Index là một cấu trúc dữ liệu riêng biệt được tạo ra để tối ưu hóa quá trình truy xuất dữ liệu từ bảng trong cơ sở dữ liệu. Một cách đơn giản, nó hoạt động tương tự như mục lục của một cuốn sách, giúp bạn tìm kiếm nhanh hơn trong các bảng dữ liệu lớn.
1. Các loại Index trong SQL
•	Clustered Index: Đây là dạng chỉ mục sắp xếp lại dữ liệu thực tế trong bảng theo thứ tự của chỉ mục. Mỗi bảng chỉ có thể có một Clustered Index vì dữ liệu chỉ có thể được sắp xếp theo một thứ tự duy nhất.
•	Non-Clustered Index: Khác với Clustered Index, Non-Clustered Index không sắp xếp lại dữ liệu bảng, mà chỉ lưu trữ thông tin chỉ mục ở một khu vực riêng biệt.
•	3. Cách sử dụng index để tối ưu hóa truy vấn
•	1. Tạo index: Tạo bằng cú pháp:
•	CREATE INDEX index\_name ON table\_name  (colimn\_name);
•	Ví dụ CREATE INDEX idx\_userid ON users (user\_id);
•	2. Sử dụng EXPLAIN để kiểm tra hiệu suất truy vấn: Lệnh EXPLAIN cho phép bạn kiểm tra xem SQL có sử dụng đúng index khi truy vấn hay không:
•	EXPLAIN SELECT user\_id, email FROM users WHERE user\_id = ‘123’;
•	3. Thêm index vào bảng: Thêm index vào các cột thường xuyên được truy vấn giúp tăng tốc độ truy vấn.
•	ALTER TABLE table\_name ADD INDEX (column\_name);
•	Ví dụ: ALTER TABLE users ADD INDEX idx\_userid (user\_id);
1. Khi nào nên sử dụng Index?
•	Khi bảng có dữ liệu lớn và bạn thường xuyên thực hiện các truy vấn phức tạp.
•	Khi cần tối ưu hóa các câu lệnh SELECT với điều kiện WHERE, JOIN, và ORDER BY.
1. Hạn chế của Index
Mặc dù Index giúp cải thiện tốc độ truy vấn, nhưng nó cũng có những hạn chế nhất định, bao gồm:
•	Index làm tăng kích thước của cơ sở dữ liệu, đặc biệt khi bảng có nhiều Index.
•	Việc cập nhật, chèn thêm, hoặc xóa dữ liệu trong bảng có Index sẽ mất nhiều thời gian hơn vì SQL cần cập nhật cả chỉ mục.
1. Kết luận
Việc sử dụng Index trong SQL là một kỹ thuật quan trọng để tối ưu hóa hiệu suất truy vấn. Tuy nhiên, cần sử dụng Index một cách hợp lý, tránh lạm dụng để không gây quá tải cho hệ thống cơ sở dữ liệu.
1. Cách tạo Index trong SQL
Tạo Index trong SQL là một bước quan trọng để cải thiện hiệu suất truy vấn dữ liệu. Các Index giúp tăng tốc độ tìm kiếm và sắp xếp dữ liệu. Dưới đây là các bước cơ bản để tạo các loại Index phổ biến trong SQL.
1.	Clustered Index:
Clustered Index được tạo bằng cách sử dụng câu lệnh CREATE CLUSTERED INDEX. Dữ liệu trong bảng sẽ được sắp xếp theo thứ tự của cột mà bạn chỉ định.
Cú pháp:
CREATE CLUSTERED INDEX idx_name ON table_name (column_name);
Ví dụ:
CREATE CLUSTERED INDEX idx_emp_id ON Employees (EmployeeID);
Ký hiệu toán học:
•	Clustered Index = Sắp xếp vật lý của bảng
•	Non-Clustered Index:
•	Non-Clustered Index không thay đổi cách sắp xếp vật lý của bảng, chỉ tạo ra một chỉ mục riêng biệt trỏ đến dữ liệu thực tế. Bạn có thể sử dụng cú pháp tương tự nhưng thay thế từ "CLUSTERED" bằng "NONCLUSTERED".
•	Cú pháp:
•	CREATE NONCLUSTERED INDEX idx_name ON table_name (column_name);
•	Ví dụ:
•	CREATE NONCLUSTERED INDEX idx_emp_name ON Employees (EmployeeName);
•	Ký hiệu toán học:
•	Non-Clustered Index = Bảng tham chiếu đến dữ liệu thực
•	3. Unique Index:
•	Unique Index đảm bảo rằng không có hai hàng trong bảng có cùng giá trị trên các cột được lập chỉ mục. Đây là cách tuyệt vời để duy trì tính duy nhất cho dữ liệu trong các cột quan trọng.
•	Cú pháp:
•	CREATE UNIQUE INDEX idx_name ON table_name (column_name);
•	Ví dụ:
•	CREATE UNIQUE INDEX idx_emp_email ON Employees (Email);
•	Ký hiệu toán học:
•	4. Full-Text Index:
•	Full-Text Index được sử dụng cho các cột văn bản dài, cho phép tìm kiếm từ hoặc cụm từ một cách hiệu quả. Cú pháp cụ thể của Full-Text Index có thể khác nhau tùy vào hệ quản trị cơ sở dữ liệu bạn sử dụng.
•	Cú pháp:
•	CREATE FULLTEXT INDEX ON table_name (column_name) KEY INDEX idx_name;
•	Ví dụ:
•	CREATE FULLTEXT INDEX ON Documents (DocumentText) KEY INDEX idx_doc_id;
•	Ký hiệu toán học:
•	Full-Text Index = Tìm kiếm văn bản hiệu quả
1. Cách sử dụng Index hiệu quả
Để sử dụng Index một cách hiệu quả trong SQL, cần chú trọng vào việc tối ưu hóa hiệu suất truy vấn và giảm thiểu tác động không mong muốn. Dưới đây là một số cách thức giúp bạn sử dụng Index hiệu quả nhất.
1.	Chọn cột đúng để lập chỉ mục:
Các cột thường được sử dụng trong các câu truy vấn SELECT, WHERE, JOIN hoặc ORDER BY là ứng cử viên tốt cho việc tạo Index. Những cột chứa nhiều giá trị duy nhất sẽ mang lại hiệu suất cao hơn khi được lập chỉ mục.
Ký hiệu toán học:
Tối ưu chỉ mục = Cột thường xuyên được truy vấn
2.	Giới hạn số lượng Index:
Tạo quá nhiều Index trên một bảng có thể làm chậm tốc độ chèn, cập nhật và xóa dữ liệu do hệ thống phải quản lý nhiều chỉ mục. Cần cân nhắc giữa lợi ích truy vấn nhanh và chi phí xử lý dữ liệu.
3.	Sử dụng Index trong các trường hợp cụ thể:
Khi làm việc với các bảng lớn, các chỉ mục sẽ có hiệu quả hơn trong việc lọc và sắp xếp dữ liệu. Tuy nhiên, với các bảng nhỏ hoặc các truy vấn yêu cầu xử lý toàn bộ bảng, việc sử dụng Index có thể không cần thiết.
4.	Thường xuyên tái xây dựng hoặc cập nhật Index:
Khi dữ liệu thay đổi liên tục, chỉ mục có thể trở nên phân mảnh, dẫn đến giảm hiệu suất. Do đó, cần tái lập hoặc cập nhật các Index theo định kỳ.
Cú pháp:
ALTER INDEX idx_name ON table_name REBUILD;
5.	Tránh tạo Index trên các cột có nhiều giá trị trùng lặp:
Việc lập chỉ mục trên các cột có giá trị trùng lặp cao, chẳng hạn như cột chứa các giá trị Boolean hoặc giới tính, sẽ không mang lại hiệu quả cao và có thể làm giảm hiệu suất.
Ký hiệu toán học:
Hiệu suất Index thấp = Nhiều giá trị trùng lặp
6.	Chọn loại Index phù hợp:
Tùy thuộc vào nhu cầu truy vấn, việc chọn đúng loại chỉ mục sẽ mang lại hiệu suất cao nhất. Ví dụ, Clustered Index giúp sắp xếp vật lý dữ liệu trong bảng, trong khi Non-Clustered Index chỉ lập chỉ mục và trỏ đến dữ liệu thực tế.
Áp dụng các phương pháp trên sẽ giúp tăng hiệu suất hệ thống và tối ưu hóa thời gian truy vấn, đồng thời giảm thiểu tác động tiêu cực từ việc sử dụng chỉ mục không hiệu quả.
II.	Khái niệm Transaction, ACID, dirty read, dirty write

•	
-	
