# Ghi chép câu lệnh PING


###Mục lục:
[1. Giới thiệu về PING ](#1)

[2. Một số thông điệp trả về khi PING ](#2)

[3. Một số tham số thường dùng ](#3)

- [3.1 PING thông thường tới 1 host](#3.1)
- [3.2 PING với số gói ](#3.2)
- [3.3 PING với tùy chọn kích cỡ gói tin ](#3.3)
- [3.4 Tùy chọn thời gian gửi gói tin tiếp theo ](#3.4)
- [3.5 Gửi gói tin liên tiếp đến host ](#3.5)
- [3.6 Phân tích kết quả với PING ](#3.6)
- [3.7 PING trong khoảng thời gian ](#3.7)

[4. Kết luận ](#4)

<a name="1"></a>
### 1. Giới thiệu về PING

`PING` (viết tắt của Packet InterNet Groper) là một tiện ích để kiểm tra hoặc xác định có một địa chỉ IP hay một host/server đang hoạt động hay không. Công cụ này thường dùng để kiểm tra và chuẩn đoán lỗi với các vấn đề trong mạng máy tính. Cách hoạt động của nó vô cùng đơn giản là gửi đi một bản tin đến một địa chỉ, host/server nào đó và chờ bản tin phản hồi trả về.

PING là một phương pháp chính để troubleshoot cho bất kỳ các kết nối mạng nào. Nó gửi đi các thông điệp chứa thông tin PING và nhận về các phản hồi từ các host/server. Nó cho ta biết thời gian gói tin được trả về từ các host/server.

Ngày nay, tiện ích này được cài đặt sẵn trên các hệ điều hành máy tính. Trên Windows, bạn có thể dùng Command Prompt. Trên một số hệ điều hành khác như LINUX, MAC OS là Terminal.

**Chú ý**: Một số host hoặc server vì lý do bảo mật mà có thể chặn các gói tin PING.

<a name="2"></a>
### 2. Một số thông điệp trả về khi PING:

- Request time out: thực hiện gửi gói thành công nhưng không nhận được gói phản hồi. (Lỗi ở Phía xa)
- Destination host unreachable: đích đến không tồn tại hoặc đang cô lập. (Lỗi ở phía mình)
- Reply from 203.162.4.190 byte=32 time <1ms TTL 124: Gửi gói đến địa chỉ IP: 203.162.4.190 với độ dài gói 32 byte, thời gian phản hồi dưới 1 mili giây, TTL (time to live - vòng đời gói) 124. Phản ánh trạng thái gói gửi và tín hiệu phản hồi. TTL mỗi khi đi qua một ROUTER thì sẽ giảm đi 1 đơn vị và đi qua không quá 30 host. Host sử dụng Windows (Mặc định 128) TTL > 98, Dùng Linux (Mặc định 64) > 34.

<a name="3"></a>
### 3. Một số tham số thường dùng

Thực hiện trên Terminal của CentOS 6
<a name="3.1"></a>
#### 3.1 PING thông thường tới 1 host

```
ping meditech.vn
```

<img src="http://image.prntscr.com/image/9beec10f16a84321947f2476812f4869.png" />

Trên Windows, nếu không có tham số nào đi cùng thì mặc định 4 gói tin được gửi đến host với size là 32 byte.

Khác với Windows, ở LINUX, nếu không có tham số đi cùng thì mặc định số gói tin sẽ không giới hạn với size là 64 byte.
Để ngừng việc ping, chúng ta thao tác [Ctrl] + [C]. Ở Windows, nếu muốn PING không giới hạn thêm tham số `-t`.
<a name="3.2"></a>
#### 3.2 PING với số gói

```
ping -n 8 meditech.vn
```
<img src="http://image.prntscr.com/image/eff6a5bd5e5a48d6a75597dde4b63782.png" />

Thông tin phía dưới cho ta thấy được, tổng số gói tin đã gửi. Bao nhiêu gói tin gửi thành công, và bao nhiêu gói tin gửi lỗi.

Với LINUX, chúng ta thay -n bằng -c.

```
ping -c 8 meditech.vn
```

<img src="http://image.prntscr.com/image/4b7cc2907aae4ef5a2ad559a5709d1ca.png" />

<a name="3.3"></a>
#### 3.3 PING với tùy chọn kích cỡ gói tin

```
ping -l 1024 meditech.vn
```

<img src="http://image.prntscr.com/image/3ea6013218b44b629f3067238dd387f9.png" />

Giá trị cao nhất của tham số `-l` là 65500, điều này có nghĩa gói tin ping lớn nhất chỉ 65500 bytes (~65,5KB)
<a name="3.4"></a>
#### 3.4 Tùy chọn thời gian gửi gói tin tiếp theo

```
ping -i 2 meditech.vn
```

<img src="http://image.prntscr.com/image/addee54a217947f494ac4712166cf0a9.png" />

2 giây sẽ gửi gói tin tiếp theo đến host.

<img src="http://image.prntscr.com/image/43698ea0d91f4969b87fd9c9449cd0f0.png" />

Ví dụ cụ thể hơn, tôi sẽ ping 2 lần. 
- Lần thứ nhất có tham số `-i`, nhìn vào `time` chúng ta thấy nó mất 2 giây để gửi 2 gói tin. 
- Lần thứ hai không có tham số `-i`, chúng ta thấy thời gian gửi đi 2 gói tin chỉ mất 1 giây, bằng thông số thời gian mặc định của lệnh.
<a name="3.5"></a>
#### 3.5 Gửi gói tin liên tiếp đến host

```
ping -f meditech.vn
```

Với tham số `-f`, ping sẽ gửi liên tiếp các gói tin đến host cho đến khi bạn bấm Ctrl + C.

Lấy ví dụ cụ thể, tôi sẽ PING 10 gói tin liên tiếp với tham số `-f` với thời gian 0.13 giây (như hình).

<img src="http://image.prntscr.com/image/8aa1d281becc4a13988d3f96e3f9fdad.png" />
<a name="3.6"></a>
#### 3.6 Phân tích kết quả với PING

```
ping -c 5 -q meditech.vn
```

<img src="http://image.prntscr.com/image/c0bd1e3bfafb413b9b776ac08fdbc75b.png" />

Với `-q`, chúng ta chỉ thấy kết quả phân tích mà không thấy gói tin cụ thể được gửi.
<a name="3.7"></a>
#### 3.7 PING trong khoảng thời gian

```
ping -w 10 meditech.vn
```

<img src="http://image.prntscr.com/image/3288f033403a4803abcbbbc065a53367.png" />

Câu lệnh trên sẽ ping đến host trong khoảng thời gian 10s.

<a name="4"></a>
### 4. Kết luận

Trên đây là một vài giới thiệu về tham số của PING. Tùy vào những nhu cầu thực tế mà chúng ta sử dụng các tham số này sao cho hiệu quả, giúp các bạn xử lý các vấn đề troubleshoot trong mạng của mình một cách tối ưu nhất. Hy vọng có thể giúp các bạn hiểu thêm phần nào về PING - công cụ mà một kỹ sư mạng, kỹ sư hệ thống nào cũng phải biết và dùng ít nhất một lần trong đời.