# Fix darkwake

> Trước hết ta phải biết, darkwake là gì? Darkwake là khi bạn wake máy sau sleep máy chỉ thức dậy 1 phần còn phần còn lại vẫn còn đang ngủ 

B1: Xác định những phần cần patch theo bảng sau 

![](https://lh3.googleusercontent.com/TjtlF8DFtuJsl0Snd-1I_auyFN-Fr9P9ZZscbBG_RAnNn928URtNr_GnAgqe0qZfGTupa0_n9G3ta8N-102NlqAcS_evaXsh6xTIa3wuWrPetYLuY0s1R6o3qavZqniVFuPBpR9p=s0)

B2: Cộng tất cả value của các phần bạn cần patch như ở đây mình cần patch 

`1,3,512` thì mình sẽ làm như sau `1+3+512 = 516 `

B3: Thêm vào boot-arg đoạn code sau: `darkwake=XXX` (thay XXX bằng value mà bạn thu được)

B4: Save lại và reboot

**Lưu ý: Source tham khảo** [**Các vấn đề về đánh thức bàn phím | OpenCore Sau cài đặt (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/usb/misc/keyboard.html#method-3-configuring-darkwake)

**Lưu ý 2: Nếu quá bối rối không biết nên chọn cái nào thì các bạn thêm đoạn code sau vào boot-arg `darkwake=0` ( nó sẽ vô hiệu hoá darkwake )**
