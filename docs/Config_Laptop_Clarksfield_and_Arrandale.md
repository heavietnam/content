# Config Laptop Clarksfield và Arrandale

## Chuẩn bị

B1: Xác định phần cứng, xem chi tiết [tại đây](https://heavietnam.ga/2022/04/29/cach-xac-dinh-phan-cung/)

B2: Tải python trên Microsoft Store

B3: Tải ProperTree [tại đây](https://github.com/corpnewt/ProperTree) và chạy file ProperTree.bat(Windows) hoặc ProperTree.py(Linux)

B4: Tải GenSMBIOS [tại đây](https://github.com/corpnewt/GenSMBIOS)

## Tiến hành

<details>
    <summary>Support<summyar>
    <p>Min version: OS X 10.4.1 (tiger)</p>
    <p>Max version: Macos 10.13.6 (high sierra)</p>
    <p>Hầu hết laptop Clarksfield và Arrandale không hỗ trợ UEFI</p>
</details>

### ACPI

![acpi-arrendale.6a878ada.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-56-52-acpi-arrendale.6a878ada.png)

- Add: Để inject ACPI qua config
- Delete: Để chặn các `ACPI table`
- Patch: Để add các patch rename xem chi tiết [ở đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/)
- Quirks: 1 vài setting liên quan đến ACPI

> Ở mục này các bạn không cần chỉnh gì nhiều hãy để nó ở mặc định

### Booter

![booter-duetpkg.5a27db69.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-55-36-booter-duetpkg.5a27db69.png)

> Legacy

![aptio-iv-booter-sl.5e63b543.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-55-52-aptio-iv-booter-sl.5e63b543.png)

> UEFI

- MmioWhitelist: xem chi tiết [tại đây](https://heavietnam.ga/2022/01/31/using-devirtualisemmio/)

- Quirks
  
  - Legacy setting
    
    - AvoidRuntimeDefrag: No
      - Bắt buộc enable với bigsur
    - EnableSafeModeSlide: No
    - EnableWriteUnprotector: No
    - ProvideCustomSlide: No
    - RebuildAppleMemoryMap: Yes
      - Bắt buộc khi boot từ 10.4-10.6
    - SetupVirtualMap: No
  
  - UEFI setting
    
    - RebuildAppleMemoryMap: Yes
      - Bắt buộc khi boot từ 10.4-10.6

Chi tiết về Quirks

### DevicesProperties

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/DP-no-igpu.7de6ce5b.png)

Dùng để patch các device xem chi tiết [ở đây](https://heavietnam.ga/2021/09/29/tim-hieu-ve-hackintosh/). Nó còn dùng để patch igpu xem chi tiết [tại đây](https://heavietnam.ga/2021/10/22/patch-gma-gpu/)

### Kernel

![kernel-sandy-usb.8c9e4d73.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-55-06-kernel-sandy-usb.8c9e4d73.png)

- Add: Đây là mục quyết định sự load kext, thứ tự load kext. Thông thường bạn nên để đúng thứ tự load kext của propertree đã sắp xếp. Tuy nhiên với các cpu 32 bit thì bạn có thể tham khảo thông tin sau:

1 số điều cần nhớ về việc load kext

- Emulate: dùng để fake cpuid xem chi tiết [ở đây](https://heavietnam.ga/2021/09/29/xxiv-fake-cpu-id/)
- Force: dùng để force các kext ngoài phân vùng hệ thống
- Block: chỉ định 1 số kext không được phép load
- Patch: để tiến hành patch kernel và kext
- Quirks
  - DisableIoMapper: Yes
    - không cần quirks này nếu VT-D bị disable trong bios
  - LapicKernelPanic: No
    - Main hp thì cần enable quirks này
  - PanicNoKextDump: Yes
    - không bắc buộc từ 10.12+
  - PowerTimeoutKernelPanic: Yes
    - không bắc buộc từ 10.14+

Chi tiết về quirks kernel

- Scheme
  - cài đặt này cho leagcy boot. Phần lớn người dùng có thể bỏ qua tuy nhiên nếu bạn cân tìm hiểu có thể xem thông tin bên dưới

Thông tin chi tiết về Scheme

### Misc

![misc.60f4894d.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-54-51-misc.60f4894d.png)

- Boot: Dùng để tạo gui cho opencore xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xvii-tao-gui/)
- Debug:
  - Bạn sẽ cần thay đổi mọi thứ trừ DisplayDelay
    - Apple Debug: Yes
    - ApplePanic: Yes
    - Target: 67
    - DisableWatchDog: Yes

Thông tin chi tiết về debug

- Security
  - AllowNvramReset: YES
  - AllowSetDefault: Yes
  - BlacklistAppleUpdate: Yes
  - ScanPolicy: 0
  - SecureBootModel: Default
    - Set là `Default` ở bigsur+
    - Set là `Disabled` ở cata-
  - Vault: Optional

Chi tiết về Security

- Tools
  - Được dùng đẻ load các tool cho bạn chức năng snapshot của propertree sẽ giúp load chúng
- Entries
  - Được dùng để xác định irregular boot paths điều mà bạn không thể xác định khi boot một cách tự nhiên với opencore
  - Chi tiết điều này bạn có thể xem ở mục 8.6 trong file [Configuration.pdf](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/Configuration.pdf)

### NVRAM

![nvram.8e75ddde.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-54-37-nvram.8e75ddde.png)

- Add
  - 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14: được dùng cho OpenCore’s UI scaling xem chi tiết ở phần dưới
  - 4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102: được dung cho 1 số bạn cần RTCMemoryFixup xem chi tiết [tại đây](https://heavietnam.ga/2021/10/14/fix-rtc/)
  - 7C436110-AB2A-4BBB-A880-FE41995C9F82: System Integrity  cho system và bảo vệ bitmask

| BOOT-ARGS       | DESCRIPTION                                                                                                                                |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **-v**          | enable verbose giúp bạn có thể xem các big ngay trên màn hình                                                                              |
| **debug=0x100** | Nó giúp disable watchdog trên macos. Giúp không bị reboot khi gặp kernel panic giúp bạn có thể đọc lỗi dễ dàng hơn                         |
| **keepsyms=1**  | Dùng chung với debug=0x100 để giúp bạn có thể dễ dàng đọc các lỗi kernel panic                                                             |
| **alcid=1**     | dùng để fix audio bằng apple alc xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/) |

- **Boot arg đặc biệt cho gpu**

| BOOT-ARGS          | DESCRIPTION                                                                                                                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **agdpmod=pikera** | Sử dụng để tắt board ID checks trên Navi GPUs (RX 5000 & 6000 series) nếu không sử dụng bạn sẽ nhận được 1 màn hình đen và chẳng có gì khác ngoài nó<br>Không sử dụng nó nếu gpu của bạn không phải Navi GPU |
| **nvda_drv_vrl=1** | Để enable web driver trên highsierra. Nhưng 1 sự thật đáng buồn là ở thời điểm hiện tại nvidia đã bị tước chứng chỉ                                                                                          |
| **-wegnoegpu**     | dùng để disable tất cả các gpu trừ igpu xem chi tiết các disable gpu [tại](https://heavietnam.ga/2022/06/13/disable-dgpu-laptop/) [đây](https://heavietnam.ga/2022/06/13/disable-dgpu-laptop/)               |

- csr-active-config: 00000000
  - Dùng để điều khiến sip chi tiết về cách tắt sip xem ở đây
    - 00000000: sip enable
    - 03000000: Disable kext signing (0x1) và filesystem protections (0x2)
    - FF030000: Disable hoàn toàn sip ở highsierra
    - FF070000: Disable hoàn toàn sip ở mojave và catalina
    - FF0F0000: Disable hoàn toàn sip ở bigsur
- run-efi-updater: No
  - Dùng để ngăn cả các Apple’s firmware update packages tôi đã nói rõ ở phần trên
- prev-lang:kbd: en-US:0
  - Mặc định là tiếng nga hãy chuyển nó về string formart để có thể sử dụng tiếng anh
  - Hoặc convert en-US:0 sang hex (ASCII to Hex)

Thông tin chi tiết về 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14

- Delete
  - WriteFlash: yes

Thông tin chi tiết về Delete Nvram

### PlatformInfo

![smbios.51b5b579.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-54-13-smbios.51b5b579.png)

Các bạn sẽ tiến hành dùng tool Gen Smbios đã tải ở trên để inject thông tin

Mở terminal và chạy lệnh sau

```
git clone https://github.com/corpnewt/GenSMBIOS && cd GenSMBIOS && chmod +x GenSMBIOS.command && ./*.command
```

Ấn 1 để  download MacSerial và ấn 3 để generate SMBios chọn SMbios theo bản sau

| SMBIOS        | CPU TYPE                | DISPLAY SIZE |
| ------------- | ----------------------- | ------------ |
| MacBookPro6,1 | Quad Core 45W(High End) | 17″          |
| MacBookPro6,2 | Quad Core 45W(Low End)  | 15″          |

Các mục tương ứng với gen Smbios trong config

- SystemProductName: tên SMbios
- SystemSerialNumber: Serial
- MLB: Board Serial
- SystemUUID: SmUUID
- Rom bạn có thể sử dụng rom apple hoặc rom của bạn xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xix-fix-imessage/)

Bạn cũng nên check serial mình dump được ở trang [Apple Check Coverage page](https://checkcoverage.apple.com/) xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xix-fix-imessage/)

- Automatic: YES
  - Tạo PlatformInfo dựa trên Generic thay vì  DataHub, NVRAM, and SMBIOS

Thông tin chi tiết về Generic

### UEFI

![uefi-legacy-laptop.02008210.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-10-53-54-uefi-legacy-laptop.02008210.png)

- ConnectDrivers: YES
  - sẽ làm cho các driver tự động load
- Drivers
  - Đây là nơi load các driver
  - những driver khuyến khích load đối với cpu này là
    - HfsPlusLegacy.efi
    - OpenRuntime.efi
    - OpenUsbKbDxe.efi (chỉ load nếu bạn có uefi main)
- APFS
  - Theo mặc định opencore chỉ load apfs driver từ macos bigsur+ vđối với các verrsion cũ hơn bạn sẽ cần phải đặt giá trị mindate và minversion để cho phép apfs driver được load

| MACOS VERSION           | MIN VERSION        | MIN DATE   |
| ----------------------- | ------------------ | ---------- |
| High Sierra (`10.13.6`) | `748077008000000`  | `20180621` |
| Mojave (`10.14.6`)      | `945275007000000`  | `20190820` |
| Catalina (`10.15.4`)    | `1412101001000000` | `20200306` |
| No restriction          | `-1`               | `-1`       |

- Audio
  - Hãy để nó mặc định nó không pahỉ là âm thanh của opencore, liên quan đến AudioDxe settings tạo âm thanh khi boot lên picker (tôi thấy nó không cần thiết)
- Input
  - Nó liên quan đến việc sử dụng keyboard và các thiết bị input ở giao diện picker opencore. Nó còn ảnh hướng tới FileVault và Hotkey support
  - KeySupport: No
    - Nếu mainboards của bạn có hỗ trợ uefi thì hãy enable nó lên
- Output
  - liên quan đến OpenCore’s visual output hãy để nó mặc định chúng ta sẽ không sài tới nó
- ProtocolOverrides
  - liên qua đến máy ảo, máy cũ và FileVault
- Quirks
  - Đây là nơi tập trung những quirks liên quan đến UEFI bạn sẽ cần chỉnh 1 số thứ như sau
  - IgnoreInvalidFlexRatio: Yes
  - ReleaseUsbOwnership: Yes
  - UnblockFsConnect: No
    - Nếu là main hp hãy enable nó lên

Thông tin chi tiết về quirks

- ReservedMemory
  - Đây là 1 phần khá thụ vị bạn sẽ cần dùng nó nếu sử dụng Sandy Bridge. Nó giúp ngăn load 1 số vùng memory nhất định ở macos giúp khắc phục tương đối faulty memory trên các igpu Sandy Bridge (tương lai chúng mình sẽ có 1 post thú vị về nó)
