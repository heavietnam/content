# Patch GMA GPU

Các GMA GPU support:

GMA 900 (10.4 and 10.5).

GMA 950 (10.4-10.7).

- GMA 3150 có thể được Support nếu spoof device id.

GMA X3100 (10.5-10.7).

- Chỉ hỗ trợ Laptop.

## Chuẩn bị

- Hãy chắc rằng Kext của bạn thuộc 32-Bit hoặc FAT.

Bằng cách vào Terminal và gõ

```
lipo -archs path to kext
```

Hãy chắc rằng bạn đang Boot ở 32-Bit.

- GMA 900, 950 and 3150.

```
# Grantsdale
0x2582 - GMA 900 - 945GM/GMS/940GML
0x258A - GMA 900 - E7221
0x2782 - GMA 900 - 82915G

# Alviso
0x2592 - GMA 900 - 915GM/GMS/910GML
0x2792 - GMA 900 - 915GM/GMS/910GML

# Lakeport
0x2772 - GMA 950 - 915GM/GMS/910GML
0x2776 - GMA 950 - 915GM/GMS/910GML

# Calistoga
0x27A2 - GMA 950 - 82915G/GV/910GL
0x27A6 - GMA 950 - 945GM/GMS/GME, 943/940GML
0x27AE - GMA 950 - 945GSE
```

- GMA X3100.

```
# Crestline
0x2a02 - GMA X3100 - GM965/GL960

# Calistoga
0x2A02 - GMA X3100 - GM965/GL960
0x2A03 - GMA X3100 - GM965/GL960
0x2A12 - GMA X3100 - GME965/GLE960
0x2A13 - GMA X3100 - GME965/GLE960
```

Các bạn xem iGPU của mình thuộc nhóm nào thì tiến hành fake-id.

Ví dụ như ở đây mình Fake thành GMA 950(Calistoga).

B1: Chọn 1 trong số các device id trên (Ví dụ 0x27A2).

B2: Các bạn bỏ số “0” đi và chia thành từng cặp (VD: 0x27A2 -> 27 A2).

B3: Các bạn tiến hành hoán đổi vị trí từng cặp (27 A2 -> A2 27).

B4: Các bạn tiến hành thêm 2 cặp số 0 vào sau (A2 27 -> A2 27 00 00).

B5: Viết lại dãy số này vào mục device properties -> add -> PciRoot(0x0)/Pci(0x2,0x0) -> device-id | data | A2270000

## Clover

## Cách 1:

B1: Các bạn tải Clover Configurator [tại đây](https://mackie100projects.altervista.org/download-clover-configurator/).

B2: Các bạn tick vào mục Graphics ==> Inject Intel.

![](https://imgur.com/P5m6BH3.png)

## Cách 2:

- GMA 900/950/3150.

B1: Các bạn Add đoạn sau vào Device Properties-> PciRoot(0x0)/Pci(0x2,0x0) | dictionary

```
| built-in                  | Data | 01       |
| AAPL,HasPanel             | Data | 01000000 |
| AAPL01,BacklightIntensity | Data | 3F000008 |
| AAPL01,BootDisplay        | Data | 01000000 |
| AAPL01,DataJustify        | Data | 01000000 |
| AAPL01,Dither             | Data | 00000000 |
| AAPL01,Interlace          | Data | 00000000 |
| AAPL01,Inverter           | Data | 00000000 |
| AAPL01,InverterCurrent    | Data | 00000000 |
| AAPL01,LinkFormat         | Data | 00000000 |
| AAPL01,LinkType           | Data | 00000000 |
| AAPL01,Pipe               | Data | 01000000 |
| AAPL01,Refresh            | Data | 3B000000 |
| AAPL01,Stretch            | Data | 00000000 |
| AAPL01,T1                 | Data | 00000000 |
| AAPL01,T2                 | Data | 01000000 |
| AAPL01,T3                 | Data | C8000000 |
| AAPL01,T4                 | Data | C8010000 |
| AAPL01,T5                 | Data | 01000000 |
| AAPL01,T6                 | Data | 00000000 |
| AAPL01,T7                 | Data | 90100000 |
| AAPL01,DualLink           | Data | 00       |
// thay đổi thành 01 nếu bạn đang dùng màn hình lớn hơn 1366x768
| model                     | String | GMA 950| // thay đổi theo dòng igpu
```

Đối với GMA 3150 các bạn cần add mục sau vào kernel and kext patches -> kextstopatch.

```
Comment    = GMA 3150 Cursor corruption fix
disable    = false
Name       = com.apple.driver.AppleIntelIntegratedFramebuffer
Find       = 8b550883bab0000000017e36890424e832bbffff
Replace    = b800000002909090909090909090eb0400000000
```

- GMA X3100.

B1: các bạn add đoạn sau vào Device Properties-> PciRoot(0x0)/Pci(0x2,0x0) | dictionary

```
| built-in                       | Data | 01       |
| AAPL,HasPanel                  | Data | 01000000 |
| AAPL,SelfRefreshSupported      | Data | 01000000 |
| AAPL,aux-power-connected       | Data | 01000000 |
| AAPL,backlight-control         | Data | 01000008 |
| AAPL00,blackscreen-preferences | Data | 00000008 |
| AAPL01,BootDisplay             | Data | 01000000 |
| AAPL01,BacklightIntensity      | Data | 38000008 |
| AAPL01,blackscreen-preferences | Data | 00000000 |
| AAPL01,DataJustify             | Data | 01000000 |
| AAPL01,Dither                  | Data | 00000000 |
| AAPL01,Interlace               | Data | 00000000 |
| AAPL01,Inverter                | Data | 00000000 |
| AAPL01,InverterCurrent         | Data | 08520000 |
| AAPL01,LinkFormat              | Data | 00000000 |
| AAPL01,LinkType                | Data | 00000000 |
| AAPL01,Pipe                    | Data | 01000000 |
| AAPL01,Refresh                 | Data | 3D000000 |
| AAPL01,Stretch                 | Data | 00000000 |
| AAPL01,T1                      | Data | 00000000 |
| AAPL01,T2                      | Data | 01000000 |
| AAPL01,T3                      | Data | C8000000 |
| AAPL01,T4                      | Data | C8010000 |
| AAPL01,T5                      | Data | 01000000 |
| AAPL01,T6                      | Data | 00000000 |
| AAPL01,T7                      | Data | 90100000 |
| AAPL01,DualLink                | Data | 00       |
// thay đổi thành 01 nếu bạn đang dùng màn hình lớn hơn 1366x768
| model                          | String | GMA 950| // thay đổi theo dòng igpu
```

## OpenCore

- GMA 900, 950 and 3150.

## Desktop

B1: Các bạn tiến hành tạo DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) | dictionary

B2: các bạn Add đoạn sau vào:

```
| model         | String | GMA 950  | // thay đổi theo dòng igpu
| AAPL,HasPanel | Data   | 00000000 |
```

## Laptop

B1: Các bạn tiến hành tạo DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) | dictionary

B2: Các bạn add đoạn sau vào:

```
| model                     | String | GMA 950  | // thay đổi theo dòng igpu
| AAPL,HasPanel             |  Data  | 01000000 |
| AAPL01,BacklightIntensity |  Data  | 3F000008 |
| AAPL01,BootDisplay        |  Data  | 01000000 |
| AAPL01,DataJustify        |  Data  | 01000000 |
| AAPL01,DualLink           |  Data  | 00       | 

// thay đổi thành 01 nếu bạn đang dùng màn hình lớn hơn 1366x768
```

Đối với người dùng GMA 3150 các bạn sẽ cần add thêm patch sau vào Kernel -> Patch

```
Comment    = GMA 3150 Cursor corruption fix
Enabled    = True
Identifier = com.apple.driver.AppleIntelIntegratedFramebuffer
Find       = 8b550883bab0000000017e36890424e832bbffff
Replace    = b800000002909090909090909090eb0400000000
MaxKernel  = 11.99.99
MinKernel  = 8.00.00
```

- GMA X3100 

B1: Các bạn tiến hành tạo DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) | dictionary

B2: Các bạn Add đoạn sau vào:

```
| model                     | String | GMA X3100 | // thay đổi theo dòng igpu 
| AAPL,HasPanel             |  Data  | 01000000  |
| AAPL,SelfRefreshSupported |  Data  | 01000000  | // tùy chọn
| AAPL,aux-power-connected  |  Data  | 01000000  | // tùy chọn
| AAPL,backlight-control    |  Data  | 01000008  | // tùy chọn
| AAPL01,BacklightIntensity |  Data  | 38000008  |
| AAPL01,BootDisplay        |  Data  | 01000000  |
| AAPL01,DataJustify        |  Data  | 01000000  |
| AAPL01,DualLink           |  Data  | 00        |

// thay đổi thành 01 nếu bạn đang dùng màn hình lớn hơn 1366x768
```

## Fix 1 số lỗi

- Dell Laptop

Ở 1 số dòng Laptop của Dell thường bị lỗi đen màn khi khởi động do DIV của GPU bị lỗi cách fix là các bạn tạo 1 SSDT-DIV như sau

```
DefinitionBlock ("", "SSDT", 2, "DRTNIA", "SsdtDvi", 0x00001000)
{
    External (_SB_.PCI0.SBRG.GFX0.DVI_, DeviceObj)

    Scope (\_SB.PCI0.SBRG.GFX0.DVI)
    {
        Method (_STA, 0, NotSerialized)  // _STA: Status
        {
            If (_OSI ("Darwin"))
            {
                Return (0)
            }
            Else
            {
                Return (0x0F)
            }
        }
    }
```

- Kernel Panic sau 30s.

Do ở 10.6 vã cũ hơn thì PciRoot’s _UID phải là 0 đây là 1 Ví Dụ:

```
Device (PCI0)  {
 Name (_HID, EisaId ("PNP0A08")) // Use PNP0A08 to find your PciRoot
 Name (_CID, EisaId ("PNP0A03"))
 Name (_ADR, One)
 Name (_UID, Zero)               // Needs to be patched to Zero
}
```
