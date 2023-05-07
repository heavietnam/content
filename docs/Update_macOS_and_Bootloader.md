# Update macOS và Bootloader

## ***Update OpenCore***

### Cách 1: Update manual

#### Chuẩn bị

B1: Tải OpenCorePkg: [Releases · acidanthera/OpenCorePkg · GitHub](https://github.com/acidanthera/OpenCorePkg/releases)

B2: Tải tool cần thiết:

- Hackintool: [Releases · headkaze/Hackintool · GitHub](https://github.com/headkaze/Hackintool/releases) (tool này dùng để check các file EFI và Update các kext)
- OpenCore Configration [OpenCore Configurator | The Original | mackie100 projects](https://mackie100projects.altervista.org/opencore-configurator/) (tool này dùng để check config)
- OCConfigCompare [GitHub – corpnewt/OCConfigCompare: Python script to compare two plists and list missing keys in either.](https://github.com/corpnewt/OCConfigCompare)  (tool này dùng để update config)
  
  #### Tiến hành :

B1: Mở OpenCore Pkg và thay thế các mục 

- BootX64.efi 
- OpenCore.efi
- Driver (lưu ý driver OpenCanopy các bạn phải Update Resource và lấy đúng phiên bản OpenCanopy).

 B2: Update Kext:

- *Mở Hackintool vào mục Extensions.* 
- *Bấm vào dấu*
- *Tiếp các bạn bấm vào dấu* 
- *Sau đó các bạn Copy các Kext đã tải về vào mục Kext trong EFI của các bạn.* 
- *Snapshot lại config (lưu ý nếu các bạn có các Kext không nằm trong cơ sở dữ liệu của Hackintool thì bắt buộc các bạn phải tải thủ công)*

B3: Các bạn mở tool OCConfigCompare lên. 

- Tiếp các bạn chạy file .command lên (đối với bạn nào có cài python3 thì có thể chạy file .py cũng được).
- Tiếp các bạn bấm 3 kéo file sample.plist vào sau đó Enter.
- Rồi bấm 4 kéo file config vào rồi Enter (sau khi làm xong các bạn sẽ được như hình).

![](https://lh3.googleusercontent.com/1C611v8Q-69XGJePURGLgmadCOLb2bdICtTnVI4QsIo2Z7b14Rt3ByUViP2mynpPWatn0MJMYSgHTyAfeXJSHLmK6BWk3M6L9uuZcPsNWtiKmsEyxNU7SBhGzXoAsVm8Vd_vHdzf=s0)

- Rồi ấn 7 sau khi ấn sẽ được như hình.

![](https://lh6.googleusercontent.com/FqSs6M31YMT1MGwC7RLPphrch8kLnYBmJzYEloHTuoVC9KYGxq1lnbovIn-uf7F_2KfNO5wr5sKs5EeK_Oesk9NcpAqq_ZG2axd_oqKZ15k8lVXafT90-H5zP6M6IKm1syP1x3SB=s0)

B4: Mở file config.plist và file sample.plist lên bằng ProperTree (link ở trên).

- Các bạn nhìn vào Terminal dòng “Checking for values missing from User plist” đây là phần mà config bị thiếu các bạn dùng ProperTree để thêm các dòng này vào (lưu ý phần định dạng và phần vualt các bạn dựa vào file sample.plist để chỉnh sửa file config.plist).
- Tiếp các bạn nhìn vào phần “Checking for values missing from Sample” đây là phần mà config bị dư (nói đúng hơn là file sample.plist bị thiếu) các bạn dùng ProperTree xóa các dòng này đi **(lưu ý những patch mà các bạn add nó cũng sẽ báo dư nên hãy đọc kỹ trước khi xóa)**.

B5: Check lại file config.

- Các bạn mở OpenCore Configration ra sau đó nhấn option+c hoặc vào mục tool chọn config checker (sau khi làm các bạn sẽ được như hình).

![](https://lh6.googleusercontent.com/OEHDl7WKCHB-Ciffm9_Oqel7qvckO6Cf4Ft2jRPawgBElkHM1qPpKEn5u1SfaGNKvTxU_j-xWbW4UrrBCY7j4OMAxrIe9eFRJuFmYe68EjYLP1Qq4jilo2aHhGiGEifq1DnixArG=s0)

![](https://lh5.googleusercontent.com/79GfbZOstiqC_z3UjZSgaT0yERJyDz09MVEDmvHi4lNOn7rzwz4ACUJk4jtWOJnCktMJj_SzNCCevzbPmerF5YiBMvWPj9DfdH_9PdYr2k69R9Lwr0dewuxydFOCH0Ze74AFuwGA=s0)

- Chọn đúng phiên bản OpenCore rồi chọn lại Gen Chip tiếp các bạn tick vào mục drag and drop kéo file config vừa sửa vào rồi các bạn nhấn check để kiểm tra nhé.

B6: Reboot và tận hưởng thành quả (các bạn có thể mở Hackintool vào mục boot để xem nó đã hiện đúng chưa | cái này kiểm tra chơi cho vui chứ không chính xác vì các bạn chỉ cần up mỗi OpenCore.efi thì nó đã báo là up rồi).

**Lưu ý: Cách này chỉ dành cho các bạn lấy EFI trên mạng ko tự build EFI thôi còn nếu EFI là do các bạn tự build thì các bạn cứ tải bản mới nhất về rồi build lại thôi nha**.

**Lưu ý 2: Ở cách này mình dùng OpenCore Configuration chỉ để check Config mình không khuyến khích dùng phần mềm này.**

### Cách 2: Update bằng OCAuxiliaryTools

B1: Các bạn tải app `OCAuxiliaryTools` [tại đây](https://github.com/ic005k/OCAuxiliaryTools/releases)

B2: Bỏ app vừa tải vào mục `Applications`

B3: Tiến hành load config bằng app vừa tải

![](https://i.imgur.com/oqIliUy.png)

B4: ấn vào nút `Upgrade OpenCore and Kexts`

![](https://i.imgur.com/OVj3Msl.png)

B5: Các bạn sẽ tiến hành chọn `select all kext ==> Check for Kexts update ==> Update kexts`

![](https://i.imgur.com/zUcYjtV.png)

B6: các bạn tiến hành chọn `Get OpenCore ==> Start Sync`

![](https://i.imgur.com/MbeW8XN.png)

B7: Ấn nút `Save`

![](https://i.imgur.com/4jR2TGN.png)

B8: Reboot

## Update Clover

B1: Tải app [DiffMerge](https://sourcegear.com/diffmerge/).

B2: Tải file [Clover.zip](https://github.com/CloverHackyColor/CloverBootloader/releases).

B3: Tải tool [OCConfigCompare](https://github.com/corpnewt/OCConfigCompare)

B4: Mở tool OCConfigCompare ra và chọn 3.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-00.02.07.png?w=740)

B5: Kéo file Clover ==> EFI ==> config-sample.plist vào và nhấn Enter.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-00.01.10.png?w=740)

B6: Nhấn 4 và kéo file config.plist vào và nhấn Enter.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-00.04.03.png?w=740)

B7: Nhấn phím 7 sau đó Enter.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-00.04.56.png?w=848)

B8: Tải [ProperTree](https://github.com/corpnewt/ProperTree) hoặc bất cứ trình đọc file .plist nào mà bạn muốn.

B9: Chỉnh file config.plist bằng cách thêm những mục bị thiếu vào theo tool OCConfigCompare và xóa những mục dư (lưu ý là 1 số mục patch thêm nó vẫn sẽ báo là dư hãy cẩn thận khi xóa).

B10: Dùng app [DiffMerge](https://sourcegear.com/diffmerge/) để check làm như hình.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-00.10.23.png?w=1024)

B11: Thay thế cá tệp sau ở mục EFI

- CloverX64.efi
- Driver
- BootX64.efi
- Tool (nếu dùng)

B12: Update kext các bạn tải app Hackintool [tại đây](https://github.com/headkaze/Hackintool).

B13: Vào mục Extensions như hình:

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-00.36.24.png?w=874)

B14: Copy mục kext trong hackintool_kext sang folder EFI ==> Clover ==> Kext ==> Other và Restart

## ***Update macOS qua trình Update OTA***

Đầu tiên ta cần biết trình Update OTA là gì nó có nghĩa là trình update được do Apple cung cấp. 

B1: Bạn cần Update phiên bản OpenCore của mình (xem lại phần 1).

B2: Bạn phải chắc rằng SMBIOS của mình nằm trong danh sách có thể cập nhật được. 

B3: Bạn phải bật SIP lên:

- Vào Recovery từ Menu Boot của OpenCore. 
- Chọn Utility ⇒ Terminal. 
- Gõ lệnh sudo csrutil enable. 
- Gõ lệnh reboot.

B4: Restart NVRAM.

B5: Vào Setting ⇒ Software Update ⇒ Check for update. 

B6: Bấm Download và như thế là done.

**Lưu ý: Khi Update bạn phải cắm sạc ko để máy tắt nguồn ko tự dừng update.** 

**Lưu ý 2: Ngoài cách vào Recovery các bạn có thể bật Allow Toogle SIP để có thể bật tắt SIP ngay Menu Boot**.

### Cách ngoài lề update bằng OCAT tool

nếu muốn update lên version mới nhất của opencore bằng tool OCAT config các bạn sẽ tiến hành làm như sau

- Chọn vào menu version của tool sau đó chọn latest version

![](https://i.imgur.com/qxHFyFW.png)

- Ấn vào nút get opencore

![](https://i.imgur.com/gGKQCpU.png)
