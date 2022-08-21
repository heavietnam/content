# Fix RTC write issues

## Tìm hiểu

Một số dòng máy khi boot khởi động sẽ gặp tính trạng lỗi như hình.

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/cmos-error.d7acd2cd.png)



Do AppleRTC ghi vào một số khu vực nhất định không được phần cứng hỗ trợ đúng cách dẫn đến lỗi.

Vậy nên để Fix lỗi ta sẽ cần đưa các Bad RTC Region để Disable các Region bị lỗi khi Boot.

Các Region nằm rải rác từ 0x00-0x255

Vậy tại sao ta phải tìm vùng lỗi mà không Disable hết tất cả các Region. Lý do rất đơn giản bởi vì khi cùng lúc Disable hàng loạt Region, sẽ có 1 số lỗi do đó ra cần thu hẹp phạm vi Region bị Disable bằng cách tìm vùng lỗi.

## Tiến hành

## General

B1: Các bạn tải SSDT-Time [tại đây](https://github.com/corpnewt/SSDTTime).

B2: Các bạn Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

B3: Các bạn mở SSDT-Time lên và nhấn “D”.

![](https://imgur.com/dIWeYYU.png)

B4: Kéo DSDT vào và nhấn Enter.

![](https://imgur.com/kpMEmK7.png)

B5: Các bạn nhấn phím “6”.

![](https://imgur.com/y2MfcCZ.png)

B6: Các bạn bỏ SSDT vừa dump vào EFI -> OC -> ACPI và Snapshot lại.

## OpenCore

### Cách 1: Dùng “DisableRtcChecksum”



B1: Các bạn Download [RTCMemoryFixup.kext](https://github.com/acidanthera/RTCMemoryFixup/releases) và bỏ vào mục EFI ==> OC ==> Sau đó Snapshot lại.

B2: Các bạn vào config mục Quirks và bật DisableRtcChecksum lên.



**Lưu ý: Cách này chỉ hoạt động với 1 số máy vì nó chỉ Disable các Region từ 0x58-0x59 nên các máy không hoạt động thì các bạn sẽ tiến hành làm thủ công**.

## Cách 2: Thủ Công

B1: Các bạn tiến hành thêm boot-arg sau vào “rtcfx_exclude=00-FF” để check xem có phải lỗi do CMOS hay không.

Nếu boot lên bình thường các bạn bạn tiến hành sang bước 2 (giải thích nguyên lý 1 chúc như đã nói ở trên các Region rãi rác từ 00-255 để sử dụng ta chuyển từ thập phân sang thập lúc phân 00-FF)

B2: Các bạn tiến hành check tiếp 0x00-0x7F và 0x80-0xFF bằng cách lần lượt add 2 boot-arg sau vào “rtcfx_exclude=00-7F” và “rtcfx_exclude=80-FF” nếu cái nào boot vào được bình thường thì bạn chọn cái đó và tiến hành tiếp bước 3.

B3: Các bạn tiến hành Check tiếp như sau:

- B1: Chuyển vùng Region xác định được ở bước 2 sang số thập phân sau đó các bạn tiến hành tính trung bình cộng (ví dụ ở bước 2 mình nhận được vùng rtcfx_exclude=00-7F) thì ta sẽ làm như sau (0+127)/2=63,5 (ví dụ nó là số chẵn như 64).
- B2: Các bạn chuyển nó sang số thập lúc phân như sau:

Đầu tiên mở Hackintool lên vào mục Calc sau đó nhập số thập phân lúc nãy vào ô Hex hoặc nhập số thập phân vào ô Decimal.

- B3: Nếu ra số chẵn các bạn sẽ tiến hành lấy như sau 0x00-0x40/0x40-0x7F (nếu là số thập phân các bạn sẽ tiến hành làm trong như sau vd làm tròn thành 63 và 64 ta có 0x00-0x3F/0x40-0x7F)

B4: Các bạn cứ tiếp tục như thế cho đến khi tìm được kết quả cuối cùng nó có thể là 1 vùng hoặc có thể là 1 số VD như kết quả cuối cùng của mình sẽ là rtcfx_exclude=55-56 các bạn sẽ chuyển nó về dạng thập phân là 85-86 sau đó sẽ add vào mục NVRAM -> Add -> 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102 -> rtc-blacklist | data | 8586

Và đảm bảo bạn có mục sau Nvram -> Delete -> 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102 -> rtc-blacklist

Sau khi Add xong sẽ được như hình.



![](https://dortania.github.io/OpenCore-Post-Install/assets/img/rtc-blacklist.31a3b28e.png)

**Hãy Lưu ý rằng ở bước này bạn đã xóa hàm rtc_exclude trong boot-arg.**

## Clover

## Cách 1:

B1: Các bạn Tick vào ô RTC Fixup ở mục ACPI

![](https://imgur.com/84Khr2P.png)

## Cách 2:

B1: Các bạn làm như trên cách 2 ở OpenCore nhưng thay vì vào blacklist thì các bạn add thẳng vào boot-arg.
