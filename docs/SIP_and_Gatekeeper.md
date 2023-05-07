# SIP và Gatekeeper

## Tìm hiểu chung

Gatekeeper là một tính năng bảo mật của hệ điều hành macOS của Apple. Nó thực thi việc ký mã và xác minh các ứng dụng đã tải xuống trước khi cho phép chúng chạy, do đó giảm khả năng vô tình thực thi phần mềm độc hại. Đối với cata+ nếu bạn không tắt nó bạn sẽ không thể cài app bên thứ 3

Từ 10.11 Apple đã tăng tính năng bảo mật với System Integrity Protection (SIP), vì thế một số phần mềm truy cập cao tới hệ thống báo lỗi không thể cài được. Đặc biệt trên macOS Catalina, tắt gatekeeper không là chưa đủ. Bạn cần disable SIP để có thể chạy được một số ứng dụng ngoài App Store và các tool hackintosh.

## Tắt Gatekeeper

B1: Mở [hackintool](https://github.com/headkaze/Hackintool)

B2: truy cập vào tab Utilities

![](https://i.imgur.com/c7Uqxul.png)

B3: Ấn vào tuỳ chọn `Disable Gatekeeper`

![](https://i.imgur.com/j8l2SQD.png)

B4: Nhập password và nhấn enter

## Tắt SIP

### Sử dụng Toogle SIP (Opencore only)

#### 0.8.0 trở xuống

B1: mở config bằng propertree

B2: Nhấn tổ hợp phím command + F và search `sip`

B3: Enable tuỳ chọn `AllowToggleSip`

![](https://i.imgur.com/j9PjBBy.png)

B4: reboot và ngay giao diện picker opencore các bạn chọn vào tuỳ chọn `toggle sip` để cho nó thành `Disabled`

B5: Boot vào macos và như vậy là đã done

#### 0.8.1 trở lên

B1: Các bạn bỏ driver `ToggleSipEntry.efi` ở `EFI --> OC --> Drivers`

> Driver này nằm trong gói [opencorepkg](https://github.com/acidanthera/OpenCorePkg/releases) (lấy đúng phiên bản opencore hiện tại của bạn)

B2: snapshot config

B3: reboot

B4: chọn vào option toggle sip ở picker opencore cho nó thành Disabled

B5: boot vào mac như bình thường và đã done

> Nhưng hãy nhớ rằng khi bạn rest nvram thì toggle sip sẽ không còn hiệu lực và bạn phải làm lại từ đầu

### Sửa csr-active-config

| SIP STATUS                                               | OPENCORE | CLOVER |
| -------------------------------------------------------- | -------- | ------ |
| Sip enable                                               | 00000000 | 0x00   |
| Cho phép load kext sle và Disable filesystem protections | 03000000 | 0x03   |
| Tắt hoàn toàn sip ở macOS High Sierra                    | FF030000 | 0x3FF  |
| Tắt hoàn toàn sip ở Catalina và Mojave                   | FF070000 | 0x7FF  |
| Tắt sip ở bigsur+                                        | FF0F0000 | 0xFFF  |
| Tắt authenticated root sip ở bigsur+                     | EF0F0000 | 0xFEF  |

#### Opencore

B1: các bạn config và search `csr-active-config`

B2: Tra theo bảng trên và sử dụng đúng giá trị

B3: tìm đến mục `NVRAM -> Delete -> 7C436110-AB2A-4BBB-A880-FE41995C9F82` và nhấn dấu `+` trên bàn phím

B4: Đặt giá trị mới cho child vừa tạo là `csr-active-config`

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/sip.904350e6.png)

#### Clover

B1: mở clover và tìm đến mục `Rt Variables --> csr-active-config`

B2: Tra theo bảng trên và điền giá trị thích hợp

B3: reboot

**Source tham khảo: [Post-Install Issues | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/post-issues.html#disabling-sip) | [Disable SIP Opencore 069 – OSx86 11.0 (Big Sur) | InsanelyMac](https://www.insanelymac.com/forum/topic/348187-disable-sip-opencore-069/)**
