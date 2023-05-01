# SercurityWeb
Chèn mã HTML và Cross-site Scripting (XSS)
Vị trí có thể bị chèn mã XSS
Nguyên nhân chính trang web bị dính lỗ hổng XSS là do ứng dụng web đã không filter kỹ và tin tưởng tuyệt đối vào input do người dùng nhập vào. Malicious user có thể lợi dụng điều này để chèn các mã javascript để thực hiện những hành vi xấu, tiêu biểu là đánh cắp cookie và giả danh người dùng.
Các dạng XSS: stored XSS, reflected XSS, dom-base XSS.
Ở trang web, chỉ có chức năng thêm, sửa thông tin sinh viên, nhân viên và điểm là nhận các input từ người dùng và lưu chúng vào database, dễ dàng bị stored XSS.
	Phòng chống
	Sử dụng hàm escapeXml có sẵn trong thư viện jstl để encode các thẻ <script></script>
2.1.1.3.	Thử nghiệm
 
Figure 1: Thử nghiệm XSS
2.1.2.	SQL injection
2.1.2.1.	Vị trí có thể chèn mã SQL injection
	SQL injection là hành vi tiêm các câu lệnh sql độc hại để thực hiện hành vi xấu như bypass login, thêm sửa xoá Database, Remote Code Excution...
	Các chức năng có thể bị SQL injection: Login, thêm sửa xoá thông tin sinh viên, nhân viên, điểm.
2.1.2.2.	Phòng chống
	Sử dụng các PrepareStatement. PreparedStatement là một interface con của Statement. Nó được sử dụng để thực thi các truy vấn được tham số hóa. Khi chúng ta truyền các tham số cho các values, giá trị của nó sẽ được thiết lập bằng việc gọi phương thức setter (setShort, setString…) của PreparedStatement.
	Các phương thức setter của PreparedStatement coi các giá trị truyền vào chỉ có thể là một value, nó sẽ loại bỏ hoàn toàn các ký tự lạ mà người dùng nhập vào trái phép. Do đó loại bỏ mối nguy hại của SQL Injection.
	Mã hoá Password lưu trong cơ sở dữ liệu.
2.1.2.3.	Thử nghiệm
 
Figure 2: Thực hiện SQLi để bypass login
 
Figure 3: Kết quả trả về của PreparedStatement là 0 (null)
 
Figure 4: Mã hoá mật khẩu lưu trong cơ sở dữ liệu
 
Figure 5: Mã hoá RSA trên Server
2.1.3.	Authentication Attack
2.1.3.1.	Vị trí có thể bị lỗi Authentication
	Authentication (xác thực) là một hành động nhằm thiết lập hoặc chứng thực một cái gì đó (hoặc một người nào đó) đáng tin cậy, từ đó được cung cấp các quyền lợi, truy vấn tương ứng với vật/người đã được xác thực. Sau khi bạn được xác thực, hệ thống sẽ biết người đang sử dụng tài khoản/dịch vụ đó chính là bạn.
	Lỗ hổng xác thực thường xuất hiện khi hệ thống chứa cơ chế xác thực lỏng lẻo, do người lập trình chưa được tiếp cận với các vấn đề an toàn lập trình dẫn đến kẻ tấn công có thể dễ dàng vượt qua (bypass) các lỗi logic hoặc ngoại lệ không mong muốn (unintended problems) trong cơ chế xác thực của hệ thống.
	Một số loại lỗ hổng xác thực:
•	Lỗ hổng trong xác thực mật khẩu (password): sử dụng kiểu tấn công brute-force, dựa vào thông tin phản hồi...
•	Lỗ hổng trong xác thực đa yếu tố (multi-factor authentication): bypass 2FA...
•	Lỗ hổng qua các cách xác thực khác: cookie dễ đoán và dễ bị bẻ khoá, chứng năng reset password...
2.1.3.2.	Phòng chống
	Sử dụng Captcha để chống brute-force
 
Figure 6: Thử nghiệm Captcha
	Thông tin phản hồi không chứa thông tin dễ đoán, hoặc không phản hồi về trường nào bị sai...
	Password phải được sử dụng có độ dài ít nhất 8 ký tự, chữ, chữ số, kí tự đặc biệt.
2.1.4.	Logic Attack
	Ứng dụng Web sử dụng Framework Java Spring MVC, nên với mỗi yêu cầu POST/GET sẽ có các xử lý khác nhau.
 
Figure 7: Xử lý request GET/POST với endpoint sinh-vien
	Các Endpoint yêu cầu trước đó phải xác thực mới truy cập được.
 
Figure 8: Người dùng phải đăng nhập trước đó để có session truy cập vào các enpoint
	Không tin tưởng tuyệt đối vào Input của Users.
 
Figure 9: HTML entity các giá trị
	Sử dụng mã hoá RSA256
2.1.5.	Tấn công vào trình duyệt web và sự riêng tư của người dùng
	Các giải pháp: Khi đưa ứng dụng lên môi trường product, nên sử dụng thêm WAF như cloudflare, chứng chỉ SSL...
2.1.6.	Xác thực người dùng và trao quyền truy cập (Access control)
	Yêu cầu người dùng đăng nhập bằng username/password.
 
Figure 10: Authentication
	Cấu hình đầy đủ Authentication, các endpoint trong ứng dụng Web đều yêu cầu Authentication để có thể truy cập và thay đổi.
 
Figure 11: Người dùng phải login thì mới có thể có cooke để truy cập vào các endpoint
 
Figure 12: Nhân viên thì sẽ không thể thêm nhân viên mới
 
Figure 13: Admin có thêm nhân viên mới
2.1.7.	Bảo mật phiên làm việc
	Các token được sinh ra ngẫu nhiên trên Framework Java Srping MVC, có độ dài lớn và khó đoán, khó thực hiện brute-force, không phụ thuộc vào các yếu tố khác.
	Không để token của phiên vài URL.
	Với Java Spring MVC, sau một thời gian người dùng không hoạt động sẽ tự động huỷ token.
 
Figure 14: Bảo mật phiên làm việc
	Ứng dụng Web có tính năng Log-Out, và session-cookie phiên đó sẽ bị huỷ và không thể sử dụng lại.
 
Figure 15: Tính năng Log-Out

2.1.8.	Bảo vệ máy chủ web
	Ứng dụng Web được xây dựng trên Framework Java Spring MVC nên máy chủ chỉ xử lý các Request đến các Endpoint được chỉ định, và các Endpoint không được chỉ định trước đó sẽ không thể truy cập. Do đó Attacker sẽ không thể duyệt các đường dẫn.
 
Figure 16: Bảo mật máy chủ Web
	Cũng như không có các tài khoản/nội dung ngầm định, không có chế độ debug.
	Chỉ định những phương thức nào sẽ được đưa vào những hàm nào xử lý.
2.1.9.	Bảo mật hệ thống file
	Các chức năng trong ứng dụng Web Quản Lý Điểm Sinh Viên đều là chức năng nội bộ, yêu cầu có Username & Password với được truy cập.
	Các quyền của mỗi User được giới hạn nhất định.
	Trang Web không có bất cứ file backup nào, không public source.
	Với Java Spring MVC thì không thể liệt kê và duyệt các thư mục.
2.2.	An Toàn Cơ Sở Dữ Liệu
2.2.1.	Default and Weak Passwords
2.2.1.1.	Mô tả
	Trong quá trình cài đặt các hệ Quản trị Cơ sở dữ liệu thì sẽ có nhiều tài khoản được tạo mặc định ở trong các DBMS. Những tài khoản đó được tạo những mật khẩu mặc định hoặc không có mật khẩu. Điều này dẫn đến khi các kẻ tấn công kết nối vào được các cổng của CSDL dùng các mật khẩu mặc định hoặc là có thể dò tìm ra tài khoản đăng nhập vào được CSDL thực hiện hành vi phá hoại
	VD: 
•	Oracle: User: system / Password: manager
•	MySQL: User: root /Password: null
•	SQL Server: User: SA /Password: null
2.2.1.2.	Phòng chống
	Đặt hoặc thay đổi các mật khẩu mặc định cho các tài khoản tạo mặc định trong các CSDL. Mật khẩu phải đảm bảo dộ dài, độ phức tạp để đảm bảo các kẻ tấn công không thể dễ dàng dò tìm ra được.
2.2.1.3.	Thử nghiệm
	Sử dụng SQL Server thay đổi mật khẩu mạnh cho tài khoản SA.
 
Figure 17: Thay đổi mật khẩu mạnh cho tài khoản mặc định SA
 
Figure 18: Kết nối trong ứng dụng web
2.2.2.	SQL Injection in the DBMS ( Lỗi chèn mã SQL)
2.2.2.1.	Mô tả
	Là một lỗ hổng cực kì nghiêm trọng khi Input từ ứng dụng Web truyền đến Database không được kiểm tra chặt chẽ cho kẻ tấn công thực thi câu truy vấn ngay trên ứng dụng Web lên Database thực hiện các hành vi phá hoại như: chèn, sửa, xóa dữ liệu, đánh cắp thông tin trong CSDL hoặc chiếm quyền điều khiển hệ thống máy chủ CSDL.
2.2.2.2.	Phòng chống
	Phòng chống trên ứng dụng Web. Không được truyền các biến trực tiếp vào các câu truy vấn. Nên sử dụng các Framework cho chức năng chống SQL injection.
2.2.2.3.	Thử nghiệm
	Ứng dụng Web phòng chống SQL Injection bằng cách khi sử dụng câu truy vấn thông qua thư viện PreparedStatement. Kẻ tấn công không thể thực hiện câu truy vấn độc hại được.
 
Figure 19: Sử dụng thư viện để thực hiện câu truy vấn chống SQLi
2.2.3.	Excessive User & Group Privileges
2.2.3.1.	Mô tả
	Lỗ hổng này xuất hiện khi có nhiều người dùng hoặc nhóm người dùng được cấp quyền nhập cao quá mức cần thiết so với các công việc được giao dẫn tới lạm dùng quyền và khi kẻ tấn công có được tài khoản có quyền hạn cao có thể thực hiện những hành vi nguy hiểm.
	Trên thực tế nhiều người dùng CSDL chỉ được tạo để truy cập và nhập dữ liệu thôi mà được cấp quyền quản trị hoặc là chủ sở hữu CSDL. Người dùng đó có thể thực hiện các thao tác cao khác như : xóa, sửa, thêm table, xóa table,…
2.2.3.2.	Phòng chống
	Xem xét cấp quyền cho người dùng đúng với quyền hạn của người dùng đó. Thường xuyên kiểm soát, rà soát các người dùng có thực hiện đúng với quyền hạ trên CSDL.
2.2.3.3.	Thử nghiệm
	Cấp quyền cho user DBRead chỉ có quyền truy xuất dữ liệu trên Database QLDSV.
 
Figure 20: Tạo User DBRead chỉ có quyền trên database QLDSV
 
Figure 21: User chỉ có quyền đọc trong Database QLDSV
 
Figure 22: Thử trường hợp User DBRead muốn quyền Tạo Database
2.2.4.	Unnecessary Enabled DBMS Features
2.2.4.1.	Mô tả
	Trong các DBMS có những tính năng nâng cao có thể tương tác trực tiếp với hệ điều hành mà người dùng quản trị không cần thiết. Khi người quản trị không tắt các tính năng này. Điều này dẫn đến kẻ tấn công khi có được tài khoản DBMS có thể sử dụng các tính năng để làm tổn hại đến cả hệ thống hoặc chiếm cả quyền kiểm soát hệ điều hành.
	Ví dụ các tính năng nâng cao trong các DBMS:
•	Oracle: UTL_FILE cho phép người dùng có đặc quyền hệ thống tạo hoặc thay đổi đối tượng DIRECTORY.
•	MySQL: cho phép quyền trên User Table (mysql.user)
•	SQL Server: OLEDB Ad Hoc Query – OPENROWSET, xp_cmdshell : cho phép thực hiện command lên hệ điều hành
