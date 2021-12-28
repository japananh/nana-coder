## Viết blog trên Medium - Phần 2

[Phần 1](https://nanacoder.hashnode.dev/viet-blog-tren-medium-phan-1), tôi đã viết về cách viết bài dùng markdown trên Medium. Cả 2 cách đều khá tốn công và khó quản lý. 

Hôm nay tôi sẽ giới thiệu một cách hay đó là tạo 1 repository chứa tất cả các bài viết dưới dạng markdown, để trên Github, rồi muốn post bài nào chỉ cần gõ 1 dòng lệnh là xong. 

Và bạn còn có thể post lên được nhiều nền tảng blog khác như [dev.to](https://dev.to/).

# mdm

Thư viện tôi dùng là [mdm](https://reposhub.com/nodejs/command-line-apps/pavanjadhaw-mdm.html).

**Bước 1,** bạn phải cài thư viện mdm.

```bash
npm i -g @pavanjadhaw/mdm
```

**Bước 2**, tạo intergration token trên Medium ở phần [setttings](https://medium.com/me/settings).

![Screenshot from 2021-12-29 00-04-21.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640711121441/_zIEjnsDL.png)

**Bước 3**, sau khi tạo xong token, bạn copy token, thêm vào cuối file `~/.bashrc` hoặc `~/.zshrc` như sau.

```bash
echo '# Medium configs' >> ~/.bashrc
echo 'export MEDIUM_TOKEN="token here"' >> ~/.bashrc
source ~/.bashrc
```

**Bước 4**, tạo `MEDIUM_ID` bằng lệnh

```bash
mdm init
```

**Bước 5**, thêm `MEDIUM_ID` vào `~/.bashrc` hoặc `~/.zshrc`.

```bash
echo 'export MEDIUM_ID="..."' >> ~/.bashrc
source ~/.bashrc
```

**Bước 6**, publish bài lên Medium

```bash
mdm publish path/to/markdown.md
```

File markdown của bạn phải theo format ở dưới. `status` có thể là `draft` (bài nháp) hay là `public`. 

```md
---
title: My Awesome Post
tags: ['some', 'tags', 'here']
status: draft
---
## markdown here
```

Và thế là chúng ta đã post bài lên Medium thành công mà không cần phải gõ trên cái editor khó chịu nữa.

![264435404_241232288150314_668586592651422605_n.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640712091761/5nv6bLYZI.png)

# Post bài lên dev.to

Để post bài lên dev.to, bạn cần chạy script sau.

```bash
curl -X POST -H "Content-Type: application/json" -H "api-key: <your-api-key>" -d '<your-content>' https://dev.to/api/articlesL161UPYsb"
```

Trong đó:
- `<your-api-key>` bạn lấy ở dev.to phần settings.
- `<your-content>` là nội dung bài viết của bạn. Ví dụ

```bash
{"article":{"title":"Title","body_markdown":"<h1>Body</h1>","published":false,"tags":["discuss", "javascript"]}}
```

# Refferences

- https://reposhub.com/nodejs/command-line-apps/pavanjadhaw-mdm.html
- https://developers.forem.com/api#operation/createArticle

