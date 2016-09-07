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

- **Request time out**: thực hiện gửi gói thành công nhưng không nhận được gói phản hồi. (Lỗi ở Phía xa)
- **Destination host unreachable**: đích đến không tồn tại hoặc đang cô lập. (Lỗi ở phía mình)
- **Reply from 203.162.4.190 byte=32 time <1ms TTL 124**: Gửi gói đến địa chỉ IP: 203.162.4.190 với độ dài gói 32 byte, thời gian phản hồi dưới 1 mili giây, TTL (time to live - vòng đời gói) 124. Phản ánh trạng thái gói gửi và tín hiệu phản hồi. Gói tin khi đi qua một ROUTER thì TTL giảm đi 1 đơn vị và đi qua không quá 30 host. Host sử dụng Windows (Mặc định 128) TTL > 98, Dùng Linux (Mặc định 64) > 34.
- **Unknown host / Ping Request Could Not Find Host**: Điều này cho chúng ta biết hostname bạn ping đến không có trên bất cứ Hệ thống phân giải tên miền nào hay lý do đơn giản là bạn viết sai chính tả

```
ping meidtech.vn
```

<a name="3"></a>
### 3. Một số tham số thường dùng

Thực hiện trên Terminal của CentOS 6
<a name="3.1"></a>
#### 3.1. PING thông thường tới 1 host

```
ping meditech.vn
```

<img src="http://image.prntscr.com/image/bef7739da03446d084fa29efdfe0bc9f.png" />

Trên Windows, nếu không có tham số nào đi cùng thì mặc định 4 gói tin được gửi đến host với size là 32 byte.

Khác với Windows, ở LINUX, nếu không có tham số đi cùng thì mặc định số gói tin sẽ không giới hạn với size là 64 byte.
Để ngừng việc ping, chúng ta thao tác [Ctrl] + [C]. Ở Windows, nếu muốn PING không giới hạn thêm tham số `-t`.
<a name="3.2"></a>
#### 3.2. PING với số gói

```
ping -c 8 meditech.vn
```
<img src="http://image.prntscr.com/image/0cacc8b12f1c42309078ee012c62b8bd.png" />

Thông tin phía dưới cho ta thấy được, tổng số gói tin đã gửi. Bao nhiêu gói tin gửi thành công, và bao nhiêu gói tin gửi lỗi.

Với Windows, chúng ta thay `-c` bằng `-n`.

```
ping -c 8 meditech.vn
```

<img src="http://image.prntscr.com/image/4b7cc2907aae4ef5a2ad559a5709d1ca.png" />

<a name="3.3"></a>
#### 3.3. PING với tùy chọn kích cỡ gói tin

```
ping -s 100 meditech.vn
```

<img src="http://image.prntscr.com/image/635b7e77273743c2bc7da5d13f968d25.png" />

Giá trị cao nhất của tham số `-s` là 65507, điều này có nghĩa gói tin ping lớn nhất chỉ 65507 bytes (~65,5KB)
<a name="3.4"></a>
#### 3.4 Tùy chọn thời gian gửi gói tin tiếp theo

Mặc định, PING sẽ chờ khoảng thời gian là 1 giây để gửi gói tin tiếp theo đến host. Nhưng bạn có thể tăng hoặc giảm thời gian này bằng tham số `-i`.

Thêm tham số `-i 2` như ví dụ dưới để PING gửi request tiếp theo tới host sau 2 giây.
```
ping -i 2 meditech.vn
```

<img src="http://image.prntscr.com/image/0590355b5a6b4512a26bb6eddb04d95d.png" />

Với ví dụ trên, PING sẽ gửi gói tin tiếp theo đến host sau khoảng thời gian là 2 giây.

Để nhìn rõ hơn, tôi sẽ kết hợp với tham số `-c`. Ý nghĩa của của nó là sẽ gửi đi 3 gói tin với thời gian chờ gửi gói tin tiếp theo là 2 giây.

<img src="http://image.prntscr.com/image/c4666d3d690d412da337c52106178c57.png" />

Gói tin đầu tiên sẽ được gửi từ giây 0. Cách 2s, gói tin tiếp theo được gửi đi. Như vậy trong trường hợp này, PING mất 4s để gửi 3 gói tin đi. 

<a name="3.5"></a>
#### 3.5. Gửi gói tin liên tiếp đến host

Đây là cách gửi gói tin liên tiếp đến một host nào đó, thường dùng để kiểm tra hiệu năng hoạt động của mạng.

```
ping -f meditech.vn
```

<img src="http://image.prntscr.com/image/3f29f70a542142c09ad9bac83ed82ad9.png" />

Lấy ví dụ cụ thể, tôi sẽ PING 10 gói tin liên tiếp với tham số `-f` với thời gian 0.13 giây (như hình). Mặc định khi gửi các bản tin đi, PING sẽ chờ khoảng thời gian là 1 giây để gửi đi bản tin tiếp theo.

<img src="http://image.prntscr.com/image/8aa1d281becc4a13988d3f96e3f9fdad.png" />

<a name="3.6"></a>
#### 3.6. Phân tích kết quả với PING

```
ping -c 5 -q meditech.vn
```

<img src="http://image.prntscr.com/image/c0bd1e3bfafb413b9b776ac08fdbc75b.png" />

Với `-q`, chúng ta chỉ thấy kết quả phân tích mà không thấy gói tin cụ thể được gửi.
<a name="3.7"></a>

#### 3.7. PING trong khoảng thời gian

```
ping -w 10 meditech.vn
```

<img src="http://image.prntscr.com/image/3288f033403a4803abcbbbc065a53367.png" />

Với tham số `-w`, câu lệnh trên sẽ ping đến host trong khoảng thời gian 10s.

<a name="4"></a>
### 4. Kết luận

Trên đây là một vài giới thiệu về tham số của PING. Tùy vào những nhu cầu thực tế mà chúng ta sử dụng các tham số này sao cho hiệu quả, giúp các bạn xử lý các vấn đề troubleshoot trong mạng của mình một cách tối ưu nhất. Hy vọng có thể giúp các bạn hiểu thêm phần nào về PING - công cụ mà một kỹ sư mạng, kỹ sư hệ thống nào cũng phải biết và dùng ít nhất một lần trong đời.