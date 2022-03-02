# Một vạn câu hỏi phỏng vấn Spring boot
## _Phần 1_

### Spring là gì? Spring boot là gì?
Spring là framework mã nguồn mở được phát triển dựa trên nền tảng là Java, giúp đơn giản hóa việc xây dựng và phát triển các ứng dụng java doanh nghiệp
>>Spring Boot là một module nằm trong Spring Framework được sử dụng rộng rãi để phát triển các REST APIs.

SB cung cấp RAD (Rapid Application Development) cho Spring Framework bằng các starters giúp dễ dàng quản lý dependencies và khả năng tự động config.

### Khái niệm tight-coupling (liên kết ràng buộc) và cách loosely couple
tight-coupling hay "liên kết ràng buộc" là một khái niệm trong Java ám chỉ việc mối quan hệ giữa các Class quá chặt chẽ. Khi yêu cầu thay đổi logic hay một class bị lỗi sẽ dẫn tới ảnh hưởng tới toàn bộ các Class khác.

loosely-coupled là cách ám chỉ việc làm giảm bớt sự phụ thuộc giữa các Class với nhau.


### Khái niệm Dependency Injection (DI)
>>Dependency Injection là 1 design pattern; là một mô hình triển khai từ nguyên lý IoC

Có thể hiểu Dependency Injection một cách đơn giản như sau:
1. Các module không giao tiếp trực tiếp với nhau, mà thông qua interface. Module cấp thấp sẽ implement interface, module cấp cao sẽ gọi module cấp thấp thông qua interface.
2. DI được dùng để làm giảm sự phụ thuộc giữa các module, dễ dàng hơn trong việc thay đổi module, bảo trì code và testing.

Các cách để Inject dependency vào một đối tượng có thể kể đến như sau:
- Constructor Injection
- Setter Injection
- Interface Injection


### Khái niệm Inversion of Control (IOC)
>>Inversion of Control (IoC) là một nguyên lý thiết kế trong công nghệ phần mềm trong đó các thành phần nó dựa vào để làm việc bị đảo ngược quyền điều khiển khi so sánh với lập trình hướng thủ thục truyền thống.

![N|Solid](https://xuanthulab.net/photo/ioc-4477.png)
- Với mô hình không sử dụng IoC thì Class A cần phải khởi tạo và điều khiển hai class B và C, bất kỳ thay đổi nào ở Class A đều dẫn đến thay đổi ở Class B và C.
- Trong khi đó, nếu trong mô hình sử dụng IoC, các class B và C sẽ được đưa đến độc lập so với class A thông qua một bên thứ ba, từ đó các class không phụ thuộc lẫn nhau mà chỉ phụ thuộc vào interface. Điều này cũng đồng nghĩa rằng sự thay đổi ở class cấp cao sẽ không ảnh hưởng tới các class cấp thấp hơn

### Spring container là gì?
Trong Spring, công việc của Spring Container (IoC Container) là sẽ tạo các đối tượng rồi lắp ráp chúng lại với nhau, cấu hình các đối tượng và quản lý vòng đời của chúng từ lúc được tạo ra cho đến khi bị hủy
Trong Spring boot interface ApplicationContext đại diện cho IoC Container

### Bean là gì
Spring container sẽ sử dụng DI để quản lý các thành phần, đối tượng để tạo nên 1 ứng dụng. Các thành phần, đối tượng này được gọi là Spring Bean

### Các scope của spring bean
- Singleton: Chỉ một instance của bean sẽ được tạo trong Sping IoC container. Đây là scope mặc định cho Spring bean
- Prototype: Một instance mới được tạo mỗi khi bean được yêu cầu
- Request: Tương tự prototype scope. Tuy nhiên, nó được sử dugnj cho các ứng dụng web. Một instance mới của bean sẽ được tạo cho mỗi yêu cầu HTTP
- Session: Một bean mới sẽ được tạo cho mỗi phiên HTTP bởi vùng chứa
- Global-session: Được sử dụng để tạo các global session bean cho các ứng dụng Portlet

### Singleton Bean có Thread safe trong Spring Framework?
>>Không, trong Spring framework, singleton bean không thread safe.

### Bean lifecycle trong Spring framework
- Spring container tìm các bean definition trong file XML và khởi tạo các bean.
- Spring cài đặt tất cả các thuộc tính được định nghĩa trong bean definition (Dependency Injection).
- Nếu bean implement BeanNameAware interface, spring sẽ truyền bean id vào trong hàm setBeanName().
- Nếu bean implement BeanFactoryAware interface, spring sẽ truyền beanfactory vào hàm setBeanFactory().
- Nếu có bất cứ bean BeanPostProcessors nào được liên kết với bean đang khởi tạo, spring sẽ gọi hàm postProcesserBeforeInitialization() và postProcessAfterInitialization().
- Nếu bean implement InitializingBean, phương thức afterPropertySet() sẽ được gọi. Nếu bean đã được khai báo phương thức khởi tạo, thì phương thức đó sẽ được gọi.
- Nếu bean implement DisposableBean interface, phương thức destroy() sẽ được gọi.

### Bean wiring là gì?
Wiring, hoặc là Bean wiring là trường hợp mà các bean được kết hợp lại trong Spring container.
>>Spring container có khả năng autowire quan hệ giữa các bean có mối quan hệ hợp tác với nhau.

### @Autowired là gì?
**@Autowired** đánh dấu cho Spring biết rằng sẽ tự động inject bean tương ứng vào vị trí được đánh dấu.
>>Trong thực tế, sẽ có trường hợp chúng ta sử dụng @Autowired khi Spring Boot có chứa 2 Bean cùng loại trong Context.
Lúc này thì Spring sẽ bối rối và không biết sử dụng Bean nào để inject vào đối tượng.
Có 2 cách để giải quyết vấn đề này: **@Primary** và  **Qualifier**
### @Primary là gì?
**@Primary** là annotation đánh dấu trên một Bean, giúp nó luôn được ưu tiên lựa chọn trong trường hợp có nhiều Bean cùng loại trong Context.
### @Qualifier là gì
**@Qualifier** xác định tên của một Bean mà bạn muốn chỉ định inject.

### Có bao nhiêu cách định nghĩa bean?
Trong spring để định nghĩa bean chúng ta có thể sử dụng @Component, @Service, @Repository, @Bean
### @Configuration và @Bean là gì?
**@Configuration** là một Annotation đánh dấu trên một Class cho phép Spring Boot biết được đây là nơi định nghĩa ra các Bean.
>>Về bản chất, @Configuration cũng là @Component. Nó chỉ khác ở ý nghĩa sử dụng.

**@Bean** là một Annotation được đánh dấu trên các method cho phép Spring Boot biết được đây là Bean và sẽ thực hiện đưa Bean này vào Context.
### @Component là gì ?
Là một class-level annotation. Nó yêu cầu Spring sử dụng class này để tạo một bean nếu có nơi nào khác sử dụng class này như một dependency. Việc khởi tạo class hoàn toàn do Spring kiểm soát và chỉ một bean được tạo cho mỗi class.

>>Về bản chất @Service và @Repository cũng chính là @Component. Nhưng đặt tên khác nhau để giúp chúng ta phân biệt các tầng với nhau.

###  @Service là gì?
**@Service** Đánh dấu một Class là tầng Service, phục vụ các logic nghiệp vụ.
### @Repository là gì?
**@Repository** Đánh dấu một Class Là tầng Repository, phục vụ truy xuất dữ liệu.

### Spring Boot JPA là gì
Spring Boot JPA là một phần trong hệ sinh thái Spring Data, nó tạo ra một layer ở giữa tầng service và database, giúp chúng ta thao tác với database một cách dễ dàng hơn, tự động config và giảm thiểu code thừa thãi.
JPA tuân theo Object-Relation Mapping (ORM)
### Hibernate là gì
**Hibernate** là 1 thư viện ORM mã nguồn mở (Framework)
Hibernate chính là cài đặt của JPA (JPA là 1 tập các interface, còn Hibernate implements các interface ấy 1 cách chi tiết).

### JpaRepository là gì?
Để sử dụng Spring JPA, bạn cần sử dụng interface JpaRepository.
Yêu cầu của interface này đó là bạn phải cung cấp 2 thông tin:
1. Entity (Đối tượng tương ứng với Table trong DB)
2. Kiểu dữ liệu của khóa chính (primary key)

### @RestController là gì?
Khác với **@Controller** là sẽ trả về một template. **@RestController** trả về dữ liệu dưới dạng JSON.

### @RequestBody là gì?
Các thông tin từ phía Client gửi lên Server sẽ nằm trong Body (thường cũng dưới dạng JSON).
**@RequestBody** giúp chuyển chuỗi JSON trong request thành một Object Java

### @PathVariable là gì
**@PathVariable** được sử dụng để lấy giá trị trên URI theo template (còn gọi là URI template).
```sh
http://localhost:8080/springmvc/hello/1

@RequestMapping("/spring/hello/{id}")
public String showHello(@PathVariable(value="id") String id){}
```
### @RequestParam là gì?
@RequestParam được sử dụng để truy cập giá trị của parameters trên URL(kiểu query string)
```sh
http://localhost:8080/spring/hello/param1="abc"&param2="def"

@RequestMapping("/spring/hello")
public String showHello(@RequestParam(value="param1", required=true) String parameter1,
                        @RequestParam(value="param2", required=false) String parameter2) {}
```

### Specification là gì?
**Specification** là một interface giúp tối ưu việc viết query một cách linh động và có thể tái sử dụng 
Bản chất Specification là một funtional interface với 1 hàm duy nhất như sau:
```sh
public interface Specification<T> {
  Predicate toPredicate(Root<T> root, CriteriaQuery query, CriteriaBuilder cb);
}
```
### Sử dụng Specification
Để có thể sử dụng được Specification, bạn cần kế thừa JpaSpecificationExecutor từ Spring JPA
```sh
public interface UserRepository extends JpaRepository<User, Long>,
                                        JpaSpecificationExecutor<User> {
}
```

### Spring Security
Spring Security là một framework quan trọng của Spring , nó giúp chúng ta phân quyền và xác thực người dùng trước khi cho phép họ truy cập vào các tài nguyên của chúng ta.
>>Về cốt lõi, Spring Security thực sự chỉ là một loạt các bộ lọc servlet giúp bạn thêm authentication và authorization vào ứng dụng web của mình.