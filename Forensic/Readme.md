# NiceEars
Mở file audio lên trong sonicvisual thì tìm được key là: M0nK3y_doNkeY
Sau đó Giải nén file thì tìm được flag
# Maquerade
Chúng ta có file  *.pcap, bypass các check. Flag được chi làm 2 phần
Phần đầu tiên trong file OTP.mp3 (pwd785 $ ⇒ 1/2 cờ: HCMUS-CTF {Just_Network_Stuff_)
Phần thứ hai tại file Checkpass.class. BruteForce => nhận được key thứ hai và đây là phần 2 của flag: 897268 $}
# memory
Đầu tiên dùng voltality để dump consoles ra thì biết được là flag nằm trên mạng, và lấy được 1/2 key đầu tiên
Tiếp theop dùng option memdump để dump tất cả process ra, tìm "https" thì tìm được file driver đã được mã hoá
chú ý key 1 cung cấp có cụm từ "first pharse". Thế tìm key còn lại theo từ khoá key hoặc second phrase ⇒ ra key 2
# metadata
Bài này đơn giản chỉ cần pull docker sau đó đọc log docker với command `docker image history [image] --no-trunc` là ra flag.
# TestYourCmd
Chall này thì có rất nhiều file, focus vào file .log thì phát hiện ra có 1 file ảnh Master.png nhưng lại không mở được. 
Sau khi chỉnh sửa file signature về của file png thì nhận được file ảnh bảo là có 1 email đã gửi từ Messi đến Ronando
Lục trong đống log đó ra được email đó, sau khi decode bs64 ta passphrase
Mình code 1 scrypt để dùng passphase này làm pass cho các ảnh đó
```bash
file_path=/mnt/c/Users/groot/Downloads/Evidences/Evidences/Secret
for entry in $file_path/*
do
    steghide extract -sf $entry -p SuPer_Gold_3ymArJr.
done
```
Từ đó tìm được flag
