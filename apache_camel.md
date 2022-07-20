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

### DSL domain-specific language
- Để wire endpoint và processor tạo thành route, camel dùng dsl
- Có nhiều kiểu dsl:
    ```sh
    from("file:data/inbox")
        .filter().xpath("/order[not(@test)]")
        .to("jms:queue:order")
    ```

    hoặc
    ```sh
    <route>
        <from uri="file:data/inbox"/>
        <filter>
            <xpath>/order[not(@test)]</xpath>
            <to uri="jms:queue:order"/>
        </filter>
    </route>
    ```
### Routing and EIPs
#### 1. Content-based routers
[![N|Solid](https://res.cloudinary.com/nmq126/image/upload/v1658195612/Camel%20EIPs/111_nhw0vv.png)]()

```sh
    <route>
        <from uri="file:data/inbox"/>
        <filter>
            <xpath>/order[not(@test)]</xpath>
            <to uri="jms:queue:order"/>
        </filter>
    </route>
```
    
[![N|Solid](https://res.cloudinary.com/nmq126/image/upload/v1658196762/Camel%20EIPs/112_srbn5m.png)]()
```sh
    from("jms:incomingOrders")
    .choice()
     .when(predicate).to("jms:xmlOrders")
     .when(predicate).to("jms:csvOrders")
     .otherwise().to("jms:badOrders").stop()
     .end().to("jms:vao day het tru thang bad order do no stop roi")
```
- Dùng Spring DSL thì như lày
    ```sh
    <route>
        <from uri="jms:incomingOrders"/>
        <choice>
            <when>
                <simple>${header.CamelFileName} regex '^.*xml$'</simple>
                <to uri="jms:xmlOrders"/>
            </when>
            <when>
                <simple>${header.CamelFileName} regex '^.*(csv|csl)$'</simple>
                <to uri="jms:csvOrders"/>
            </when>
            <otherwise>
                <to uri="jms:badOrders"/>
                <stop/>
            </otherwise>
        </choice>
        <to uri="jms:continuedProcessing"/>
    </route>
    ```
#### 2. Message filter
```sh
    from("jms:xmlOrders").filter(xpath("/order[not(@test)]"))
        .process(new Processor() {
            public void process(Exchange exchange) throws Exception {
                System.out.println("Received XML order: "
                    + exchange.getIn().getHeader("CamelFileName"));
             }
    });
```
```sh
    <route>
        <from uri="jms:xmlOrders"/>
         <filter>
            <xpath>/order[not(@test)]</xpath>
            <process ref="orderLogger"/>
        </filter>
    </route>
```
#### 3. Multicasting
    
[![N|Solid](https://res.cloudinary.com/nmq126/image/upload/v1658198973/Camel%20EIPs/113_ykhjah.png)]()
- Default thì multicast send message tuần tự từ accouting rồi đến production
    ```sh
        from("jms:xmlOrders").multicast().to("jms:accounting", "jms:production");
    ```
- Parallel multicast: 1 thread pool size 10 dc dùng nếu k config gì thêm -> bên dưới config pool 16
    ```sh
        from("jms:xmlOrders").multicast()
        .parallelProcessing()
        .to("jms:accounting", "jms:production");
    ```

    ```sh
        ExecutorService executor = Executors.newFixedThreadPool(16);
        from("jms:xmlOrders")
        .multicast().parallelProcessing().executorService(executor)
        .to("jms:accounting", "jms:production");
    ```
- Default thì multicast k dừng lại nếu có lỗi, message sẽ vẫn tiếp tục được gửi: khi enable stopOnException thì multicast sẽ dừng khi gặp exception đầu tiên
    ```sh
        ExecutorService executor = Executors.newFixedThreadPool(16);
        from("jms:xmlOrders")
         .multicast().stopOnException()
         .parallelProcessing().executorService(executor)
         .to("jms:accounting", "jms:production");
    ```
    
    ```sh
    <bean id="executor" class="java.util.concurrent.Executors" 
        ➥ factory-method="newFixedThreadPool">
         <constructor-arg index="0" value="16"/>
    </bean>
    
    <route>
         <from uri="jms:xmlOrders"/>
         <multicast stopOnException="true" parallelProcessing="true"
            ➥ executorServiceRef="executor">
             <to uri="jms:accounting"/>
             <to uri="jms:production"/>
         </multicast>
    </route>
    ```

#### 4. Recipient list
[![N|Solid](https://res.cloudinary.com/nmq126/image/upload/v1658200012/Camel%20EIPs/114_blv5e8.png)]()
```sh
    ➥ from("jms:xmlOrders")
        .recipientList(header("recipients"));
```

```sh
    ➥ from("jms:xmlOrders")
        .setHeader("customer", xpath("/order/@customer"))
        .process(new Processor() {
         public void process(Exchange exchange) throws Exception {
             String recipients = "jms:accounting";
             String customer =
             exchange.getIn().getHeader("customer", String.class);
             if (customer.equals("honda")) {
                recipients += ",jms:production";
            }  
            exchange.getIn().setHeader("recipients", recipients);
        }
    })
    .recipientList(header("recipients"));
```

```sh
    <route>
        <from uri="jms:xmlOrders" />
        <setHeader headerName="customer">
            <xpath>/order/@customer</xpath>
        </setHeader>
        <process ref="calculateRecipients" />
        <recipientList> 
            <header>recipients</header>
        </recipientList>
    </route>
```
- Dùng annotation
    ```sh
        @RecipientList
        public String[] route(@XPath("/order/@customer") String customer) {
            if (isGoldCustomer(customer)) {
                return new String[] {"jms:accounting", "jms:production"};
            } else {
                return new String[] {"jms:accounting"};
            }
        }
        private boolean isGoldCustomer(String customer) {
            return customer.equals("honda");
        }
    ```
    ```sh
        ➥ from("jms:xmlOrders").bean(RecipientListBean.class);
    ```

### Trasforming data

#### 1. Khái quát


### In-memory components
- Direct Là 1 endpoint đồng bộ
- SEDA là bất đồng bộ

### Components

#### 2. Asynchronous messaging with JMS component
```sh
      <!-- set up ActiveMQ broker -->
      <broker:broker useJmx="false" persistent="false" brokerName="localhost">
        <broker:transportConnectors>
          <broker:transportConnector name="tcp" uri="tcp://localhost:61616" />
        </broker:transportConnectors>
      </broker:broker>
  
        <bean id="jms" class="org.apache.camel.component.jms.JmsComponent">
            <property name="connectionFactory">
                <bean class="org.apache.activemq.ActiveMQConnectionFactory">
                    <property name="brokerURL" value="tcp://localhost:61616" />
                </bean>
            </property>
        </bean>    
```
- Default JMS connection factory k có connection pool đến broker nên với mỗi message nó sẽ tạo  1 connection mới. Để tránh cái này thì dùng connection factory của ActiveMQ Component để tăng hiệu suất.
- Cái tcp://localhost:61616 là JMS Provider. Vì dùng ActiveMQConnectionFactory nên cái uri bảo thằng active mq kết nối đến broker thông qua tcp ở port 61616
- Ngoài ra JMS còn hỗ trợ request-reply cho phép gửi rồi đợi reply đoạn này đọc thêm trong sách đi



## References:
- https://medium.com/@knoldus/apache-camel-the-beginners-guide-5fc25f1461d6
- https://medium.com/walmartglobaltech/introduction-to-apache-camel-part-1-66914874cc78
- Camel in Action - CLAUS IBSEN & JONATHAN ANSTEY