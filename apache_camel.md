# Apache Camel, EIPs and stuff

## Why EIP
- Trong 1 hệ thống các ứng dụng phân tán, việc kết nối giữa các ưd với nhau là khá phức tạp vì mỗi thằng có những kênh vận chuyển (transportation eg: http, queue,..), quy trình, giao thức (http, jms, amqp), format message khác nhau nên khi kết nối cần phải chuyển đổi các cái message này.
- Để phát triển code giúp kết nối dc các hệ thống là 1 chuyện, tuy nhiên nó lại dẫn đến các hệ thống liên kết chặt với nhau (tight-coupling) dẫn đến sự thay đổi của 1 hệ thống làm ảnh hưởng đến nhiều hệ thống khác
- Cần đến EIPs, là tập hợp các design pattern giúp integrate các hệ thống.
- Cách tiếp cận tốt nhất (hiện tại?)là dùng 1 middleware framework

## Apache Camel
### 1. Khái quát
[![N|Solid](https://miro.medium.com/proxy/0*ViOeqiPNJHezoQHb)]()

- Apache camel is a light weight middleware framework that implements all EIPs
- Cấu trúc camel gồm 3 phần:
    - Routing engine
    - Processor
    - Components

**Routing engine**: Cung cấp 1 cái endpoint interface để kết nối tới các hệ thống "external"
**Processor**: là chỗ xử lý message giữa các endpoint. EIP dc implement ở module này
**Component**: camel cung cấp 1 đống các prebuilt component dùng cho các thể loại database, queue, api, cloud integration; 200+ protocol, transport, data format; 300+ converter

>> **CamelContext**: là cái container, là runtime system nắm giữ mọi thứ kiểu applicationcontext
[![N|Solid](https://camel.apache.org/manual/_images/images/camel-context.png)]()
### 2. Hoạt động
[![N|Solid](https://miro.medium.com/proxy/0*rtIj9WEzf2YFrlZz)]()

Có 3 kiểu component: Routing, filtering, processing
- Route: transfer data từ s1 sang s2: file system to filesystem, fs to MQ, fs to DB, ...
- Filter: check điều kiện, remove invalid data, ....
- Process: tính toán, chuyển đổi dữ liệu (xml -> json, text -> json)







https://medium.com/@knoldus/apache-camel-the-beginners-guide-5fc25f1461d6
https://medium.com/walmartglobaltech/introduction-to-apache-camel-part-1-66914874cc78