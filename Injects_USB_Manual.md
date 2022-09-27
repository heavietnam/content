# Injects USB Manual

> Ở bài hướng dẫn này mình sẽ chỉ các bạn cách tạo ra 1 Kext Map USB cho riêng mình nghe có vẻ thú vị đúng ko nào (mình đã có 1 bài hướng dẫn Map USB bằng cách dùng Tool rồi nhé các bạn có thể xem [tại đây](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/)) và giờ bắt đầu thôi.

## Chuẩn bị

- Tải IORegistryClone [tại đây](https://github.com/khronokernel/IORegistryClone/blob/master/ioreg-302.zip).
- 1 số tên Device USB Control.

```
XHC
XHC0
XHC1
XHC2
XHCI
XHCX
AS43
PTXH // thường gặp ở các main amd
PTCP // thường gặp ở AsRock X399 
PXSX // đây là 1 tên thường gặp hãy kiểm tra kỹ xem nó có phải là bộ điều khiển usb không
```

## Tạo Kext

Ở đây mình sẽ hướng dẫn cho AMD Intel làm tương tự vì cùng 1 nguyên tắc hoạt động (với Intel các bạn sử dụng trên Kext USB-Inject-All).

B1: các bạn Search `XHC` trên IOReg và nhìn 1 danh sách sổ xuống tìm gốc của các thành phần ta được tên bộ điều khiển USB.

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/controller-name.65fee9c7.png)

Ở đây ta có bộ điều khiển USB là PTXH (nếu các bạn có các bộ điều khiển trùng với nhau thì các bạn sẽ lấy hết tên của nó là PTXH@000000, phần này mình sẽ nói rõ hơn ở mục sau).

B2: Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

B3: Các bạn mở DSDT lên và Search tên bộ điều khiển USB vừa xác định ở IOReg.

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/dsdt-1.9fd16334.png)

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/dsdt-2.fcac4b17.png)

Ở đây ta có thể thấy số Port trong IOReg đang bị thiếu tức macOS đang nhận thiếu Port trong DSDT là do Kext AppleUSBHostPlatformProperties nó chỉ tạo USB Map theo SMBios của Apple vì vây nên ta sẽ bị thiếu các Port USB để khắc phục các bạn sẽ tạo 1 Kext cho riêng mình (hoặc Map USB theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/)). Đầu tiên các bạn sẽ Download Kext mẫu [tại đây](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/AMD-USB-Map.kext.zip) (đối với Intel thì các bạn sửa trên [USB-Inject-All](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/)).

B4: Các bạn Click chuột phải chọn Show Package Contents.

![](https://imgur.com/HutDs5k.png)

B5: Các bạn mở file Info.plist bằng ProperTree (cách sử dụng ProperTree các bạn có thể xem hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/tim-hieu-ve-hackintosh/)).

![](https://imgur.com/QFPihWX.png)

![](https://imgur.com/hrz6KiI.png)

Ở đây các bạn chú ý các mục sau:

1. Các bạn sẽ sửa lại theo SMBios và bộ điều khiển USB.
2. Mục IONameMatch là tên bộ điều khiển USB.
   - Đối với 1 số bộ điều khiển USB (thường là PXSX sẽ dùng IOPathMatch thay cho IONameMatch | xem chi tiết ở mục dưới).
3. Port-Count là giá trị của Port cao nhất (VD như khi nãy ta có Port P022 có giá trị 0x16 cao nhất thì khi đưa vào Port Count nó sẽ là 16 00 00 00 00).
4. Port đây là mục các bạn sẽ làm việc chủ yếu để Add thêm các Port bị thiếu.
5. Mode ở mục này các bạn sẽ chỉnh về SMBios hiện tại đang sử dụng.

B6: Các bạn sẽ tiến hành nhìn vào DSDT kết hợp IOReg để xác định Type dựa vào tên (thông thước 3 ký tự đầu của USB 3.0 sẽ trùng nhau và 2.0 cũng vậy)

B7: Tiến hành xác định giá trị Port các bạn sẽ dựa vào mục

```
Name (_ADR
```

để xác định giá trị Port. VD như ở đây mình có

```
Device (PO18)
   {
   Name (_ADR, 0x12) // _ADR: Address
   Name (_UPC, Package (0x04) // _UPC: USB Port Capabilities
      {
         Zero,
         0xFF,
         Zero,
         Zero
      })
   }
```

thì port P018 ta có giá trị Port là 0x12=1200000000 ta sẽ được (do ở trên IOReg ta có port P010 có usbconnector là 0x3 tức là 3.0 nên cũng trương tự với P018 | nhớ rằng USB 3.0 sẽ có 2 Port được xuất ra)

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/port-info.64852be6.png)

**Lưu ý : 1 số DSDT mục**

```
Name (_ADR
```

**sẽ bị ẩn đi do đó ta sẽ phải tự đọc DSDT. VD như ở đây mình có**

![](https://imgur.com/cey13IE.png)

Ta sẽ có PR3 là 0xFFFFFFF0.

## Trường hợp ngoại lệ thứ 1: DSDT bị thiếu Port

Đối với DSDT bị thiếu Port ta sẽ cần Remove tất cả các bộ điều khiển USB và để cho macOS tự khởi tạo dữ liệu bằng cách cho DSDT đã xóa device vào EFI -> OC -> ACPI (các bạn sẽ cần Convert định dạng DSDT sang .dsl [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/))

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/dsdt-missing.fdae3b86.png)

Ở đây ta thấy dsdt này thiếu các port usb như HS02, HS03, HS04, HS05,….

## Trường hợp ngoại lệ thứ 2: Tên của các Port bị nhầm hoặc chưa đặt tên

Đối với trường hợp này các bạn chỉ việc dựa vào DSDT và đổi tên hoặc đặt tên trong Kext của các bạn ở trên là được.

Before:

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/pre-map.29fe7763.png)

Kext:

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/genirc-plist.47f64f03.png)

Do ở đây có 2 bộ điều khiển USB nên mình dùng IOPathMatch.

After:

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/post-map.6b7c1e5e.png)

## Trường hợp ngoại lệ thứ 3: Có 2 Method XHC0

Trường hợp này thường xảy ra trên bộ điều khiển USB PXSX.

## Cách 1: dùng Kext với phương thức IOPathMatch (khuyến khích)

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/iopathmatch.ce488cb3.png)

Ở đây ta có thể thấy có 2 Device XHC0 ta có thể thấy mục XHC0@61000000 ở dưới mục XHC0 và là gốc của tất cả các mục trên nên ta sẽ lấy path của mục này là:

```
IOService:/AppleACPIPlatformExpert/S0D1@0/AppleACPIPCI/D1C0@7,1/IOPP/XHC0@0,3/XHC0@61000000
```

(dựa vào vault củaport trên usb và ioreg để xme mục nào là bộ điều khiển usb ta sài thực sự)

Tiếp theo ta chỉ cần Add Path trên vào mục IOPathMatch.

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/path-match-config.1139f1d8.png)

## Cách 2: Dùng SSDT

B1: các bạn tải SSDT [tại đây](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/SSDT-SHC0.dsl).

B2: Các bạn nhìn vào mục ACPI-Path trong bộ điều khiển USB đã xác định ở trên.

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/acpi-path.b5562f4f.png)

Ta có thể thấy `IOACPIPlane:/_SB/PC00@0/RP05@1c0004/PXSX@0` có nghĩa là `SB.PC00.RP05.PXSX`

B3: Ta sẽ chuyển các mục sau tương ứng sau bằng số liệu bạn lấy được trong IOReg tương tự như sau

```
External (_SB_.PCI0.GP13, DeviceObj) -> External (_SB_.PC00.RP05, DeviceObj)
External (_SB_.PCI0.GP13.XHC0, DeviceObj) -> External (_SB_.PC00.RP05.PXSX, DeviceObj)
Scope (\_SB.PCI0.GP13) -> Scope (\_SB.PC00.RP05)
Scope (XHC0) -> Scope (PXSX)
```

**Lưu ý: các này chỉ là Injects USB chủ yếu dành cho AMD và Gen 10 Intel sau khi làm theo các bạn phải Map lại USB bằng Hackintool** theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/).

**Source tham khảo: [USB Mapping | OpenCore Post-Install (dortania.github.io)](https://dortania.github.io/OpenCore-Post-Install/usb/amd-mapping/amd.html#amd-and-3rd-party-usb-mapping)**
