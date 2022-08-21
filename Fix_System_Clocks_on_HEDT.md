# Fix System Clocks on HEDT

## Chuẩn bị:

- Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).
- [Maciasl](https://github.com/acidanthera/MaciASL/releases).
- [SSDT-RTC0-RANGE](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-RTC0-RANGE.dsl).

## Xác định vấn đề:

Các bạn cần xác định vấn đề của mình có thật sự cần dùng SSDT-RTC0 hay không.

B1: Mở SSDT ra và ấn tổ hợp phím `Command + F` và gõ từ khóa `PNP0B00` nhìn vào dòng `_CRS`.

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/rtc-range-check.d964e9c9.png)

Chúng ta có:

```
                Name (_CRS, ResourceTemplate ()  // _CRS: Current Resource Settings
                {
                    IO (Decode16,
                        0x0070,             // Range Minimum 1
                        0x0070,             // Range Maximum 1
                        0x01,               // Alignment 1
                        0x02,               // Length 1
                       )
                    IO (Decode16,
                        0x0074,             // Range Minimum 2
                        0x0074,             // Range Maximum 2
                        0x01,               // Alignment 2
                        0x04,               // Length 2
                       )
                    IRQNoFlags ()
                        {8}
                })
```

Các bạn để ý vào các dòng `IRQNoFlags` (số vùng bao phủ) và 2 dòng `IO (Decode16,` chúng ta có rtc bao phủ 8 vùng là 0x70 (0x007 chính là 0x70 do được viết bằng hex), 0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77 ta có:

- Dòng io đầu tiên:
  - Bắt đầu ở 0x70.
  - Bao gồm 2 vùng.
  - Chứa 0x70, 0x71.
- Dòng io thứ hai:
  - Bắt đầu ở 0x74.
  - Bao gồm 4 vùng.
  - Chứa 0x74, 0x75, 0x76, 0x77.

Thiếu 0x72 và 0x73.

Ta cần sửa phạm vi ở dòng io đầu tiên thành 4.

## Tìm ACPI Path:

B1: Mở DSDT lên và ấn `Command + F`

B2: Search các từ khóa sau:

- `PNP0B00` : Dùng để tìm tên của Device RTC.
- `Name (_ADR, 0x001F0000)`: Dùng để tìm LPC Path (LowPinCount Path).
- `PNP0A08`: Dùng để tìm PCI Path (có thể có nhiều kết quả cùng xuất hiện hãy chọn cái đầu tiên).

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/rtc-name.afa4c3f4.png)

RTC Pathing

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/lpc.bfa9cf23.png)

LPC Pathing

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/pci0.4477f361.png)

PCI Pathing

Bây giờ chúng ta có các Path của ACPI là `rtc,lpc,pci0`

## Edit SSDT:

B1: Mở SSDT-RTC0 vừa tải ở bước chuẩn bị lên và sửa lại các Path như sau:

```
//Path mặc định sẽ là

PC00.LPC0.RTC

//chúng ta cần sửa lại là

PCI0.LPC.RTC 
```

B2: Rename trong SSDT:

```
//Trước khi rename

External (_SB_.PC00.LPC0, DeviceObj) <- Rename 

External (_SB_.PC00.LPC0.RTC_, DeviceObj) <- Rename 

Scope (_SB.PC00.LPC0) <- Rename

//Sau khi rename

External (_SB_.PCI0.LPC, DeviceObj) <- Renamed

Scope (_SB.PCI0.LPC.RTC) <- Renamed

Scope (_SB.PCI0.LPC) <- Renamed
```

B3: Tiếp theo search `device(ACPI000E)` ( hoặc `ACPI000E` ) Nếu DSDT của bạn có dòng này thì

```
  /* <- xóa nó trong ssdt
  Scope (RTC)
        {
            Method (_STA, 0, NotSerialized)  // _STA: Status
            {
                If (_OSI ("Darwin"))
                {
                    Return (Zero)
                }
                Else
                {
                    Return (0x0F)
                }
            }
        }
  */ <- xóa nó trong ssdt

// Nếu không có thì xóa luôn phần ghi chú này trong ssdt
```

B4: Chúng ta có phần này:

```
            Name (_CRS, ResourceTemplate ()  // _CRS: Current Resource Settings
            {
                IO (Decode16,
                    0x0070,             // Range Minimum 1
                    0x0070,             // Range Maximum 1
                    0x01,               // Alignment 1
                    0x04,               // Length 1      (Expanded to include 0x72 and 0x73)
                    )
                IO (Decode16,
                    0x0074,             // Range Minimum 2
                    0x0074,             // Range Maximum 2
                    0x01,               // Alignment 2
                    0x04,               // Length 2
                    )
                IRQNoFlags ()
                    {8}
            })
// Sửa các giá trị theo phân tích ở bước xác định vấn đề ở đây mình đã sửa dòng io đầu tiên có phạm vi giá trị là 0x04
```

![](https://imgur.com/oBdHjTa.png)

Chúng ta có:

![](https://imgur.com/pHiUOhF.png)

![](https://imgur.com/H0oVCx0.png)

![](https://imgur.com/id7Wk74.png)

B5: Save lại dưới dạng file aml theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvii-patch-dsdt-phan-2/).

B6: Bỏ file SSDT vừa sửa vào EFI ==> OC ==> ACPI và Snapshot lại ( hoặc EFI ==> Clover ==> ACPI ) sau đó Restart.

Lưu ý: Source tham khảo: [Fixing System Clocks on HEDT: Manual | Getting Started With ACPI (dortania.github.io)](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac-methods/manual-hedt.html#edits-to-the-sample-ssdt) | [OpenCorePkg/SSDT-RTC0-RANGE.dsl at master · acidanthera/OpenCorePkg (github.com)](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-RTC0-RANGE.dsl)
