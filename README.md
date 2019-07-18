# Training_HttpRequest_PostMan

## I. Cấu trúc của Http message

### 1. Các khái niệm

### a. Tổng quan

Http message là cách mà data chuyển giữa người dùng và server. Có 2 loại message:

-  **request**: gửi bởi người dùng tới server để trigger 1 action nào đó

- **responses**: kết quả trả về từ server

Http request và response có chung cấu trúc như sau:

- **start-line**: miêu tả request để thực hiện (GET, POST, PUT, ...), trạng thái thành công hay thất bại (qua các mã code)

- **HTTP headers**: có thể có hoặc không, chứa các thông tin của request (host, content-length, encoding, ...)  hoặc miêu tả phần body của message

- **Dòng kẻ trống**: một dòng trống cho biết tất cả thông tin meta cho yêu cầu đã được gửi.

- **body**: Phần thân tùy chọn chứa dữ liệu được liên kết với r (như nội dung của biểu mẫu HTML) hoặc tài liệu được liên kết với reponse.

Sự hiện diện của cơ thể và kích thước của nó được chỉ định bởi các tiêu đề bắt đầu và HTTP.


start-line + HTTP headers = head của r
equest

<img src="img/ht1.png"/>

### b. Http requests

- **Start-line** gồm 3 thành phần: 

+ HTTP method (như GET, PUT, POST, HEAD, OPTIONS, ...): miêu tả hành động sẽ được thực hiện

Ví dụ: GET là lấy dữ liệu từ server, POST là gửi dữ liệu lên server

+ **Request target**: mục tiêu request, thường là URL, hoặc đường dẫn của giao thức, cổng, ...

```
POST / HTTP/1.1
GET /background.png HTTP/1.0
HEAD /test.html?query=alibaba HTTP/1.1
GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1
```

+ **Phiên bản HTTP**:  định nghĩa cấu trúc còn lại của message, cũng là phiên bản của reponse trả về

- **HTTP headers**: cấu trúc dạng key : value, có thể được chia thành nhiều nhóm:

+ **General headers**: áp dụng cho cả message request và response, ko liên quan gì đến data được truyền trong body

Ví dụ: Connection: keep-alive: kiểm soát xem kết nối mạng có mở hay không sau khi giao dịch hiện tại kết thúc.

 Nếu giá trị được gửi là keep-alive, kết nối vẫn tồn tại và không bị đóng, cho phép các yêu cầu tiếp theo đến cùng một máy chủ được thực hiện.

Ngoài ra còn Upgrade-Insecure-Requests: 1equest

+ **Request headers**: như  User-Agent, Accept-Type, sửa request bằng việc chỉ định rõ hơn, đưa ra context (như Referer), ...

+ **Entity headers**: như Content-Length, áp dụng cho body của request

<img src="img/ht2.png"/>

- **Body**: không phải request nào cũng có body, request lấy dữ liệu về như GET, HEAD, DELETE, OPTIONS thường không có. Request gửi data tới server
để update nó như POST thì có

Body được chia làm 2 loại:

Body 1 resource: gồm 1 file đơn lẻ, định nghĩa bởi 2 headers: Content-Type, Content_length

Multiple-resource bodies: gồm một multipart body, mỗi cái bao gồm những bit khác nhau của thông thường

### c. Http response

- **Start-line** gồm 3 thành phần: 

+ Protocol version: thường là HTTP/1.1

+ status code: code trả về từ server

+ status text: miêu tả status code

Ví dụ: HTTP/1.1 404 Not Found.

- **Headers**: cấu trúc dạng key : value

Có 3 nhóm:

- **General headers**: áp dụng cho cả message 

- **Response headers**: bổ thêm thông tin về server 

- **Entity headers**: áp dụng tới body của reponse, như Content-Length

<img src="img/ht3.png"/>

- **Body**: không phải response nào cũng có, response với status code 201, 204 thường không có

Có 3 loại:

+ Body 1 resource: gồm 1 file đơn lẻ biết trước kích thước, định nghĩa bởi 2 headers: Content-Type(chỉ loại media của tài nguyên - MIME types), Content_length

+ Body 1 resource: gồm 1 file đơn lẻ không biết trước kích thước, mã hóa bởi các mảnh bằng Transfer-Encoding

+ Multiple-resource bodies: gồm một multipart body, mỗi cái chứa những thông tin khác nhau, rất hiếm tồn tại

### d. HTTP/2 Frames

- HTTP/1.x có một vài nhược điểm về hiệu suất:

+ Header, không giống như body, không nén được

+ Header thường giống nhau giữa các message, và nó cũng bị lăp lại giữa các kết nối

+ Không ghép kênh có thể được thực hiện, nhiều kết nối cần mở trên cùng 1 server.

HTTP/2 thêm tính năng: nó chia HTTP/1.x message thành các frame mà sẽ được gắn vào 1 luồng. Dữ liệu và header frame được tách ra, điều này giúp header nén được. 

<img src="img/ht4.png"/>

Nhiều luồng có thể được kết hợp với nhau, giúp tạo ra ghép kênh, tối ưu sử dụng băng thông, tiết kiệm thời gian

<img src="img/ht5.png"/>


### e. Http request method

HTTP định nghĩa một set các phương thức request để chỉ định action có thể thực hiện với một resource có sẵn.

Có nhiều method như:

- GET: thường dùng GET để lấy dữ liệu

- HEAD: yêu cầu có kết quả trả về như GET, nhưng không có body

- POST: thường là để gửi 1 đối tượng lên server, nhằm thay đổi dữ liệu hoặc tác dụng nào đó

- PUT: thay thế toàn bộ  thông tin của đối tượng hiện tại bằng cái gửi lên

- PATCH: ghi đè các thông tin được thay đổi của đối tượng.

- CONNECT: thiết lập 1 kết nối tới server theo URL

- OPTIONS: mô tả các tùy chọn giao tiếp cho resource

 So sánh

### GET vs POST

Liệu GET có thể dùng để đầy dữ liệu lên và POST để lấy dữ liệu về

==> Hoàn toàn được

Nhưng không nên vì nó sẽ phá vỡ quy tắc thiết kế. Và GET thì không có body nên khi gửi thì tất cả các parameter sẽ bị hiển thị lên url của request, xét về bảo mật
thì đây sẽ không tốt. Post nó giấu paremeter trong Body và mã hóa chúng, sẽ an toàn hơn

Ví dụ: 

```
GET http://demo7904624.mockable.io/getAllNote?**one=1123&two=2123** http/1.1
```

```
POST /test/demo_form.php HTTP/1.1
Host: w3schools.com

name1=value1&name2=value2
```


### POST/PUT/PATCH

Điểm khác biệt giữa post và put đơn giản là put là idempotent còn post thì không. Kết quả của PUT sẽ luôn như nhau,
còn POST sẽ tạo ra các kết quả khác nhau

post: tạo mới

put: ghi đè(toàn bộ) hoặc tạo mới 1 resource

patch: cập một 1 phần của resource

### SAFE

Một method được coi là safe khi nó không làm thay đổi trạng thái "sate" của server.

Nói cách khác, an toàn là chỉ đọc mà không làm thay đổi bất kì điều gì. 

Các method được coi là safe chỉ có: GET, HEAD và OPTIONS.

Unsafe: PUT, DELETE, POST và PATCH.

### 2. MIME type

### a. Tổng quan

- Giao thức mở rộng thư điện tử Internet đa mục đích hay MIME (Multipurpose Internet Mail Extensions) là một tiêu chuẩn Internet về định dạng cho thư điện tử. 
Hầu như mọi thư điện tử Internet được truyền qua giao thức SMTP theo định dạng MIME. Ngoài ra nó còn xác định kiểu định dạng của document, file, 

- Cấu trúc: type/subtype

+ Type: tổng quan về loại data, ví dụ như video, text

+ Subtype: chi tiết hơn cho Type. 

Ví dụ như text/plain (plain text), text/html (htlm source code)


 **Type**

**Discrete**(rời rạc): đại diện cho 1 tệp hoặc phương tiện, chẳng hạn như 1 tệp văn bản, nhạc hoặc video.

- Có nhiều loại như: 

application: bất kì loại dữ liệu nhị phân nào không rơi vào các loại khác, dữ liệu sẽ được thực thi theo cách nào đó hoặc cần một ứng dụng hoặc danh mục ứng dụng cụ thể để sử 

Ví dụ: application/pdf, application/pkcs8, and application/zip.

audio: audio hoặc dữ liệu nhạc, bao gồm audio/mpeg, audio/vorbis

example: chỉ là để ví dụ, ko nên dùng trong thực tế

image: ảnh hoặc dữ liệu đồ họa, gồm bitmap, vector, ảnh GIF, ... Ví dụ như image/jpeg, image/png,
image/svg+xml

model: mẫu dữ liệu cho 3D object hoặc scene. Ví dụ model/3mf, model/vml 

video: video data hoặc file, ví dụ như MP4 movies(video/mp4) 


**Multipart**: một tài liệu bao gồm nhiều thành phần, mỗi phần có một lại MIME riêng, hoặc 1 loại nhiều phần có thể gói gọn nhiều tệp cùng nhau trong 1 giao dịch. Được gọi là tài liệu tổng h

Ví dụ: nhiều loại MIME được sử dụng khi đính kèm nhiều tệp vào email.

- Có 2 loại:

+ message: message đóng gói message khác ở trong.

+ multipart: dữ liệu bao gồm nhiều thành phần có thể có các loại MIME khác nhau.















