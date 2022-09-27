# Fix Internal/External Card Reader

## Chuẩn bị:

B1: Mở Terminal và dán đoạn code sau vào:

```
// dán từng dòng 1 vào terminal

defaults write com.apple.Finder AppleShowAllFiles true

killall Finder
```

B2: Tải [ProperTree](https://github.com/corpnewt/ProperTree) (hoặc các trình edit file plist khác).

B3: Tải [Hackintool](https://github.com/headkaze/Hackintool) hoặc [Kext-Droplet](https://github.com/chris1111/Kext-Droplet-Big-Sur) cho Big Sur.

## Tiến hành

B1: Mở Finder và nhấn tổ hợp phím `Shift + Command + G` và gõ đường dẫn `/System/Library/Extensions` (S/L/E).

![](https://i.imgur.com/b3ziwa6.png)

B2: Ấn Command + F và search `AppleStorageDriver`.

![](https://i.imgur.com/bBnKxpo.png)

B3: Copy Kext đó ra ngoài Desktop (hoặc bất cứ chỗ nào bạn thích).

![](https://i.imgur.com/a47Jh2i.png)

B4: Bạn ấn chuột phải vào kext sau đó chọn `Show Package Contents` và truy cập vào `Contents > Plugin` sau đó tìm kext `AppleUSBCardReader.kext` trong đó.

![](https://imgur.com/AU6fSld.png)

B5: Bạn tiếp tục chọn `Show package Contents` > `Contents` và edit file `info.plist` bằng ProperTree hoặc trình edit file plist của bạn.

B6: Bạn ấn Command + F và Search `Physical Interconnect` sau đó edit giá trị của nó thành `External`.

![](https://i.imgur.com/Ve4S4HW.png)

B7: Bạn có thể edit Vendor Identification thành tên của bạn hoặc bất cứ cái gì bạn thích (nếu tôn trọng tác giả thì thay đổi thành `Generic Reader by NoobsPlanet`).

B8: Vào System Report.

![](https://imgur.com/fNWCjVv.png)

B9: Vào tab USB sau đó Copy `Product ID and Vendor ID`.

![](https://i.imgur.com/frzLwOV.png)

B10: Bạn ấn Command + F và search `Apple_Internal_SD_Card_Reader_1_00`.

B11: Sửa `idProduct` và `idVenedor` bằng các giá trị vừa lấy ở Bước 9 (lưu ý chuyển nó về decimal trước khi nhập).

- B1: Mở Hackintool tap Caculator.
- B2: Nhập dữ liệu lấy được ở bước 9 vào ô Hex.

![](https://i.imgur.com/unbgWtf.png)

B12: Chỉnh 2 mục `dProduct` và `idVenedor` ở `Apple_Internal_SD_Card_Reader_2_00` với cùng giá trị dữ liệu như ở B11.

B13: Chỉnh mục `Physical Interconnect` ở `Apple_internal_SD_Card_Reader_1_00` và `Apple_Internal_SD_Card_Reader_2_00` thành `External`.

![](https://i.imgur.com/1Yl1PAP.png)

B14: Save Kext lại (Ấn tổ hợp phím Command + S).

## Inject Kext

### Catalina:

B1: Bạn mở Hackintool lên.

B2: Bạn chọn như ảnh.

![](https://i.imgur.com/qWXwm70.png)

![](https://i.imgur.com/KufJX3R.png)

B3: Reboot

### Big Sur:

B1: Các bạn chỉnh `CFBundleShortVersionString` và `CFBundleVersion` trong cả 2 file `info.plist` và `version.plist` cao hơn file gốc (cao hơn trong hình VD 511.141.2)

![](https://i.imgur.com/uIqevTJ.png)

B2: Các bạn sử dụng Kext-Droplet để inject vào L/E (nhớ tắt `Gatekeeper` và `SIP`).

B3: Reboot.

**Lưu ý: Source tham khảo https://noobsplanet.com/threads/32/ | [SD CARD Reader NOT WORKING? FIX! - YouTube](https://www.youtube.com/watch?v=rIvoIfG6yzc)**  
