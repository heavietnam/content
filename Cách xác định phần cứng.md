# Cách xác định phần cứng

## CPU

Ta dễ thấy đối với những guide hướng dẫn hackintosh hiện nay hầu hết đều dùng `Code Name` . Để xác định được `code name` của CPU và `iGPU` ta làm như sau:

B1: Bạn chuột phải vào biểu tượng `windows` ở góc dưới cùng bên trái và chọn `system`.

![](https://i.imgur.com/3QEza7m.png)

B2: Copy mục `processor`.

![](https://i.imgur.com/wwrPl8K.png)

B3: Ta tiến hành search mục `processor` trên trình duyệt.

![](https://i.imgur.com/wdh3pK5.png)

B4: Các bạn tiến hành mở kết quả tìm kiếm có domain là `ark.intel.com`

![](https://i.imgur.com/HeOXlJI.png)

B5: Bạn sẽ chú ý vào phần `tên mã` hoặc `code name`. Đây chính là `code name CPU` của bạn.

![](https://i.imgur.com/OADzcWa.png)

như ở đây ta có code name là Comet Lake

## iGPU

Để có thể patch được `i`GPU thì ta phải xác định được `iGPU name.` Làm như sau:

B1: Ta sẽ tiến hành search `processor` trên trình duyệt và mở trang `ark.intel.com` như hướng dẫn ở `CPU`.

![](https://i.imgur.com/ngljFRK.png)

B2: Các bạn chú ý vào mục `Đồ họa Bộ xử lý --> Đồ họa bộ xử lý ‡`

Đây chính là `iGPU name` của bạn.

![](https://i.imgur.com/Y724Y6p.png)

ở đây igpu name là HD4000

## Chipset model và Mainboard

Trước hết ta sẽ cần tìm hiểu chipset là gì? *Chipset máy tính là một* mạch tích hợp giúp xử lý giao tiếp giữa CPU, RAM, bộ lưu trữ và các thiết bị ngoại vi khác. Chipset xác định số lượng thành phần tốc độ cao hoặc thiết bị USB mà bo mạch chủ của bạn có thể hỗ trợ. Chipset thường bao gồm một đến bốn chip và các bộ điều khiển tính năng cho các thiết bị ngoại vi thường được sử dụng, như bàn phím, chuột hoặc màn hình.

Vậy còn mainboard là gì? Mainboard hay main máy tính hay bo mạch chủ là một bảng mạch đóng vai trò nền tảng trên máy tính, laptop có tác dụng kết nối các linh kiện bên trong thành thể thống nhất. Mainboard PC sẽ nằm ở thùng máy, hoặc tích hợp đằng sau màn hình đối với máy tính AIO.

Để xác định được mã của chúng ta sẽ làm như sau

### Dùng system info

B1: Chuột phải vào logo `Windows` ở góc dưới cùng bên trái và search `System Information`.

![](https://i.imgur.com/ifY4jbQ.png)

B2: Ta chú ý vào dòng `Baseboard manufacturer` (đây là dòng hiện thị main) và `Baseboard product` (đây là dòng hiển thị chipset).

![](https://i.imgur.com/IMq9aiW.png)

> Nhưng một số main sẽ không hiển thị chipset vậy ta sẽ tiến hành làm như sau

### Xác định thông qua device manager

B1: mở `device manager`

B2: chọn tới tab `IDE ATA/ATAPI controllers` 

B3: Chọn `properties` ta sẽ có thể thấy thế hệ chipset

![299460458_838685037052875_6042626987910816592_n.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-11-49-06-299460458_838685037052875_6042626987910816592_n.png)

## Audio Codec

B1: Các bạn tải `aida64` [tại đây](https://www.aida64.com/downloads).

B2: Các bạn sẽ vào tab `Multimedia --> PCI/PnP Audio` ở đây ta sẽ có thể thấy `audio codec`.

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/audio-controller-aida64.c4a94a0b.png)

## Network Controller models

B1: Các bạn tải `aida64` [tại đây](https://www.aida64.com/downloads).

B2: Các bạn sẽ chú ý đến mục sau `Network --> PCI/PnP Network`.

![](https://i.imgur.com/XiTPa8B.png)

## Drive Model

B1: Chuột phải vào logo Windows ở góc cuối bên trái và chọn `device manager`.

![](https://i.imgur.com/oNBcTmN.png)

B2: Bạn chọn vào disk ở đây ta sẽ có được tên ổ cứng.

![](https://i.imgur.com/vL2khbW.png)

## dGPU

B1: Bạn nhấn tổ hợp phím `Windows + R`.

Và gõ `dxdiag` sau đó enter.

![](https://i.imgur.com/XHk4Jxy.png)

B2: Hệ thống sẽ mở `DirectX Diagnostic Tool` và bạn hãy chuyển đến tab `render` chú ý đến dòng name.

![](https://i.imgur.com/zzNSJad.png)

## Keyboard, Trackpad and Touchscreen Connection Type

B1: Các bạn tải aida64 [tại đây](https://www.aida64.com/downloads)

B2: Các bạn truy cập vào đường dẫn `Devices --> Physical Devices --> PnP Devices` ở đây các bạn sẽ tiến hành tìm kiếm name Touchpad.

![](https://i.imgur.com/PgeqvQY.png)

## Xác định `location path`

Trước tiên ta cần tìm hiểu `location path` là gì? Nó là đường dẫn đến `devices` trong hệ thống và thường được hệ thống đọc dưới 2 dạng là `ACPI path` và `Device path`. `ACPI path` thường được sử dụng trong việc `patch DSDT` còn `Device path` thường được dùng trong `config.plist` để inject `properties` của các `Device`.

B1: Các bạn nháy chuột phải vào biểu tượng `Windows` ở góc dưới bên trái và chọn `device manager`.

![](https://i.imgur.com/oNBcTmN.png)

B2: Tiến hành chuột phải vào device cần lấy `location path` và chọn `properties`.

![](https://i.imgur.com/2bzgPTP.png)

B3: Chọn tab `Details --> location paths`.

![](https://i.imgur.com/strrnkL.png)

B4: Ta sẽ tiến hành chuyển value vừa lấy được ở trên về dạng path để có thể sử dụng.

```bash
//value mặc định
PCIROOT(0)#PCI(1C01)#PCI(0000)
ACPI(_SB_)#ACPI(PCI0)#ACPI(RP02)#ACPI(WLAN)

//Device path mặc định
PCIROOT(0)#PCI(1C01)#PCI(0000)

//ACPI path mặc định 
ACPI(_SB_)#ACPI(PCI0)#ACPI(RP02)#ACPI(WLAN)

//Convert Device path mặc định
PCIROOT(0)#PCI(1C01)#PCI(0000)
# Tiến hành chuyển dấu "#" thành dấu "/"
PCIROOT(0)/PCI(1C01)/PCI(0000)
# Ta tiến hành chia các giá trị nhỏ trong "()" thành các cặp 2 số. Nếu cặp số không bắt đầu bằng số "0" thì thêm số "0" vào trước
PCIROOT(00)/PCI(01C 01)/PCI(00 00)
# Viết thế chữ "x" vào giữa các cặp 2 chữ số
PCIROOT(0x0)/PCI(0x1C 0x1)/PCI(0x0 0x0)
# chuyển các dấu cách trong ngoặc thành dấu ","
PCIROOT(0x0)/PCI(0x1C,0x1)/PCI(0x0,0x0)

// Convert ACPI path nguyên mẫu
# Ta tiến hành giữ lại các ký tự trong ngoặc những ký tự khác thì lược bỏ 
_SB_ PCI0 RP02 WLAN
# Tiếp đó tiến hành thay dấu cách bằng dấu "."
_SB_.PCI0.RP02.WLAN
```

## Xác định vendor id và device id của usb controller

Khi tiến hành map usb các bạn cần xác định device id và vendor id để sử dụng đúng kext. Việc này rất dễ thực hiện ở mac nhưng khi không có mac thì các bạn làm theo sau:

B1: chuột phải vào biểu tượng windows chọn `device manager`

B2: chuột phải vào tên usb controller và chọn `Properties`

> tên usb controller thường bắt đầu bằng `Intel (R)`

![](https://i.imgur.com/8l5Fdg8.png)

B3: tới tab `Details` và chọn `Hardware ID` và nhìn vào dòng đầu tiên

![](https://i.imgur.com/pu11YG6.png)

Ở đây ta có vendor id là `8086` device id là `1E31`

**Source tham khảo: [Finding your hardware | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/find-hardware.html#finding-hardware-using-windows)**
