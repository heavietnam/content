# [EB|#LOG:EXITBS:START]

Phần này sẽ được chia làm 3 phần:

- Vấn đề về booter
- Vấn đề về Patch Kernel
- Vấn đề về UEFI
- Vấn đề về máy ảo

## **Lỗi liên quan đến booster**

Những nguyên nhân chính gây ra lỗi này là:

### **DevirtualiseMmio**

- Một vài vùng MMIO vẫn bắt buộc phải có để hoạt động bình thường, vì vậy bạn cần phải loại những vùng này ở trong Booter -> MMIOWhitelist hoặc là bạn phải tắt cái dòng config này luôn. Các bạn có thể xem ở đây: [Using Devirtualise MMIO](https://heavietnam.ga/2022/01/31/using-devirtualisemmio/)
  
  - Đối với những bạn dùng mainboard chipset TRx40 của AMD thì hãy bật cái dòng config này
  
  - Đối với những bạn dùng Mainboard chipset X99 thì hãy tắt cái dòng config này vì nó có thể vô tác dụng đối với một số firmware

### **SetupVirtualMap**

- Dòng config này là bắt buộc phỉa bật đối với hầu hết các firrmware, nếu không có nó thì máy các bạn sẽ dễ bị dính lỗi kernel pannic ở đây, vì vậy các bạn nên bật nó nếu như các bạn vẫn chưa bật nó trong config

- Những máy nào có chipset Z390 hoặc cũ hơn thì bắt buộc phải bật cái này

- **Tuy nhiên**, một số firmware(chủ yếu là những firmware từ năm 2020 trở đi) sẽ không tương thích với cái setting này và làm cho máy bạn bị kernel pannic chảng hạn như:
  
  - CPU Intel dòng Icelake
  
  - Chipset Intel dòng Commet Lake(B460, H470, Z490, vv)
  
  - Mainboard AMD chipset B550 ,A520(bao gồm cả chipset X570 với bios bản mới nhất)
  
  - Một số mainboard AMD chipset B450 và X470 cũng giống với những mainboard ở trên khi cập nhật bios lên bản mới nhất
  
  - Mainboard chipset TRx40 của AMD
  
  - Các máy ảo chẳng hạn như QEMU
  
  - Mainboard chipset X299 với bios từ năm 2020 trở đi (cái này cũng áp dụng cho những mainboard X299 với bản BIOS từ cuối năm 2019 và sau từ năm 2020 trở đi)

### **EnableWriteUnprotector**

    Một vấn đề khác chính là macOS có thể đang bị xung đột với lại chức năng chống ghi từ CR0, để fix cái này chũng ta có 2 lựa chọn:

- Nếu frmware của máy các ban hỗ trợ MATs (firmware tù năm 2018 trở đi)
  - EnableWriteUnprotector -> False
  - RebuildAppleMemoryMap -> True
  - SyncRuntimePermissions -> True
- Đối với những máy có firmware cũ hơn:
  - EnableWriteUnprotector -> True
  - RebuildAppleMemoryMap -> False
  - SyncRuntimePermissions -> True

Lưu ý: Một vài laptop (ví dụ như Dell Inspiron 5370) mặc dù có hỗ trợ MATs nhưng chũng vẫn bị lỗi khi boot lên, đối với nhũng máy như thế này các bạn có 2 lựa chọn:

- Boot máy với setting của máy có firrmware cũ hơn (*Đối với những máy có firmware cũ hơn)

- Bật DevirtualiseMmio và làm theo [Using DevirtualiseMmio](https://heavietnam.ga/2022/01/31/using-devirtualisemmio/)

Về vấn đề hỗ trợ MATs những firmware chống lại EDK 2018đã hỗ trợ MATs và nhều OEM đã support nó từ thời laptop Skylake(2016) vấn đề chỉ là chũng ta không rx liệu các OEM đã cập nhật firrmware đề hỗ trợ tính năng này hay chưa. Để kiểm tra xem amsy các bạn có hỗ tợ MATS hay không, các bạn có thể dùng tính năng lấy log của OpenCore   

`OCABC: MAT support is 1`

*Lưu ý:  có nghĩa là firrmware của máy các bạn hỗ trợ MAT còn 0 có nghĩ là nó không hỗ trợ

## **Lỗi liên quan đến** **Patch Kernel**

Lỗi liên quan đến cái này sẽ được phân ra làm 2 giữa Intel và AMD

### AMD

Thiếu các [patch kernel](https://github.com/AMD-OSX/AMD_Vanilla) (chỉ áp dụng cho CPU AMD, hãy đảm bảo là chũng là nhũng patch OpenCore chứ không phải Clover). Clover sử dụng MatchOS trong khi OpenCore dùng MinKernel and Maxkernel)

**Lưu ý là những bản patch cũng gây ra những lỗi tương tự nên các bạn hãy đảm bảo rằng các bạn đang dùng bản patch mới nhất cho AMD OS X**

### Intel

- AppleXcpmCfgLock và AppleCpuPmCfgLock
- Nếu thiếu patch CFG hoặc XCPM thì các bạn hãy bật AppleXcpmCfgLock và AppleCpuPmCfgLock trong config
  - Các máy dùng CPU Haswell và mới hơn chỉ cần AppleXcpmCfgLock    
  - Các máy dùng CPU Ivy Bridge và cũ hơn chỉ cần AppleCpuPmCfgLock
  - Các máy dùng CPU Broadwell và cũ hơn cần phải bật AppleCpuPmCfgLock khi các bạn chạy OSX 10.10 trở xuống

**Ngoài ra các bạn co thể tắt hoàn toàn CFG-Lock: Fixing CFG Lock**

AppleXcpmExtraMsrs Có thể bắt buộc, seting này thường chỉ dành cho các CPU như Pentinum, HEDT và một số các máy khác có CPU không được hỗ trợ chĩnh thức trong macOS

- Các CPU Intel đời rất cũ:
  - Đối với macOS Big Sur, nhiều firmware gặp vấn đề trong việc các định số nhân CPU và khi đó sẽ gặp kernel pannic sớm nên không thể in lỗi này lên màn hình . Thông qua serial, các bạn có thể thấy nó báo như sau: `max_cpus_from_firmware not yet initialized`
  - Cách giải quyết:
    - Bật AvoidRuntimeDefrag ở trong Booter -> Quirks
    - Cách này sẽ hoạt động đối với hầu hết các firmware
    - Trong trường hợp các bạn đã làm cách trên những nó vẫn không fix được thfi các bạn hãy thêm như sau vào mục Kernel -> Patch

## Lỗi liên quan đến UEFI

- ProvideConsoleGop
  
  - Bắt buộc phải có cho việc chuyển từ màn hình log của OpenCore. Đây từng là một phần trong AptioMemoryFix nhưng hiện tại thì tính năng này đã được chuyển sang dạng setting trong OpenCore.
  - Từ bản OpenCore version 0.5.6, tính năng này đã được bật từ động trong config.plist

- IgnoreInvalidFlexRatio
  
  - Bắt buộc phải có đối với các CPU từ đời Broadwell trờ xuống, Không bắt buộc đối với CPU AMD và CPU Intel từ đời Skylake trờ lên

Sources: [Dortania](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/kernel-issues.html#stuck-on-eb-log-exitbs-start)
