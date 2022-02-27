## Viết một cái tool tính tạ

Cuối tuần rảnh nên tôi có viết một cái **tool tính tạ** tên là [Plate Calculator](https://plate-calculator.herokuapp.com/). Tool này dành cho những ai làm biếng tính nhẩm hoặc tính nhầm toàn sai (như tôi).

# Dành cho các bạn là dev

Tool nhà dùng nên tôi cố gắng viết càng nhanh càng tốt, bỏ qua việc thiết kế UI lung linh, miễn sao là số thao tác của tôi để tính tạ ở mức nhỏ nhất. Do vậy, code hết sức bình thường: **HTML, Bootstrap, Jquery**, dùng **Expressjs** làm server, và host trên **Heroku**.

Source code của tool: https://github.com/japananh/plate-calculator.

# Tool này để làm gì?

## Giải thích khái niệm cho người mới

Với những bạn chưa quen tập tạ, tôi sẽ giải thích ngắn gọn như sau. Còn ai biết rồi thì thôi kéo xuống mục sau đọc tiếp nhé.

![oso-color-collars-web3-1.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1645433266257/h18myYTnD.jpg)

Trong hình là 3 dụng cụ tối thiểu bạn cần có để tập:

- **Thanh đòn** (Barbells) có KÍCH CỠ CHUẨN là **15kg (dài 1.8m)** hoặc **20kg (dài 2.2m)**.
- **Bánh tạ** (Plates) có khối lượng từ 0.25 kg tới 25 kg, bao gồm **0.25, 0.5, 1, 2, 5, 10, 15, 20, 25**.
![rogue_competition_plates_kglb__1600274002_9c368352_progressive.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1645433283159/HGGG2WKtz.jpeg)
- **Khóa tạ** (cái màu xanh dương trên hình) để giữ cho các bánh tạ cố định trên thanh đòn, có khối lượng cỡ 50-150 gram (thường được bỏ qua khi tính khối lượng tạ nên chúng ta sẽ bỏ qua khối lượng của em này).

Đối với những bạn nâng tạ theo phương pháp Linear Progression (hiểu đơn giản tăng tạ liên tục qua mỗi buổi tập hoặc mỗi tuần tập) thì việc tính toán tạ rất hay bị nhầm. Ví dụ, có hôm tôi lắp nhầm 45 kg thay vì 55 kg và cứ thấy tạ nhẹ thế. :D

## Mục đích

Với cái tool này thì mọi người chỉ cần **cài đặt sẵn số lượng loại bánh tạ** tôi có, **nhập số tạ mong muốn** là tool sẽ tự động tính số loại bánh tạ cần thiết cho mức tạ giúp tôi lắp tạ dễ dàng hơn.

![273905932_929792614406242_1302445064532442680_n.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1645436073871/bMY4ygSxY.png)

# Cách dùng tool

Đường link tới tool: https://plate-calculator.herokuapp.com

Tool này sẽ có 2 chế độ: **Single** và **Range**. (Tôi đặt tạm hai cái tên, ai thấy tên nào hay hơn có thể comment nhé.)

## Chế độ 1 - Single

Video hướng dẫn: https://www.youtube.com/watch?v=r875bSJjcYo

Chế độ **Single** dùng để tính cách lắp tạ từ một mức tạ nhất định dựa trên bánh tạ và đĩa tạ bạn có.

## Chế độ 2 - Range

Video hướng dẫn: https://www.youtube.com/watch?v=7w1HeyF-AIY

Chế độ **Range** dùng để kiểm tra bánh tạ và đĩa tạ bạn có đủ để lắp một khoảng tạ nào đó không.