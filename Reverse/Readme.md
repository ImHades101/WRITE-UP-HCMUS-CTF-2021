### Written by Konoha

# Faded
+ Vì đây là file được viết bằng python nên sử dụng `pyi-archive_viewer` để decompile lại file thực thi.
+ Cú pháp: `pyi-archive_viewer authentication` Sau đó `X authentication` để extract file, tiếp tục `output.pyc` để lưu vào file *output.pyc* 
+ Vì flag không biến đổi gì nên có thể xem flag bằng câu lệnh `cat`. Nếu flag bị biến đổi thì phải đưa file `.pyc` về file `.py` để đọc code
> FLAG: HCMUS-CTF{Python_is_fun_somehow}

# AndroidRev
+ Các tool, web sử dụng: **Bytecode-Viewer-2.9.22.jar** để decompile và đọc code java, **apktool_2.5.0.jar** để dump file để có thể xem phần resources, *https://www.md5online.org/md5-decrypt.html* để decrypt MD5.
+ Đầu tiên dump file **androidrev.apk** sau đó đọc đưa file **androidrev.apk** vào **Bytecode-Viewer-2.9.22.jar**, đọc  code tại file **/com/hcmusctf/androidrev/FlagChecker.class** hàm **checkFlag** để hiểu rõ bản chất vấn đề.
+ Giải thích một chút về `var0.getString(2131492904)` tại hàm "me": chương trình sẽ lấy chuỗi tương ứng với số *2131492904*; để tìm ra được chuỗi tương ứng với số này, vào file `R.class` và tìm kiếm số này và lấy tên biến được gán chuỗi đó (Ví dụ: số *2131492904* thì lấy tên biến tương ứng là *ml*) sau đó vào folder được dump file **androidrev.apk** ra bởi **apktool_2.5.0.jar**, ở **androidrev\res\values** sẽ thấy được file *strings.xml*, mở lên và tìm chuỗi tương ứng với tên biến (Ví dụ chữ *ml* thì sẽ được chuỗi *slauqe*). Tương tự các phần còn lại, ta rút gọn được vấn đề:
```java
me(var0, dh(MD5, var3[0]), 6e9a4d130a9b316e9201238844dd5124) 
me(var0, dh(MD5, var3[1]), 7c51a5e6ea3214af970a86df89793b19) 
me(var0, dh(MD5, var3[2]), e5f20324ae520a11a86c7602e29ecbb8) 
me(var0, dh(MD5, var3[3]), 1885eca5a40bc32d5e1bca61fcd308a5) 
me(var0, dh(MD5, var3[4]), da5062d64347e5e020c5419cebd149a2) 
me(var0, dh(SHA-256, var1), 58150e58ae8a7275fcce5aea7d983ab5654f549cbeecedec27c89fe8246937d5) 
```
+ Sử dụng *https://www.md5online.org/md5-decrypt.html* để decrypt các mã MD5 trên.
> FLAG: HCMUS-CTF{peppa-9876543-BAAAM-A1z9-3133337}

# WeirdProtocol
+ Sử dụng `Detect it Easy` xem sơ qua file, và phát hiện ở phần resources có một tệp nhị phân.
+ Sử dụng `pestudio` để dump file từ resource ra và reverse cả 2 file. Hiểu được chương trình `WeirdProtocol.exe` khi thực thi sẽ cho nhập password, sau đó chương trình sẽ gửi 2 lần, lần 1 sẽ là gửi độ dài của chuỗi nhập, lần 2 sẽ là gửi chuỗi nhập.
+ Ở file server chú ý tại:
```c
	if ( !j__memcmp(Buf1, Buf2, 32u) )
	{
		for ( i = 0; i < 30; ++i )
	  		v6[i] ^= v12[(int)i % v10[0]];
		Str = v6;	
	}
	else
	{
		Str = "Nice try buddy";
	}
```
+ Đoạn này sẽ kiểm tra Buf1(sử dụng chuỗi đã nhập và biến đổi các thứ) và Buf2 (được gán phía trên) có giống nhau không, sau đó sẽ biến đổi chuỗi nhập *V12* với *V6* thành flag và gán vào *Str*.
+ Sử dụng v6[0] đến v6[9] để XOR với 'HCMUS-CTF{' thì thấy được của chuỗi *hellohello*. Thử sử dụng chuỗi input là *hellohellohellohellohellohellohellohello* để nhập vào từ client.
+ Tại server, path tại địa chỉ *00327994* từ `jnz     short loc_3279FC` thành `jz     short loc_3279FC`. Flag sẽ gửi xuống server.
```
[+] Please enter the password: hellohellohellohellohellohellohellohello
[+] Connecting to the online service ...
[+] Fail, reconnecting ...
[+] Fail, reconnecting ...
[+] Fail, reconnecting ...
[+] Fail, reconnecting ...
[+] Fail, reconnecting ...
[+] Fail, reconnecting ...
[+] Fail, reconnecting ...
-----> HCMUS-CTF{not_so_weird_hehexd}
```
> FLAG: HCMUS-CTF{not_so_weird_hehexd}

# Mix_VM
+ Load vào IDA và phân tích, tôi thấy hành động chính của chương trình là chia đoạn **input** thành từng khối, mỗi khối 4 byte và xử lý theo luồng và so sánh với chuỗi trong chương trình tạm gọi là `cipher`: ( `input` ^ `cipher trước đó` ) + `0x13371337` == `cipher`.
+ Debug chương trình lấy được khối của `cipher` : `{0xdeadbeef, 0x9f1810de, 0xde9250c4, 0x95323eb9,  0x3906b0c, 0x7e1a318a, 0x1b7c6a1b, 0x873f48ad, 0xfb844c29, 0xa31e3c94, 0xf9e6502, 0x7c282aa9, 0x147c5f12}`
+ Bài toán ta có:
```
(cipher[1] – 0x13371337) ^ cipher[0] = input[0] 
(cipher[2] – 0x13371337) ^ cipher[1] = input[4]
(cipher[3] – 0x13371337) ^ cipher[2] = input[8]
(cipher[4] – 0x13371337) ^ cipher[3] = input[12]
(cipher[5] – 0x13371337) ^ cipher[4] = input[16]
(cipher[6] – 0x13371337) ^ cipher[5] = input[20]
(cipher[7] – 0x13371337) ^ cipher[6] = input[24]
(cipher[8] – 0x13371337) ^ cipher[7] = input[28]
(cipher[9] – 0x13371337) ^ cipher[8] = input[32]
(cipher[10] – 0x13371337) ^ cipher[9] = input[36]
(cipher[11] – 0x13371337) ^ cipher[10] = input[40]
(cipher[12] – 0x13371337) ^ cipher[11] = input[44]
(cipher[13] – 0x13371337) ^ cipher[12] = input[48]
```
+ CODE (C):
```c
#include <stdio.h>
unsigned int cipher[13] = {0xdeadbeef, 0x9f1810de, 0xde9250c4, 0x95323eb9, 0x3906b0c, 0x7e1a318a, 0x1b7c6a1b, 0x873f48ad, 0xfb844c29, 0xa31e3c94, 0xf9e6502, 0x7c282aa9, 0x147c5f12};
int main()
{
	int i=0;
	unsigned int key = 0x13371337;
	char input[4] = {0};
	for (;i<12;i++)
	{
		unsigned int temp = cipher[i+1] - key;
		temp ^= cipher[i];
		input[3] = (temp&0xff000000) >> 24;
		input[2] = (temp&0xff0000) >> 16;
		input[1] = (temp&0xff00) >> 8;
		input[0] = (temp&0xff);
		printf("%c%c%c%c",(char) input[0],(char)input[1],(char)input[2],(char)input[3]);
	}
	return 0;
}
```
> FLAG: HCMUS-CTF{i_like_using_vm_to_protect_my_program}