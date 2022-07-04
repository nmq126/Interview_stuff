# Một vạn câu hỏi Docker 
## _Phần 1_
oke

## 1. Docker là gì?

Docker là nền tảng phần mềm cho phép bạn dựng, kiểm thử và triển khai ứng dụng một cách nhanh chóng. Docker đóng gói phần mềm vào các đơn vị tiêu chuẩn hóa được gọi là container có mọi thứ mà phần mềm cần để chạy, trong đó có thư viện, công cụ hệ thống, mã và thời gian chạy.

>>Docker là công cụ hỗ trợ đóng gói các thành phần của 1  ứng dụng thành một khối  giúp  nó chạy nhanh hơn và chạy được ở mọi nơi 

### 2. Lợi ích của Docker
- Tốc độ làm việc nhanh: Docker Containers có tốc độ chạy nhanh hơn hẳn các VMs truyền thống 
- Tự do trong chọn hệ thống: người dùng có thể khởi chạy  container trong (hầu hết) bất cứ hệ thống nào họ muốn.
- Đồng nhất: Khi nhiều người cùng phát triển trong cùng một dự án sẽ không bị sự sai khác về mặt môi trường.
- Nhẹ:  Container cũng sử dụng chung các images nên cũng không tốn nhiều disks.

### 3. Docker Images là gì?
Docker image có thể được hiểu như là 1 mẫu template từ đó Docker container được tạo ra. N Các container dựa trên các image để tạo nên những môi trường tương ứng.
>>Bạn có thể tự build một image riêng cho mình hoặc sử dụng những image được chia sẽ từ cộng đồng Docker Hub. Một image sẽ được build dựa trên những chỉ dẫn của Dockerfile.

### 4. Container là gì?
Docker container giữ mọi thứ chúng ta cần để ch để phần mềm chạy được. Mỗi container được tạo từ Docker image. Docker containers về cơ bản là runtime instances của Docker images.

### 5. Docker Hub là gì?
Docker HUB là dịch vụ đám mây cho phép bạn liên kết với các kho code, xây dựng và test images của bạn, bạn có thể build môi trường máy chủ của bạn bằng các image bạn tạo ra hoặc sử dụng các image public được build bưởi cộng đồng trên Docker Hub.

### 6. Dockerfile là gì?
Dockerfile là file để chứa những dòng lệnh để build lên 1 images bạn cần. Nó có thể pull các image từ Docker hub.

### 7. Quy trình thực thi của một hệ thống sử dụng Docker
1. Build: Dockerfile được Build tại một máy tính đã cài đặt Docker Engine. Sau khi build ta sẽ có được Container, trong Container này chứa ứng dụng kèm bộ thư viện của chúng ta.
2. Push: Sau khi có được Container, chúng ta thực hiện push Container này lên cloud và lưu tại đó.
3. Pull & run: Nếu một máy tính khác muốn sử dụng Container chúng ta thì bắt buộc máy phải thực hiện việc Pull container này về máy, tất nhiên máy này cũng phải cài Docker Engine. Sau đó thực hiện Run Container này.

### 8. Docker compose là gì?
Docker Compose được dùng để định nghĩa và thực hiện quá trình run multi-container cho Docker Application. Với nền tảng này các bạn có thể sử dụng file YAML để config với các services dành cho Application. Sau đó người dùng có thể dùng command để create và run những Config đó. Để có thể sử dụng, người dùng cần làm ba bước như sau:

- Tiến hành khai báo app’s environment trong Dockerfile.
- Khai báo các services cần thiết để chạy application ở trong file docker-compose.yml.
- Run docker-compose up để start và run app.

### 9. Câu lệnh thao tác với Docker Image

    docker images : Câu lệnh kiểm tra danh sách các image đã cài đặt
    docker rmi  : Câu lệnh xóa 1 image chưa chạy dựa vào id hoặc tên
    docker rmi -f  : Câu lệnh xóa 1 image kể cả khi đang chạy dựa vào id hoặc tên

### 10. Câu lệnh thao tác với Docker Container

    docker ps : Câu lệnh liệt kê danh sách các container đang chạy
    docker ps -a: liệt kê danh sách các container đang chạy và đã tắt
    docker inspect { container_id } : Xem thông tin chi tiết của container được tạo ra
    docker logs { container_id } : xem lịch sử container
    docker rm : xóa 1 container dựa vào id hoặc tên
    docker rm -f : xóa 1 container kể cả đang chạy
    docker rm $(docker ps -a -q): xóa tất cả các container
