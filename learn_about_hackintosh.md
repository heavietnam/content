# Tìm hiểu về Hackintosh

## Tìm hiểu về thuật ngữ

- Trước hết, kext là gì? Kext (Kernel Extension) là 1 trình điểu khiển phần cứng mà bên Windows gọi là drivers. 

- Thế thêm kext vào đâu? Có 3 nơi thêm kext:

- O/K **(OC ⇒ Kext)** hoặc C/K/O **(CLOVER ⇒ Kexts ⇒ Other)**

- L/E **(Library ⇒ Extensions)**

- S/L/E **(System ⇒ Library ⇒ Extentions)**

- Thứ tự ưu tiên O/K (C/K/O) ⇒ L/E ⇒ S/L/E 

- Khi bỏ kext vào L/E hoặc S/L/E ta cần prebuilt kext cache (để bỏ kext vào L/E ta dùng [kext droplet](https://github.com/chris1111/Kext-Droplet-Big-Sur/releases)  | để bỏ kext vào S/L/E ta có thể dùng [kext utility](https://taimienphi.vn/download-kext-utility-for-mac-34434))

- Khi bỏ hoặc xoá kext ta cần empty trash (đối với OC và Clover không cần) 

- Khi bỏ kext vào O/K ta cần snapshot kext (khi bỏ kext vào C/K/O ta chỉ việc bỏ kext vào thôi)

- Mở file config.plist bằng [ProperTree](https://github.com/corpnewt/ProperTree) (có thể dùng các công cụ khác như [OpenCore Configurator](https://mackie100projects.altervista.org/download-opencore-configurator/)). Đối với các bạn dùng app này thì cần chọn phiên bản phù hợp với phiên bản OC hiện tại ) hoặc nếu đang sử dụng clover bạn có thể dùng [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/).

- Chọn `File` ⇒ `OC Snapshot` (Đối với OpenCore)![](https://lh6.googleusercontent.com/SCT2Gq_55sCnidkxh8Kn2GjrTrkFc-lj0-5v9yBHi_YUZEK9Zy-OLojhKH-VaT_78oZfvBH8gCl-sdLuwnCyBg5RZv4PLtaeWq81Xlg4oRc9G3AiAXvU21W2A0Q8DdcuLZQSH5W8=s0)

- Build ProperTree thành app: 

- Tải ProperTree theo links trên. 

- Mở folder Scripts.

- Mở file ​​buildapp-select.command (sẽ được như hình).

![](https://lh5.googleusercontent.com/7ug14hH_jAB2IVMR43n7KPWJVohXGU62HF2rjNrAXxvPIoGJpAr4PaEaoxGtohtXga44oqI7Qzqc9sPTyys9uYsKeLeMgLHYHoCEQIH8NPc0Ameq5ij6eMvV9WtgSIjtKpVNkvF7=s0)

- Tuỳ theo sở thích mà các bạn chọn phiên bản phù hợp nhưng ở đây mình khuyến khích các bạn nên chọn bản đầu tiên. 
- Sau khi build app sẽ được như hình: 

![](https://lh3.googleusercontent.com/mGEcV3zarD_ztiXZ21ZGLeYVyedjRLkDQ3JyUda6T62XZ4S_gP7Li6sQFX_7G3ykzOmSpMBnuZS-z14q8YAdcEX1TXC8DcE_j4wPSnVLAwLrbmughjhs6kn-EgCoBtTpOq94jcc1=s0)

- Các bạn ra ngoài folder sẽ thấy có app ProperTree. 

![](https://lh5.googleusercontent.com/7oU6l35cuJRTITzdgOpx9pIAYsg1lYXnA25D4Zzp84ATaovZx20os6PYht7nZe_GGE8DMDTACYUsfmkBN8tAjj28jKdH6H7-gRr3cPmlaGOoMu63e4lby0tWBjvDhwUxJhzx9YKq=s0)

- Các bạn chỉ việc kéo app vừa built vào mục applications là xong.

## SSDT/DSDT là gì?

- Ta có thể hiểu nó là các bản giao thức điều khiển thiết bị, xem thêm: [Advanced Configuration and Power Interface - Wikipedia](https://en.wikipedia.org/wiki/Advanced_Configuration_and_Power_Interface) (tiếng Anh)

- Tại sao cần patch DSDT/SSDT khi hackintosh ? 

- Do DSDT/SSDT của macOS được Apple quản lý theo 1 quy định chặt chẽ tối ưu cho các máy Mac, do là hackintosh nên 1 số phần cứng như pin , card âm thanh , đồ hoạ v.v có thể không hoạt động nên ta phải patch DSDT/SSDT để máy hoạt động giống Mac thật nhất có thể.

- Cấu trúc của DSDT và SSDT có thể nói là giống nhau nhưng DSDT thì sẽ chứa hầu hết các phần cứng trên main (card đồ hoạ , âm thanh , chip v.v), còn SSDT đa phần đều là trích xuất từ DSDT (hiểu nôm na là đã được compile).

- Ta có thể dump DSDT bằng các công cụ như như `acpidump.exe`, `rw-everything`, `acpidump.efi`, `ssdt-time`, v.v….. (cách dump DSDT mình có hướng dẫn [ở đây](https://docs.google.com/document/d/1CUwpBGi_dOMRoIpiNbbnMChfeDZz3JW7_9XF6TzmsQw/edit?usp=sharing))

- Để patch DSDT ta cần fix sạch error trên DTSDbằng công cụ [MaciASL](https://github.com/acidanthera/MaciASL/releases).

- Các cách patch DSDT chủ yếu:

- Có 2 cách patch dsdt chủ yếu là Static patch và Hot patch. 

- Static patch là gì? Đây là cách patch truyền thống đối với cách patch này ta cần dump DSDT ra và fix lỗi trực tiếp trên DSDT, sau đó bỏ nó vào bộ nạp khởi động nó sẽ được inject khi vào OS, cần gì fix đó, còn nhược điểm của cách patch này là khi update firmware ta sẽ phải patch lại toàn bộ.

- Hot patch là gì? Hot patch là cách patch tìm những đoạn mã cần sửa trong DSDT và sửa lại, không sửa trực tiếp đối nó sẽ tự động inject ra khi khởi động vào OS, với cách patch này khi update firmware ta không cần patch lại.

- Các cách dump DSDT:

- Có rất nhiều tool dump DSDT (`SSDT-Time`, `terminal`, `sysreport`, `Clover`, `rw-everything`, `UEFI shell`, v.v………) nhưng không phải cách nào cũng cho ra 1 DSDT chuẩn (native) 

- Theo mình các bạn chỉ nên dùng các cách dump sau `Clover`, `sysreport`, UEFI shell và Terminal đối với SSDT-Time trên win nếu các bạn dual bằng OpenCore thì DSDT sẽ không còn là của gốc máy nữa (native) nữa dẫn đến sinh ra nhiều error các bạn nhớ lưu ý kỹ.

## Các tool UEFI

- Trong cả OpenCore và Clover, ta có 1 folder tên là Tools, đây là nơi chứa các ứng dụng UEFI mà bạn có thế mở từ bootloader, có thể không cần cũng được.

- 1 số tool hữu ích :

- OpenShell.efi : giúp gỡ lỗi opencore (kèm sẵn trong OpenCorePkg ⇒ x64 ⇒ EFI ⇒ OC ⇒ Tools)

- [ACPIdump.efi](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/acpidump.efi.zip) : giúp dump DSDT (chỉ dùng được trên OpenCore)

## SIP và Gatekeeper

- SIP là gì ?

- Kể từ macOS 10.11 trở lên, Apple đã thêm tính năng System Integrity Protection vốn dùng để bảo vệ phân vùng hệ thống khỏi malware và các phần mềm độc hại, nhưng điều này cũng sẽ ngăn cản chúng ta cài app crack. (Do cách inject kext của Clover, SIP không được hỗ trợ nếu bạn dùng Clover)

- Gatekeeper là một tính năng bảo mật của hệ điều hành macOS của Apple. Nó thực thi ký mã và xác minh các ứng dụng đã tải xuống trước khi cho phép chúng chạy, do đó làm giảm khả năng vô tình thực thi phần mềm độc hại. 

- Tại sao phải tắt SIP và Gatekeeper? (từ catalina trở lên muốn cài app bên thứ 3 ta bắt buộc phải tắt)

- Cách tắt SIP: Vào file `config.plist`, search `AllowToggleSIP` và bật nó lên, sau đó các bạn restart nhấn vào tool toggle SIP.

- Cách tắt Gatekeeper: Dán code sau vào terminal: *`sudo spctl --master-disable`*.

- *Một số app hay dành cho newbie:* 

- [*Clean My Mac*](https://maclife.vn/cleanmymac-x4-cong-cu-don-dep-toi-uu-he-thong-hieu-qua-nhat.html) *: Giúp xoá app tận gốc và dọn dẹp*.

- [*Macs Fan Control*](https://maclife.vn/huong-dan-cai-dat-va-cau-hinh-macs-fan-control.html) *: Điều khiển tốc độ* quạt máy tính.

- [*Neet-downloadmanager*](https://www.neatdownloadmanager.com/index.php/en/) *: Hỗ trợ download nhanh*.

- [*EVKey*](https://evkeyvn.com/) *: Hỗ trợ gõ tiếng việt.* 

- [*iStat Menus*](https://maclife.vn/istat-menus-theo-doi-thong-tren-menubar.html) *: Hỗ trợ theo dõi các chỉ số cpu, fan, nhiệt độ.* 

- [*Archiver*](https://maclife.vn/archiver-phan-mem-nen-giai-nen-nho-gon-manh-me.html) *: Công cụ giải nén mạnh mẽ trên Mac*.

- [*Parallels Desktop*](https://maclife.vn/parallels-desktop-16-for-mac-ho-tro-cai-windows-tren-mac-ban-moi-nhat.html) *:  Công cụ ảo hoá mạnh mẽ*.

- [*Swish*](https://maclife.vn/swish-ho-tro-them-rat-nhieu-thao-tac-tren-trackpad.html) *: Hỗ trợ nhiều thao tác cho Trackpad.* 

## Boot-arg là gì ?

*Boot-arg là nơi chưa các tham số khi khởi động* (tên đầy đủ là boot-arguments)

*1 số tham số quan trọng*:

- *Verbose (`-v`) : giúp hiện thị các debug khi boot*.
- *darkwake ( `-darkwake=` ) : giúp fix darkwake sau khi sleep (wake)*
- *`-cpus=1`: thường dùng để fix 1 số lỗi liên quan đến core (hay dùng ở các máy HP)*
- *`-debug=0x100`: thường dùng kèm với verbose giúp hiển thị lỗi khi panic*.
- *`-nvda_drv=1` : dùng khi muốn dùng NVIDIA Web Driver*.
- –*`igfxonln=1` : dùng để force-online*.
- *-igfxblr : dùng để fix backlight.* 
- *`-wegnoegpu` : dùng để disable dGPU (card rời)*
- *`-x` : safe mode*.
- *`alcid=“codec”` (dùng để fix âm thanh)*.

*Add tham số vào boot-arg như thế nào ?*

- *Các bạn vào mục NVRAM ⇒ 7C436110-AB2A-4BBB-A880-FE41995C9F82 ⇒ boot-arg*  (chỉ OpenCore còn đối với Clover các bạn thêm vào boot ==> arguments)

![](https://lh5.googleusercontent.com/CtWqLw5-5wT8cOnGnj_5W2Ot8R6siAOO_F8-bp5Jn_bnsfgm0YZ9RCnJcNf4Dpy01AD4TL3Wtjj7rnzp3tDYfmyNm-hGLYH0RIoqCZULEgOqZwntj6W0cLQ42_LhrwoMWil9I961=s0)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-12-at-11.25.28.png?w=1024)

- *Các bạn gõ các tham số vào .*
- *Save lại và reboot*.

## DeviceProperties là gì?

- *Dây là nơi các bạn có thể inject thông tin các thiết bị PCle, ví dụ như iGPU, Audio, NIC (Card mạng LAN),…..*
- Tại đây bạn có thể patch iGPU, Audio và các cổng xuất màn hình.
