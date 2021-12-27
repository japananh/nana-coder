## Viết blog trên Medium - Phần 1

Hầu hết bạn bè tôi đều đọc bài trên **Medium** và chính tôi cũng thấy trang này khá thú vị. Đã quen viết **markdown** (**md**) trên **Hashnode** và vốn là một đứa ngây thơ nên tôi đã nghĩ chắc editor của Medium cũng xịn nên copy nguyên md vào và nhấn nút **Publish**.

![Screenshot from 2021-12-28 01-04-42.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640628381610/u7m7BaHtx.png)

Ai ngờ editor xịn quá lại không nhận markdown. Có một sự thất vọng không hề nhẹ. =))

![Screenshot from 2021-12-28 01-05-10.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640628364752/cGNpJS3kN.png)

Tôi mò lên google search thử thì phát hiện ra hai cách dùng markdown trong Medium. 

- Cách 1: Tạo 1 file `gist` có nội dung bạn mong muốn rồi import vào Medium. Chọn mục `Stories -> Import a story` hoặc vào đường dẫn https://medium.com/p/import.

![Screenshot from 2021-12-28 01-17-08.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640629061869/zDOVZ08Oc.png)

- Cách 2: Dùng tool convert markdown sang định dạng text của Medium, rồi copy vào editor. Bạn có thể tham khảo tool [markdown-to-medium](http://markdown-to-medium.surge.sh/).

Vốn là một đứa lười biếng, tôi ngồi mò tiếp để viết script tự động post bài từ repository trên Github lên Medium. Tôi tìm được 2 thư viện là [md-publisher](https://github.com/andremueller/md-publisher) và [mdm](https://github.com/fanderzon/markdown-to-medium-tool).

Bài hôm nay tạm dừng ở đây. Mai tôi sẽ viết tiếp về phần chạy hai thư viện này.