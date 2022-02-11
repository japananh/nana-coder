## [UPDATED] Tối ưu bộ nhớ cho struct trong Go

Hôm nay, tôi đọc một bài báo khá hay trên **Medium** về cách tăng tốc cho struct trong ngôn ngữ Go. Sau đây là những ý chính của bài báo. Các bạn có thể đọc bài gốc bằng tiếng anh ở [đây](https://betterprogramming.pub/how-to-speed-up-your-struct-in-golang-76b846209587). 

**[UPDATED]**: Hôm sau, tôi có tình cờ đọc một bài khác có cùng cách giải thích nhưng trình bày kỹ hơn về phần CPU - điều mà tôi thấy hơn khó hiểu ở bài trên.  Tôi nghĩ tác giả đã viết tốt hơn. Bạn có thể đọc ở [đây](https://anandadwirahma.medium.com/save-memory-in-golang-by-compose-struct-correctly-f649d1f457dd).

Khác với ```type``` trong **TypeScript** là thay đổi thứ tự khai báo các trường không ảnh hưởng tới perfomance. 

Ví dụ, ```FirstObject``` và ```SecondObject``` có các trường giống hệt nhau, chỉ thay đổi thứ tự và chúng không hề ảnh hưởng tới performance.

```typescript
type FirstObject = {
	age          number
	passportNum  number
	siblings     number
}

type SecondObject = {
	age          number
	passportNum  number
	siblings     number
}
```

Trong **Go**, khi sắp xếp lại thứ tự các trường trong ```struct```, bạn có thể cải thiện đáng kể tốc độ và mức sử dụng bộ nhớ của chương trình Go của mình.

Trong ví dụ dưới, ```BadStruct``` chiếm nhiều bộ nhớ hơn ```GoodStruct```.

```go
type BadStruct struct {
	age         uint8
	passportNum uint64
	siblings    uint16
}

type GoodStruct struct {
	age         uint8
	siblings    uint16
	passportNum uint64
}
```

Lời giải thích nằm trong cách máy tính điều chỉnh cấu trúc dữ liệu (**Data Structure Alignment**).

CPU đọc dữ liệu theo kích thước của một word. Trong hệ điều hành 64 bit thì 1 word tương ứng với 8 bytes,còn với hệ điều hành 32 bit thì 1 word tương ứng với 4 bytes.

Từ đó, ta sẽ có hình ảnh mô tả việc bộ nhớ dùng cho ```BadStruct``` ở hệ điều hành 64 bit như sau.

![1_CZcHPvsxBLAzcSJy3fVshg (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644150028818/Au6VqV3yZ.png)

Để truy cập biến ```passportNum```, CPU cần phải đọc 2 words. Để chạy hiệu quả hơn thì CPU cần tới data structure alignment. Biến ```passportNum``` giờ nằm trên 1 word.

![1_x4iN2ZOCdOHFPAtZsPRi5g.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644150465749/6-pYRvbJz.png)

Bằng việc thay đổi thứ tự các trường, việc lưu trữ 13 bytes cần 16 bytes (2 words) thay vì 24 bytes (3 words).

![1_jgC5vTRNDpS-ndq2iiH-ig.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644150619128/bLe5A6IWf.png)
