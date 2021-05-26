# Dodge
* Bài này thì chúng ta không thể dùng các lệnh cơ bản để đọc flag.
* Idea là thử excute cái file flag xem nó bị error base mà thông báo flag không
* Dể excute thì có nhiều cách như dùng `sh` ,`.`, ...
* Thì chúng tôi solved bằng cách sử dụng `.` flag

# StrangerThing
* Đầu tiên chúng ta chỉ cần cat flag1.txt đầu tiên
* Sau đó chúng ta sẽ dùng python để đọc được nội dung của file -flag 2.txt
* Tiếp đó chúng ta sẽ cd vào secret và dùng ls -a chúng ta sẽ thấy thêm 1 file .flag3.txt và chỉ việc cat nó và ghép 3 flag lại là ok

![StrangerThing](https://user-images.githubusercontent.com/51597903/119295256-92395a00-bc80-11eb-843e-58c90a822bda.png)

# Escapeme
* Connect to server we will see the flag.txt. But we must root to cat it. Find some thing in server help us become root by command `sudo -l`.
* After login as root, we `cat flag`: 
* 
