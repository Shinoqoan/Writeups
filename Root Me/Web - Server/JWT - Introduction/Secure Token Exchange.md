# JWT - Introduction

**Tên challenge:**  Secure Token Exchange

**Link challenge:** [Here](https://www.root-me.org/en/Challenges/Web-Server/JWT-Introduction)

**Tác giả challenge:** Kn0wledge

**Mục tiêu challenge:** To validate the challenge, connect as admin.

**Tác giả Writeup:** Shino

---

# Bài giải

**B1:** Đầu tiên, ta thấy trang Web có 1 chức năng đăng nhập như sau:
	
![alt text](./images/image.png)

**B2:** Ta click vào `Login as Guest`

![alt text](./images/image-1.png)

Lúc bấy giờ, khi bắt lại gói tin thì ở phần Cookie đã được cấp JWT Token như sau:
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk
```

Ta thử đem lên trang Web `https://jwt.io/` để decode JWT Token này.

![alt text](./images/image-2.png)

=> Ta chỉ có thể biết được phần Header và Payload của JWT Token.

Do ta không chắc được liệu Website có xử lý đúng cách với phần Signature không nên ta sẽ thứ đổi phần `guest` trong Payload thành `admin` và đổi luôn `alg` trong Header thành `none` để xem Server sẽ xử lý ra sao.

**B3:** Ta tiến thành thử giả thuyết trên

![alt text](./images/image-3.png)

Từ ảnh trên, ta đã thành công thay đổi JWT Token và lấy được `Flag`.