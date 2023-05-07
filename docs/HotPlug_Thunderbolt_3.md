# HotPlug Thunderbolt 3

## Tìm hiểu chung

> Patch Thunderbolt là 1 việc khá vất vả bạn nên tìm hiểu việc Patch DSDT trước khi đọc guide này ([Patch DSDT Phần 1](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/), [Patch DSDT Phần 2](https://heavietnam.ga/2021/09/29/xxvii-patch-dsdt-phan-2/), [Patch DSDT Phần 3](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/)).

> Trước tiên chúng ta hãy cùng nhau tìm hiểu sự hoạt động của Thunderbolt trên Windows và macOS.

> Ở trên Windows cổng Thunderbolt được cấp nguồn khi có 1 thiết bị được cắm vào cổng Thunderbolt. Khi bị ngắt kết nối thì nguồn điện cũng tự ngắt khỏi cổng Thunderbolt.

> Mặt khác ở macOS Thunderbolt luôn được cấp nguồn khi được kết nối thì nó sẽ được kết nối vào Thunderbolt cho đến khi tắt máy.

> sNhưng đối với Hackintosh nó là sự hỗ hợp của Windows và macOS do đó bạn chỉ có thể sử dụng Thunderbolt nếu bạn cắm nó trước khi khởi động do đó chúng ta cần hotplug.

## Setting BIOS

### ***Lưu ý:***

Nhưng giá trị mặc định tức là giữ nguyên sau khi bạn đặt lại BIOS.

Nếu dòng máy của bạn không có những Option trong đây thì hãy để mặc định.

Nếu bạn đang dùng Thunderbolt cards or chips with flashed firmware thì disable `Thunderbolt USB Support` hoặc không tìm thấy option này thì disable `Thunderbolt Boot Support`.

Nếu máy không có tùy chọn GPIO Force Pwr thì hãy xem cách enable nó ở phần dưới.

### ***Desktop***

#### General:

- Discrete Thunderbolt(TM) Support: Enabled.
- TBT Vt-d base security: Disable.
- Thunderbolt Boot Support: Enabled / Boot Once.
- GPIO Force Pwr: Enable.
- Skip PCI OptionRom: Enable.
- Wake from Thunderbolt(TM) Devices: Enable.
- Security Level: SL0-No Security.
- DTBT Controller 0: Enable.
- TBT Host Router: Two port.
- Extra Bus Reserved: Mặc định.
- Reserved Memory: Mặc định.
- Reserved PMemory: Mặc định.
- Reserved I/O: Mặc định.

#### Asus 600 series:

- PCIE Tunneling Over USB4: Disable.
- Discrete Thunderbolt(TM) Support: Enable.
- Wake from Thunderbolt(TM) Devices: Enable.
- Thunderbolt Boot Support: Enable.
- DTBT Go2Sx Command: Enable.
- Windows 10 Thunderbolt support: Enable.
- DTBT Controller 0: Enable.
- TBT Host Router: Enable.
- Extra Bus Reserved: Mặc định.
- Reserved Memory: Mặc định.
- Memory Alignment: Mặc định.
- Reserved PMemory: Mặc định.
- PMemory Alignment: Mặc định.
- Reserved I/O: Mặc định.

#### MSI:

- Discrete Thunderbolt(TM) Support: Enable.
- Wake from Thunderbolt(TM) Devices: Enable.
- Current Security Level: Enable.
- Native OS Security for TBT: Enable.
- Thunderbolt USB Support: Enable.
- Thunderbolt Boot Support: Enable.
- Titan Ridge Workaround for OSUP: Enable.
- Tbt Dynamic AC/DC L1: Enable.
- GPIO Force Pwr: Enable.
- Wait time in ms after applying Force Pwr: Mặc định.
- GPIO filter: Enable.
- DTBT Controller 0: Enable.
- TBT Host Router: two port
- Extra Bus Reserved: Mặc định.
- Reserved Memory: Mặc định.
- Memory Alignment: Mặc định.
- Reserved PMemory: Mặc định.
- PMemory Alignment: Mặc định.
- Reserved I/O: Mặc định.
- Windows 10 Thunderbolt support: Enable + RTD3

### ***Laptop***

#### ASUS/Clevo:

- Intel Thunderbolt Technology: Enable.
- Security:
  - Unique ID: Trên Laptop đời mới.
  - Normal Mode w/o NHI: trên lap cũ hơn như Haswell.
- Thunderbolt Boot Support: Enable.
- Security Level: SL0-No Security.
- Extra Bus Reserved: Mặc định.
- Reserved Memory: Mặc định.
- Reserved PMemory: Mặc định.
- PMemory Alignment: Mặc định.
- Reserved I/O: Mặc định.

#### General:

- Intel Thunderbolt Technology: Enable.

- Discrete Thunderbolt(TM) Support: Enable.

- TBT Vt-d Base Security: disable

- Thunderbolt Boot Support: disable

- Wake from Thunderbolt(TM) Devices: Enable.

- Security Level:
  
  - No Security: Đối với hầu hết các loại máy.
  
  - SL0-No Security: Giống như tùy chọn No Security sử dụng khi không có tùy chọn No Security.
  
  - Legacy Mode: Thiết bị Legacy.
  
  - Unique ID: Chỉ áp dụng đối với laptop.
  
  - One time saved Key: One time saved Key.
  
  - DP++ only: DP++ only.
  
  - Normal Mode w/o NHI: Ở trên 1 số laptop cũ như haswell sử dụng tùy chọn này thay cho debug mode.
  
  - Debug Mode: Ở trên 1 số laptop cũ như haswell sử dụng tùy chọn `Normal Mode w/o NH` thay cho tùy chọn này.

- Thunderbolt Usb Support: Enable/ Disable.

- GPIO Force Pwr: Enable.

- DTBT Controller 0: Enable.

- TBT Host Router: Mặc định.

- Extra Bus Reserved: Mặc định.

- Reserved Memory: Mặc định.

- Memory Alignment: Mặc định.

- Reserved PMemory: Mặc định.

- PMemory Alignment: Mặc định.

- Reserved I/O: Mặc định.

- TBT Root port Selector:
  
  - Auto Detect: Tự động chọn cổng Thunderbolt.
  - Thunderbolt USB: Nếu Thunderbolt USB hoạt động thì nên chọn tùy chọn này.

- Thunderbolt(TM) PCI Cache-line size: Mặc định.

- Wait time in ms after applying Force Pwr: Mặc định.

- Skip PCI OptionRom: enable

- Reserve memory per phy slot: Mặc định.

- Reserve P memory per phy slot: Mặc định.

- Reserve IO per phy slot: Mặc định.

- Delay before SX Exit: Mặc định.

- GPIO Filter: Enable.

- Enable CLK REQ: Disable.

- Enable ASPM: Disable.

- Enable LTR: Disable.

- Alpine Ridge XHCi WA: Enable.

- AR XHCI Host Pre-Wake: Enable.

- AR XHCI Host Active LTR: Mặc định.

- AR XHCI Host High LTR: Mặc định.

- AR XHCI Host Medium LTR: Mặc định.

- AR XHCI Host Low LTR: Mặc định.

## Enable GPIO Force Pwr

### Cách 1: Dùng UEFITool

B1: Tải UEFITool [tại đây](https://github.com/LongSoft/UEFITool/releases) Không tải version Axx

B2: Tải file BIOS của các bạn ( định dạng .bin ).

B3: Mở file BIOS ra bằng UEFITool và nhấn Command + F sau đó search `Force Pwr` (định dạng là unicode).

B4: Bạn sẽ nhận được kết quả tìm kiếm ở mục có subtext là `DXE driver` text là `setup`.

B5: Nhấn extra body.

![/](https://www.tonymacx86.com/attachments/screen-shot-2020-06-29-at-13-01-53-png.478421/)

B6: Tải [Universal-IFR-Extractor](https://github.com/LongSoft/Universal-IFR-Extractor/releases/) và run ở trên Terminal (các run tham khảo [XXXVIII. Disable CFG-Clock – Heavietnam February 2022](https://heavietnam.ga/2021/10/28/xxxviii-disable-cfg-clock/#Cach_1_Disable_trong_config)).

![/](https://www.tonymacx86.com/attachments/screen-shot-2020-06-29-at-13-07-03-png.478422/)B7: Mở file text vừa dump ra và search `GPIO3 Force Pwr` (mặc định là disable | hoặc search `GPIO Force Pwr`).

B8: Nhìn phía trên dòng tìm được:

![/](https://www.tonymacx86.com/attachments/screen-shot-2020-06-29-at-13-12-03-png.478423/)

B9: Copy `12 06 25 2B 00 00` và thay thế bằng `12 06 25 2B FF 00`.

B10: Sử dụng hex editor để thay thế (ấn replace on).

![/](https://www.tonymacx86.com/attachments/screen-shot-2020-06-29-at-13-15-32-png.478424/)

B11 Save file lại.

B12: Mở lại file bin ban đầu và ấn `replace body` sau đó chọn đến file vừa sửa và save lại tiếp theo Reboot.

![/](https://www.tonymacx86.com/attachments/screen-shot-2020-06-29-at-13-17-01-png.478425/)

### Cách 2: Dùng Driver TbtForcePower.efi

B1: Tải [TbtForcePower.efi](https://github.com/al3xtjames/ThunderboltPkg).

B2: Bỏ file driver vào `EFI ==> OC ==> driver` hoặc `EFI ==> Clover ==> driver ==> UEFI`.

B3: Snapshot lại nếu ở OpenCore.

B4: Reboot.

##### Lưu ý:

Đối với 1 số PC thì khi dùng cách 2 sẽ bị Panic bạn phải dùng cách 1.

## Hotplug Thunderbolt 3

### Xác định ACPI Path

#### Cách 1: dùng IOReg

B1: Tải IOReg [tại đây](https://github.com/vulgo/IORegistryExplorer).

B2: Cắm Thunderbolt (cắm trước khi mở máy).

B3: Nhấn Command + F và search Thunderbolt.

![/](https://elitemacx86.com/attachments/screenshot-2020-01-28-at-5-52-53-am-png.2047/)Ta có thể thấy đường dẫn dẫn tới thunderbolt sẽ là `PCI0.RP21.PXSX`

#### Cách 2: Dùng Device Manager

B1: Chọn Option Thunderbolt

B2: Chọn vào mục `Details ==> Location Paths`.

B3: Lấy đường dẫn (chi tiết về cách lấy xem ở bài [này](https://heavietnam.ga/2021/09/29/xxiii-patch-card-doi-hoa-amd/)).

#### Lưu ý cho **Multiple Cards**:

Đối với những máy có nhiều Card Thunderbolt thì bạn sẽ xác định đường dẫn card bằng cách cắm vào từng port trước khi khởi động.

Ngoài ra để sử dụng Type-C bạn sẽ cần 1 cầu nối trong SSDT Thunderbolt và thường cầu nói đó sẽ là `PEGP`.

```
// ví dụ chúng ta có các đường dẫn là 

PCI0.RP21.PXSX
PCI0.BR1A.SL01
PCI0.BR3A.SL09

// khi thêm cầu nối sẽ là 

PCI0.RP21.PEGP
PCI0.BR1A.PEGP
PCI0.BR3A.PEGP
```

### Lấy mẫu SSDT từ [HackinDrom](https://hackindrom.zapto.org/)

B1: Các bản tải SSDT từ [HackinDrom](https://hackindrom.zapto.org/).

B2: Các bạn chọn 1 mẫu Thunderbolt Device phù hợp với cấu hình của bạn.

B3: Đối với những bạn sử dụng Multiple Cards thì hãy chọn vào mục Custom và và định dạng Busid là 0 hoặc 1. Số 0 hoặc 1 chính là Thunderbolt Card thứ nhất của bạn. Ví dụ bạn có 3 Card Thunderbolt nếu card đầu tiên có Busid là 0 thì các card tiếp theo sẽ là 1 và 2. Ngược lại thì Busid của card đầu tiên là 0 thì 2 Vard tiếp theo tương ứng sẽ là 2 và 3.

B4: Ấn COMPILE.

B5: Ấn Download.

### Thay đổi đường dẫn của SSDT Thunderbolt

B1: Tìm đường dẫn mặc định của SSDT.

B2: Ấn Replace.

B3: Rồi thay bằng đường dẫn bạn đã tìm được ở phần trên ví dụ như thay `RP05 to RP21`.![](https://imgur.com/9LKnktd.png)

#### Lưu ý cho Lưu ý cho **Multiple Cards**:

Như đã nói ở trên đối với Multiple Cards thì ta sẽ cần dùng cầu nối nên ta vẫn cần sử dụng Method gốc.

B1: Thêm cầu nối vào đường dẫn:

![/](https://imgur.com/aR0JH68.png)B2: Add Method gốc (paste trước dòng method pegp)

```
Scope (PXSX)
            {
                Name (_STA, Zero)  // _STA: Status
            }
// thay method thích hợp
```

B3: Khai báo method vừa Add:

```
External (_SB_.PCI0.RP21.PXSX, DeviceObj)   
```

Sau khi hoàn thành sẽ được  
![/](https://imgur.com/zvkylQQ.png)

## Lưu ý cuối bài

Các bạn đừng quên cho SSDT vào EFI ==> OC ==> ACPI hoặc EFI ==> Clover ==> ACPI (nhớ Snapshot nếu ở OpenCore)

Đối với Multiple Cards. Bạn nên thử bus id cho card đầu tiên là busid 0 và thử tiếp sau đó.

Để hotplug cho Alpine Ridge Cards thì bạn phải disable THB_C Header.

Ngoài ra khi patch Multiple Cards có thể sẽ không hiện trong About This Mac đó là vì macOS chưa nhận dạng được chipset của Thunderbolt. Nhưng cũng không cần quan tâm vì mọi chức năng đều hoạt động (1 số chức năng không chạy như kết nối màn 5k)

**Source tham khảo: [GUIDE – How to Enable Thunderbolt 3 Hotplug on macOS | EliteMacx86 Forum](https://elitemacx86.com/threads/how-to-enable-thunderbolt-3-hotplug-on-macos.462/) | [GUIDE – How to Enable ThunderBolt 2, Thunderbolt 3 and Thunderbolt 4 on macOS | EliteMacx86 Forum](https://elitemacx86.com/threads/how-to-enable-thunderbolt-2-thunderbolt-3-and-thunderbolt-4-on-macos.461/) | [HowTo: Real Thunderbolt HotPlug/HotSwap on the fly | Page 3 | tonymacx86.com](https://www.tonymacx86.com/threads/howto-real-thunderbolt-hotplug-hotswap-on-the-fly.282953/page-3)**
