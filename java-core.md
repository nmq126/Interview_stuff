# Một vạn câu hỏi phỏng vấn Java Core
## _Phần 1_
### 1. Sự khác nhau giữa JVM, JRE, JDK
- JDK là một gói các công cụ phát triển và thư viện hỗ trợ cung cấp môi trường để phát triển và thực thi ct Java.
- JDK = JRE (thực thi) + development tools (phát triển)
- JRE = JVM + một số file và thư viện mà JVM sử dụng
- JRE cung cấp môi trường để thực thi (không bao gồm phát triển) ct Java.
- JVM cung cấp môi trường runtime để java bytecode thực thi. Java “Write once, run anywhere” là nhờ JVM
- **Java độc lập nền tảng, JDK JRE JVM thì không**
### 2. JIT là gì
- Just In Time compiler thuộc JVM là 1 công cụ biên dịch convert bytecode sang ngôn ngữ máy
### 3. Sự khác nhau giữa Heap và Stack
| Stack | Heap |
| ------ | ------ |
| Stack chứa các biến local nguyên thủy và các biến tham chiếu|Heap chứa object được tạo mới bởi từ khóa new(reference đến mấy cái object này thì ở stack), biến instance và biến static |
| Stack rất nhỏ so với Heap |  |
| Stack nhanh hơn Heap do tham chiếu đơn giản: Last In First Out | Heap tham chiếu phức tạp hơn |
| Stack tự động cấp phát và giải phóng bộ nhớ khi 1 method dc gọi dến, 1 block dc tạo trong stack để lưu trữ biến local và tham số, sau khi pthuc trả về thì block bị đẩy ra|Heap không tự giải phóng bộ nhớ, cần có Garbage Collector để làm công việc này |
|thread safe do mỗi thread có stack riêng?|không thread safe|
|java.lang.StackOverFlowError|java.lang.OutOfMemoryError|
### 4. Garbage collector
- Garbage là các object k được tham chiếu đến nữa
- Garbage collector sẽ truy tìm garbage để giải phóng bộ nhớ
- GC là tự động và có thể tùy chỉnh
- Có thể request JVM chạy GC bằng **System.gc()** hoặc **Runtime.getRuntime().gc()** 
### 5. Tại sao Java không phải 100% lập trình hướng đối tượng (OOP)
 >>Vì Java vẫn sử dụng các kiểu dữ liệu nguyên thủy, mấy cái này k phải object
### 6. Sự khác nhau giữa biến instance và biến local
| Instance | Local |
| ------ | ------ |
| Instance khai báo bên trong class, bên ngoài method, constructor và block | Local ngược lại |
| Instance lưu trong heap |  Local lưu trong stack |
| Instance có access modifier | Local không có |
| Instance có giá trị mặc định |Local không có |
### 7. Từ khóa static? Có thể ghi đè static method không?
- Từ khóa static thuộc về lớp chứ không thuộc về instance (thể hiện) của lớp.
- Từ khoá static biểu thị cho biến hoặc phương thức có thể được truy cập (sử dụng) mà không cần tạo ra thực thể của lớp chứa nó.
>> Không thể override static method vì override là kĩ thuật dựa trên quá trình gán (binding) động khi ct đang chạy (runtime polymophism), static method được gán tĩnh trong  compile time
### 8. Từ khóa final
Tùy thuộc ngữ cảnh có ý nghĩa khác nhau, nhìn chung là để hạn chế người dùng “cái này k thể thay đổi được”
- Biến final là hằng số k thể thay đổi giá trị
- PT final k thể override
- Class final k thể extend
### 9. Từ khóa this
>>Từ khóa this trong java là một biến tham chiếu được sử dụng để tham chiếu tới đối tượng của lớp hiện tại.
### 10. Từ khóa super
>>Từ khóa super trong java là một biến tham chiếu được sử dụng để tham chiếu trực tiếp đến đối tượng của lớp cha gần nhất.
>>Bất cứ khi nào bạn tạo ra instance(thể hiển) của lớp con, một instance của lớp cha được tạo ra ngầm định, nghĩa là được tham chiếu bởi biến super.

### 13. Access Modifier
| Modifier | Class | Package | Subclass | Global |
| ------ | ------ | ------ | ------  | ------ |
| Public |✅ |  ✅|  ✅ | ✅|
| Protected |✅ |✅  |✅   |✖ 
| Default |✅ |✅ | ✖|✖ |
| Private |✅ |  ✖|  ✖ |✖ |
### 14. Constructor
- Là một block code dùng khởi tạo giá trị, trạng thái của object, dc gọi đến = new
- Tên phải trùng tên class
- Không thể dùng static, final hay abstract cho constructor
**2 loại constructor**
- Default constructor - no arg constructor: không nhận vào parameter. Trong class luôn có 1 constructor mặc định dù khai báo hay k; tuy nhiên khi có para constructor cần khai báo default
- Parameterized constructor
 **constructor vs method**
- Constructor không có kiểu dữ liệu trả về tường minh (dù trả về instance của class)
- Constructor được gọi một cách ngầm định (new) method thì k
### 15. Các kiểu dữ liệu trong java
Có 2 loại data type:
- Primitive data types: Kiểu dữ liệu nguyên thủy gồm: boolean, char, byte, short, int, long, float and double.
- Non-primitive data types: Kiểu dữ liệu không-phải-nguyên-thủy: Classes, Interfaces, ...
### 16. Boxing và unboxing là gì?
 >>Autoboxing là quá trình mà trình biên dịch của Java tự động chuyển đổi giữa kiểu dữ liệu cơ bản (Primitive type) về đối tượng tương ứng với lớp (Wrapper class) của kiểu dữ liệu đó. Unboxing là ngược lại
### 17. Overriding và Overloading
>>Overloading là một kĩ thuật cho phép trong cùng một class có thể có nhiều phương thức cùng tên nhưng khác nhau về số lượng tham số hoặc kiểu dữ liệu tham số. 
>>Overrding được sử dụng trong trường hợp lớp con kế thừa từ lớp cha và muốn định nghĩa lại một phương thức đã có mặt ở lớp cha.

**So sánh**

| Overloading | Overriding |
| ------ | ------ |
| Thể hiện đa hình tại compile time |Thể hiện đa hình tại runtime |
| Xảy ra trong cùng một class | Xảy ra ở 2 class có quan hệ kế thừa |
| Có thể khác nhau về số lượng và kiểu dữ liệu của tham số, kiểu dữ liệu trả về |Số lượng và kiểu dữ liệu của tham số, kiểu dữ liệu trả về phải giống nhau |
### 18. Java có hỗ trợ đa kế thừa (multiple inheritance) không?
>>Không, Java không hỗ trợ đa kế thừa (và cả kiểu kế thừa lai – Hybrid inheritance) thông qua các class. Tuy nhiên, bạn vẫn có thể đạt được tính đa kế thừa trong Java thông qua các interface
### 19. Interface vs abstract class
| Abstract class | Interface |
| ------ | ------ |
| Lớp trừu tượng **không** hỗ trợ đa kế thừa | Interface hỗ trợ đa kế thừa |
| Lớp trừu tượng có thể có các biến final, non-final, static và non-static | Interface chỉ có các biến **static và final** |
|Lớp trừu tượng có thể có phương thức static, phương thức main và constructor|Interface không thể có phương thức static, main hoặc constructor|
|Từ khóa abstract được sử dụng để khai báo lớp trừu tượng|Từ khóa interface được sử dụng để khai báo Interface|
| Abstract class có thể có private, protected ... |AM trong interface mặc định là public|
| Abstract class implement dc interface | Interface không extend được class |
### 20. Truyền theo tham chiếu (Pass by reference) và truyền theo giá trị (pass by value) là gì?
- Pass-by-value được hiểu là khi thay đổi biến trong hàm thì ngoài hàm sẽ không bị ảnh hưởng. Nó giống như bạn copy giá trị của biến vào biến khác rồi truyền vào hàm.
- Pass-by-reference là khi thay đổi biến trong hàm cũng làm ngoài hàm bị ảnh hưởng. Nó giống như bạn truyền địa chỉ ô nhớ của biến đó vào hàm.

### 22. Kế thừa là gì?
>>Kế thừa trong java là sự liên quan giữa hai class với nhau, trong đó có class cha (superclass) và class con (subclass). Khi kế thừa class con được hưởng tất cả các phương thức và thuộc tính của class cha. Tuy nhiên, nó chỉ được truy cập các thành viên public và protected của class cha.
 
**Sử dụng từ khóa extends để kế thừa**
### 23. Đa hình
là một khái niệm mà chúng ta có thể thực hiện một hành động bằng nhiều cách khác nhau.
### 24. Trừu tượng
>>Tính trừu tượng là một tiến trình ẩn các cài đặt chi tiết và chỉ hiển thị tính năng tới người dùng.Tính trừu tượng giúp bạn trọng tâm hơn vào đối tượng thay vì quan tâm đến cách nó thực hiện.

**Có 2 cách để đạt được sự trừu tượng hóa trong java**
- Sử dụng lớp abstract
- Sử dụng interface
### 25. Vài câu hỏi trừu tượng?
-  Có phương thức trừu tượng không nằm trong lớp trừu tượng không?
    + Không, nếu có bất kỳ phương thức trừu tượng nào trong lớp, thì lớp đó phải là lớp trừu tượng.
- Có thể sử dụng cả abstract và final cho một phương thức không?
    + Không, vì phuong thức trừu tượng (abstract) cần phải được ghi đè, trong khi đó không thể ghi đè được phương thức final.
- Có thể khai báo một phương thức của interface với từ khóa static không?
    + Không. Vì mặc định các phương thức của một interface là trừu tượng, từ khóa static và abtract không thể được sử dụng chung với nhau.
- Sự khác nhau giữa trừu tượng và đóng gói là gì?
    + Trừu tượng là ẩn đi cài đặt chi tiết còn đóng gói là gói code và data vào một khối duy nhất.

## String
### 26. Immutable String trong java
 String trong Java là immutable nghĩa là chúng ta sẽ không thể cập nhật giá trị một String sau khi đã khởi tạo

>>Bởi vì String có tính chất Immutable, JVM có thể tối ưu hóa bộ nhớ bằng cách lưu trữ một bản sao giá trị của một String duy nhất trong đó và sử dụng lại khi có một String object khác khởi tạo có cùng giá trị với các String đã được lưu trong String pool.
### 11. Java String Pool 
- Là 1 vùng nhớ nằm trong Heap lưu trữ các object string
- Một dạng cache, tiết kiệm bộ nhớ, tăng tái sử dụng
![N|Solid](https://media.geeksforgeeks.org/wp-content/uploads/20200602014728/string_pool_3.png)

```sh
String value = “abc”
```
- **String literal**: Khi khai báo string kiểu trên JVM sẽ truy cập vào String pool tìm xem có cái ô nhớ nào có giá trị như này k, nếu có thì string mới sẽ tham chiếu tới địa chỉ của ô nhớ đấy; không có thì tạo ô nhớ mới

```sh
String value = new String(“abc”);
```
- Khai báo kiểu này thì Java sẽ tạo ô nhớ trong Heap luôn, nếu có ô nhớ có cùng giá trị vẫn tạo ô nhớ mới
![N|Solid](https://media.geeksforgeeks.org/wp-content/uploads/20200602104736/output_string_4.png)

### 12. Sự khác nhau giữa .equals() và == ( trong String)
- equals() là method, == là operator (toán tử).
- equals() so sánh nội dung , == so sánh địa ch.
### 27. Có bao nhiêu đối tượng được tạo ra trong đoạn code sau?
```sh
String s = new String("Hê hê");
```
Có 2 đối tượng được tạo ra. Một đối tượng được tạo ra trong string constant pool và một đối tượng được tạo ra trong bộ nhớ heap bởi từ khóa new. **(Not sure)**
Update: Confirmed by chat gpt và cái hình trên là chỉ có 1 đối tượng được tạo ra trong heap

## Java Collections Framework
### 28. Cấu trúc 
- 2 root Interface: collection và map
- Kế thừa Collection: 3 interface
    + Set
    + List
    + Queue
### 21. Sự khác nhau giữa Array và ArrayList là gì?
| Array |  ArrayList  |
| ------ | ------ |
| là một cấu trúc dữ liệu có kích thước cố định |một lớp Collection có thể thay đổi được kích thước |
| Chứa được kiểu dữ liệu nguyên thủy | Không thể chứa kiểu dữ liệu nguyên thủy |
| length | size |
| Nhanh hơn do kích thước cố định | Chậm hơn do dynamic |
### 29. Sự khác nhau giữa List và Set là gì?
- **List vs Set:** List có thể chứa các phần tử trùng lặp (dublicate), trong khi Set chỉ chứa các phần tử duy nhất.
- **Set vs Map:** Set chỉ chứa giá trị, trong khi Map chứa cặp key và value.










