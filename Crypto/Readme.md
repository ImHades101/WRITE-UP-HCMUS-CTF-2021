# CrackMe
+ Để giải nén được file zip đầu tiên ta crack sha512 bằng Hatcat.
+ Sau khi giải nén được file zip đầu tiên ta được file zip thứ 2, và ta cần giải nén nó để có được flag. 
+ Để giải nén được nó ta cần crack id_rsa, bằng cách tương tự như cách crack sha512.
+ Sau khi Crack thành công id_RSA ta được key để giải nén, chuyển key sang base 64 ta được pass giải nén và có được flag.
> FLAG: HCMUS_CTF{cracking_for_fun}

# SanityCheck 
* Đầu tiên chúng ta thấy chuỗi mật mã đề cho có dạng giống base32 do đó chúng ta thử dán vào công cụ giải mã 
* Sau khi thử thì thấy khi đưa vào base32 thì nó lại trả về 1 chuỗi ở dạng base64(có dấu ==)
* Chúng ta lại tiếp tục đi giải mã base64 bằng trang kt.gy thì chúng ta đã tìm thấy flag ở dòng ROT13
> just make you open up your eyesHCMUS-CTF{We_are_Blackpinker_welcome_to_hcmus_ctf_2021}

# SingleByte
* Đề bài cho chúng ta 1 file chứa cipher và tên bài, chúng ta nghĩ ngay về việc xor
* Chúng ta sẽ viết 1 đoạn mã để xor cipher đề cho và key thì chúng ta brute force 
```python
from pwn import *
from base64 import b64decode
with open("singlebyte.txt", "rb") as file:
  ciphertext = b64decode(file.read())
for key in range(256):
    print "key: ",key
    plaintext = xor(key, ciphertext)
    if "HCMUS-CTF{" in plaintext:
        print plaintext
```