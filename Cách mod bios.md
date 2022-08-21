# Cách mod bios

Hiện nay có nhiều bios đặc biệt là hp bios nó rất tù bạn hầu như chả làm gì được điều này gây khó khăn rất lớn cho việc hackintosh

## Chuẩn bị

B1: tải uefi tool [tại đây](https://github.com/LongSoft/UEFITool/releases)

B2: tải [modGRUBShell.efi](https://github.com/datasone/grub-mod-setup_var/releases/download/1.4/modGRUBShell.efi)

B3: tải [Universal-IFR-Extractor](https://github.com/LongSoft/Universal-IFR-Extractor/releases) (or [Universal-IFR-Extractor](https://www.bios-mods.com/software-releases-and-updates/2014/01/universal-ifr-extractor/) windows only)

B4: Tải tool dump [file](https://github.com/mostav02/Remove_IntelME_FPT/tree/master/Intel_ME_System_Tools) thông tin bios và giải nén

> Ở đây các bạn sẽ tiến hành tra thông tin chipset hướng dẫn chi tiết [tại đây](https://heavietnam.ga/2022/04/29/cach-xac-dinh-phan-cung/). Sau đó tiến hành dò với danh sách chipset để tải đúng version

![](https://i.imgur.com/9oe7Oz1.png)

## Dump file thông tin bios

B1: các bạn tiến hành copy path dẫn đến thư mục `Flash Programming Tool --> Windows6` ở trong file đã tải ở phần chuẩn bị bước 4

![](https://i.imgur.com/Cbmd2TY.png)

Nhấn dup vào chỗ khoanh đỏ để copy

B2: chạy `cmd` với quyền admin

![](https://i.imgur.com/SLVu9Qk.png)

B3: cd đến đường dẫn folder vừa copy

![](https://i.imgur.com/TIHyZGv.png)

B4: Tiến hành chạy lệnh sau

```
fptw64.exe -BIOS -D biosimage.bin
```

![](https://i.imgur.com/CIGPxSW.png)

B4: kiểm tra thư mục đã copy đường dẫn ở bước 1 ta sẽ thấy file `biosimage.bin`

## Tiến hành tìm offset

B1: mở file vừa dump được bằng uefi tool

![](https://i.imgur.com/Feis1Nf.png)

B2: nhấn tổ hợp command + F và search tên của option bạn muốn chỉnh trong bios vd như ở đây mình muốn chỉnh `DVMT Pre-Allocated` lên 64mb trong bios (thay vì dùng config) thì mình search từ khoá là `DVMT` (tuỳ thuộc vào số lượng kết quả tìm được mà bạn tiến hành tăng hoặc giảm ký tự từ khoá)

![](https://i.imgur.com/Ib1uEwb.png)

B3: click dup vào kết quả tìm được ở ổ tìm kiếm

B4: chuột phải chọn `export as is` sau đó save nó vào một nơi bất kì

B5: các bạn sẽ tạo 1 file text rỗng có đuôi là txt (khuyến khích dùng sublimetext)

B6: tiến hành kèo thả các file theo thứ tự sau và terminal

```
path/to/ifrextract path/to/Setup.bin path/to/Setup.txt
```

<div>
<a href="https://www.flickr.com/photos/194144154@N04/52204845911/play/720p/913314a520/">
<img src="https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-37-10-68747470733a2f2f6938372e73657276696d672e636f6d2f752f6638372f31372f39392f34382f39382f36383734373431302e706e67.png">
</a>
</div>

> video hướng dẫn ở trên nha

B7: ta mở file text vừa dump ra và search tên option bios ta muốn mod

![](https://i.imgur.com/Mgw84WR.png)

B8: chú ý kỹ vào phần `VarStoreInfo (VarOffset/VarName)` phía sau nó chính là offset ta cần tìm ở đây mình có là `0x1F7`

B9: tiếp tục chú vào dòng one of phía sau offset vừa tìm được

![](https://i.imgur.com/eWoFegW.png)

Ta có đây là những tuỳ chọn trong option mà bạn muốn mod cho bios của mình. Phía sau nó chính là offset của tuỳ chọn đó ví dụ như ở đây mình muốn mod lên 64 mb thì sẽ chọn là `0x2`

Tiếp theo ta sẽ cần chú ý đến dòng `VarStore: 0x1` ở đây ta có vaule của varStore là 0x1 tiếp theo bạn sẽ tiến hành search `varstoreid: 0x1` thay thế 0x1 thành vaule của varStore

![](https://i.imgur.com/jyAIGVL.png)

Ta sẽ chú ý mục name và mục size

## Tiến hành mod bios bằng setup_var

B1: Bạn tiến hành boot vào modGRUBShell tool

B2: gõ lệnh

```bash
setup_var_cv nameOfVarStore offsetInVarStore [optional variable size] [optional value to write]

# replace nameOfVarStore thành name của varstore
# replace mục offsetInVarStore thành offset mà bạn đã xác định ở trên
# replace mục [optional variable size]  thành size của varstore
# replace mục [optional value to write] thành vaule mà bạn muốn set cho option của bios
```

Như ở đây ta có

```
setup_var_cv Setup 0x1F7 0x2AD 0x02
```

B4: gõ lệnh `reboot` và done

> Đây là bài viết do mình tổng hợp từ nhiều nay nhiều nguồn riêng lẽ để cấu thành 1 bài viết hoàn chỉnh có những mục là do kinh nghiệm của mình hoặc của các tiền bối đi trước tích góp vì vậy vui lòng tôn trọng bản quyền
