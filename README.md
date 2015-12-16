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

![Cấu trúc gói tin TCP](https://2466b9f4-a-62cb3a1a-s-sites.googlegroups.com/site/lexuandin/home/hhhhhhh.png?attachauth=ANoY7cqUORJMnJlfmq_UOidhurrNimAD3KNDazIfpijXXODeXsf2cAvAPgJ926IO_OiD8yAdRbFg5ApnJGHppQIIjCnTLLBD5-VzNcLNe7Q6KXpbdqWGwrhnScZbekZuj5x1cEdFSUlT_Eghpm3t425DNgimsuUxRHMyQMY_iVbYjfDIdDIuTQWZG4HDfu9sgxY1bq32FBeJ8dL1tI6IpgV8HShLrSSjxw%3D%3D&attredirects=0.png)


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


#**IV.Các câu lệnh về Network trong Linux**

##1.**Curl và Wget**
- Sử dụng lệnh curl hoặc wget để tải một file từ internet mà không cần đầu cuối. Với lệnh curl, gõ curl-O đường dẫn tới file. Người sử dụng có thể sử dụng lệnh wget mà không cần thêm tùy chọn nào. File sẽ xuất hiện ở đường dẫn.
- Curl-O website.com/file
- Wget website.com/file

##2.**Ping**
-	Cú pháp : `$ ping ADDRESS`
- Lệnh ping gửi các gói ECHO_REQUEST tới địa chỉ chỉ định. Câu lệnh nhằm kiểm tra máy tính có thể kết nối với Internet hay một địa chỉ IP cụ thể nào đó hay không. 
- lệnh ping trên Linux sẽ duy trì gửi các gói tin cho đến khi bạn kết thúc nó. Có thể định số lượng gói tối đa gửi đi bằng cách gõ thêm tùy chọn –c.

##3.**Tracepath và Traceroute**
- Lệnh tracepath cũng tương tự như traceroute nhưng nó không đòi hỏi các quyền quản trị. Nó cũng được cài đặt mặc định trên Ubuntu còn tracerout thì không. Lệnh tracepath lần dấu đường đi trên mạng tới một đích chỉ định và báo cáo về mỗi nút mạng (hop) dọc trên đường đi. Nếu gặp phải các vấn đề về mạng, lệnh tracepath có thể chỉ ra vị trí lỗi mạng.
- Tracepath example.com

##4.**Mtr**
- Lệnh mtr là sự kết hợp ping và tracepath trong một câu lệnh đơn lẻ. mtr sẽ gửi liên tục các gói và hiển thị thời gian ping cho mỗi nút mạng. Câu lệnh cũng giúp phát hiện một số vấn đề mạng. 

##5.**Host**
- Lệnh host sẽ thực hiện tìm kiếm DNS. Nhập vào tên miền khi muốn xem địa chỉ IP đi kèm và ngược lại, nhập vào địa chỉ IP khi muốn xem tên miền đi kèm.
- Host howtogeek.com
- Host 208.43.115.82

##6.**Whois**
- Lệnh whois sẽ đưa ra các bản ghi trên server whois (whois record) của website, vì vậy bạn có thể xem thông tin về người hay tổ chức đã đăng ký và sở hữu website đó.
- whois example.com

##7.**Ifplugstatus**
- Lệnh ifplugstatus giúp kiểm tra dây cáp có được cắm vào giao diện mạng hay không. Câu lệnh này không được cài đặt mặc định trên Ubuntu. Sử dụng câu lệnh sau để cài đặt nó :
- sudo apt-get install ifplugd
+ Chạy các câu lệnh sau để xem trạng thái tất cả các giao diện hay chỉ xem trạng thái một giao diện cụ thể.
 - ifplugstatus
 - ifplugstatus eth0

##8.**Ifconfig**
- **Ifconfig <tên interface>:** để xem thông tin chi tiết về các giao diện mạng; thông thường giao diện mạng ethernet có tên là eth(). Bạn có thể cài đặt các thiết lập mạng như địa chỉ IP hoặc bằng cách dùng lệnh này (xem man ifconfig). Nếu có điều gì đó chưa chính xác, bạn có thể stop hoặc start (tức ngừng hoặc khởi_động) giao diện bằng cách dùng lệnh ifconfig<tên_giao_diện> up/down.
Câu lệnh ifconfig có rất nhiều tùy chọn để cấu hình, điều chỉnh và dò lỗi trên các giao diện mạng hệ thống. Đây cũng là cách để xem nhanh các địa chỉ IP và các thông tin khác của giao diện mạng. Gõ ifconfig để xem trạng thái các giao diện mạng hiện đang hoạt động bao gồm tên của chúng. Bạn cũng có thể chỉ định tên một giao diện để xem thông tin trên duy nhất giao diện đó.
- ifconfig
- ifconfig eth0

##9.**Ifdown và ifup**
- Câu lệnh ifdown và ifup giống như ifconfig up hay ifconfig down. Hai câu lệnh thực hiện bật hoặc tắt giao diện chỉ định. Điều này yêu cầu quyền quản trị nên bạn phải dùng thêm từ khóa sudo trên Ubuntu.
- sudo ifdown eth0
- sudo ifup eth0

-Màn hình Linux sẽ báo lỗi khi được nhập những câu lệnh này. Nó thường sử dụng bộ NetworkManager cho phép quản lý giao diện mạng. Mặc dù vậy, các câu lệnh này vẫn sẽ hoạt động trên các server mà không cần dùng NetworkManager.

- Nếu bạn thực sự cần cấu hình NetworkManager từ giao diện dòng lệnh, sử dụng câu lệnh nmcli.
dhclient
- Lệnh dhclient giúp làm mới địa chỉ IP trên máy bằng cách giải phóng địa chỉ IP cũ và nhận một địa chỉ mới từ DHCP server. Công việc này yêu cầu quyền quản trị, vì vậy phải dùng thêm từ khóa sudo trên Ubuntu. Chạydhclient để nhận địa chỉ IP mới hoặc sử dụng tùy chọn –r để giải phóng địa chỉ IP hiện tại.
 - sudo dhclient –r
 - sudo dhclient

##10.**Netstat**
- Netstat là một công cụ cơ bản về gỡ lỗi mạng(network). Nó cung cấp cho chúng ta biết về các kết nối mạng, bảng định tuyến, cổng mạng nào đang được kích hoạt, v.v.
- Lệnh netstat có cú pháp đơn giản như sau: **$netstat -tùy chọn**
+ Dưới đây là một số tùy chọn kèm theo của lệnh netstat này.
 - **a:**  hiển thị tất cả các kết nối liên quan đến mạng
 - **n:**  không phân giải địa chỉ IP thành tên.
 - **t:**  hiển thị kết nối TCP
 - **u:** hiển thị kết nối UDP
 - **s:**  hiển thị kết nối theo giao thức: TCP,UP,ICMP .v.v.. Lệnh này còn đưa ra thống kế các gói tin ra vào trên cổng mạng.
 - **o:**  hiển thị thông tin về thời gian của kết nối.
 - **p:**  hiển thị PID và tên chương trình đang mở kết nối.
 - **l:**  chỉ hiển thị những Unix sockets.
 - **i:**  hiển thị các cổng mạng đang hoạt động.
 - **c:**  tùy chọn này sẽ làm mới (refresh) các kết quả trả về. Mặc định là 1 giây, ta có thể thay đổi thông số này.
 - **r:**  hiển thị bảng định tuyến của kernel
- Và còn nhiều tùy chọn khác.



 




































 
