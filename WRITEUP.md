# web-server-attack-seminar writeup

## Apache Server
*Target:* Đọc được nội dung file /etc/passwd.

*Process:*
- Đề bài cho chúng ta một trang web giới thiệu về một công ty nào đó, web có các chức năng giới thiệu về công ty. 

![Giao diện trang web](images/apache_1.jpg)

- Trong quá trình recon, team đã sử dụng công cụ nmap để tìm hiểu về dịch vụ đang chạy trên web server này.  Sử dụng cờ `-sV` và ta có kết quả scan như sau:

![Kết quả scan nmap](images/apache_2.jpg)

- Osint trên google về các lỗi bảo mật của phiên bản **Apache 2.4.49** ta tìm được *CVE-2021-41773* về lỗi Path Traversal:

:![Kết quả tra cứu google](images/apache_3.jpg):

- Lỗi này xảy ra khi phiên bản này không kiểm tra được đầy đủ các kĩ thuật encoding url truyền vào. Cụ thể ở đây phiên bản này ko nhận ra dấu chấm thứ hai do đó không hiểu được `.%2/` là `../`.
- Dùng lệnh curl cùng với bypass `.%2/` để đọc file passwd (Ta có thể truy xuất trực tiếp file này nên dự đoán web này còn có lỗi miss config khi cho phép mọi user truy cập trực tiếp được đến các file trong directory của nó. Ví dụ như config Require all granted):

![Nội dung file passwd](images/apache_4.jpg)

*Flag:*  F1301{Apache2_ư3b_s3rv3r_vuln_1337}

## Nginx Server
*Target:* RCE và lấy được nội dung file flag trong thư mục root.

*Process:*
- Truy cập vào trang web, ta sẽ thấy nơi để upload file. Team tiến hành kiểm tra các loại lỗi liên quan đến upload file.

![Giao diện trang web](images/nginx_1.jpg)

- Vì team được đưa source code nên ta có thể xác định file type:

![Nội dung file source code](images/nginx_2.jpg)

- Code không nói tới file `.php`, ta sẽ thử file có đuôi này và kết quả như sau:

![Upload file đuôi php](images/nginx_3.jpg)

- Team thử viết 1 đoạn mã php bất kì sau đó chuyển định dạng (đuôi file) thành .png/jpg...(các loại được phép) thì phát hiện hệ thống chỉ check theo tên file chứ ko validate theo nội dung &rarr; suy ra có thể thao túng được nội dung file. Idea là sẽ tìm các nào để hệ thống thực thi file dưới dạng code php.

![Đổi tên đuôi file rồi upload lên](images/nginx_4.jpg)

&rarr; Upload file thành công. Nhưng khi truy cập vào đường dẫn tới file đó không hề có webshell được gửi lại.
- Xem file config:

![Nội dung file conf](images/nginx_5.jpg)

- Ở đây các file kết thúc bằng .php sẽ chuyển cho fastcgi xử lý. Tại file php.ini mặc định tham số `cgi.fix_pathinfo=1`, khi nhận ra tệp abcd.php không tồn tại, nên tệp tin được xử lý sẽ là tệp tin trước tệp tin hiện tại - chính là file ảnh vừa được upload, được coi như một file php để thực thi, dẫn đến kết quả trả về nội dung của tệp ảnh.
- Ta thử truy cập đường dẫn tới file vừa upload và thêm `./php` vào sau cùng:

![Thêm đuôi ./php](images/nginx_6.jpg)

- Trang web hiện lỗi php.
- Kiểm tra gói tin có phương thức POST:

![Nội dung gói tin có phương thức post trong burpsuite](images/nginx_7.jpg)

- Ta thấy đoạn code php mà ta đính vào file vừa mới upload lên.
- Thử với `phpinfo()` và bấm **Send**, sau đó **F5** trang web:

![Gửi gói tin trong burpsuite](images/nginx_8.jpg)
![Nội dung trang web hiển thị](images/nginx_9.jpg)

- Kiểm tra mục **disable_functions** và thấy web đã tắt nhiều lệnh dùng để thực thi như **exec**, **shell_exec**,... Mục đích kiểm tra là để xác định các hàm có thể hoặc không thể sử dụng để exploit.

![Các function đã bị tắt đi](images/nginx_10.jpg)

- Thay vì cố gắng sử dụng mọi cách để lấy shell, ta sẽ sử dụng các hàm của php và dùng chúng thay cho các lệnh bên shell.
- Trước tiên ta cần biết directory có những cái gì &rarr; sử dụng scandir và đường dẫn là thư mục root ( / ).

![Upload lện scandir](images/nginx_11.jpg)

- Sau khi bấm **Send** và **F5**, ta được:

![Kết quả lệnh scandir](images/nginx_12.jpg)

- Phát hiện file flag trong thư mục này, tiến hành sử dụng readfile: 

![Đọc nội dung file flag](images/nginx_13.jpg)

- Kết quả:

![Nội dung file flag](images/nginx_14.jpg)

*Flag:* F1301{Nginx_parsing_is_not_safe,right?}