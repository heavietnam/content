# Map USB

## Tìm hiểu chung

### Type port

Các conector type của usb được thể hiện dưới dạng decimal nhưng khi các bạn viết ssdt-uiac (hay ssdt-usb,..) thì cần chuyển nó về dạng hex (cách chuyển decimal to hex mình đã hướng dẫn rất nhiều ở các post trước các bạn có thể xem cách chuyển ở trong bài [patch i2c](https://heavietnam.ga/2021/09/29/v-fix-trackpad/))

#### Usb type A

- USB type A 2.0 connector type: 0  
  ![](https://i.imgur.com/Fr9JZxi.png)
- usb type A 3.0 connector: 3  
  ![](https://i.imgur.com/OzyqrsH.png)

#### Usb type Mini

- Usb type mini-ab connector type: 1  
  ![](https://i.imgur.com/ufoNYJd.png)

#### Usb type B

- Usb 3 type B connector type: 4  
  ![](https://i.imgur.com/9eCxIRi.png)

#### Usb Type Micro

- USB 3 Type Micro-B có connector type: 5  
  ![](https://i.imgur.com/43haG29.png)
- USB 3 Type Micro-AB có connector type: 6  
  ![](https://i.imgur.com/0QGX1Cc.png)

#### USB Type Power-B

- USB 3 Type Power-B có connector type: 7  
  ![](https://i.imgur.com/aBC9J8x.gif)

#### USB Type C

![](https://i.imgur.com/b8fjAvM.png)

- USB Type C – USB 2 có connector type: 8
- USB Type C -switch có connector type: 9
- USB Type C – without switch có connector type: 10

#### Special:

- ExpressCard có connector type: 2
- Internal (cam, bluetooth,…) có connector type: 255

## Tiến hành:

### Usb toolbox version windows:

B1: các bạn tải usbtoolbox [tại đây](https://github.com/USBToolBox/tool/releases)

![](https://i.imgur.com/f9ieXrf.png)

B2: các bạn tải và cài đặt `USBToolBox.kext` [tại đây](https://github.com/USBToolBox/kext/releases)

![](https://i.imgur.com/rZxwgf1.png)

B3: các bạn cài python [tại đây](https://www.python.org/downloads/)

B4: Các bạn chạy tool lên và nhấn D

![](https://i.imgur.com/FelbSzo.png)

B5: Các bạn cắm lần lượt usb vào hết các cổng (cổng nào nhận sẽ hiện màu xanh và không nhận sẽ hiện màu trắng)

![](https://i.imgur.com/n4ClFIg.png)

B6: nhấn B

B7: Nhấn S

Ta sẽ dùng cú pháp T:port:type (đọc ở phần tìm hiểu chung để biết type port ở decimal là gì)

![](https://i.imgur.com/itTke7q.png)

- Ta thấy port 2 là 2.0 nhưng nhận dạng lại là 3.0 ta sẽ cần set nó lại thành 2.0 `T:1:0`
- Tiếp đó là port 3 ta thấy usb 2.0 nhận dạng là type A tức 2.0 --> đúng không cần sửa
- port 4 là 2.0 nhận dạng là 2.0 --> đúng
- port 5 là card mạng nhận dạng internal --> đúng
- Port 7 là bluetooth nhận dạng internal --> đúng
- Port 8 là web cam nhận dạng internal --> đúng
- Port 14 usb 3.0 nhận dạng là 3.0 --> đúng

B8: nhấn K

B9: các bạn sẽ copy kext `UTBmap.kext` và `Usbtoolbox.kext` vào EFI --> OC --> kext sau đó snaps (hoặc EFI --> clover --> kext --> other)

B10: reboot

### Usb toolbox version macos

B1: tải usbtoolbox [tại đây](https://github.com/USBToolBox/tool/releases)

![](https://i.imgur.com/q4zUsBP.png)

B2: tải và cài đặt `USBToolBox.kext` và `UTBDefault.kext` [tại đây](https://github.com/USBToolBox/kext)

![](https://i.imgur.com/Mfatjy6.png)

B3: chạy tool vừa tại về và nhấn D

![](https://i.imgur.com/uySyTsf.png)

B4: các bạn cắm lần lược usb vào các cổng (các cổng nhận sẽ hiện màu xanh)

![](https://i.imgur.com/zRyRaKs.png)

B5: nhấn B

![](https://i.imgur.com/9p5F7X6.png)

B6: nhấn S

sử dụng cú pháp T:port:type để chọn type cho port (xem ở phần tìm hiểu chung để biết type phù hợp)

![](https://i.imgur.com/qTwcdnA.png)

Do mình không kiếm được 1 máy chưa map usb nên các bạn xem cú pháp và áp dụng vào máy các bạn

- Port 1 là 1 hub device nhận dạng internal --> đúng
- Port 2 là cam và bluetooth nhận dạng là internal --> đúng
- Port 3 là usb 2.0 nhận dạng type A tức 2.0 --> đúng
- Port 4 là usb 3.0 nhận dạng là 3.0 --> đúng

B7: nhấn K

![](https://i.imgur.com/UvRqJxF.png)

![](https://i.imgur.com/xFt1P3u.png)

B8: Bỏ kext `UTBmap.kext` và xóa kext `UTBDefault.kext` sau đó snaps

B9: save và reboot

### Map by hackintool with uia_include

B1: tải kext usbinjectall [tại đây](https://github.com/daliansky/OS-X-USB-Inject-All)

B2: Các bạn tiến hành nhìn vào mục type của controller trong tab usb hackintool

![](https://i.imgur.com/2a13Ghj.png)

![](https://i.imgur.com/nemXgWt.png)

Nếu controller của bạn có type khác với

- EH01
- EH02
- XHCI
- XHC

Thì bạn sẽ cần rename chúng thành các tên tương ứng. Một số rename thường gặp là

- `EHC1 to EH01`
- `EHC2 to EH02`
- `XHC1 to XHCI`

Trường hợp trong DSDT tên XHCI đã được sử dụng thì các bạn có thể rename là `XHC1 to XHC_`

Xem chi tiết hướng dẫn add patch rename [tại đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/) mẫu patch rename xem [tại đây](https://www.mediafire.com/file/dkclyfcox4jay60/Heavietnam_usb_rename.plist/file)

Có trường hợp DSDT dã sử dụng cả `XHCI` và `XHC` khi này các bạn sẽ tiến hành rename `XHCI` thành `XHC2` và rename controller thành `XHCI`

B3: reboot

B4: Các bạn cắm usb vào port usb 2.0

![](https://i.imgur.com/7TdvZo4.png)

B5: add boot-arg `-uia_exclude_ss uia_include=XX` (thay XX bằng các port usb 2.0)

```
-uia_exclude_ss uia_include=PR11,PR21,HP21,HP23,HS01,HS02
```

B6: reboot

B7: cắm tiếp usb 2.0 vào các port 2.0 sau đó xóa hết những port không xanh (các usb khi đã nhận có màu xanh)

B8: add boot-arg `-uia_exclude_hs` và xóa boot-arg `-uia_exclude_ss`

B9: reboot

B10: cắm các port usb 3.0 và type C vào sau đó tiếp tục xóa các port không xanh

B11: điều chỉnh type các port

- Bluetooth và web cam set là internal
- usb 2.0 set là 2.0
- Usb 3.0 set là 3.0
- type C
  - Nếu mà nhận cùng là HSxx/SSxx(hs03/ss03) thì set là TypeC+Sw
  - Ngược lại nếu 2 port nhận khác thì set là TypeC

B12: ấn export

- Có thể sử dụng ssdt-uiac kèm usb injectall.kext
- Hoặc usbport.kext

B13: copy ssdt-uiac vào EFI --> OC --> ACPI (EFI --> Clover --> ACPI --> Patched) hoặc usbport.kext vào EFI --> OC --> kext (EFI --> Clover --> kext --> other)

B14: snaps nếu là OC và reboot

### Map by hackintool with uia_exclude

B1: Cắm 1 thiết bị USB 2.0 vào tất cả cổng trong hệ thống (mở Hackintool, chọn tab `USB`)

B2: Ở đây các cổng thực sẽ hiện màu xanh các bạn hãy ghi nhớ phần name của các cổng ko hiện xanh ( có thể gh‌i tên các cổng đây ra )

B3: các bạ‌n thêm đoạn code sau vào boot-arg 

`uia_exclude=“tên các cổng không hiện xanh”` ( khi viết code không có dấu “ ”) 

Đây là đoạn code của mình sau khi viết ra các bạn nhớ thêm vào boot-arg nhé

![](https://lh4.googleusercontent.com/gYzSUtlhSnlAD_u4tQC2GKCo0D2DGhFK9AzBzJXJOyFol9Z9bgyOdLflgc2jDY2Tk2v8gnN7DSR4kudYjRbD_LCAICVxfqozFxelTYS0QMVJgTGlqgzeg7YiUvizva_9Na2O2ggj=s0)

**B4**: Restart

**B5**: Các bạn cắm 1 thiết bị USB 3.0 và USB Type-C vào tất cả cá‌c cổng trên hệ thống 

**B**6: Các bạn sửa lại đoạn code trên theo đúng với hiện tại ( loại bỏ 1 số cổng vừa hiện xanh trong code ) tiếp các bạn xóa đoạn code ở phần 1 đi và dán đoạn code mới vào boot-arg 

**B**7: Restart

B8: mở và cắm các usb 2.0 vào các cổng 2.0 usb 3.0 vào các cổng 3.0 sau đó ấn dấu ![](https://i.imgur.com/K3uqfX7.png) để loại bỏ các port không xanh (port rỗng)

B9: Sau khi các bạn đã làm xong cả 2 phần trên thì hãy điều chỉnh lại loại cổng trong hackintool cho đúng nhé 

- Bluetooth và web cam set là internal
- usb 2.0 set là 2.0
- Usb 3.0 set là 3.0
- type C
  - Nếu mà nhận cùng là HSxx/SSxx(hs03/ss03) thì set là TypeC+Sw
  - Ngược lại nếu 2 port nhận khác thì set là TypeC

Đây là của mình sau khi điều chỉnh xong:

![](https://lh4.googleusercontent.com/-CcjUexMrlgpj8X1LhxCbmgVSCXtDBYSAM94AVXohQJvqMvWvZlo3qt9UyoRvXD835h-xQJV77L1P6EAsyaqKWpOhzzhNlpycuASsEBbKDDNM6gxA48hGbMnw6_FzviyrdpSKD4h=s0)

B10: ấn export

- Có thể sử dụng ssdt-uiac kèm usb injectall.kext
- Hoặc usbport.kext

B11: copy ssdt-uiac vào EFI --> OC --> ACPI (EFI --> Clover --> ACPI --> Patched) hoặc usbport.kext vào EFI --> OC --> kext (EFI --> Clover --> kext --> other)

B12: snaps nếu là OC và reboot

**Lưu ý: USB 3.0 type A và USB type C xuất ra 2 port là 2.0 và 3.0. Lưu ý cắm usb 2.0 và usb 3.0 lần lược vào USB 3.0 type A và USB type C**

**Lưu ý 2: Sau khia add file đã xuất từ hackitool các bạn có thể xóa code ở boot-arg**

**Lưu ý 3: đối với usbtoolbox ở mac thì sẽ không có khả năng tiên đoán port các bạn phải tự set bằng tay. Còn đối với windows có khả năng tiên đoán port các bạn có thể bỏ qua bước set port bằng tay và trực tiếp build kext**

**Lưu ý 4: Khi map usb bằng usbtoolbox 1 số controller có thể sẽ không nhận. Biện pháp khắc phục là bạn sẽ cần load kext [xhci-unsupported](https://github.com/RehabMan/OS-X-USB-Inject-All) chung với kext usb vừa map**

> Cách để biết usb controller của bạn có cần kext XHCI-unsupported.kext hay không

- B1: mở hackintool vào tab usb chú ý và vendor id và device id

![](https://i.imgur.com/WgIjlzQ.png)

- B2: nếu bạn có vendor id là `8086` và device id là 1 trong những id sau:
  - 8D31
  - A2AF
  - A36D
  - 9DED

Thì bạn sẽ cần thêm kext trên. Đối với những bạn chưa boot được vào macos thì có thể xác định vendor-id và device-id theo hướng dẫn [tại đây](https://heavietnam.ga/2022/04/29/cach-xac-dinh-phan-cung/)

**Lưu ý 5: các bạn hoàn toàn có thể dựa vào cấu trúc của ssdt-uiac để tự map usb bằng ssdt manual (mình thấy nó không cần thiết nên sẽ không hướng dẫn)**

**Source tham khảo: [USBToolBox/tool: the USBToolBox tool (github.com)](https://github.com/USBToolBox/tool) | [tool/TYPES.md at master · USBToolBox/tool (github.com)](https://github.com/USBToolBox/tool/blob/master/TYPES.md) | [Hackintosh USB Port Mapping Guide 2021 / 2022 – Mojave – Catalina – BigSur – Monterey – Guides and Tutorials – Olarila](https://www.olarila.com/topic/14220-hackintosh-usb-port-mapping-guide-2021-2022-mojave-catalina-bigsur-monterey/) | [headkaze/Hackintool: The Swiss army knife of vanilla Hackintoshing (github.com)](https://github.com/headkaze/Hackintool)**
