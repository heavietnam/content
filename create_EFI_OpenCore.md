# Cách tạo EFI OpenCore

# Chuẩn bị

B1: Tải xuống `OpenCorePkg` [tại đây](https://github.com/acidanthera/OpenCorePkg/releases)

B2: Chọn thư mục `IA32` hoặc `X64`

- < 4GB RAM chọn bản 32bit (IA32)
- > 4GB RAM chọn bản 64 bit (X64)

B3: các bạn cần loại bỏ 1 số mục và chỉ giữ lại những mục sau đây

- Driver :
  - OpenRuntime
  - [HFSPlus](https://github.com/JrCs/CloverGrowerPro/blob/master/Files/HFSPlus/X64/HFSPlus.efi?raw=true)
  - ResetNvramEntry
  - OpenCanopy (nếu dùng [gui](https://heavietnam.ga/2021/09/29/xvii-tao-gui/) thì thêm driver này không dùng có thể xoá đi)
- Tool
  - Xóa tất cả hoặc chừa lại OpenShell
- OpenCore.efi
- ACPI
- Kext
- Boot

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-12-at-23.34.21.png?w=1024)

B4: Vào file docs và copy file `sample.plist` sau đó đổi tên nó thành `config.plist`

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-12-at-23.41.10.png?w=1024)

## Thêm các kext

Thêm các kext sau vào EFI ==> OC ==> Kexts

### Các kext bắt buộc

- [Lilu](https://github.com/acidanthera/Lilu/releases) : đây là mục kext vô cùng quan trọng nếu không có nó bạn sẽ không thể boot được
- [WhateverGreen](https://github.com/acidanthera/whatevergreen/releases) : đây là kext giúp patch đồ họa
- [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases) : dùng để mô phỏng SMC, không có nó bạn sẽ không thể boot được

### Kext không bắt buộc

Các plugin của VirtualSMC

- SMCProcessor.kext : Dùng để theo dõi nhiệu độ CPU (Không áp dụng cho các CPU AMD)

- SMCSuperIO.kext : Dùng để theo dõi tốc độ quạt (Không áp dụng cho các CPU AMD)

- SMCLightSensor.kext : Dùng để fix cảm biến ánh sáng

- SMCBatteryManager.kext : Dùng để hiển thị phần trăm pin

- SMCDellSensors.kext : Dùng để theo dõi tốc độ quạt trên các máy Dell

[AppleALC](https://github.com/acidanthera/AppleALC/releases) : dùng để patch âm thanh

Các Kext về Ethernet xem chi tiết ở bài [này](https://everythingforhackintosher.wordpress.com/2021/09/09/fix-ethernet/)

Các Kext về WiFi và Bluetooth xem chi tiết ở bài [này](https://everythingforhackintosher.wordpress.com/2021/09/09/40/)

[USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/): Dùng để inject usb

[XHCI-unsupported](https://github.com/RehabMan/OS-X-USB-Inject-All): Cần trên các chipset

- H370
- B360
- H310
- Z390 (Không cần thiết trên Mojave và mới hơn)
- X79
- X99
- Mainboard AsRock (Trên bo mạch chủ Intel cụ thể, bảng B460 / Z490+ không cần nó tuy nhiên)

[XLNCUSBFIX](https://cdn.discordapp.com/attachments/566705665616117760/566728101292408877/XLNCUSBFix.kext.zip) : Inject USB trên các máy sử dụng CPU AMD

[VoodooHDA](https://sourceforge.net/projects/voodoohda/) : Fix âm thanh trên các máy sử dụng CPU AMD

[AppleMCEReporterDisabler](https://github.com/acidanthera/bugtracker/files/3703498/AppleMCEReporterDisabler.kext.zip) : Dùng để vô hiệu hóa AppleMCEReporter trên các máy AMD, yêu cầu macOS 10.15+

[CpuTscSync](https://github.com/lvs1974/CpuTscSync/releases) : Cần thiết để đồng bộ hóa TSC trên một số bo mạch chủ HEDT và máy chủ của Intel, nếu không có macOS này có thể cực kỳ chậm hoặc thậm chí không thể vào được.

[VoodooPS2](https://github.com/acidanthera/VoodooPS2/releases) : Dùng để patch bàn phím và trackpad

[VoodooRMI](https://github.com/VoodooSMBus/VoodooRMI/releases/) : Dùng cho các trackpad Synaptics

[VoodooSMBus](https://github.com/VoodooSMBus/VoodooSMBus/releases) : Dùng cho cho các trackpad Elan SMBus

[VoodooI2C](https://github.com/VoodooI2C/VoodooI2C/releases): Dùng cho các trackpad có giao thức I2C

[ECEnabler](https://github.com/1Revenger1/ECEnabler/releases) : Dùng để Enable EC (patch pin là chính)

[BrightnessKeys](https://github.com/acidanthera/BrightnessKeys/releases) : Dùng để patch hotkeys chỉnh độ sáng

[SATA-RAID-unsupported.kext](https://drive.google.com/drive/folders/1r5kISeTwwJ5TmQuk2zzgk14cR5H_V1IG?usp=sharing) : Cho phép chuẩn Raid hoạt động trên mac thay vì ahci

- Dùng cho trường hợp bios không có tuỳ chọn ahci

Mình khuyến khích bạn thêm 5 kext cơ bản sau đây

- Lilu
- VirtualSMC
- WhateverGreen
- Voodoops2 controller
- Usb injectall

1 số máy có thể sẽ cần thêm CpuTscSync

## Thêm các file SSDT

Các bạn chỉ cần mỗi [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-DESKTOP.aml) để boot. nhưng để hoạt động tốt bạn cần nhiều hơn thế nữa, xem chi tiết về SSDT [tại đây](https://heavietnam.ga/2022/03/10/ssdt-recomend/)

## Chỉnh sửa config

- Mở file `config.plist` bằng [ProperTree](https://github.com/corpnewt/ProperTree) 
- Chọn File ⇒ OC Snapshot ![](https://lh6.googleusercontent.com/SCT2Gq_55sCnidkxh8Kn2GjrTrkFc-lj0-5v9yBHi_YUZEK9Zy-OLojhKH-VaT_78oZfvBH8gCl-sdLuwnCyBg5RZv4PLtaeWq81Xlg4oRc9G3AiAXvU21W2A0Q8DdcuLZQSH5W8=s0)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.23.31.png?w=874)

Xem cách xác định phần cứng [tại đây](https://heavietnam.ga/2022/04/29/cach-xac-dinh-phan-cung/) sau đó setting config theo hướng dẫn

### Intel Desktop

| CODE NAME                                                                                          | SERIES       | PHÁT HÀNH |
| -------------------------------------------------------------------------------------------------- | ------------ | --------- |
| [Yonah, Conroe and Penryn](https://heavietnam.ga/2022/06/04/cach-tao-efi-desktop/)                 | E8XXX, Q9XXX | 2006-2009 |
| [Lynnfield and Clarkdale](https://heavietnam.ga/2022/06/06/config-desktop-lynnfield-va-clarkdale/) | 5XX-8XX      | 2010      |
| [Sandy Bridge](https://heavietnam.ga/2022/06/07/config-desktop-sandy-bridge/)                      | 2XXX         | 2011      |
| [Ivy Bridge](https://heavietnam.ga/2022/06/08/config-desktop-ivy-bridge/)                          | 3XXX         | 2012      |
| [Haswell-Broadwell](https://heavietnam.ga/2022/06/08/config-desktop-haswell-va-broadwell/)         | 4XXX-5XXX    | 2013-2014 |
| [Skylake](https://heavietnam.ga/2022/06/09/config-desktop-skylake/)                                | 6XXX         | 2015-2016 |
| [Kaby Lake](https://heavietnam.ga/2022/06/09/config-desktop-kaby-lake/)                            | 7XXX         | 2017      |
| [Coffee Lake](https://heavietnam.ga/2022/06/10/config-desktop-coffee-lake/)                        | 8XXX-9XXX    | 2017-2019 |
| [Comet Lake](https://heavietnam.ga/2022/06/10/config-desktop-comet-lake/)                          | 10XXX        | 2020      |

### Intel Laptop

| CODE NAME                                                                                                         | SERIES     | PHÁT HÀNH |
| ----------------------------------------------------------------------------------------------------------------- | ---------- | --------- |
| [Clarksfield and Arrandale](https://heavietnam.ga/2022/06/13/config-laptop-clarksfield-va-arrandale/)             | 3XX-9XX    | 2010      |
| [Sandy Bridge](https://heavietnam.ga/2022/06/12/config-laptop-sandy-bridge/)                                      | 2XXX       | 2011      |
| [Ivy Bridge](https://heavietnam.ga/2022/06/12/config-laptop-ivy-bridge/)                                          | 3XXX       | 2012      |
| [Haswell](https://heavietnam.ga/2022/06/12/config-laptop-haswell/)                                                | 4XXX       | 2013-2014 |
| [Broadwell](https://heavietnam.ga/2022/06/12/config-laptop-broadwell/)                                            | 5XXX       | 2014-2015 |
| [Skylake](https://heavietnam.ga/2022/06/12/3279/)                                                                 | 6XXX       | 2015-2016 |
| [Kaby Lake and Amber Lake](https://heavietnam.ga/2022/06/12/config-laptop-kabylake/)                              | 7XXX       | 2017      |
| [Coffee Lake and Whiskey Lake](https://heavietnam.ga/2022/06/12/config-laptop-coffee-lake-va-whiskey-lake/)       | 8XXX       | 2017-2018 |
| [Coffee Lake Plus and Comet Lake](https://heavietnam.ga/2022/06/12/config-laptop-coffee-lake-plus-va-comet-lake/) | 9XXX-10XXX | 2019-2020 |
| [Ice Lake](https://heavietnam.ga/2022/06/12/config-laptop-icelake/)                                               | 10XXX      | 2019-2020 |

Một số setting cho laptop:

**HP**:

- Kernel -> Quirks -> LapicKernelPanic -> True
  - Nếu tắt quirk này bạn sẽ bị kernel panic ngay LAPIC
- UEFI -> Quirks -> UnblockFsConnect -> True

**Dell**:

Cho skylake và mới hơn

- Kernel -> Quirk -> CustomSMBIOSGuid -> True
- PlatformInfo -> UpdateSMBIOSMode -> Custom

## Chỉnh cài đặt firmware :

### Disable

- Fast Boot

- Secure Boot

- Serial/COM Port

- Parallel Port

- VT-d
  
  - bạn có thể enable nó nếu bạn set `DisableIoMapper` là YES

- CSM

- Thunderbolt
  
  - Nếu có thunderbolt thì hãy setting cẩn thận mục này xem chi tiết [tại đây](https://heavietnam.ga/2022/02/13/hotplug-thunderbolt-3/)

- Intel SGX

- Intel Platform Trust

- CFG Lock (MSR 0xE2 write protection)
  
  - Nếu là Yonah, Conroe and Penryn thì không cần quan tâm mục này vì chúng không có xcpm
  
  - nó phải được disable nếu bạn không tìm thấy nó có thể enable `AppleCpuPmCfgLock` hoặc `AppleXcpmCfgLock` dưới Kernel -> Quirks xem chi tiết ở mục setting config

### Enable

- VT-x (Virtualization Support)
- Above 4G decoding
  - 2020+ BIOS: khi enabling Above4G, Resizable BAR Support có thể trở nên khả dụng ở Z490 và mainboard mới hơn. chắc rằng Booter -> Quirks -> ResizeAppleGpuBars được set là `0` nếu enable nó.
- Hyper-Threading
- Execute Disable Bit
- EHCI/XHCI Hand-off
- OS type: Windows 8.1/10 UEFI Mode
- DVMT Pre-Allocated(iGPU Memory): 64MB
- SATA Mode: AHCI

Lưu ý : đối với các CPU Pentium hoặc Celeron, nếu các bạn muốn hackinotsh cần phải có card đồ họa rời mới được hỗ trợ vì các iGPU của dòng này đều tạch và bắt buộc phải fake CPUID, xem chi tiết [ở đây](https://heavietnam.ga/2021/09/29/xxiv-fake-cpu-id/)

Lưu ý 2 : Các cpu 11th gen desktop các bạn cần fake cpuid thành gen 10 theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxiv-fake-cpu-id/)

## Check lại config

> Phân này chỉ mang tính chất tham khảo

B1 : Download OpenCore Configurator [tại đây](https://mackie100projects.altervista.org/download-opencore-configurator/)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.51.42.png?w=1024)

B2: Bấm tổ hợp phím Option + C

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.52.31.png?w=1024)

B3: Chọn đời CPU

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.52.54.png?w=1024)

B4: Chọn phiên bản OpenCore

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.53.11.png?w=1024)

B5: Bật Drag and Drop

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.53.45.png?w=1024)

B6: Kéo file config vào ô

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.54.08.png?w=1024)

B7: Bấm `Check`

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-13-at-08.55.25.png?w=1024)

B8: Nhìn vào những mục màu vàng hoặc màu đỏ sau đó check lại các mục theo setting config ở trên

## Issue

Nếu trong quá trình cài đặt có bất cứ lỗi nào bạn có thể tham khảo cách fix lỗi ở đây

- [Boot issue](https://heavietnam.ga/su-co-khoi-dong-opencore/)
  - Các vấn đề gặp phải từ khi khởi động usb cho đến trước khi chọn option boot macos
- [Kernel issue](https://heavietnam.ga/2022/04/09/kernel-issue/)
  - Các vấn đề gặp phải từ khi chọn option boot macos ở picker cho đến khi vào giao diện cài đặt
- [Bigsur issue](https://heavietnam.ga/2022/04/10/1-so-loi-o-xuat-hien-o-bigsur/)
  - Các lỗi đặt trưng ở bigsur
- [Monterey issue](https://heavietnam.ga/2022/04/10/fix-loi-khong-mount-duoc-file-dmg-tren-monterey/)
  - Các lỗi đặt trưng ở monterey
- [Propertree issue](https://heavietnam.ga/2022/04/10/1-so-loi-propertree/)
  - Các vấn đề gặp phải khi sử dụng propertree

**Source tham khảo: [OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/)**
