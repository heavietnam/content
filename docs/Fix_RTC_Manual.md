# Fix RTC Manual

Đối với lỗi thiếu RTC thì có 3 cách để sửa do macOS cần RTC mà ko cần AWAC do đó ta nên disable AWAC và Enable RTC.

## Static Patch

B1: Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

B2: Mở DSDT ra và search `ACPI000E` ta sẽ thấy như sau:

![](https://i.imgur.com/UpcoVpV.png)

B3: search `PNP0B00` ta sẽ thấy như sau

![](https://i.imgur.com/xsvaKc3.png)

B4: Bạn chú ý phần khoanh đỏ ta sẽ có thể hiểu như sau:

- Method(_STA thể hiện status của device đó ở đây là device RTC và AWAC
- Tiếp đó sẽ là câu lệnh if ta sẽ thấy ở AWAC là `If (LEqual (STAS, zero))` ở RTC là `If (LEqual (STAS, One))`
- Vvà trả về 2 giá trị là 0x0f và 0x00
  - 0x0f: enable
  - 0x00: disable
- Từ đó ta có thể thấy như sau khi STAS=zero tức là điều kiện của AWAC đúng sẽ trả về giá trị là `0x0f`. Ngược lại điều kiện ở RTC sai tức là trả về giá trị là `0x00`
- Ngược lại khi STAS=one tức là điều kiện của AWAC sai và trả về là `0x00`. Khi này điều kiện ở RTC đúng và trả về là `0x0f`
- Như vậy có thể hiểu rằng khi STAS=one thì enable RTC và disable AWAC và ngược lại
- Nhưng STAS thì được các os set

B5: Như vậy ta đã hiểu được nguyên lý hoạt động của method _STA. Vì vậy để fix nó ta có 3 cách đi ở phần này mình sẽ hướng dẫn cách đi đầu tiên là static patch.

- Ta nhận thấy rằng giá trị method _sta phụ thuộc vào if ((STAS vậy nếu như ta xóa if đi và chỉnh cho giá trị method _STA của rtc luôn hoạt động thì RTC được enable

![](https://i.imgur.com/WfXnZuR.png)

- Sao khi xóa ta được:

![](https://i.imgur.com/AoHhCha.png)

B6: Như vậy là xong vì macOS chỉ cần RTC không cần AWAC.

## Hotpatch

### Sửa giá trị STAS cho nó luôn bằng one.

B1: Ta cần xác định biến được dùng để so sánh trong method `_sta`

- Search `PNP0B00` nhìn vào method STA ta sẽ thấy biến được dùng để so sánh như ở trên là STAS có 1 số máy là STSl vân vân

B2: Tạo SSDT-AWAC với nói dung như sau:

```
DefinitionBlock ("", "SSDT", 2, "heavn", "AWAC", 0x00000000)
{
    External (STAS, IntObj)

    Scope (_SB)
    {
        Method (_INI, 0, NotSerialized)  // _INI: Initialize
        {
            If (_OSI ("Darwin"))
            {
                STAS = One
            }
        }
    }
}
```

B3: Thay `STAS` bằng biến mà method `_STA` dùng để so sánh.

B4: Save lại.

B5: Bỏ file vào EFI –> OC –> ACPI hoặc EFI –> clover –> ACPI –> patched (snaps nếu ở OpenCore).

### Sử dụng SSDT-RTC0

B1: Tải SSDT-RTC0 [tại đây](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-RTC0.dsl).

B2: Xác định đường dẫn:

- Search `PNP0B00` ta sẽ thấy được đường dẫn

![](https://i.imgur.com/zz4tg5t.png)

- Ở đây ta có đường dẫn là _SB.PCI0.LPCB.RTC

B3: Tiến hành chỉnh sửa vào SSDT-RTC0

```
//thay _SB_.PCI0.LPCB bằng đường dẫn mà bạn tìm thấy

DefinitionBlock ("", "SSDT", 2, "ACDT", "RTC0", 0x00000000)
{
    External (_SB_.PCI0.LPCB, DeviceObj)    // (from opcode)

    Scope (_SB.PCI0.LPCB)
    {
        Device (RTC0)
        {
            Name (_HID, EisaId ("PNP0B00"))  // _HID: Hardware ID
            Name (_CRS, ResourceTemplate ()  // _CRS: Current Resource Settings
            {
                IO (Decode16,
                    0x0070,             // Range Minimum
                    0x0070,             // Range Maximum
                    0x01,               // Alignment
                    0x08,               // Length
                    )
                IRQNoFlags ()
                    {8}
            })
            Method (_STA, 0, NotSerialized)  // _STA: Status
            {
                If (_OSI ("Darwin")) {
                    Return (0x0F)
                } Else {
                    Return (0);
                }
            }
        }
    }
}
```

B4: Bỏ nó vào EFI –> OC –> ACPI hoặc EFI –> Clover –> ACPI –> Patched (snaps nếu là OC)

**Source tham khảo: [(7) Văn Hùng Nguyễn | Facebook](https://www.facebook.com/groups/2631037280276507/user/100007125750144) | [Fixing System Clocks: Manual | Getting Started With ACPI (dortania.github.io)](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac-methods/manual.html#finding-the-acpi-path) | [Howtohackintosh.top](http://howtohackintosh.top/)**
