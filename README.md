# Nguyễn Đình Tú , k225480106067
# k58.ktp-baitap2
nội dung bài tập  hai

Bài tập 02: Lập trình web.
==============================
NGÀY GIAO: 19/10/2025
==============================
DEADLINE: 26/10/2025
==============================
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.
2. NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
==============================
TIÊU CHÍ CHẤM ĐIỂM:
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ
==============================
GHI CHÚ:
1. yêu cầu trên cài đặt trên ổ D, nếu máy ko có ổ D có thể linh hoạt chuyển sang ổ khác, path khác.
2. có thể thực hiện trực tiếp trên máy tính windows, hoặc máy ảo
3. vì csdl là tuỳ ý: sv cần mô tả rõ db chứa dữ liệu gì, nhập nhiều dữ liệu test có nghĩa, json trả về sẽ có dạng như nào cũng cần mô tả rõ.
4. có thể xây dựng nhiều API cùng cơ chế, khác tính năng: như tìm kiếm, thêm, sửa, xoá dữ liệu trong DB.
5. bài làm phải có dấu ấn cá nhân, nghiêm cấm mọi hình thức sao chép, gian lận (sẽ cấm thi nếu bị phát hiện gian lận).
6. bài tập thực làm sẽ tốn nhiều thời gian, sv cần chứng minh quá trình làm: save file `readme.md` mỗi khoảng 15-30 phút làm : lịch sử sửa đổi sẽ thấy quá trình làm này!
7. nhắc nhẹ: github ko fake datetime được.
8. sv được sử dụng AI để tham khảo.
==============================
DEADLINE: 26/10/2025
==============================
./.

-----BÀI LÀM----

2. NỘI DUNG BÀI TẬP:
   
2.1. Cài đặt Apache web server:

🧩 CÀI ĐẶT APACHE WEB SERVER (E:\Apache24 – nguyendinhtu.com)

🔹 Bước 1: Vô hiệu hóa IIS (nếu đang chạy)

- Mở CMD với quyền Administrator

- Gõ lệnh:

iisreset /stop

<img width="1120" height="390" alt="Screenshot 2025-10-20 202519" src="https://github.com/user-attachments/assets/a946deb1-4c79-4e93-a1d4-a4a8980f330d" />

Bước 2: Tải và giải nén Apache

Tải Apache từ: https://www.apachelounge.com/download/

<img width="1758" height="975" alt="Screenshot 2025-10-20 202114" src="https://github.com/user-attachments/assets/4862e495-b34b-474e-8766-49305e7bda12" />

<img width="998" height="459" alt="Screenshot 2025-10-20 203106" src="https://github.com/user-attachments/assets/f926dbf1-47e5-43ac-abd6-512c81fcdbdc" />

sau khi tải về có được file như này

<img width="1084" height="289" alt="Screenshot 2025-10-20 203225" src="https://github.com/user-attachments/assets/7a089e0e-6f45-4c73-b456-905c8fa64f32" />

Giải nén vào ổ E:

<img width="795" height="485" alt="Screenshot 2025-10-20 203550" src="https://github.com/user-attachments/assets/5b42fd60-9e3a-4dab-ae00-aa0728de6b71" />

E:\Apache24\

Bước 3: Cấu hình file httpd.conf

Mở file:

E:\Apache24\conf\httpd.conf

Bằng Notepad hoặc Notepad++ → tìm và sửa các dòng sau:

Thay đổi thư mục DocumentRoot và Directory (đặt code web tại E:\Apache24\fullname):

DocumentRoot "E:/Apache24/nguyendinhtu"
<Directory "E:/Apache24/nguyendinhtu">
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>

Bật module VirtualHost (nếu bị comment):

Tìm dòng:

#Include conf/extra/httpd-vhosts.conf

→ bỏ dấu #:

Include conf/extra/httpd-vhosts.conf

<img width="921" height="220" alt="image" src="https://github.com/user-attachments/assets/84432bbb-06a2-4e7c-8134-30cab58c08a3" />

🔹 Bước 4: Cấu hình Virtual Host

Mở file:

D:\Apache24\conf\extra\httpd-vhosts.conf

Thêm đoạn cấu hình sau (thay fullname bằng tên không dấu liền nhau của bạn, ví dụ doduycop):

<VirtualHost *:80>
    ServerAdmin webmaster@nguyendinhtu.com
    DocumentRoot "E:/Apache24/nguyendinhtu"
    ServerName nguyendinhtu.com
    ErrorLog "logs/nguyendinhtu-error.log"
    CustomLog "logs/nguyendinhtu-access.log" common
</VirtualHost>

<img width="769" height="239" alt="image" src="https://github.com/user-attachments/assets/b99324df-6269-44f4-8851-adb557fa415e" />

- Tạo thư mục web:

E:\Apache24\nguyendinhtu

- Trong đó tạo file index.html để kiểm tra:

<!DOCTYPE html>
<html>
<head>
  <title>Website nguyendinhtu.com</title>
</head>
<body>
  <h1>Xin chào! Đây là website của Nguyễn Đình Tú</h1>
</body>
</html>

<img width="1177" height="504" alt="image" src="https://github.com/user-attachments/assets/0ae0d787-5813-4f48-8425-00892d120f2e" />

kết quả đạt được

<img width="1567" height="559" alt="image" src="https://github.com/user-attachments/assets/f06e1cf9-2e1f-4535-9162-5f50eddcfbe2" />

Bước 5: Cấu hình file hosts

Mở file:

C:\Windows\System32\drivers\etc\hosts

Chạy Notepad Run as Administrator, rồi thêm dòng sau vào cuối file:

127.0.0.1   nguyendinhtu.com

<img width="1002" height="216" alt="image" src="https://github.com/user-attachments/assets/361ea37e-6b18-4306-ad32-e3d30b94c97d" />

🔹 Bước 6: Cài đặt và khởi động Apache Server

- Mở CMD (Administrator) và chuyển đến thư mục:

cd D:\Apache24\bin

- Cài đặt Apache làm dịch vụ:

httpd.exe -k install

- Khởi động dịch vụ:

httpd.exe -k start

<img width="1077" height="555" alt="image" src="https://github.com/user-attachments/assets/61d3fabd-1d5c-4a09-9b03-30963b6336ff" />

- Kiểm tra xem Apache đang chạy:

Mở trình duyệt → gõ:
👉 [http://doduycop.com](http://nguyendinhtu.com/)

<img width="1602" height="577" alt="image" src="https://github.com/user-attachments/assets/ed44d338-77b9-4e6c-8001-4c09fa8ff7e6" />

Nếu thấy trang HTML bạn tạo → ✅ thành công!


2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
<img width="1094" height="115" alt="image" src="https://github.com/user-attachments/assets/f08f1c7b-3a48-486d-8a7e-5a0425ff1865" />
   + cài đặt vào thư mục `D:\nodejs`
<img width="773" height="96" alt="image" src="https://github.com/user-attachments/assets/deef6cb7-aa46-48a1-b408-f3ebe390312c" />
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
<img width="975" height="317" alt="image" src="https://github.com/user-attachments/assets/4afaf364-0295-43c9-9500-864d63f0fe99" />
   + download file: https://nssm.cc/release/nssm-2.24.zip
<img width="875" height="114" alt="image" src="https://github.com/user-attachments/assets/561569fa-5f6e-48f6-a578-2e7869b87db5" />
     giải nén được file nssm.exe
  <img width="844" height="118" alt="image" src="https://github.com/user-attachments/assets/469ced3c-a26e-43c4-9003-889e5b8b08c1" />
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
<img width="1022" height="405" alt="image" src="https://github.com/user-attachments/assets/14bc17cd-0383-4ced-ab08-0d1e51cc827d" />
   + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):

@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
<img width="988" height="179" alt="Screenshot 2025-10-21 201419" src="https://github.com/user-attachments/assets/9e25b96c-96fe-48ce-9a10-786ff78e903a" />
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`/
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  <img width="861" height="98" alt="Screenshot 2025-10-21 201601" src="https://github.com/user-attachments/assets/6f54c380-208b-408b-9112-c9898476eb7b" />
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
<img width="703" height="136" alt="Screenshot 2025-10-21 201755" src="https://github.com/user-attachments/assets/fd882b5f-23a3-4250-945a-e7e212ed3a39" />
kết quả đạt được 
<img width="1919" height="1051" alt="image" src="https://github.com/user-attachments/assets/d0dfd218-ca44-43de-8f39-01b56adc3a07" />








-

