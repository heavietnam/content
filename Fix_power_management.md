# Fix power management

> Patch power manager là 1 patch khá quan trọng với laptop, pc có thể không patch. Nó giúp các mẫu laptop có thể tiết kiện điện năng, không quá nóng và đặc biệt là nó còn giúp fix sleep. Vậy patch power manager làm như thế nào?

## Gen 4+

### Chuẩn bị:

B1: Tải [SSDT-plug](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) và bỏ nó vào `EFI ==> OC ==> ACPI` (hoặc `EFI ==> Clover ==> ACPI ==> patched`)

B2: tải kext [cpu friend](https://github.com/acidanthera/CPUFriend/releases)

B3: tải tool [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend)

### Tiến hành

B1: chạy tool CPUFriendFriend lên

![](https://i.imgur.com/x93f06O.png)

B2: ta có mục `Low Frequency Mode` tức là xung thấp nhất của cpu khi máy ở chế độ tĩnh (không là việc) có 2 cách để tính LFM

- Tra cứu thông tin cpu
  
  - B1: các bạn sẽ mở hackintool vào mục `system ==> system info ==> CPU` để tìm mã cpu
  - B2: search mã đó và vào trang intel ark
  - B3: nhìn vào mục `Tần số TDP-down có thể cấu hình`  hoặc `TDP-down Frequency`![](https://i.imgur.com/mXXkWBp.png)
  - B4: các bạn sẽ tiến hành chuyển nó về dạng MHz bằng công thức sau
    - `TDP-down*1000=1.7*1000=1700`
  - B5: bạn sẽ chuyển đổi các chứ số hàng trăm thành hex tức chuyển `17` thành `hex   ![](https://i.imgur.com/JPfDCqf.png)`
  - B6: ta sẽ tiến hành nhập `11` vào terminal

- Manual
  
  - B1: các bạn sẽ mở hackintool vào mục `system ==> system info ==> CPU` để tìm tần số xung tối đa của CPU  
    ![](https://i.imgur.com/I4aI0MF.png)
  - Ở đây ta có xung tối đa của CPU này là `1,8 GHz`. Để tính `LFM` ta sẽ tiến hành chia 2 con số này ra (hoặc bao nhiêu tùy thích nhưng mình khuyên nên chia 2) và tính theo công thức sau
    - `MAX/2*1000 = 1,8/2*1000=900 MHz` 
  - B2: ta sẽ đổi LFM vừa tìm được  ra hex nhưng không phải đổi con số `900 MHz` mà ta sẽ đổi từ hành trăm tức là đổi số `9`  
    ![](https://i.imgur.com/3ij8Mhr.png)
  - Ta được `0x09` là giá trị hex của `LFM`. Ở đây ta sẽ tiên hành bỏ `0x` đi và nhập giá trị vào terminal

- Tra cứu theo danh sách LFM sau
  
  - Laptop gen 5+ chọn LFM là 08
  - Desktop gen 5+ chọn LFM là 0A
  - Haswell/Broadwell HEDT/Server (tức X99) chọn LFM là 0D
  - Skylake+ HEDT/Server chọn LFM là 0C

Đến đây nếu các bạn là SMbios Broadwell- thì trực tiếp tới bước 6 nếu là SMbios Skylake+ thì tiếp tục bước 3

B3: Tính toán EPP. Khả năng tăng tốc xung CPU (Energy Performance Preference). Các bạn chọn theo bảng sau

- `0x00-0x3F`: Hiệu suất tối đa
- `0x40-0x7F`: Hiệu suất cân bằng
- `0x80-0xBF`: Công suất cân bằng
- `0xC0-0xFF`: Tiết kiêm điện năng tối đa

Giải thích 1 chút là với `00` nó sẽ ép cpu của bạn tăng tốc càng nhanh càn tốt còn với `FF` thì nó sẽ tăng tốc 1 cách từ từ

B4: chọn `Performance Bias` tức là hiệu suất tổng thể của CPU. Ở mục này các bạn cần dựa cào trải nghiệm để lựa chọn giá trị phù hợp nhất

B5: `Enable features` các bạn gõ y

![](https://i.imgur.com/sZIEPJr.png)

B6: Các bạn sẽ dump được 1 folder `Results`. Ở folder này các bạn sẽ chú ý các mục

- SSDT-data
- CPUFriendDataProvider.kext

Ta có thể sử dụng chỉ SSDT-data hoặc sử dụng CPUFriendDataProvider.kext + SSDT-plug

B7: bỏ kext [CPUFriend](https://github.com/acidanthera/CPUFriend) vào EFI ==> OC ==> kext (hoặc EFI ==> Clover ==> kext)

B8: snaps nếu opencore và reboot

## Gen 3-

### Cách 1: Enable XCPM (gen 3 only)

B1: chỉnh sử config Root ⇒ Kernel ⇒ Quirks enable những mục sau

- AppleCpuPmCfgLock
- AppleXcpmCfgLock
- AppleXcpmExtraMsrs

> Đối với gen 3 đang dùng các smbios có hỗ trợ xcpm (tức smbios haswell) thì bạn chỉ cần enable quirks và không cần add patch

B2: Tải file patched về [tại đây](https://drive.google.com/file/d/1xjAcZhTHY83WMuJoPvpQuJWb7RVgyL2J/view?usp=sharing)

![](https://i.imgur.com/hCuMywE.png)

B3: các bạn sẽ copy mục 0 của file patched vào config của các bạn

![](https://lh5.googleusercontent.com/IDHWdoDZrGbnoX9aJ-VLtDdwKs5-Nx4p8LQnYfB7edahL8CbEk6pRuNR4Z8l9J3WbuJ3ps5lt3HWa5Eih8Rh5YB0mKazqW0liEzlGfsTvCCJQl9RyGjth1WuDF4loVVWLob53hsf=s0)

Tới đây thì các bạn sẽ có thể patch như gen 4+ hoặc dừng SSDTPrgen với lệnh sau

```
sudo [kéo SSDTPrgen vào] -x 1
```

Lưu ý: đối với clover các bạn chỉ cần enable `Kernel XCPM` lên là được còn lại thì tương tự OC

![](https://i.imgur.com/8RTMrlc.png)

### Cách 2: **ssdtPRgen** (for both gen 2 and gen 3)

B1: Các bạn chỉnh `CpuPm` và `Cpu0Ist` ở trong `ACPI -> Delete` như sau

| KEY            | TYPE    | VALUE            |
| -------------- | ------- | ---------------- |
| All            | Boolean | YES              |
| Comment        | String  | Drop CpuPm       |
| Enabled        | Boolean | YES              |
| OemTableId     | Data    | 437075506d000000 |
| TableLength    | Number  | 0                |
| TableSignature | Data    | 53534454         |

| KEY            | TYPE    | VALUE            |
| -------------- | ------- | ---------------- |
| All            | Boolean | YES              |
| Comment        | String  | Drop Cpu0Ist     |
| Enabled        | Boolean | YES              |
| OemTableId     | Data    | 4370753049737400 |
| TableLength    | Number  | 0                |
| TableSignature | Data    | 53534454         |

B2: save reboot

B3: tải [ssdtPRgen](https://github.com/Piker-Alpha/ssdtPRGen.sh)

B4: kéo file ssdtPRGen.sh vào terminal và nhấn enter

![](https://i.imgur.com/5y4Q1oe.png)

Sau khi chạy xong bạn sẽ nhận được file SSDT.aml tại đường dẫn `/Users/your-name>/Library/ssdtPRGen/`

B5: copy nó vào `EFI ==> OC ==> ACPI` (hoặc `EFI ==> Clover ==> ACPI ==> Patched`)

B7: rename nó thành `SSDT-PM` và snapshot (nếu OC)

B8: save và reboot

## Check Power manager

### Gen 4+

Hoặc với các máy gen 3 đã enable `XCPM` thì hoàn toàn có thể check theo cách này

B1: Tải [ioreg](https://github.com/khronokernel/IORegistryClone/blob/master/ioreg-302.zip) về

B2: Mở ioreg lên và tìm kiếm `AppleACPICPU`

![](https://i.imgur.com/3CkBHAk.png)

B3: tiến hành tắt tìm kiếm để hiện đầy đủ thông tin

![](https://i.imgur.com/IImN2UL.png)

Ở đây nếu có X86PlatformPlugin trong tree tức là power manager của bạn đã hoạt đông nếu nó không có như ảnh dưới thì có nghĩa là chưa hoạt động

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/pm-not-working.2706013b.png)

### Gen 3-

Đối với các CPU gen 3- thì apple đã loại bỏ xcpm ra khỏi chúng ở sierra (gen 3 vẫn có thể enable lên được) nên chúng ta sẽ cần dùng cách sau đây

B1: tải [Intel Power Gadget](https://www.intel.com/content/www/us/en/developer/articles/tool/power-gadget.html)

B2: mở lên và chú ý vào mục `MAX`

![](https://i.imgur.com/2V6OrkK.png)

B3: mở hackintool lên chú ý vào phần `system ==> system info ==> CPU`

![](https://i.imgur.com/JdKYcnq.png)

B4: check xung ở cả 2 phần xem có bằng nhau không nếu bằng thì bạn đã patch power manager thành công nếu không bằng thì bạn hãy patch lại nhé

Ngoài ra với gen 3 enable xcpm để chắc chắn bạn hãy check `method int` của `SSDT-PM`

![](https://i.imgur.com/Kw6zggx.png)

## Lưu ý

### X99

> XCPM căn bản không được hỗ trợ cho Haswell-E và Broadwell-E vì vậy chúng ta cần fake cpu id

- Haswell-E
  
  - `Kernel -> Emulate`:
    - Cpuid1Data: `C3060300 00000000 00000000 00000000`
    - Cpuid1Mask: `FFFFFFFF 00000000 00000000 00000000`

- Broadwell-E:
  
  - `Kernel -> Emulate`:
    - Cpuid1Data: `D4060300 00000000 00000000 00000000`
    - Cpuid1Mask: `FFFFFFFF 00000000 00000000 00000000`

### Gen 3-

Ngoài phần max bạn cũng nên check những phần khác đối chiếu chúng với windows để chắc chắn rằng mọi đã hoạt động

**Source tham khảo: [[GUIDE] X86PlatformPlugin (XCPM) für Ivy Bridge CPUs unter Catalina und Big Sur aktivieren – Anleitungen und Builds – Hackintosh-Forum – Deine Anlaufstelle für Hackintosh & mehr…](https://www.hackintosh-forum.de/forum/thread/53009-guide-x86platformplugin-xcpm-f%C3%BCr-ivy-bridge-cpus-unter-catalina-und-big-sur-akti/) | [Optimizing Power Management | OpenCore Post-Install (dortania.github.io)](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#sandy-and-ivy-bridge-power-management)**
