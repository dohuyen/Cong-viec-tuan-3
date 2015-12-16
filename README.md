#**I.Tạo sửa xóa User , Group - Linux : Phân quyền File , User**

##**A.Tạo sửa xóa User , Group - Linux**
###**1. Tạo user:** 
*# useradd [options]*  
 - Các tùy chọn:
- u UID
- g GID
- G GID : Danh sách các Secondary group có thể cách nhau bằng dấu phẩy ","
- c : ghi chú
- d directory
- m Nếu như thư mục của -d chưa có, thì tự động tạo mới.
- s shell : mặc nhiên sẽ dùng bash shell

###**2. Sửa user:** 
*#usermod [options] [-l ]*
- Options: hầu hết giống như của lệnh useradd.
-l, login_name : thay đổi tên đăng nhập của người dùng. Trong một số trường hợp, tên thư mục riêng của người dùng có thể sẽ thay đổi để tham chiếu đến tên đăng nhập mới.

###**3.Xóa user:** 
*# userdel [-r]*
- r : xóa luôn home directory

###**4.Tạo group:** 
*# groupadd [-g groupid]*

###**5.Sửa group:** 
*# groupmod [-n New name] [-g new goupid]*
- u, uid : thay đổi chỉ số người dùng.

###6. Xóa group:**
*# groupdel<groupname>*

##**B.Phân quyền file , user**

 Mỗi đối tượng file được gắn với ba loại quyền: read (đọc), write (sửa đổi) và execute (thực thi). Và mỗi quyền này lại được chỉ định cho ba loại user:
- **owner** : chủ sở hữu của đối tượng – mặc định ban đầu là user tạo ra đối tượng đó.
-	**group** : nhóm các user chia sẻ chung quyền hạn truy cập - mặc định ban đầu là group mà owner ở trên thuộc về.
-	**others** : tất cả các user không thuộc hai nhóm trên.

+ **User root (*siêu người dùng*)** có đủ tất cả các quyền đối với mọi đối tượng trên hệ thống. Ngoài ra, root có thể thay đổi quyền hạn truy cập đối tượng cho bất kỳ user nào và còn có thể chuyển quyền sở hữu đối tượng qua lại giữa các user.
+ Ý nghĩa của ba loại quyền này là:
 - **Read:** cho phép đọc.
 - **Write:** cho phép chỉnh sửa nội dung, xóa file; cho phép tạo và xóa các đối tượng trong thư mục.
 - **Excute:** cho phép thực thi hoặc truy cập nếu đó là thư mục.

**Các quyền cho mỗi đối tượng được biểu diễn theo hai cách**

**Cách thứ nhất** là biểu diễn bằng một chuỗi gồm 10 ký tự:
+	Ký tự đầu thể hiện loại file:
+ Tệp tin thông thường
 - **d** : Thư mục
 - **l** : Liên kết
 - **c*** : Special file
 - **s** : Socket
 - **p** : Named pipe
 - **b** : Thiết bị
-	Ba ký tự tiếp là các quyền cho owner.
-	Kế đến là ba ký tự biểu diễn các quyền cho group.
-	Còn lại ba ký tự cuối dành cho other.

+ Quyền được phép read sẽ là r, write là w, e là excute. Các quyền bị cấm được biểu diễn bằng dấu –  .Để xem chi tiết các quyền của tệp tin, bạn sử dụng lệnh ls với tùy chọn -l .
$ ls -l 
 - rwxr-xr-x 1 ubuntu ubuntu 9144 2011-07-07 15:16 a.out
 - drwxr-xr-x 2 ubuntu ubuntu 4096 2011-07-07 15:29 Desktop
 - rw-r--r-- 1 ubuntu ubuntu 68 2011-07-07 15:16 main.c
+ Giải thích chi tiết:
 - **a.out** : đây là file thực thi thông thường
 - **Desktop** : đây là một thư mục
 - **main.c** : đây là file thường, chủ sở hữu có quyền đọc và ghi; nhóm có quyền đọc; người khác có quyền đọc

**Cách hai:** ngắn gọn hơn, gồm ba con số biểu diễn theo hệ bát phân.
 + Số đầu cho owner, số thứ hai cho group, số còn lại cho other. Mỗi một số nhận một trong tám giá trị sau:
- **0** : cấm tất cả các quyền
- **1** : execute
- **2** : write
- **3** : execute + write
- **4** : read
- **5** : read + execute
- **6** : read + write
- **7** : read + write + execute

*Chỉ cần đổi ra dạng nhị phân, bạn sẽ hiểu ngay được các con số này*
- Để thay đổi quyền hạn truy cập cho các user sử dụng lệnh **chmod** (bạn phải là owner của file hoặc có quyền root)
$ chmod 744 main.c # main.c sẽ có thuộc tính là: -rwxr--r--

+ Có một cách khác là sử dụng chmod bằng cách sử dụng ký tự đại diện. Các ký tự đại diện cho nhóm là:
 -	**u** : user sở hữu file
 -	**g** : group của user trên
 -	**o** : others
 -	**a** : tất cả ba ký hiệu ở trên

+ Các toán tử:
 -	**+** : thêm quyền
 -	**-** : bớt quyền
 -	**=** : gán quyền

- Để thay đổi owner cho đối tượng sử dụng lệnh chown (bạn phải có quyền root): $ chown User_Name File_Name
+  Để thay đổi group cho đối tượng sử dụng lệnh chgrp nhưng phải thỏa một trong hai điều kiện sau:
 - **1**.Bạn sử dụng quyền root
 - **2**.Bạn là chủ sở hữu và thuộc Group (có tên là Group_Name trong lệnh) mà bạn muốn thay đổi Group cho file: $ chgrp Group_Name File_Name .

 + Để biết mình thuộc các nhóm nào, bạn sử dụng lệnh 'id'. Lệnh này cho biết id cùng với tên của bạn và các nhóm mà bạn tham gia.
Các thuộc tính khác của tệp tin liên quan đến quyền truy cập rất đáng chú ý khác nữa là: Set user ID, set group ID, sticky bit. Các bít này bổ xung thêm cho các quyền đã mô tả ở trên. Sau đây là mô tả chi tiết ba bít này:
-	**SUID hay setuid:** thay đổi việc thực thi dành cho user ID. Nếu như setuid được đặt, khi tệp tin được thực thi bởi người dùng (user), tuyến trình được tạo ra sẽ có cùng quyền với tệp tin.
-	**SGID hay setgid:** thay đổi việc thực thi dành cho group ID. Giống như ở trên, nhưng kế thừa các quyền của nhóm. Đối với các thư mục điều đó có nghĩa là khi một tệp tin được tạo ra trong thư mục nó sẽ kế thừa nhóm của thư mục (và không ai là người tạo ra cả).
-	**Bít Sticky:** Hiện nay nó ứng xử phụ thuộc vào từng hệ thống và nó thường được sử dụng để chống việc xóa các tệp tin mà tập tin này có quan hệ với những người dùng khác trong thư mục nơi bạn có quyền "write".
Để biểu diễn các bít này, các bạn có thể sử dụng chữ số theo hệ bát phân như ở trên. Hay có thể biểu diễn theo dạng chuỗi ký tự như sau:
- **SUID:** Nếu được đặt, sẽ thay thế "x" trong quyền chủ sở hữu thành "s", nếu chủ sở hữu có quyền thực thi, còn nếu không thì thành "S". 
-	**SGID:** Nếu được đặt, sẽ thay thế "x" trong nhóm thành "s", nếu nhóm có quyền thực thi, còn nếu không thì thành "S". 
-	**Sticky:** Nếu được đặt, sẽ thay thế "x" trong others thành "t", nếu others có quyền thực thi, còn nếu không thì thành "T". 


#**II.Các câu lệnh tạo sửa xóa File , Folder**
- **mkdir** : tạo folder mới.
- **touch** : tạo file.
- **mkdir -p thưmục1/thưmục2** : tạo ra thư mục thưmục1 và thưmục2 cùng lúc.
- **rm file** :xóa bỏ tập tin file trong thư mục hiện hành.
- **mv** : Đổi tên hay di chuyển file hoặc folder.
- **rmdir** : Xóa một thư mục (chỉ xóa được file rỗng), muốn xóa thư mục không rỗng, các bạn dùng lệnh sau: rm–rf.
- **rmdir thưmục** :xóa bỏ folder trống mang tên thưmục .
- **rm -rf thưmục** : xóa bỏ folder mang tên thưmục với tất cả các tập tin trong đó (force).
- **lệnh >> file** : bổ sung kết quả của lệnh lệnh ở phần cuối của tập tin file.


#**III.Phân tích gói tin TCP**
+ Một gói tin TCP bao gồm 2 phần :
 - **header**
 - **dữ liệu**

![Cấu trúc gói tin TCP](http://voer.edu.vn/file/6227.png)
- *lưu ý* : Phần header có 11 trường trong đó 10 trường bắt buộc. Trường thứ 11 là tùy chọn (trong bảng minh họa có màu nền đỏ) có tên là: **Options**

##**Header**
- **Source port** : Số hiệu của cổng tại máy tính gửi.
- **Destination port** : Số hiệu của cổng tại máy tính nhận.
- **Sequence number** :Trường này có 2 nhiệm vụ. Nếu cờ SYN bật thì nó là số thứ tự gói ban đầu và byte đầu tiên được gửi có số thứ tự này cộng thêm 1. Nếu không có cờ SYN thì đây là số thứ tự của byte đầu tiên.
- **Acknowledgement number** : Nếu cờ ACK bật thì giá trị của trường chính là số thứ tự gói tin tiếp theo mà bên nhận cần.
- **Data offset** : Trường có độ dài 4 bít quy định độ dài của phần header (tính theo đơn vị từ 32 bít). Phần header có độ dài tối thiểu là 5 từ (160 bit) và tối đa là 15 từ (480 bít).
- **Reserved** : Dành cho tương lai và có giá trị là 0.
+ **Flags (hay *Control bits*)** : Bao gồm 6 cờ :
 - **URG** :Cờ cho trường Urgent pointer
 - **ACK** :Cờ cho trường Acknowledgement
 - **PSH** :Hàm Push
 - **RST** :Thiết lập lại đường truyền
 - **SYN** :Đồng bộ lại số thứ tự
 - **FIN** :Không gửi thêm số liệu
- **Window** : Số byte có thể nhận bắt đầu từ giá trị của trường báo nhận (ACK)
+ **Checksum** : 16 bít kiểm tra cho cả phần header và dữ liệu. Phương pháp sử dụng được mô tả trong RFC 793:
 - 16 bít của trường kiểm tra là bổ sung của tổng tất cả các từ 16 bít trong gói tin. Trong trường hợp số octet (khối 8 bít) của header và dữ liệu là lẻ thì octet cuối được bổ sung với các bít 0. Các bít này không được truyền. Khi tính tổng, giá trị của trường kiểm tra được thay thế bằng 0. Nói một cách khác, tất cả các từ 16 bít được cộng với nhau. Kết quả thu được sau khi đảo giá trị từng bít được điền vào trường kiểm tra.
- Các địa chỉ nguồn và đích là các địa chỉ IPv4. Giá trị của trường protocol là 6 (giá trị dành cho TCP). Giá trị của trường TCP length field là độ dài của toàn bộ phần header và dữ liệu của gói TCP.

- **Urgent pointer** : Nếu cờ URG bật thì giá trị trường này chính là số từ 16 bít mà số thứ tự gói tin (sequence number) cần dịch trái.
- **Options** : Đây là trường tùy chọn. Nếu có thì độ dài là bội số của 32 bít.

##**Dữ liệu**
- Trường cuối cùng không thuộc về header. Giá trị của trường này là thông tin dành cho các tầng trên (trong mô hình 7 lớp OSI). Thông tin về giao thức của tầng trên không được chỉ rõ trong phần header mà phụ thuộc vào cổng được chọn.


#**IV.Các câu lệnh về Network**

##**1. Lệnh Ping**
- Cú pháp: **ping ip/host/[/t][/a][/l][/n]**
- **Ip:** địa chỉ IP của máy cần kiểm tra; host là tên của máy tính cần kiểm tra kết nối mạng (có thể sử dụng địa chỉ IP hoặc tên của máy tính).
- **/t:** sử dụng để máy tính liên tục "ping" đến máy tính đích, bấm Ctrl +C để dừng.
- **/a:** nhận địa chỉ IP từ tên máy tính (host).
- **/l:** xác định độ rộng của gói tin gửi đi kiểm tra.
- **/n:** Xác định số gói tin gửi đi.

- Công dụng : Sử dụng lện Ping để kiểm tra xem một máy tính có kết nối mạng không. Lệnh PING gửi các gói tin từ máy tính bạn tới máy tính đích, các bạn có thể xác định được tình trạng đường truyền hoặc xác định máy tính đó có kết nối hay không.

##**2. Lệnh Tracert :**
- Cú pháp : **Code: tracert ip/host** 
- Công dụng : Lệnh này sẽ cho phép bạn "nhìn thấy" đường đi của các gói
tin từ máy tính của bạn đến máy tính đích, xem gói tin của bạn vòng qua
các server nào, các router nào... Quá hay nếu bạn muốn thăm dò một
server nào đó.

##**3.Lệnh Net Send :** gửi thông điệp trên mạng (chỉ sử dụng trên hệ thống máy tình Win NT/2000/XP).
- Cú pháp: **Net send ip/host thông_điệp_muốn_gửi**
- Công dụng: Lệnh này sẽ gửi thông điệp tới máy tính đích (có địa chỉ IP hoặc tên host) thông điệp: thông_điệp_muốn_gửi. 
Trong mạng LAN, ta có thể sử dụng lệnh này để chat với nhau. Trong
phòng vi tính của trường các bạn có thể dùng lệnh này để ghẹo mọi
người!
 - Cũng có thể gửi cho tất cả các máy tính trong mạng LAN theo cấu trúc sau :** Code: Net send * thông_điệp_muốn_gửi** 

##**4. Lệnh Netstat :** 
- Cú pháp: **Code: Netstat [/a][/e][/n]**
 - Tham số /a: Hiển thị tất cả các kết nối và các cổng đang lắng nghe (listening) 
 - Tham số /e: hiển thị các thông tin thống kê Ethernet 
 - Tham số /n: Hiển thị các địa chỉ và các số cổng kết nối... Ngoải ra còn một vài tham số khác
- Các bạn hãy gõ Netstat /? để biết thêm Công dụng :Lệnh Netstat cho phép ta liệt kê tất cả các kết nối ra và vào máy tính của chúng ta. 

##**5. Lệnh IPCONFIG :** 
- Cú pháp: **Code: ipconfig /all**
- Công dụng: Lệnh này sẽ cho phép hiển thị cấu hình IP của máy tính bạn đang sử dụng, như tên host, địa chỉ IP, mặt nạ mạng... 

##**6. Lệnh FTP (truyền tải file):**
- Cú pháp: **Code: ftp ip/host**
- Nếu kết nối thành công đến máy chủ, bạn sẽ vào màn hình ftp, có dấu nhắc như sau: 
Code: ftp>_ Tại đây, bạn sẽ thực hiện các thao tác bằng tay với ftp,
thay vì dùng các chương trình kiểu Cute FTP, Flash FXP. Nếu kết nối
thành công, chương trình sẽ yêu cầu bạn nhập User name, Password. Nếu
username và pass hợp lệ, bạn sẽ được phép upload, duyệt file... trên
máy chủ. 

+ *Một số lệnh ftp cơ bản:* 
 - **cd thu_muc:** Chuyển sang thư mục khác trên máy chủ
 - **dir:** Xem danh sách các file và thư mục của thư mục hiện thời trên máy chủ
 - **mdir thu_muc:** Tạo một thư mục mới có tên thu_muc trên máy chủ
 - **rmdir thu_muc:** Xoá (remove directory) một thư mục trên máy chủ 
 - **put file:** tải một file file (đầy đủ cả đường dẫn. VD: c:\tp\bin\baitap.exe) từ máy bạn đang sử dụng lên máy chủ. 
 - **close:** Đóng phiên làm việc 
 - **quit:** Thoát khỏi chương trình ftp, quay trở về chế độ DOS command.

- Công dụng : FTP là một giao thức được sử dụng để gửi và nhận file
giữa các máy tính với nhau. Windows đã cài đặt sẵn lệnh ftp, có tác
dụng như một chương trình chạy trên nền console (văn bản), cho phép
thực hiện kết nối đến máy chủ ftp.

##**7. Lệnh Net View :**
- Cú pháp: **Code: Net View [\\computer|/Domain[:ten_domain]]**
- Công dụng: Nếu chỉ đánh net view [enter], nó sẽ hiện ra danh sách các
máy tính trong mạng cùng domain quản lý với máy tính bạn đang sử dụng. 
+ Nếu đánh net view \\tenmaytinh, sẽ hiển thị các chia sẻ tài nguyên
của máy tính tenmaytinh . Sau khi sử dụng lệnh này, các bạn có thể sử
dụng lệnh net use để sử dụng các nguồn tài nguyên chia sẻ này.

##**8. Lệnh Net Use :**
- Cú pháp: **Code: Net use \\ip\ipc$ "pass" /user:"***"**
- ip: địa chỉ IP của victim. 
- ***: user của máy victim 
- pass: password của user Giả sử ta có đc user và pass của victim có IP
là 68.135.23.25 trên net thì ta đã có thể kết nối đến máy tính đó rùi
đấy! 
- Công dụng: kết nói một IPC$ đến máy tính victim (bắt đầu quá trình xâm nhập). 

##**9. Lệnh Net User :**
- Cú pháp: **Code: Net User [username pass] [/add]**
- Username : tên user cấn add 
- pass : password của user cần add Khi đã add được user vào rùi thì ta tiến hành add user này vào nhóm administrator.
Code: Net Localgroup Adminstrator [username] [/add] 
+ Công dụng: 
 - Nếu ta chỉ đánh lệnh Net User thì sẽ hiển thị các user có trong máy .
 - Nếu ta đánh lệnh Net User [username pass] [/add] thì máy tính sẽ tiến hành thêm một người dùng vào. 

##**10. Lệnh Shutdown :**
- Cú pháp: **Code: Shutdown [-m \\ip] [-t xx] [-i] [-l] [-s] [-r] [-a] [-f] [-c "commet] [-d up x:yy] (áp dụng cho win XP)**

- **Tham số -m\\ip** : ra lệnh cho một máy tính từ xa thực hiên các lệnh shutdown, restart,.. 
- **Tham số -t xx** : đặt thời gian cho việc thực hiện lệnh shutdown.
- **Tham số -l** : logg off (lưu ý ko thể thực hiện khi remote)
- **Tham số -s** : shutdown 
- **Tham số -r** : shutdown và restart
- **Tham số -a** : không cho shutdown
- **Tham số -f** : shutdown mà ko cảnh báo 
- **Tham số -c "comment"** : lời cảnh báo trước khi shutdown 
- **Tham số -d up x:yy** : ko rõ Code: shutdown \\ip (áp dụng win NT) 

- Công dụng:  Shutdown máy tính. 

##**11. Lệnh DIR :**
- Cú pháp: **Code: DIR [drive:][path][filename]**
- Công dụng: Để xem file, folder. 

##**12. Lệnh DEL :** 
- Cú pháp: **Code: DEL [drive:][path][filename]**
- Công dụng: Xóa một file, thông thường sau khi xâm nhập vào hệ thống, ta
phái tiến hành xóa dấu vết của mình để khỏi bị phát hiện.

##**13. Lệnh tạo ổ đĩa ảo trên computer :**
- Cú pháp: **Code: Net use z: \\ip\C$ ( hoặc là IPC$ )**
- Công dụng: Tạo 1 đĩa ảo trên máy tính.

##**14. Lệnh Net Time :**
- Cú pháp: **Code: Net Time \\ip**
- Công dụng: Cho ta biết thời gian của victim, sau đó dùng lệnh AT để
khởi động chương trình. (các bạn có thể tham khảo lệnh AT tại phần
basic to hacking ở phần hacking and securities trong diễn đàn dttx.org)

##**15. Lệnh AT :**
- Cú pháp: **Code: AT \\ip** 
- Công dụng: Thông thường khi xâm nhập vào máy tính victim khi rút lui thì
ta sẽ tặng quà lưu niệm lên máy tính victim, khi đã copy troj hoặc
backdoor lên máy tính rùi ta sẽ dùng lệnh at để khởi động chúng.

##**16. Lệnh Telnet :**
- Cú pháp: **Code: telnet host port** 
- Công dụng: Kết nối đến host qua port xx 

##**17. Lệnh COPY :**
- Cú pháp: **Code: COPY /?** *Dùng lệnh trên để rõ hơn!* 
- Công dụng: Copy file.
- *lưu ý* : code ở đây được hiểu là mã lệnh các bạn không nên gõ vào chữ
code, nếu gõ code thì các bạn không thực hiện được những dòng mã lệnh
phía trên đâu.  

##**18. Lệnh SET :**
- Cú pháp: **Code: SET** 
- Công dụng: Displays, sets, or removes cmd.exe enviroment variables. 

##**19. Lệnh Nbtstat :**
- Cú pháp: **Code: Nbtstat /?** *Gõ lệnh trên để rõ hơn về lệnh này* 
- Công dụng: Display protocol statistic and curent TCP/IP connections using NBT (netbios over TCP?IP).








































 
