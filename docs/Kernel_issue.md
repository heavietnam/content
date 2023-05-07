# Kernel issue

## Stuck on [EB|#LOG:EXITBS:START]

Xem chi tiết [tại đây](https://heavietnam.ga/2022/03/03/eblogexitbsstart/).

## Stuck on `EndRandomSeed`

- Fix giống như lỗi Stuck on [EB|#LOG:EXITBS:START]
- Đối với các máy đang sử dụng OpenCore version 0.7.3+ boot Catalina bị stuck lỗi này thì chuyển `SecureBootModel` trong config về `Disable`.

## Stuck sau khi select macOS partion ở OpenCore

- Fix giống như lỗi Stuck on [EB|#LOG:EXITBS:START]
- Nên sử dụng [EFI debug](https://heavietnam.ga/2022/04/09/opencore-debug/) trong trường hợp này.

## Kernel Panic on `Invalid frame pointer`

Đây là lỗi ở `Booter --> quirks` các bạn sẽ tiến hành check các phần sau:

- DevirtualiseMmio
  - Một số Certain MMIO vẫn yêu được bật để có thể hoạt động được vì vậy bạn cần loại trừ các Certain này trong `Booter -> MmioWhitelist` hoặc disable hoàn toàn tính năng này
  - Xem chi tiết [tại đây](https://heavietnam.ga/2022/01/31/using-devirtualisemmio/)
- SetupVirtualMap
  - Tính năng này là bắt buộc đối với hầu hết các model. Nếu chưa có bạn hãy bật lên
  - Tuy nhiên 1 số model sẽ không hoạt động với quirks này và có thể xảy ra panic
    - Intel’s Ice Lake series
    - Intel’s Comet Lake series
    - AMD’s B550
    - AMD’s A520
    - AMD’s TRx40
    - VMs like QEMU

1 số lỗi khác có thể xảy ra do macos xung đột với tính năng write protection from CR0 register để giải quyết ta có thể làm như sau:

- Nếu firmware của bạn support MATs (2018+) thì làm theo sau:
  - EnableWriteUnprotector -> False
  - RebuildAppleMemoryMap -> True
  - SyncRuntimePermissions -> True
- Đối với những firmware cũ hơn thì bạn chỉnh theo sau:
  - EnableWriteUnprotector -> True
  - RebuildAppleMemoryMap -> False
  - SyncRuntimePermissions -> False

Tuy nhiên đối với 1 số máy thì gen 6 đã hộ trợ MATs để check xem máy bạn có hỗ trợ không bạn có thể check ở OpenCore log.

```
OCABC: MAT support is 1

// Nếu hiện là 1 tức là support còn hiện là 0 tức là không support
```

## Stuck on `[EB|LD:OFS] Err(0xE)` when booting preboot volume

```
// full error

[EB|`LD:OFS] Err(0xE) @ OPEN (System\\Library\\PrelinkedKernels\\prelinkedkernel)
```

Lỗi này xảy ra khi bạn preboot volume không được cập nhất đúng cách sẽ fix theo sau:

- Enable `JumpstartHotplug` ở `UEFI -> APFS` (Nếu không có nó sẽ không thể boot vào recovery ở macos)
- Boot vào recovery
- mở terminal và chạy đoạn code sau:

```
// Đầu tiên cần tìm preboot volume
diskutil list

// Nhìn vào list sau, ở đây preboot volume là disk5s2
/dev/disk5 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +255.7 GB   disk5
                                 Physical Store disk4s2
   1:                APFS Volume ⁨Big Sur HD - Data⁩       122.5 GB   disk5s1
   2:                APFS Volume ⁨Preboot⁩                 309.4 MB   disk5s2
   3:                APFS Volume ⁨Recovery⁩                887.8 MB   disk5s3
   4:                APFS Volume ⁨VM⁩                      1.1 MB     disk5s4
   5:                APFS Volume ⁨Big Sur HD⁩              16.2 GB    disk5s5
   6:              APFS Snapshot ⁨com.apple.os.update-...⁩ 16.2 GB    disk5s5s

// Mount Preboot volume
diskutil mount disk5s2

// sau đó run updatePreboot trên the Preboot volume
diskutil apfs updatePreboot /volume/disk5s2

// reboot hệ thống
reboot
```

## Stuck on `OCB: LoadImage failed - Security Violation`

```
// full error

OCSB: No suitable signature - Security Violation
OCB: Apple Secure Boot prohibits this boot entry, enforcing!
OCB: LoadImage failed - Security Violation
```

Điều này là do Apple Secure Boot đã bị lỗi thời hoặc thiếu trên preboot của bạn volume dẫn đến failure to load nếu bạn có bật securebootmodel. Lý do cho các file này bị thiếu là 1 lỗi của macOS.

Để khắc phục lỗi này các bạn làm theo sau:

- Disable SecureBootModel
  -  `Misc -> Security -> SecureBootModel -> Disabled`
- cài đặt lại bản macos mới nhất
- hoặc copy the Secure Boot manifests từ `/usr/standalone/i386` to `/Volumes/Preboot/<UUID>/System/Library/CoreServices`
  - Bạn có thể chỉnh điều này qua termianl ở recovery vì preboot volume không thể chỉnh sửa qua finder
  - Chạy đoạn code sau ở terminal  

```
// Tìm preboot volume
diskutil list

// Nhìn vào đoạn sau ta thấy preboot volume là disk5s2
/dev/disk5 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +255.7 GB   disk5
                                 Physical Store disk4s2
   1:                APFS Volume ⁨Big Sur HD - Data⁩       122.5 GB   disk5s1
   2:                APFS Volume ⁨Preboot⁩                 309.4 MB   disk5s2
   3:                APFS Volume ⁨Recovery⁩                887.8 MB   disk5s3
   4:                APFS Volume ⁨VM⁩                      1.1 MB     disk5s4
   5:                APFS Volume ⁨Big Sur HD⁩              16.2 GB    disk5s5
   6:              APFS Snapshot ⁨com.apple.os.update-...⁩ 16.2 GB    disk5s5s

// mount Preboot volume
diskutil mount disk5s2

// CD vào Preboot volume
// Lưu ý rằng preboot volume có đường dẫn là /System/Volumes/Preboot
cd /System/Volumes/Preboot

// lấy UUID
ls
 46923F6E-968E-46E9-AC6D-9E6141DF52FD
 CD844C38-1A25-48D5-9388-5D62AA46CFB8

// nếu có nhiều UUID cũng hiện (bạn dual nhiều macos), bạn sẽ cần xác định UUID nào là chính xác
// Để làm như vậy bạn sẽ tiến hành in ra .disk_label.contentDetails của mỗi volume

cat ./46923F6E-968E-46E9-AC6D-9E6141DF52FD/System/Library/CoreServices/.disk_label.contentDetails
 Big Sur HD%

cat ./CD844C38-1A25-48D5-9388-5D62AA46CFB8/System/Library/CoreServices/.disk_label.contentDetails
 Catalina HD%


// thay CD844C38-1A25-48D5-9388-5D62AA46CFB8 với UUID của bạn
cd ~
sudo cp -a /usr/standalone/i386/. /System/Volumes/Preboot/CD844C38-1A25-48D5-9388-5D62AA46CFB8/System/Library/CoreServices
```

## Stuck on `OCABC: Memory pool allocation failure - Not Found`

Để khắc phục các bạn có thể chỉnh setting BIOS theo sau:

- Above4GDecoding: Enabled
- CSM: Disabled
  - 1 vài laptop phải bật CSM
- update bios

## Stuck on `Buffer Too Small`

- Enable Above4GDecoding in the BIOS

## Stuck on `Plist only kext has CFBundleExecutable key`

Đường dẫn không chính xác trong config.plist. Để khắc phục tiến hành snapshot bằng propertree xem cách snapshot [tại đây](https://heavietnam.ga/2021/09/29/tim-hieu-ve-hackintosh/)

## Stuck on `This version of Mac OS X is not supported: Reason Mac…`

Lỗi này xảy ra khi SMBios không còn được hỗ trợ nữa để khắc phục lỗi này các bạn có 2 phương pháp:

- Add boot-arg `-no_compat_check`
- Hoặc thay các SMBios được hỗ trợ:
  - Catalina:
    - iMac13,x+
    - iMacPro1,1
    - MacPro6,1+
    - Macmini6,x+
    - MacBook8,1+
    - MacBookAir5,x+
    - MacBookPro9,x+
  - Big Sur:
    - iMac14,4+
    - iMacPro1,1
    - MacPro6,1+
    - Macmini7,1+
    - MacBook8,1+
    - MacBookAir6,x+
    - MacBookPro11,x+
  - Monterey
    - iMac16,1+
    - iMacPro1,1
    - MacPro6,1+
    - Macmini7,1+
    - MacBook9,1+
    - MacBookAir7,1+
    - MacBookPro11,3+

## `Couldn't allocate runtime area errors`

Xem cách fix KASLR [tại đây](https://heavietnam.ga/2022/01/31/using-devirtualisemmio/)

## Stuck on `RTC…, PCI Configuration Begins, Previous Shutdown…, HPET, HID: Legacy…`

Đây là nơi có nhiều PCI device được thiết lập và cấu hình và là nơi các lỗi khởi động sẽ xảy ra:

- `apfs_module_start...`,
- `Waiting for Root device`,
- `Waiting on...IOResources...`,
- `previous shutdown cause...`

Những vấn đề chính gây lỗi:

- Thiếu EC (cata+):
  
  - Thêm SSDT-EC theo [link](https://heavietnam.ga/2022/03/10/ssdt-recomend/)
  - Bỏ SSDT-EC vào EFI --> OC --> ACPI và snaps (hoặc EFI --> Clover --> ACPI --> Patched)

- Xung đột IRQ:
  
  - dump SSDT-HPET theo [link](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/#Patch_Hpet_IRQ)

- PCI allocation issue:
  
  - Update BIOS.
  - Bật Above4G trong bios. Nếu bios của bạn không có option này hãy add boot-arg `npci=0x2000`
    - Các main X99 and X299 có thể yêu cầu cả ncpi boot-arg và Above4G trong bios
    - AMD CPU không đồng thời bật cả Above4G và NCPI trong boot-arg vì chúg sẽ xung đột
    - Đối với bios thế hệ 2020+ thì khi bật Above4G các bạn phải set Booter -> Quirks -> ResizeAppleGpuBars:0 vì Resizable BAR Support có thể sẽ trở nên khả dụng
  - CSM disabled, Windows 8.1/10 UEFI Mode enabled.

- NVMe or SATA issue:
  
  - không sử dụng Samsung PM981 or Micron 2200S NVMe SSD
  - cài đặt latest firmware cho Samsung 970 EVO Plus xem chi tiết [tại đây](https://semiconductor.samsung.com/consumer-storage/support/tools/)
  - Disable SATA Hot-Plug trong bios
  - Đảm bảo ổ NVMe được đặt là NVMe trong bios

- NVRAM Failing:
  
  - Đây là lỗi phổ biến trên các dòng HEDT hoặc trên các main 300 series
    - Trên 300 series: tải và cài đặt SDT-PMC [tại đây](https://heavietnam.ga/2022/03/10/ssdt-recomend/)
    - HEDT thì các bạn sẽ Emulating NVRAM theo link [sau](https://heavietnam.ga/2022/04/09/emulated-nvram/) (chỉ cần chỉnh config không cần chạy lệnh)

- RTC missing
  
  - Với các chip 300+ series bạn cần [SSDT-AWAC](https://heavietnam.ga/2022/03/10/ssdt-recomend/)
  - Đối với các máy hedt (X99 và X299) các RTC device sẽ bị thiếu cần dùng SSDT-RTC0-RANGE xem chi tiết [tại đây](https://heavietnam.ga/2022/01/22/fix-system-clocks-on-hedt/)
  - 1 số máy HP cũng gặp trường hợp này các bạn có thể dùng SSDT-RTC xem chi tiết các fix [tại đây](https://heavietnam.ga/2022/04/10/fix-rtc-manual/)

## Stuck at `ACPI table loading on B550`

Thêm SSDT-CPUR.aml [tại đây](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-CPUR.aml).

## “`Waiting for Root Device`” or “`Prohibited Sign error`“

- USB
  - Map USB theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/)
  - `Kernel -> Quirks -> XhciPortLimit -> flase`
  - 1 vài lỗi khác có thể do firmware không truyền quyền điều khiển USB cho macOS.
    - `UEFI -> Quirks -> bạn có thể đổi port usb để khắc pục lỗi nàyeleaseUsbOwnership -> True`
    - Hoặc bật XHCI Handoff
  - Đôi khi việc đổi port usb cũng có thể giúp bạn khắc phục lỗi này
  - Đối với các cpu amd gen 15 và 16 thì cần add [XLNCUSBFix.kext](https://cdn.discordapp.com/attachments/566705665616117760/566728101292408877/XLNCUSBFix.kext.zip)
    - Nếu như kext đó không hoạt động thì bạn có thể thử kext [AMD StopSign-fixv5](https://cdn.discordapp.com/attachments/249992304503291905/355235241645965312/StopSign-fixv5.zip)
  - Ngoài ra hedt X299 cũng cần Enable Above4G Decoding
  - Thiếu USB port trong ACPI
    - Các máy Intel các bạn sử dụng [usbinjectall](https://github.com/daliansky/OS-X-USB-Inject-All)
    - Còn đối với các CPU AMD thì bạn cần sử dụng SSDT-RHUB
      - chọn `7.usb reset` trong SSDT-Time
- Sata issue:
  - Xem chi tiết [tại đây](https://heavietnam.ga/2021/12/06/xxxxi-patch-sata-controller/).

## Kernel panic with `IOPCIFamily` trên hedt X99

- Bật các mục sau trong config:
  - AppleCpuPmCfgLock
  - AppleXcpmCfgLock
  - AppleXcpmExtraMsrs
- Thêm SSDT-UNC [tại đây](https://heavietnam.ga/2022/03/10/ssdt-recomend/)

## Stuck on or near `IOConsoleUsers: gIOScreenLock...`/`gIOLockState (3...`

- DGPU không support hãy disable nó đi theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxi-disable-dgpu/)
- CSM bị tắt trong bios
  - 1 số laptop phải bật CSM
- Force tốc độ PCIe 3.0 link
- Check patch igpu theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xii-patch-igpu/)
  - Desktop UHD 630 có thể thử `00009B3E`
  - `-igfxmlr` boot-arg. Điều này cũng có thể giúp fix lỗi `Divide by Zero`
  - igfxonln=1 boot-arg trên igpu coffelake+ từ 10.15.4+

## Scrambled Screen on laptops

Enable `CSM` trong bios setting nó có thể có tên là `Boot legacy ROMs` hoặc vài setting legacy khác

## Black screen after `IOConsoleUsers: gIOScreenLock...` on Navi

- Thêm `agdpmod=pikera` add vào boot-arg
- Thử thay đổi các display output khác.
- Thử smbios MacPro7,1 với boot-arg `agdpmod=ignore`
- Đối với máy MSI thì bạn hãy add patch sau vào `Kernel -> Patch` (ở bigsur không yêu cầu bản vá này).

```
Base:
Comment: Navi VBIOS Bug Patch
Count: 1
Enabled: YES
Find: 4154592C526F6D2300
Identifier: com.apple.kext.AMDRadeonX6000Framebuffer
Limit: 0
Mask:
MinKernel: 19.00.00
MaxKernel: 19.99.99
Replace: 414D442C526F6D2300
ReplaceMask:
Skip: 0
```

## Kernel Panic `Cannot perform kext summary`

- Hãy chắc rằng bạn đã add kext chính trước khi add plugin
  - Vì những plugin chỉ có file plist mà không chứa tệp thực thi nên cần pahỉ sử dụng với kext chính
- Không sử dụng nhiều kext giống nhau trong config.plist
  - như voodooinput có thể xuất hiện trong kẽt i2c và cả pss2 nên bạn chỉ nên dùng 1 kext voodooinput và xóa cái còn lại trong kext kia
- Sử dụng phương pháp fix lỗi giống như `invalid frame pointer`

## Kernel Panic `AppleIntelMCEReporter`

Thêm kext [AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip) vào EFI --> OC --> Kext hoặc EFI --> Clover --> kext --> other (snaps nếu ở OpenCore).

## Kernel Panic `AppleIntelCPUPowerManagement`

- Patch power manager [tại đây](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/)
- Thêm kext [NullCPUPowerManagement](https://www.mediafire.com/folder/rhjg943ja2cc1/NullCPUPowerManagement.kext)
- Hoặc bạn có thể enable `DummyPowerManagement` trong `Kernel -> Emulate` (chỉ đối với opencore)
- Ở 1 số các cpu thế hệ cũ cung có thể bị thiếu hpet hoặc xung đột irq
  - Fix hpet theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/)
  - Force hpet

```
// add patch sau vào ACPI --> Patch

Comment    String    Force HPET Online
Enabled    Boolean    YES
Count    Number    0
Limit    Number    0
Find    Data    A010934F53464C00
Replace    Data    A40A0FA3A3A3A3A3
```

## Kernel Panic `AppleACPIPlatform` in 10.13

![](https://heavietnam.ga/wp-content/uploads/2022/04/image-1.png)

Bật `NormalizeHeaders` trong `ACPI -> Quirks`

## macOS frozen right before login

Thêm kext [CpuTscSync](https://github.com/lvs1974/CpuTscSync) vào EFI --> OC --> kext hoặc EFI --> Clover --> kext --> other (snaps nếu là OpenCore).

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/asus-tsc.2397797f.png)

Lỗi 1

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/asus-tsc-2.029ce318.png)

Lỗi 2

## Keyboard works but trackpad does not

Fix trackpad theo [link](https://heavietnam.ga/2021/09/29/v-fix-trackpad/)

## `kextd stall[0]: AppleACPICPU`

Điều này là do macOS bị thiếu giả lập SMC hãy đảm bản bạn có những phần sau:

- [Lilu](https://github.com/acidanthera/Lilu/releases) và [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases) trong EFI --> OC --> kext hoặc EFI --> Clover --> kext --> other (snaps nếu là OpenCore)
- Nếu không được hãy thử sử dụng [Fake-SMC](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/) (không sử dụng cả 2 fake-smc và VirtualSMC)

## Kernel Panic on `AppleIntelI210Ethernet`

Đối với những main Comet lake với card I225-V NIC có thể xảy ra panic do kext I210 . Để khắc phục bạn cần có PciRoot đúng với card ethernet. 1 số đường dẫn phổ biến là

- PciRoot(0x0)/Pci(0x1C,0x1)/Pci(0x0, 0x0)
  - đối với main Asus, Gigabyte và đây cũng là mặc định
- PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)
  - được dùng để thay thế khi dường dẫn trên không hoạt động

Bạn cũng có thể sử dụng [gfxutil](https://github.com/acidanthera/gfxutil/releases) để tìm PciRoot của card ethernet 1 cách thủ công:

```
<kéo gfxutil vào terminal> | grep -i "8086:15f3"
00:1f.6 8086:15f3 /PC00@0/GBE1@1F,6 = PciRoot(0x0)/Pci(0x1F,0x6)
```

Ta sẽ dễ thấy PciRoot là PciRoot(0x0)/Pci(0x1F,0x6). Ta sẽ add PciRoot(0x0)/Pci(0x1F,0x6)|data| F2150000

## Kernel panic on “`Wrong CD Clock Frequency`” with Icelake laptop

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/cd-clock.ad48e008.jpg)

Add boot-arg `-igfxcdc`

## Kernel panic on “`cckprng_int_gen`“

```
// full error
"cckprng_int_gen: generator has already been sealed"
```

Có 2 khả năng dẫn đến lỗi

- Thiếu SMC
  - add [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases) vào EFI --> OC --> kext hoặc EFI --> Clover --> Kext --> other (snaps nếu dùng opencore)
  - sử dụng SSDT-CPUR không đúng
    - chỉ sử dụng SSDT này trên B550 and A520
    - Không sử dụng SSDT này trên X570 (B450 or A320)

## Stuck at `Forcing CS_RUNTIME for entitlement` in Big Sur

![Credit to Stompy for image](https://dortania.github.io/OpenCore-Install-Guide/assets/img/cs-stuck.bddc4a2d.jpg)

Đây thực sự không phải là lỗi. Chỉ vì quá trình này rất lâu để vượt qua mà nhiều người nghĩ đây là lỗi nhưng hãy kiên nhẫn và đừng tắt máy

## Stuck on `ramrod`(^^^^^^^^^^^^^)

bị stuck ở ramrod có nghĩa là nó boot --> gặp lỗi --> reboot --> vòng lặp

Điều này có nghĩa là giả lặp smc của bạn đã bị hỏng có 2 cách khắc phục

- Cài đặt VirtualSMC và Lilu new version kèm boot-arg `vsmcgen=1`
- Bạn cũng thể thử kext [Fake-SMC](https://bitbucket.org/RehabMan/os-x-fakesmc-kozlek/downloads/) để thay thế

## Virtual Machine Issues

VMware 15 sẽ bị stuck ở `[EB|#LOG:EXITBS:START]`. Cách fix là nâng câp lên VMware 16

## Reboot on “`AppleUSBHostPort::createDevice: failed to create device`” on macOS 11.3+

- Map usb theo link [tại đây](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/)
- Tắt `XhciPortLimit` ở `Kernel -> Quirks` (ở 11.3+)

## OC: Prelinked injection USBInjectAll.kext (USBInjectAll.kext) – Invalid Parameter

Convert usb injectall về version 2018 của rehabman [tại đây](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/RehabMan-USBInjectAll-2018-1108.zip)

## Stuck on `CpuTscSync`

![](https://i.imgur.com/YtYrwMa.jpg)

Hãy xóa kext CpuTscSync đi sau đó (snaps nếu là opencore) reboot

**Lưu ý: Tuy bài viết này dựa trên cách fix lỗi cho opencore. nhưng người dùng clover vẫn có thể áp dụng cách đọc lỗi và nguyên nhân dẫn đến lỗi vì về bản chất thì nguyên nhân dẫn đến lỗi của opencore và oc là giống nhau**

**Source tham khảo: [Kernel Issues | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/kernel-issues.html)**
