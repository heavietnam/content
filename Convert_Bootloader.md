# Convert Bootloader

> Lý do có guide này: guide này hình thành là do 1 số bạn muốn chuyển bootloader Clover và OpenCore nhưng gặp số tình trạng như ko biết tính năng này ở đâu trong bootloader mới do đó mọi thứ bạn cần đề ở đây do hiện nay mọi người có xu hướng dùng opencore nhiều hơn nên ở đây mình sẽ viết theo lối convert từ Clover sang OpenCore nhưng người dùng Clover hoàn toàn có thể làm theo.

## Cấu trúc EFI

- Config

Cả hai OpenCore và Clover đều có

- Drivers

Opencore: `EFI` --> `OC` --> `Drivers`

Clover: `EFI` –> `Drivers` –> `UEFI` (ngoài ra còn mục BIOS và OFF nhưng nó nằm ngoài phạm vi bài này)

- Tool

Cấu trúc tương đương nhau

- Kext

Opencore: `EFI` --> `OC` —> `Kexts`

Clover: `EFI` --> `Kexts` --> `Other`

- ACPI

Cấu trúc đường dẫn tương đương nhau

## Config

### ACPI

#### ACPI Rename:

- XHCI patch (bạn có thể [map usb](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/) để thay thế)
  - XHCI to XHC
  - XHX1 to XHC
- STA rename (OpenCore đã có)
  - SAT0 to SATA
  - SAT1 to SATA
- IMEI rename (dùng WhateverGreen)
  - HECI to IMEI
  - HEC1 to IMEI
  - MEI to IMEI
  - IDER to MEID
- Graphics patches (dùng WhateverGreen)
  - GFX0 to IGPU
  - PEG0 to GFX0
  - PEGP to GFX0
  - SL01 to PEGP
- EC patches (bạn có thể dùng [SSDT-EC](https://heavietnam.ga/2021/09/29/xxix-dung-ssdt-time-de-prebuilt-ssdt/))
  - EC0 to EC
  - H_EC to EC
  - ECDV to EC
  - PGEC to EC
- Audio rename (dùng [AppleALC](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/))
  - HDAS to HDEF
  - CAVS to HDEF
  - AZAL to HDEF
  - ALZA to HDEF
  - B0D3 to HDAU
- Fix RTC (sử dụng SSDT-AWAC hoặc [patch RTC](https://heavietnam.ga/2021/10/14/fix-rtc/))
  - STAS to [Blank]
  - Fix Z390 BIOS DSDT Device(RTC) bug
  - Fix 300-series RTC Bug
- Fix NVMe (dùng [NVMeFix](https://github.com/acidanthera/NVMeFix) để thay thế)
  - PXSX to ANS1
  - PXSX to ANS2
- AirPort/WiFi patches bcrm (dùng [airport bcrm fixup](https://github.com/acidanthera/AirportBrcmFixup))
  - PXSX to ARPT
- Các patch ACPI khác:
  - LPC0 to LPCB (dùng [SSDT-SBUS-MCHC](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs/AcpiSamples/Source/SSDT-SBUS-MCHC.dsl))
  - PC00 to PCIO
  - FPU to MATH
  - TMR to TIMR
  - PIC to IPIC
  - GBE1 to ETH0

#### Patches:

- TgtBridge patches:
  - `ACPI -> Patch -> ... -> Base`
- DisableASPM:
  - `DeviceProperties -> Add -> PciRoot... -> pci-aspm-default | Data | <00>`
- HaltEnabler:
  - `ACPI -> Quirks -> FadtEnableReset -> YES`

#### Fixes:

- **FixIPIC**:
  
  -  [SSDTTime](https://github.com/corpnewt/SSDTTime) là phương pháp thay thế.

- **FixSBUS**:
  
  - [SSDT-SBUS-MCHC](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs/AcpiSamples/Source/SSDT-SBUS-MCHC.dsl) là phương pháp thay thế.

- **FixShutdown**:
  
  - [FixShutdown-USB-SSDT](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/FixShutdown-USB-SSDT.dsl)
  - [`_PTS` to `ZPTS` Patch](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/FixShutdown-Patch.plist)

- **FixDisplay**:
  
  - Patch framebuffer xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xii-patch-igpu/).

- **FixHDA**:
  
  - [AppleALC](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/) sử dụng.

- **FixHPET**:
  
  -  [Fix HPET](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/).

- **FixSATA**:
  
  - `Kernel -> Quirks -> ExternalDiskIcons -> YES`

- **FixRTC**:
  
  -  [SSDTTime](https://github.com/corpnewt/SSDTTime)  dùng SSDTTime patch hpet để thay thế.

- **FixTMR**:
  
  -  [SSDTTime](https://github.com/corpnewt/SSDTTime) dùng SSDTTime để patch HPET.

- **AddPNLF**:
  
  -  [SSDT-PNLF](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/backlight.html) dùng SSDT-PNLF

- **AddMCHC**:
  
  - [SSDT-SBUS-MCHC](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs/AcpiSamples/Source/SSDT-SBUS-MCHC.dsl) dùng SSDT này để thay thế.

- **AddIMEI**:
  
  - [SSDT-SBUS-MCHC](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs/AcpiSamples/Source/SSDT-SBUS-MCHC.dsl)
  - Cũng có thế dùng whatevergreen
  - 1 số Sandy Bridge hoặc Ivy Bridge cần sử dụng [SSDT-IMEI](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs/AcpiSamples/Source/SSDT-IMEI.dsl).

- **FakeLPC**:
  
  - `DeviceProperties -> Add -> PciRoot... -> device-id`
  - fake device id để dử dụng bộ điều khiển LPC trong Apple LPC.

- **FixIntelGfx**:
  
  - Bạn có thể sử dụng [WhateverGreen](https://github.com/acidanthera/whatevergreen/releases)

- **AddHDMI**:
  
  - Bạn có thể sử dụng [WhateverGreen](https://github.com/acidanthera/whatevergreen/releases)

#### **DropTables**:

- `ACPI -> Delete`

**SSDT**:

- **PluginType**:
  - [SSDT-PLUG](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html)
- **P States**: [ssdtPRGen.sh](https://github.com/Piker-Alpha/ssdtPRGen.sh) (nếu Sandy Bridge và Ivy Bridge)
- **C States**: [ssdtPRGen.sh](https://github.com/Piker-Alpha/ssdtPRGen.sh) (nếu Sandy Bridge và Ivy Bridge)

# Boot

**Boot Argument**:

- `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`

**NeverHibernate**:

- `Misc -> Boot -> HibernateMode -> None`

**Default Boot Volume**:

- `Misc -> Security -> AllowSetDefault -> True`
  - Ấn Ctrl+Enter ở màn hình gui opencore để chọn boot mặc định.
- Hoặc bạn có thể dùng như RealMac ở trong Setting.

## Boot Graphics

**DefaultBackgroundColor**:

- `NVRAM -> Add -> 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 -> DefaultBackgroundColor`
  - `00000000`: Syrah Black
  - `BFBFBF00`: Light Gray
  - Bạn có thể tùy chọn màu bằng các convert mã màu RGB của bạn sang hex bằng hackintool hoặc bất kì trình chuyển đổi hex nào mà bạn muốn.

**EFILoginHiDPI**:

- OpenCore sử dụng UI scaling  `UEFI -> Output`

**flagstate**:

- `NVRAM -> Add -> 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 -> flagstate | Data | <>`
  - 0 -> `<00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000`>

**UIScale**:

- `NVRAM -> Add -> 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 -> UIScale | Data | <>`
  - 1 -> `<01>`
  - 2 -> `<02>`

# Devices

**USB**:

- FixOwnership: `UEFI -> Quirk -> ReleaseUsbOwnership`
- ClockID: `DeviceProperties -> Add -> PciRoot... -> AAPL,clock-id`
- HighCurrent: `DeviceProperties -> Add -> PciRoot... -> AAPL, HighCurrent`
  - Từ for OS X 10.11 and mới hơn.
  - Đối với phiên bản mới hơn được xác định ở  `IOUSBHostFamily.kext -> AppleUSBHostPlatformProperties` hoặc được add vào SSDT-EC-USBX cho Skylake SMBios mới hơn (gen 6 +)

**Audio**:

Bạn sẽ cần biết âm thanh đc đièu khiên qua device, sử dụng [gfxutil](https://github.com/acidanthera/gfxutil/releases) để tìm ra chúng:

```
// mở terminal
path/to/gfxutil -f HDEF
```

- Inject: `DeviceProperties -> Add -> PciRoot... -> layout-id`
- AFGLowPowerState: `DeviceProperties -> Add -> PciRoot... -> AFGLowPowerState -> <01000000>`
- ResetHDA: `UEFI -> Audio -> ResetTrafficClass`
  - hoặc bạn có thể add vào boot-arg  `alctsel=1` cho apple alc

**Add Properties**:

- Bạn cần xác định đường dẫn của PciRoot

**Properties**:

- `DeviceProperties -> Add`

**FakeID**: bạn sẽ cần tìm đường dẫn tới PciRoot và cho nó vào `DeviceProperties -> Add`, tìm đường dẫn PciRoot bằng [gfxutil](https://github.com/acidanthera/gfxutil/releases).

- **USB**
  
  - `device-id`
  - `device_type`
  - `device_type`

- **IMEI**
  
  - `device-id`
  - `vendor-id`

- **WIFI**
  
  - `name`
  - `compatible`

- **LAN**
  
  - `device-id`
  - `compatible`
  - `vendor-id`

- **XHCI**
  
  - `device-id`
  - `device_type: UHCI`
  - `device_type: OHCI`

- device_type: EHCI
  
  - `device-id`
  - `AAPL,current-available`
  - `AAPL,current-extra`
  - `AAPL,current-available`
  - `AAPL,current-extra`
  - `AAPL,current-in-sleep`
  - `built-in`

- device_type: XHCI
  
  - `device-id`
  - `AAPL,current-available`
  - `AAPL,current-extra`
  - `AAPL,current-available`
  - `AAPL,current-in-sleep`
  - `built-in`

**ForceHPET**:

- `UEFI -> Quirks -> ActivateHpetSupport`

## Disable Drivers

Bạn có thể ko thêm vào `UEFI -> Drivers`, hoặc thêm dấu # vào trước driver trong config để OpenCore bỏ qua nó.

## GUI (Menu boot)

Cả 2 đều có GUI, xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xvii-tao-gui/)

## Graphics

**InjectIntel**:

- [GMA Patching](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/)

**InjectAti**:

- `DeviceProperties -> Add -> PciRoot... -> device-id`
  - ví dụ: `<B0670000>` cho R9 390X
- `DeviceProperties -> Add -> PciRoot... -> @0,connector-type`
  - bạn có thể cần patch connector theo bản sau chi [ở đây](https://heavietnam.ga/2021/09/29/xiv-patch-connect-type-force-rgb-injects-edid/)

```
LVDS                    <02 00 00 00>
DVI (Dual Link)         <04 00 00 00>
DVI (Single Link)       <00 02 00 00>
VGA                     <10 00 00 00>
S-Video                 <80 00 00 00>
DP                      <00 04 00 00>
HDMI                    <00 08 00 00>
DUMMY                   <01 00 00 00>
```

**InjectNvidia**:

- [Patch nvcap](https://heavietnam.ga/2021/09/29/xiii-fix-connect-hdmi/)

**FakeIntel**:

- `DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) -> device-id`
  - ví dụ. `66010003` cho the HD 4000
- `DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) -> vendor-id -> <86800000>`

**FakeAti**:

- `DeviceProperties -> Add -> PciRoot... -> device-id`
  - ie: `<B0670000>` for the R9 390X
- `DeviceProperties -> Add -> PciRoot... -> ATY,DeviceID`
  - ie: `<B067>` for the R9 390X
- `DeviceProperties -> Add -> PciRoot... -> @0,compatible`
  - ie. `ATY,Elodea` for HD 6970M
- `DeviceProperties -> Add -> PciRoot... -> vendor-id-> <02100000>`
- `DeviceProperties -> Add -> PciRoot... -> ATY,VendorID -> <0210>`

xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xxiii-patch-card-doi-hoa-amd/)

**Custom EDID**

- xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xiv-patch-connect-type-force-rgb-injects-edid/)

**Dual Link**:

- `DeviceProperties -> Add -> PciRoot... -> AAPL00,DualLink`
  - 1 -> `<01000000>`
  - 0 -> `<00000000>`

**NVCAP**

- xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xiii-fix-connect-hdmi/)

**display-cfg**:

- `DeviceProperties -> Add -> PciRoot... -> @0,display-cfg`
- tham khảo tại [Nvidia injection](https://www.insanelymac.com/forum/topic/215236-nvidia-injection/)

**LoadVBios**:

Tạo 1 cấu trúc ssdt tương tự như sau

```
External (SB.PCI0.GFX0, DeviceObj)
External (SB.PCI0.PEG0, DeviceObj)
External (SB.PCI0.PEG0.PEGP, DeviceObj)
External ( DTGP , MethodObj)

Scope (\_SB.PCI0)
{
    Scope (PEG0)
    {
        Scope (PEGP)
        {
            Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
            {
                Store (Package ()
                {
                    Buffer (0x00010000)
                    {
                        // Put your VBIOS here (you could extract it with Linux or Windows)
                    }

                }, Local0)
                DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                Return (Local0)
            }
        }
     }
  }
}   
```

**NvidiaGeneric**:

- `DeviceProperties -> Add -> PciRoot... -> model | string | Add the GPU name`

**NvidiaSingle**:

xem chi tiết tại đây [disable dgpu](https://heavietnam.ga/2021/09/29/xxi-disable-dgpu/)

**NvidiaNoEFI**:

- `DeviceProperties -> Add -> PciRoot... -> NVDA,noEFI | Boolean | True`
- tham khảo tại đây: [GT 640 scramble](https://www.insanelymac.com/forum/topic/306156-clover-problems-and-solutions/?do=findComment&comment=2443062)

**ig-platform-id**:

- `DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) -> APPL,ig-platform-id`

**BootDisplay**:

- `DeviceProperties -> Add -> PciRoot... -> @0,AAPL,boot-display`

**RadeonDeInit**:

- Tính năng này hầu như ko cần nếu bạn có sử dụng WhateverGreen

# Kernel and Kext Patches

**KernelPm**:

- `Kernel -> Quirks -> AppleXcpmCfgLock -> YES`

**AppleIntelCPUPM**:

- `Kernel -> Quirks -> AppleCpuPmCfgLock -> YES`

**DellSMBIOSPatch**:

sử dụng cho các hệ thống dell cũ sử dụng APTIO V

- `Kernel -> Quirks -> CustomSMBIOSGuid -> YES`
- `PlatformInfo -> UpdateSMBIOSMode -> Custom`

**KextsToPatch**:

- `Kernel -> Patch`
- 1 số quy tắc đổi

```
comment : sử dụng cho cả opencore và clover

Disabled : tương ứng với mục enable ben opencore

MatchBuild : được thay thế bằng minkernel và maxkernel 

MatchOS : được thay thế bằng minkernel và maxkernel

Find : có ở opencore và clover

Replace : có cả ở opencore và clover

MaskFind : opencore sử dụng mask để thay thế

Mask replace : có cả ở opencore và clover

count ,limit, skip thường được đặt thành 0

Identifier thường được đặt thành kernel ( trông 1 số trường hợp khi bạn patch kext nào thì sẽ để đường dẫn theo kext đó vd như com.apple.iokit.IOGraphicsFamily )
```

**KernelToPatch**:

- `Kernel -> Patch`

**ForceKextsToLoad**:

- `Kernel -> Force`

**Kernel LAPIC**:

- `Kernel -> Quirks -> LapicKernelPanic -> YES`

**KernelXCPM**:

- `Kernel -> Quirks -> AppleXcpmExtraMsrs -> YES`

patch xcpm theo bảng dưới xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/)

```
Base: _xcpm_bootstrap
Comment: _xcpm_bootstrap (Ivy Bridge) 10.15
Count: 1
Enabled: YES
Find: 8D43C43C22
Identifier: kernel
Limit: 0
Mask: FFFF00FFFF
MinKernel: 19.
MaxKernel: 19.99.99
Replace: 8D43C63C22
ReplaceMask: 0000FF0000
Skip: 0
```

cho low-end haswell+ pentium Celerons

**USB Port Limit Patches**:

- `Kernel -> Quirks -> XhciPortLimit -> YES`

**External Icons Patch**:

- `Kernel -> Quirks -> ExternalDiskIcons -> YES`
- được sử dụng để cho ổ đĩa trong ở macos đc xem như ổ đĩa ngoài

**AppleRTC**

1 số lỗi về rtc

- config.plist -> Kernel -> Quirks -> DisableRtcChecksum -> true

Xem chi tiết [tại đây](https://heavietnam.ga/2021/10/14/fix-rtc/).

Bạn có thể ađ vào boot-arg để qua khỏi lỗi sau đó patch lại.

```
rtcfx_exclude=00-FF
```

**FakeCPUID**:

- `Kernel -> Emulate`:

```
Cpuid1mask:  

//remove 0x và tách thành từng cặp

0x0306A9 ==> 03 06 A9

// đảo ngược thứ tự các cặp

03 06 A9 ==> A9 06 03

//thêm các số 0 vào sau dãy sô vừa đổi sao cho đủ 4 cặp mỗi cặp gồm 8 chữ số

A9 06 03 ==> A9060300 00000000 00000000 00000000

Cpuid1Data:

FFFFFFFF 00000000 00000000 00000000
```

xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xxiv-fake-cpu-id/)

# Rt Variables

**ROM**:

- Xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xix-fix-imessage/)

**MLB**:

- `PlatformInfo -> Generic -> MLB`

**BooterConfig**:

- `NVRAM -> Add -> 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14-> UIScale`:
  - 0x28: `Data | <01>`
  - 0x2A: `Data | <02>`

**CsrActiveConfig**:

- `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> csr-active-config`:
  - 0x0: `00000000`
  - 0x3: `03000000`
  - 0x67: `67000000`
  - 0x3E7: `E7030000`

# SMBIOS

**Product Name**:

- `PlatformInfo -> Generic -> SystemProductName`

**Serial Number**:

- `PlatformInfo -> Generic -> SystemSerialNumber`

**Board Serial Number**:

- `PlatformInfo -> Generic -> MLB`

**SmUUID**:

- `PlatformInfo -> Generic -> SystemUUID`

**Memory**:

- `PlatformInfo -> CustomMemory -> True`
- `PlatformInfo -> Memory`

**Slots AAPL Injection**:

- `DeviceProperties -> Add -> PciRoot... -> APPL,slot-name | string | Add slot`

# System Parameters

**CustomUUID**:

- Tính năng này dường như ko cần thiết

**InjectSystemID**:

- gần như vô dụng

**BacklightLevel**:

- set tính năng này trong nvram
- `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> backlight-level | Data | <Insert value>`
  - 0x0101 -> `<0101>`

**InjectKexts**:

- thật sự ko cần thiết

**NoCaches**:

- được sử dụng cho macos 10.7 và opencore yêu cầu macos phải từ 10.7+ nên ko có bản tương đương

**ExposeSysVariables**:

- chỉ cần thêm nó và dưới `platformInfo`

**NvidiaWeb**:

- Điều này có nghĩa là bạn đang áp dụng dòng vào terminal `sudo nvram nvda_drv=1` mỗi lần khởi động có thể áp dụng nó vào opencore theo đường dẫn sau:
- NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> nvda_drv: <31>

**Source tham khảo: [Converting common properties from Clover to OpenCore | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/clover-conversion/Clover-config.html)**
