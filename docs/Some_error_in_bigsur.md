# Một số lỗi ở xuất hiện ở macOS Big Sur

## Stuck at `Forcing CS_RUNTIME for entitlement`

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/cs-stuck.bddc4a2d.jpg)

Nhìn có vẻ giống như 1 lỗi nhưng nó không phải lỗi bạn cần khá nhiều thời gian để vượt qua nó. Không khởi động lại vì nó có thể phá hủy quá trình cài đặt của bạn.

## Stuck at `PCI Configuration Begins` ở các main X99 và X299

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/rtc-error.d53fdc66.jpg)

Ở Big Sur các dòng main hedt bị thiếu các vùng rtc do đó macos gặp lỗi fix chi tiết theo trang [sau](https://heavietnam.ga/2022/2022/01/22/fix-system-clocks-on-hedt/index.html).

## Stuck on `ramrod`(^^^^^^^^^^^^^)

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/ramrod.55591fc5.jpg)

Khi bạn bị stuck ở dòng `ramrod`thì macOS sẽ bị reset sau đó lại gặp dòng này và tiếp tục reset. Nó đã tạo 1 vòng lập restart. Điều đó cho thấy bộ phận giả lập SMC của bạn đã bị hỏng vì vậy bạn có 2 sự lựa chọn:

- Sử dụng bản cập nhật mới nhất của VirtualSMC và Lilu sau đó add boot-arg  `vsmcgen=1`
- Hoặc bạn có thể chuyển qua kext [Fake-SMC](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/) của rehabMan.
- Lưu ý: không đồng thời sử dụng cả 2 kext VirtualSMC và Fake-SMC.

## Kernel panic on `OPCIFamily` ở X79 and X99

Lỗi này là do uncore PCI Bridges không được enable trong ACPI. Do đó IOPCIFamily sẽ bị kernel panic để giải quyết vấn đề này các bạn có thể dùng [SSDT-UNC](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-UNC.dsl) chỉ cần biên dịch bằng Maciasl và sử dụng theo guide chi tiết [tại đây](https://heavietnam.ga/2022/2021/09/29/xxviii-patch-dsdt-phan-3/index.html).

## DeviceProperties injection failing

Khi gặp lỗi này các bạn sẽ không thể tiêm được device-properties được cách fix các có thể dùng SSDT-BRG0. Fix chi tiết theo trang [sau](https://heavietnam.ga/2022/2021/09/29/xxiii-patch-card-doi-hoa-amd/index.html).

## Keyboard and Mouse broken

Lỗi này khiến cho các HID-based devices như chuột và bạn phím bị hỏng trong khi các cổng USB vẫn hoạt động, để khắc phục tình trạng này các bạn add patch như sau:

```
// add vào config.plist -> Kernel -> Patch

base | string | _isSingleUser

Count | Integer | 1

Enabled | Boolean  |  True

Find  | Data | <blank>

Identifier | String | com.apple.iokit.IOHIDFamily

Limit | Integer |  0

Mask | Data | <blank>

MaxKernel | String | <blank>

MinKernel | String | 20.0.0

Replace | Data | B801000000C3

ReplaceMask | Data | <blank>

Skip | Integer | 0
```

## Early Kernel Panic on `max_cpus_from_firmware not yet initialized`

Để fix lỗi này các bạn cần đảm bảo đang ở OpenCore version 0.6.0+ và `AvoidRuntimeDefrag` được enable.

Trên 1 số máy chủ yếu trên HP DC7900. Kernel không thể xác định chính xác có bao nhiêu luồng ở phần cứng của các bạn gây panic cách fix như sau.

```
// add vào config.plist -> Kernel -> Patch

base | string | _acpi_count_enabled_logical_processors

Count | Integer | 1

Enabled | Boolean  |  True

Find  | Data | <blank>

Identifier | String | Kernel

Limit | Integer |  0

Mask | Data | <blank>

MaxKernel | String | <blank>

MinKernel | String | 20.0.0

Replace | Data | B804000000C3

ReplaceMask | Data | <blank>

Skip | Integer | 0

// thay 04 bằng số luồng ở phần cứng của các bạn
```

## Can not update to newer versions of Big Sur

Có 2 trường hợp Broken Update Utility và Broken Seal.

### Broken Update Utility

lỗi này thường xảy ra khi bạn dùng bản beta. Để khắc phục chúng ta chỉ cần hủy đăng kí và đăng kí lại

```
# hủy đăng kí
sudo /System/Library/PrivateFrameworks/Seeding.framework/Resources/seedutil unenroll
# đăng kí lại
sudo /System/Library/PrivateFrameworks/Seeding.framework/Resources/seedutil enroll
```

Tiếp theo bản vào cài đặt và kiểm tra lại nếu vẫn chưa được hãy kiểm tra tới phần tiếp theo.

### Broken Seal

bạn gõ câu lệnh sau vào terminal

```
diskutil apfs list
```

Nhìn vào phần `Snapshot Sealed` Nếu nó trả về là `broken` thì bạn hãy fix theo các cách sau:

- Update OpenCore version 0.6.4+
- Khôi phục đến bản Snapshot cũ hơn tham khảo tại link [sau](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/post-issues.html#rolling-back-apfs-snapshot).

## Kernel Panic on `Rooting from the live fs`

Lỗi đầy đủ:

```
Rooting from the live fs of a sealed volume is not allowed on a RELEASE build
```

Do SecureBoot được khởi động để khắc phục chỉ cần update lên version 0.6.4+

## Asus Z97 and HEDT (cụ thể là X99 và X299) thất bại trong giai đoạn 2 của quá trình cài đặt.

Có 2 cách fix:

- Cài Big Sur vào 1 máy khác sau đó chuyển ổ đĩa vào máy cần cài
- Fix the motherboard’s NVRAM.
  - Chủ yếu cho Asus Z97 series.
- Chi tiết tham khảo tại trang [sau](https://www.reddit.com/r/hackintosh/comments/jw7qf1/haswell_asus_z97_big_sur_update_and_installation/).

## Laptops kernel panicking on `cannot perform kext scan`

Lỗi này thường xảy ra do có nhiều bản sao của cùng 1 kext trong bộ nhớ cache của kernel. Thường là kext voodooinput. Để khắc phục tình trạng này các bạn hãy kiểm tra trong config.plist -->  kernel --> add và chỉ enable 1 kext voodooinput duy nhất.

## Reboot on “AppleUSBHostPort::createDevice: failed to create device” on macOS 11.3+

Để fix lỗi này các bạn cần phải tắt `XhciPortLimit` ở mục `Kernel -> Quirks`  và map usb theo hướng dẫn [tại đây](https://heavietnam.ga/2022/2021/09/29/vi-1map-usb-intel-and-amd/index.html).

## An error occurred preparing the software update.

![](https://i.imgur.com/ESyQ5QL.jpg)

Có 3 cách fix cho trường hợp này:

- Bật firmwarevolume trong config.plist lên
- Có thể là do ổ cứng của bạn:
  - Thử boot bằng hdd
  - Hoặc thay ổ mới
  - Cần tránh mua các ổ sau
    - Kingston, Kingspec, Kingmax, Colorful, Fgloway,….
- Hoặc bạn có thể tạo máy ảo rồi dùng Time Machine tạo thành file Backup sao đó tiến hành bung file ra trong Recovery của macOS trên USB.

## Failed to install required firmware update

![](https://i.imgur.com/y4vGxLo.jpg)

Để khắc phụ lỗi này các bạn sẽ cần enable `AdviseFeatures` trong `EFI --> OC --> config.plist`

Source tham khảo: [OpenCore and macOS 11: Big Sur | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/extras/big-sur/#stuck-at-forcing-cs-runtime-for-entitlement)
