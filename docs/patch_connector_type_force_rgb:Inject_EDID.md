# Patch Connect Type/ Force RGB/ Inject EDID

> Nói chung đây là 1 bài hướng dẫn fix lỗi lệch màu khi xuất màn hình.

## ***Patch Connect Type:***

> Phần này mình dành cho các bạn nào khi xuất HDMI bị lệch màu. 

### Chuẩn bị

-  [IOReg](https://github.com/khronokernel/IORegistryClone/blob/master/ioreg-302.zip) 

> hoặc [Hackintool](https://github.com/headkaze/Hackintool/releases)

-   [ProperTree](https://github.com/corpnewt/ProperTree)

### Tiến hành

 B1: Cắm HDMI vào và mở `IOReg` lên bạn sẽ tìm đến mục `IGPU` 

B2: Tìm đến mục `IGPU@2` ở đây màn hình của Laptop sẽ chứa `AppleBacklightDisplay` còn cắm màn hình rời sẽ chứa `AppleDisplay`

![](https://lh4.googleusercontent.com/zBcWFoFm6MX6A_IMIWLAkcWhS1G9hdS7LoSjsUpydhFQy9QIjigI5YkDVKCfwlqf-dq37gvmcXh3XwgyYJv2cElE9K7kPYe8US0eMV4mMorKVxJsXH756sa-GB0pS5Cz0iCIaWzI=s0)

> sau khi cắm màn rời.

Hoặc các bạn có thể dùng Hackintool để xác định: (chỉ dùng cho Card On)

B1: Mở Hackintool chuyển tới tab `Patch ⇒ Connect`

B2: Xác định index màn các bạn đang dùng sẽ có màu xanh khi cắm màn rời sẽ có màu đỏ:

![](https://imgur.com/I9nmJAn.png)

> hình ảnh chỉ mang tính chất minh hoạ nên nếu các thông số có không khớp nhau mong các bạn thông cảm

B3: Xác định conX ở đây các bạn sẽ quan tâm đến mục Connector Info ta có như sau theo thứ tự là con0, con1, con2 v.v từ trên xuống dưới.

B4: Các bạn dựa vào đây để xác định type cho cổng: 

![](https://lh6.googleusercontent.com/ZpmL2YsMwBW708xu1KWzTr-XxC4q1M70RBU07LmYUf0vQDNvZfZwuVisEoBfjeHeUrts35Zvm6bmy6T4NLsVpJnXP4cxwJzzZqPNGVLS1zEzZw5oVnyDmgGDRymQBGuoL602XTYm=s0)

B5: Xác định các code cần cần thiết:

```
framebuffer-patch-enable | data | 01000000
framebuffer-conX-enable |data | 01000000
framebuffer-conX-type | data | 00080000 (ở đây mình sử dụng cổng hdmi)
```

B5: Thêm các code này vào dòng 

`DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) `

> sau khi add các bạn sẽ được như hình

![](https://lh4.googleusercontent.com/hEKHXfqUx6nMILDAEEZQaEF1syvUArZi_zyk75qIA1I4W64RiOuIHd9uuL7_s8gR3Tx8-QS-tOkC-uO5g3wWdJzzTYgBPypnYcYHEO8jR-cxBccbYWOkLsGRWFFive0Z9i5n8dJu=s0)

B6: Save và Reboot. 

**Lưu ý: Nguồn tham khảo** [**Các loại đầu nối vá lỗi | OpenCore Sau cài đặt (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/intel-patching/connector.html) **|** [**Cài driver âm thanh, đừng để Hackintosh của bạn im lặng mãi!!!**](https://hackintosh.vn/driver-audio#hdmisound) 

## ***Force RGB:***

Sử dụng cách này khi màn của bạn bị như hình 

> thường đi kèm với Injects EDID

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-11.47.18.png?w=603)

B1: Các bạn tải file [tại đây](https://gist.github.com/adaugherity/7435890). 

B2: ​​Kéo file vào Terminal và nhấn Enter. Lưu ý lúc nãy các bạn hãy chắc chắn là đã cắm màn ngoài nếu chưa cắm thì Terminal sẽ báo lại như sau: 

![](https://lh5.googleusercontent.com/V7--wurPh9a71-GYxKFQ7UgXAmAYidcJohsh3PQ7OaXi_DZ16cBkPinDiBm9AzkZ1LFqIRNvpTfAPSx1pl2iHd_lfPHbXHC3_UrQ--NGJbfzI2TGu1lNRzQAywkAbeCIYeU2-BFW=s0)

![](https://lh3.googleusercontent.com/uRYVhQnahTdPslN00qdJad-loREsAbDg6iWHvIw_jW0kOuGyrs-vxyv0glF8yzSN-b5uwtjVKKOdfpmvUnEUzTCkoY-aj9rCTDYSSyZy0dqiK-3f6X_c0nYbOb9wNtaLLtBNYKlm=s0)

B3: Các bạn Chú ý vào mục output copy đường dẫn ở mục này ấn 

`Shift + Command + G` và dán đường dẫn vào rồi nhấn `Enter` các bạn sẽ được như sau: 

![](https://lh5.googleusercontent.com/dyT2RqtJ7LaDRcMcqFtZy2MSTtekFzsCM4uWE4yXjsJhw-azXxpdzsWtnxPqr44N4kfLYtV4nXKyRj5dC5lqt55EUj4UwZ1k7JAZmySOMpD10Yv8rDLXGTxekjPZKAKoiinZGJIP=s0)

Các bạn copy thư mục display vendorid.

B4: Nhấn tổ hợp phím `Shift + Command + G` r dán đường dẫn sau vào: `/Library/Displays/Contents/Resources` nếu như các bạn không có thư mục này thì hãy tạo nó trong mục Library tên của các thư mục ứng với đường dẫn ở trên. 

B5: Tạo thư mục `Overrides` trong thư mục `Contents`. 

B6: Bỏ thư mục `display vendorid` vào mục `Content` sẽ được như sau tiếp theo restart máy và tận hưởng thôi.

![](https://lh3.googleusercontent.com/BHAnfRYXzZrFH-9EwhHHXEUYt6h9PU8YP51AL_SLEFpZ5hMIb6I7UjpLfxlEPbOwjddaK0aZr8lpv4DD1xowt2vaPUTx4fSeyi5BKdMrsjTJVyaXuZLkCmoVZXuE5PlUT8rgHbbn=s0)

**Lưu ý: Source tham khảo** [**Chia sẻ – Fix màn hình ám tím (Force RGB) Hackintosh khi connect với màn hình ngoài | Vietnam ITX**](https://vietnamitx.com/t/fix-man-hinh-am-tim-force-rgb-hackintosh-khi-connect-voi-man-hinh-ngoai.171/).

**Lưu ý 2: Đối với cách này khi mà các bạn xuất nhiều màn thì hãy mua các màn giống nhau nếu không sẽ có 1 màn bị ám tím.**

**Lưu ý 3: Đối với các macOS dưới 11 (Big Sur) các bạn phải mount thư mục Library ra bằng Hackintool**.

![](https://lh4.googleusercontent.com/nToYKXYovvHLrlDNyxbD5Bn9Md8UZV9sucWjd4rM8gvridxDPRAlwIlHD5uZJWbmAJxzJp9Y8m3bkxcvskvNLb4s0TEUV_gTFw3otBklpX3d4Huh6leOZKWfr3gC6125XZzbAvLd=s0)

## Injects EDID:

### Cách 1: Dùng Hackintool

> Chỉ igpu

> Khi nào cần Injects EDID: Máy không nhận đủ thông tin màn hoặc bị lệch màu lỗi sleep/wake v.v

B1: Tải Hackintool [tại đây](https://github.com/headkaze/Hackintool)

B2: Mở Hackintool lên và nhấn vào mục Patch ⇒ Patch

![](https://lh3.googleusercontent.com/64UGDKnWJcb8r2NCCvvkjOfmuK-yw7V4hBVns3HZ35nztn2BTpXsBAxHggJl7g_SOIONGhwkaJLV9gDE0ie7liX0LS2-v0k0WwTWRJtBx2k0Xb0lYMVFe3IMSN6FQYUHcJyrSatn=s0)

B3: Tick vào mục EDID và 1 số mục khác như hình:

**Lưu ý: nhớ tick vào mục Graphics Device**.

![](https://lh5.googleusercontent.com/obqiLU16qx5J34bjbLlUvQSZ8pgmRleI5J2NyxzXU3XjpKB8tE9R_GtRz0eiRqcFi7tCcoyIsFPCWiItwYuGbAmpVaelL0DyuzH0qIGwp4D0wkIqd7BDnxlssDHjy7VzIgaMwEVE=s0)

B4: Ấn Generate Patch ![](https://lh3.googleusercontent.com/Fg0lUovs47VwGY_r6-uREWoeOB6m9ows7i_1n0XlPfGJd_WBLA-NgOE_sUbhBgPCfYBI_BCDu0ERHJ54fdAObYZW-ghnKXxrONhW3RdSDsmVvVNA6be7ZFMWpMjSFP85BdJomHWO=s0)

![](https://lh3.googleusercontent.com/YO4z53E_gxPJbVNZk6AFQe_FE195jHK_bjQFaUITK79GSYXz3sE3QQdqurOxH7svU18eCm_-9nzu4xYEC7l2K2No8evZlWdHqr4-ShIb0y4f9YDpU4v2mU2BacOMFl9hgPcgafQR=s0)

Các bạn chuyển mục 2 từ Base64 thành hex bằng hackintool ta sẽ có 

```
00 FF FF FF FF FF FF 00 0D AE 7C 9C 00 00 00 00 23 15 01 03 80 1F 11 60 0A 6F B1 A7 55 4C 9E 25 0C 50 54 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 12 1B 56 64 50 00 14 30 30 20 14 00 35 AE 10 00 00 18 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 00 00 FE 00 43 4D 4E 0A 20 20 20 20 20 20 20 20 20 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 92
```

Hoặc các bạn ấn Patch ⇒ Export bootloader config.plist 

![](https://lh4.googleusercontent.com/meOUolnvkjWiLkStRviRZiMly6YWkrm8uUlMxgxyeCoVgUhzhY2wIjp_YELKZ43IU9FBxoZaoY9MM7WuiGHhLF4Z1Ll3iEhaRUxbn79yGHm3ISXCdGQ3fdgyPM68b7Ue8bgyN-B9=s0)

B5: Copy mục `AAPL00,override-no-connect` hoặc tạo nó dưới mục `PciRoot(0x0)/Pci(0x2,0x0)` với giá trị như sau: 

```
AAPL00,override-no-connect | data | 00 FF FF FF FF FF FF 00 0D AE 7C 9C 00 00 00 00 23 15 01 03 80 1F 11 60 0A 6F B1 A7 55 4C 9E 25 0C 50 54 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 12 1B 56 64 50 00 14 30 30 20 14 00 35 AE 10 00 00 18 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 00 00 FE 00 43 4D 4E 0A 20 20 20 20 20 20 20 20 20 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 92
```

> mỗi máy giá trị mỗi khác nhưng sẽ chung tên và type

![](https://lh6.googleusercontent.com/MemjAD5uI99KgsXv3Yoy6b3ajXByPLVPuL6JjKViwCL8rE0kquj9Hxt_286T5NsSkiASI47E9TYXZ1Owz1glAhINhEokEF0Labw10ZNghSiY5TGU15yMXpAog3ojh2qBQHOMVKwy=s0)

Đối với Clover các bạn add `EDID` vào mục `Graphics ==> Custom EDID` và bật `Injects EDID` lên như hình:

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-11.56.25.png?w=1024)

**Lưu ý: Cách này chỉ dùng cho Card On.**

**Lưu ý 2: Các bạn có thể dùng one-key HIDPI để injects cho Card rời Download phần mền** [**tại đây**](https://github.com/xzhih/one-key-hidpi). 

B6: Save lại và Reboot.

### Cách 2: Dùng One-Key-HIDPI

B1: Các bạn tải `One-Key-HIDPI` [**tại đây**](https://github.com/xzhih/one-key-hidpi). 

B2: Chạy file `.command` lên.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.00.50.png?w=740)

B3: Chọn 2.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.01.12.png?w=740)

B4: Chọn Display Icons và nhập Pass.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.01.59.png?w=740)

B5: Chọn độ phân giải màn hình sau đó nhấn Enter (nếu độ phân giải màn hình của bạn không có trong đó thì bấm 6 sau đó gõ độ phân giải màn hình ra)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.03.35.png?w=740)

B6: Reboot và tận hưởng thành quả.

### Cách 3: Dump EDID bằng Windows

Dành cho 1 số bạn EDID bị lỗi.

B1: Các bạn mở Windows lên và search `reg` hoặc nhấn `Windows + R` rồi gõ regedit.

![](https://imgur.com/4vWHezu.png)

B2: các bạn tìm đến đường dẫn sau:

`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\DISPLAY`Ở đây tùy máy sẽ có 1 hoặc 2 folder các bạn mở folder có đuôi là `&UID0`

![](https://imgur.com/7dZVhiL.png)

B3: Các bạn tiến hành Export File `EDID` ra như ảnh:

![](https://imgur.com/CyBUJ94.png)

![](https://imgur.com/73tFZdf.png)

B4: Các bạn lấy EDID theo quy tắc sau:

![](https://imgur.com/LlgjHGb.png)

Lấy cột ở giữa viết từ đâu đến dầu gạch nối sau đó các bạn xóa dấu gạch nối và viết tiếp dãy đằng sau ta sẽ được:

```
00 ff ff ff ff ff ff 00 0d ae 72 14 00 00 00 00 23 15 01 03 80 1f 11 78 0a d1 45 9b 59 57 8e 2b 23 50 54 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 12 1b 56 64 50 00 14 30 30 20 14 00 35 ae 10 00 00 18 00 00 00 fe 00 4e 31 34 30 42 47 45 2d 4c 34 32 0a 20 00 00 00 fe 00 43 4d 4e 0a 20 20 20 20 20 20 20 20 20 00 00 00 fe 00 4e 31 34 30 42 47 45 2d 4c 34 32 0a 20 00 06
```

B5: Các bạn add vào mục AAPL00,override-no-connect tạo nó dưới mục PciRoot(0x0)/Pci(0x2,0x0) với giá trị như sau: 

```
AAPL00,override-no-connect | data | 00 FF FF FF FF FF FF 00 0D AE 7C 9C 00 00 00 00 23 15 01 03 80 1F 11 60 0A 6F B1 A7 55 4C 9E 25 0C 50 54 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 12 1B 56 64 50 00 14 30 30 20 14 00 35 AE 10 00 00 18 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 00 00 FE 00 43 4D 4E 0A 20 20 20 20 20 20 20 20 20 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 92
```

> mỗi máy giá trị mỗi khác nhưng sẽ chung tên và type

![](https://lh6.googleusercontent.com/MemjAD5uI99KgsXv3Yoy6b3ajXByPLVPuL6JjKViwCL8rE0kquj9Hxt_286T5NsSkiASI47E9TYXZ1Owz1glAhINhEokEF0Labw10ZNghSiY5TGU15yMXpAog3ojh2qBQHOMVKwy=s0)

Đối với clover các bạn add EDID vào mục graphics ==> custom edid và bật injects edid lên như hình

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-11.56.25.png?w=1024)

B6: Reboot và tận hưởng.

### Cách 4: Dùng IOReg

B1: Các bạn tải IOReg [tại đây](https://github.com/vulgo/IORegistryExplorer/releases/tag/v2.1).

B2: Các bạn mở IOReg lên và search mục display:

Các bạn chọn mục bên dưới mục display 0 (là apple display nếu dùng màn rời).

![](https://imgur.com/rarfbLL.png)

B3: Các bạn copy mục IODisplayEDID.

B4: Các bạn add vào mục `AAPL00,override-no-connect` tạo nó dưới mục `PciRoot(0x0)/Pci(0x2,0x0)` với giá trị như sau :

```
AAPL00,override-no-connect | data | 00 FF FF FF FF FF FF 00 0D AE 7C 9C 00 00 00 00 23 15 01 03 80 1F 11 60 0A 6F B1 A7 55 4C 9E 25 0C 50 54 00 00 00 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 01 12 1B 56 64 50 00 14 30 30 20 14 00 35 AE 10 00 00 18 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 00 00 FE 00 43 4D 4E 0A 20 20 20 20 20 20 20 20 20 00 00 00 FE 00 4E 31 34 30 42 47 45 2D 4C 34 32 0A 20 00 92
```

> mỗi máy giá trị mỗi khác nhưng sẽ chung tên và type

![](https://lh6.googleusercontent.com/MemjAD5uI99KgsXv3Yoy6b3ajXByPLVPuL6JjKViwCL8rE0kquj9Hxt_286T5NsSkiASI47E9TYXZ1Owz1glAhINhEokEF0Labw10ZNghSiY5TGU15yMXpAog3ojh2qBQHOMVKwy=s0)

Đối với Clover các bạn add `EDID` vào mục `Graphics ==> Custom EDID` và bật `Injects EDID` lên như hình:

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-11.56.25.png?w=1024)

B5: Reboot và tận hưởng.

**Lưu ý: Source tham khảo của Cách 4 là [How to Inject EDID [Clover/OpenCore] | EliteMacx86 Forum](https://elitemacx86.com/threads/how-to-inject-edid-clover-opencore.576/)**
