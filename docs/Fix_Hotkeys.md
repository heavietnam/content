# Fix Hotkeys

## Chuẩn bị

B1: Tải Kext [OS-X-ACPI-Debug](https://github.com/RehabMan/OS-X-ACPI-Debug).

B2: Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

B3: tải [MaciASL](https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/).

B4: Add [brightness-key.kext](https://github.com/acidanthera/BrightnessKeys) (nếu Asus thì thêm [AsusSMC](https://github.com/hieplpvip/AsusSMC) thay cho brightness-key.kext).

B5: Tải [Hackintool](https://github.com/headkaze/Hackintool).

B6: Tải [ProperTree](https://github.com/corpnewt/ProperTree).

## Remap KeyBoard

### Cách 1: Mapping Keyboard EC Query:

#### Static Patch

B1: Mở DSDT lên bằng MaciASL và ấn tổ hợp phím `Command + ,` và chuyển đến Tab Source kiểm tra xem source `OS-X-ACPI-Debug` có được add chưa nếu chưa add thì add theo URL sau `http://raw.github.com/RehabMan/OS-X-ACPI-Debug/master`

![](https://i.imgur.com/NZkzVNX.png)

B2: Ấn vào tab Patch trên giao diện chính của MaciASL.

![](https://i.imgur.com/6gUZFbS.png)

B3: Các bạn Apply các patch  “Add DSDT Debug Methods” và “Instrument EC Queries” theo ảnh sau:

![](https://i.imgur.com/NEqQLwf.png)

B4: Save lại và cho DSDT vào `EFI ==> OC ==> ACPI` (nhớ Snapshot config) hoặc `EFI ==> CLOVER ==> ACPI ==> Patched`.

B5: Restart.

B6: Mở nhấn tổ hợp phím `Command + Space` và gõ `console`.

![](https://i.imgur.com/Kxs5KgR.png)

B7 Setting console.app theo ảnh.

![](https://i.imgur.com/sgi0gi3.png)

B8: Bạn nhấn Hotkey Brightness (nhấn liên tục) và quan sát log được dump ra.

![](https://i.imgur.com/04HxSET.png)

Ở Catalina-

![](https://i.imgur.com/jWOu4jF.png)

Ở Big Sur+

**Lưu ý: Hình ảnh chỉ mang tính minh họa**.

B7: Sau khi làm xong B6 ta có được F5 (brightness down) là Q0E và F6(brightness up) là Q0F.

B8: Các bạn mở DSDT bằng MaciASL và search `KBC0, PS2M, PS2K và KBD0`.

![](https://i.imgur.com/0rrg3hC.png)

![](https://i.imgur.com/5Mcfh2L.png)

**Lưu ý: Ở đây ta có thể thấy khi search thì nhận được 2 device là PS2M và PS2K đều call qua LPCB thì các bạn sẽ dùng Windows mở `Device manager --> Keyboard --> BIOS device name` sẽ thấy được patch cần tìm**.

![](https://i.imgur.com/AFmmVZQ.png)

B9: Các bạn sẽ copy patch sao vào tab patch ở MaciASL.

```
into method label _Q1D replace_content
begin
// Brightness Down\n
Notify(\_SB.PCI0.LPCB.PS2M, 0x0205)\n
Notify(\_SB.PCI0.LPCB.PS2M, 0x0285)\n
end;
into method label _Q1C replace_content
begin
// Brightness Up\n
Notify(\_SB.PCI0.LPCB.PS2M, 0x0206)\n
Notify(\_SB.PCI0.LPCB.PS2M, 0x0286)\n
end;
```

B10: Các bạn sẽ thay các code in đậm ở trên bằng các method và path đã tìm được ở trên.

```
into method label _Q0E replace_content
begin
// Brightness Down\n
Notify(\_SB.PCI0.LPCB.PS2K, 0x0205)\n
Notify(\_SB.PCI0.LPCB.PS2K, 0x0285)\n
end;
into method label _Q0F replace_content
begin
// Brightness Up\n
Notify(\_SB.PCI0.LPCB.PS2K, 0x0206)\n
Notify(\_SB.PCI0.LPCB.PS2K, 0x0286)\n
end;
```

![](https://i.imgur.com/L3bIJz6.png)

B11: Reboot.

**Lưu ý: Patch ở bước 10 là cho `Voodoops2controller.kext` nếu các bạn dùng `ApplePS2SmartTouchPad.kext` thì các bạn sẽ add patch sau**:

```
into method label _Q0E replace_content
begin
// Brightness Down\n
Notify (PS2K, 0x20)\n
end;

into method label _Q0F replace_content
begin
// Brightness Up\n
Notify (PS2K, 0x10)\n
end
```

#### Hotpatch

Các bước xác định method và path device đều giống như static patch nhưng thay vì apply vào DSDT ta sẽ hotpatch ra SSDT.

B1: các bạn sẽ thay các code in đậm trong đoạn code sau bằng các method và path device đã tìm ở trên ( Rename `_Q0E to XQ0E` và `_Q0F to XQ0F`)

```
DefinitionBlock ("", "SSDT", 2, "hack", "BRKEYS", 0x00000000)
{
    External (_SB.PCI0.LPCB.EC, DeviceObj)            
    External (_SB.PCI0.LPCB.EC.XQ0E, MethodObj)    // Brightness down method
    External (_SB.PCI0.LPCB.EC.XQ0F, MethodObj)    // Brightness up method
    External (_SB_.PCI0.LPCB.PS2K, DeviceObj)            //rename to your keyboard device

    Scope (_SB.PCI0.LPCB.EC)
    {
        Method (_Q0E, 0, NotSerialized)  // _Qxx: EC Query, xx=0x00-0xFF
        {
            If (_OSI ("Darwin"))
            {
                Notify (PS2K, 0x0365)                    //send f14,rename to your keyboard device.
            }
            Else
            {
                \_SB.PCI0.LPCB.EC.XQ0E ()            //redirects to original method for other OS.
            }
        }

        Method (_Q0F, 0, NotSerialized)  // _Qxx: EC Query, xx=0x00-0xFF
        {
            If (_OSI ("Darwin"))
            {
                Notify (PS2K, 0x0366)            //sends f15, rename to your keyboard device.
            }
            Else
            {
                \_SB.PCI0.LPCB.EC.XQ0F ()
            }
        }
    }
}
```

B2: Các bạn sẽ convert các patch rename `_Q0E to XQ0E` và `_Q0F to XQ0F` từ `ASCII to HEX` bằng Hackintool.

![](https://i.imgur.com/DMpRDmj.png)

B3: Mở config bằng ProperTree và add các patch rename theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/).

![](https://i.imgur.com/5loPluF.png)

B4: Cho SSDT vừa tạo vào `EFI ==> OC ==> ACPI` (nhớ snapshot config) và `EFI ==> CLOVER ==> ACPI ==> Patched`.

B5: Reboot.

### Cách 2: Fix BRT6

#### Static patch

> ở các máy Dell Brightness keys sẽ được call qua BRT6 method trong DSDT. BRT6 sẽ được call qua EV5 method. EV5 method sẽ được call qua SMEE. SMEE sẽ được call qua OSID nếu OSID trả về giá trị 0x20 (32 decimal). Và OSID giá trị trả về sẽ được đặt trong tham số ACOS.

B1: Đặt tham số ACOS về giá trị là 0x20 cho Darwin OS.

```
// before:

        Method (OSID, 0, NotSerialized)
        {
            If (LEqual (ACOS, Zero))
            {
                [...]

                If (CondRefOf (\_OSI, Local0))
                {
                    [...]
                    If (_OSI (WIN7))
                    {
                        Store (0x80, ACOS)
                    }
                    [...]
                }
                [...]
            }
            Return (ACOS)
        }

// after:

        Method (OSID, 0, NotSerialized)
        {
            If (LEqual (ACOS, Zero))
            {
                [...]
                If (CondRefOf (\_OSI, Local0))
                {
                    [...]
                    If (LOr (_OSI ("Darwin"), _OSI (WIN7)))     // chuyển đoạn code từ win 7 thành win 7 và Darwin
                    {
                        Store (0x80, ACOS)                      // ở đây nếu giá trị bé hơn 0x20 thì các bạn sẽ chuyển thành 0x20
                    }
                    [...]
                }
                [...]
            }
            Return (ACOS)
        }
```

B2: Ta sẽ tiến hành chèn các key-code vào BRT6 method. Tiến hành thử các key-code sau (thử từng cái 1). Ta có 1 số `key-code` đã kiểm chứng như sau `0x0365` và `0x0366` sẽ dùng được cho các model sau `dell Latitude E6x20, E6x30, E6x40, E7x50, E7x70 or other 7x90`. Và `0x0405, 0x0406` sẽ dùng được cho các model khác như `Precision 5510 or 7510`.

- Brightness increase: key codes `0x10, 0x206, 0x286, 0x366, 0x0406`
- Brightness decrease: key codes `0x20, 0x205, 0x285, 0x365, 0x0405`

```
// before
        Method (BRT6, 2, NotSerialized)
        {
            If (LEqual (Arg0, One))
            {
                Notify (LCD, 0x86)
            }
            If (And (Arg0, 0x02))
            {
                Notify (LCD, 0x87)
            }
        }

// after

        Method (BRT6, 2, NotSerialized)
        {
            If (LEqual (Arg0, One))
            {
                Notify (LCD, 0x86)
                Notify (^^LPCB.PS2K, 0x0366)     // tiến hành thay các device path và key-code vào đây
            }
            If (And (Arg0, 0x02))
            {
                Notify (LCD, 0x87)
                Notify (^^LPCB.PS2K, 0x0365)     // tiến hành thay các device path và key-code vào đây
            }
        }
```

B3: Cho DSDT vào `EFI ==> OC ==> ACPI` (snapshot config) hoặc `EFI ==> CLOVER ==> ACPI ==> Patched`.

B4: Reboot.

#### 2. Hotpatch

B1: Các bạn tiến hành tạo SSDT method theo code sau:

```
DefinitionBlock ("", "SSDT", 2, "hack", "BRT6", 0x00000000)
{
    External (_SB_.PCI0.IGPU, DeviceObj)    // (from opcode)
    External (_SB_.PCI0.IGPU.LCD_, DeviceObj)    // (from opcode)
    External (_SB_.PCI0.LPCB.PS2K, DeviceObj)    // (from opcode)
    Scope (_SB.PCI0.IGPU)
    {
        Method (BRT6, 2, NotSerialized)
        {
            If (LEqual (Arg0, One))
            {
                Notify (LCD, 0x86)
                Notify (^^LPCB.PS2K, 0x0366)     // Capture of brightness-up key stroke
            }
            If (And (Arg0, 0x02))
            {
                Notify (LCD, 0x87)
                Notify (^^LPCB.PS2K, 0x0365)     // Capture of brightness-down key stroke
            }
        }
    }
}
```

B2: Tạo SSDT-XOSI theo code sau:

```
DefinitionBlock ("", "SSDT", 2, "hack", "XOSI", 0x00000000)
{
    Method (XOSI, 1, NotSerialized)
    {
        Store (Package (0x0A)
            {
                "Windows", 
                "Windows 2001", 
                "Windows 2001 SP2", 
                "Windows 2006", 
                "Windows 2006 SP1", 
                "Windows 2006.1", 
                "Windows 2009", 
                "Windows 2012", 
                "Windows 2013", 
                "Windows 2015"
            }, Local0)
        Return (LNotEqual (Ones, Match (Local0, MEQ, Arg0, MTR, Zero, Zero)))
    }
}
```

B3: Các bạn search OSID trong DSDT nếu có thì add SSDT-OC-Work-Dell.dsl

```
DefinitionBlock("", "SSDT", 2, "ACDT", "OCWork", 0)
{
    External (_SB.ACOS, IntObj)
    External (_SB.ACSE, IntObj)

    Scope (\)
    {
        If (_OSI ("Darwin"))
        {
            \_SB.ACOS = 0x80
            \_SB.ACSE = 0 //ACSE=0:win7;;ACSE=1:win8
        }
    }
}
```

B4: add các patch rename sau theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/)

```
// BRT6 to BRTX
Find (HEX):    4252543602    
Replace (HEX): 4252545802    

// OSID to XSID
Find (HEX):    4F534944    
Replace (HEX): 58534944  

// _OSI to XOSI
Find (HEX):    5F4F5349     
Replace (HEX): 584F5349     
```

**Lưu ý: Nếu máy Dell của bạn không có method BRT6 trong DSDT thì sẽ không add `SSDT-BRT6` và ko rename `BRT6 to BRTX`**.

**Lưu ý 2: Đối với phương pháp EC query thì các bạn phải tiến hành static patch trước khi hotpatch**.

**Lưu ý 3: Nếu các bạn nào lười hoặc không làm theo được thì có thể sử dụng app [Karabiner](https://karabiner-elements.pqrs.org/)**

**Source tham khảo: [ACPI patch for brightness keys on Dell laptops – DSDT/SSDT – osxlatitude.com](https://osxlatitude.com/forums/topic/15661-acpi-patch-for-brightness-keys-on-dell-laptops/) | [GUIDE: How to Fix Brightness hotkeys in DSDT / SSDT-hotpatch – Laptops | InsanelyMac](https://www.insanelymac.com/forum/topic/305030-guide-how-to-fix-brightness-hotkeys-in-dsdt-ssdt-hotpatch/) | [OpenCore-HotPatching-Guide/SSDT-OCWork-dell.dsl at master · LAbyOne/OpenCore-HotPatching-Guide (github.com)](https://github.com/LAbyOne/OpenCore-HotPatching-Guide/blob/master/19-Dell%20Machine%20Patch%20List/SSDT-OCWork-dell.dsl) | [[Guide] – Brightness Hotkey Remapping – Guides and Tutorials – Olarila](https://www.olarila.com/topic/5722-guide-brightness-hotkey-remapping/)**
