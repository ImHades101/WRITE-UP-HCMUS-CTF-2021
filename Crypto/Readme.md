# CrackMe
+ Để giải nén được file zip đầu tiên ta crack sha512 bằng Hatcat.
+ Sau khi giải nén được file zip đầu tiên ta được file zip thứ 2, và ta cần giải nén nó để có được flag. 
+ Để giải nén được nó ta cần crack id_rsa, bằng cách tương tự như cách crack sha512.
+ Sau khi Crack thành công id_RSA ta được key để giải nén, chuyển key sang base 64 ta được pass giải nén và có được flag.
> FLAG: HCMUS_CTF{cracking_for_fun}

# Thechosenone