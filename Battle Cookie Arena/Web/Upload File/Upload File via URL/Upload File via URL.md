# Upload File

**Tên challenge:** Upload File via URL

**Link challenge:** [Here](https://battle.cookiearena.org/challenges/web/upload-file-via-url)

**Tác giả challenge:** MEME

**Mục tiêu challenge:** You can easily upload and display files from a URL. This user-friendly web application uses Python, Flask, and Bootstrap, making it simple to transfer files from the web to your local storage and view them. Enter the file URL you want to upload in the provided field. Click the "Upload" button. The website will fetch the file, save it securely, and display it for your convenience. Flag: `/flag.txt` or URL Endpoint `/flag`

**Tác giả Writeup:** Shino

---

# Bài giải

**B1:** Đầu tiên, giao diện Website chỉ có Upload file from URL:

![alt text](image.png)

**B2:** Ta thử nhập 1 URL image để xem liệu Website sẽ trả kết quả gì?

![alt text](image-1.png)

=> Xem ra trang Web này sẽ đi đến 1 URL do ta cung cấp và lấy nội dung file ảnh trong URL về.

Sau một vài lần thử chức năng Upload thì ta biết được nếu không phải định dạng URL có chứa `://` thì Website sẽ trả về `Invalid URL`.

Và trong các bài kiểm tra bảo mật web, khi ta phát hiện một trang web có khả năng đọc và trả về nội dung từ một URL cung cấp, điều này gợi ý rằng trang có thể gặp vấn đề với LFI (Local File Inclusion) hoặc RFI (Remote File Inclusion).

=> Ta có thể sử dụng payload như: `file:///etc/passwd` để kiểm tra và cũng là payload phù hợp với định dạng URL mà Website yêu cầu.

**B3:** Ta thử input payload trên: `file:///etc/passwd`

![alt text](image-1.png)

Khi ta vào gói tin Burpsuite để xem thì ta đọc được nội dung của file `/etc/passwd`

![alt text](image-2.png)

=> Ta đã thành công đọc được file `/etc/passwd`

Tiếp theo, ta chỉ cần lấy `Flag` thông qua payload `file:///flag.txt` thôi.