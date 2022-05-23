## Một vài lỗi tôi gặp phải với Go và Goland

Từ khi chuyển qua xài đồ của nhà Jetbrains (**Goland**) thì tôi khá lúng túng trong việc dùng cái IDE vì nhiều chức năng hơn. Tuy nhiên, trải nghiệm code đúng là tuyệt vời hơn hẳn VSCode ở việc gợi ý syntax và auto import code. 

Riêng extension `vim` tôi chưa dùng nổi vì nhiều phím tắt trong Goland mình chưa thành thạo. Thêm vào đó, nhiều phím tắt cũng bị ghi đề khi dùng với `vim` và việc phải config lại chúng và nhớ phím tắt mới khiến việc code trở nên rất khó chịu. Hi vọng trong thời gian tới tôi sẽ thành thạo Goland hơn.

Sau đây là một vài lỗi tôi đã từng gặp khi code Go bằng Goland. Hi vọng là chúng giúp ích cho bạn.

# How to fix IntelliJ cannot resolve symbol

Thi thoảng, tôi gặp trường hợp Goland bị làm sao đấy mà không tìm thấy được cái package được import vào ở từng file.

![Screen Shot 2022-05-22 at 11.50.23 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653238217374/7yjLLYgHc.png align="center")

Cách tôi fix là chọn `File Menu → Invalidate Caches / Restart`. Chờ một vài giây cho IDE restart lại thì lỗi sẽ được fix. Ngoài ra, nếu bạn vẫn còn gặp lỗi thì nên xem cái link [này](http://sbytestream.pythonanywhere.com/blog/How-to-fix-IntelliJ-cannot-resolve-symbol) để tham khảo các cách giải quyết khác.

# A fatal error has been detected by the Java Runtime Environment

Tôi bị lỗi này sau khi cập nhật Goland lên phiên bản mới. Chạy một lúc là IDE bị dừng lại và restart rồi lại dừng và restart. Google ra thì mới biết là mặc định Goland dùng **JRE** (Java runtime enviroment) làm JetBrains Runtime. Trường hợp của tôi theo như các issues trên Github chỉ thì do JetBrains Runtime bị lỗi nên cần đổi sang một JRE khác.

> As a Java application, IntelliJ IDEA requires a Java runtime environment (JRE). By default, IntelliJ IDEA uses [JetBrains Runtime](https://github.com/JetBrains/JetBrainsRuntime)
 (a fork of [OpenJDK](https://github.com/openjdk/jdk)
), which is included with the IDE. JetBrains Runtime fixes various known OpenJDK and Oracle JDK bugs, and provides better performance and stability. However, in some cases you may be required to use another Java runtime or a specific version of JetBrains Runtime.

Cách khắc phục là chọn JRE khác như hướng dẫn của JetBrains ở [đây](https://www.jetbrains.com/help/idea/switching-boot-jdk.html). Chọn `Help | Find Action` → `Choose Boot Java Runtime for the IDE` -> `OK`

# C compiler "gcc" not found: exec: "gcc": executable file not found in $PATH

Khi run test trong `Dockerfile` với cú pháp `RUN go test ./...`, có thể bạn sẽ gặp lỗi không tìm thấy package `gcc`.

![Lỗi ko tìm thấy package `gcc` khi chạy go test trong `Dockerfile`.](https://cdn.hashnode.com/res/hashnode/image/upload/v1653238321681/N3L89IVtY.png align="center")

Cách fix là [disable `CGO`](https://stackoverflow.com/questions/53479572/how-to-disable-cgo-for-running-tests) (công cụ hỗ trợ các lời gọi hàm từ C trong Go, nhờ đó, ta có thể sử dụng Go để đưa thư viện động của C (C dynamic libraby) sang các ngôn ngữ khác.) thông qua việc thêm `CGO_ENABLED=0`. Bên zalo có một seris bài rất hay về lập trình CGO, bạn có thể đọc để tham khảo thêm ở [đây](https://zalopay-oss.github.io/go-advanced/ch2-cgo/).

```Docker
# before: C compiler "gcc" not found: exec: "gcc": executable file not found in $PATH 
RUN go test -v ./....
# after: success
RUN CGO_ENABLED=0 go test -v ./...
```