# Patch Card đồ họa AMD

> Ở 1 số card đồ hoạ AMD khi cắm vào chỉ nhận 6MB VRAM thì các bạn làm như sau (ở đây mình mặc định rằng Card đồ hoạ AMD của các bạn nằm trong danh sách hỗ trợ).

## Xác định phần cần Patch

B1: Ta cần phải xác định ID PCI. 

B2: Xác định Patch ACPI dGPU 

B3: Các bạn tải [SSDT-GPU-SPOOF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/decompiled/SSDT-GPU-SPOOF.dsl.zip)

## Cách 1: Bằng DSDT/SSDT

### Xác định ID PCI

B1: Truy cập vào trang sau để tìm Device ID [PCI Devices (ucw.cz)](https://pci-ids.ucw.cz/read/PC/1002) các bạn lướt xuống mục Device để tìm ID đúng với tên GPU của các bạn 

> 1 số GPU sẽ không có trong danh sách này các bạn dùng app [gpuz](https://taimienphi.vn/download-gpu-z-1325) để check mã của GPU vd : R7 240 sẽ có mã là oland thì bạn sẽ fake thành dòng cũng có mã là Oland

![](https://lh5.googleusercontent.com/xgWM0VoFZ2eyVxk0q0NJ4g--t9wVvAWJEUWLXFRCvyHEyz8GwwCw7H9fcmMdx-z6UA00el2_3AeG3yYWSKyFECZkxywuDgxN_atOXfgFHC9Y-8f_YdnvTrvO_3tJsHFD6BgXT2nj=s0)

B2: Ấn vào mục id tương ứng với tên dGPU (VD mình cần Fake dòng R9 390 thì dòng gần với nó nhất là dòng R9 390X).

![](https://lh3.googleusercontent.com/0VhOXv0_OBKkXWjvmleouDsVwFJTGtAQQw7wA0UMQF81I9zKwMAVj3t9QzOvSwTgbe_yUniosxX9qQ3OsWRlfaW4FAV41rF_vVho_0fvXL5hvpsJFIFYKcTvetfSVTKnr6QB2inC=s0)

> Ta có Device ID của dòng này sẽ là 67b0.

B3: Tách device id thành như sau 67 b0 00 00.

B4: Hoán đổi vị trí của 67 và b0 ta sẽ được b0 67 00 00. 

B5: Thế 0x vào trước mỗi cặp ta sẽ được 0xb0, 0x67, 0x00, 0x00. 

B6: Đưa vào ssdt nó sẽ là: 

```
                    “device-id”,

                    Buffer (0x04)

                    {

                        0xB0, 0x67, 0x00, 0x00

                    },
```

![](https://lh3.googleusercontent.com/_AlCsfxxSYdS7B1_V9RJguo1ekf8x7-yBv19TV0kOLANX28DZkxkyOPdABEfUncruAMeW0sRxKXfluLL_chPKdKoNCYmjis6tji4Jc1yZ-IdIKSw96HHvvcf33cBvFXNKR7TO-xu=s0)

### Xác định Patch ACPI dGPU

B1: Vào Windows mở Device Manager ⇒ ​​Display Adapters ⇒ Details ⇒ Location Paths sẽ được như hình 

![](https://lh5.googleusercontent.com/BfURAw6h73JWDySToUuJXn1wnc0URJmZJzel7CU-mvIMh0JG855IBkB0sZmTB0_I4RkHdgjfjcGJAlbWk0tjVG5sv8G7ZgNZsOE-8EzxMKjLakPXB7H_v22aLfrn2tgUL2bE-9g5=s0)

nó sẽ có dạng là 

`ACPI(_SB_)#ACPI(PC02)#ACPI(BR2A)#ACPI(PEGP)#PCI(0000)#PCI(0000)`

B2: Mở `Device Manager ⇒ ​​Display Adapters ⇒ Details ⇒ BIOS Device Name`.

![](https://lh4.googleusercontent.com/N2383hpzLbYn0Vv5xeDqz6CEkQUYM1slufQQKLBQeiJzgCROVtOdNoyTSIH6dzjGfOnmAnvRCr0h8qAsfMc8gTH2a0fihm8LAUXhiyKojrGLFuZVd1YKThfvIylbqIW6Qsqct23J=s0)

B3: Tiến hành chuyển đổi như sau: 

```
ACPI(_SB_)#ACPI(PC02)#ACPI(BR2A)#ACPI(PEGP)#PCI(0000)#PCI(0000) 

⇒ \_SB_.PC02.BR2A.PEGP

(thay thế pci0 ⇒ pc02, peg0 ⇒ br2a)
```

B4: Thay thế vào SSDT nó sẽ có dạng: 

![](https://lh4.googleusercontent.com/YeS97bJZMSqVY12I_Rv-G9CvBgM82R29Qyuf-GN7Kdu5n-hTOvDOsjxYwwwBxAtIHwz1SCeIJgSRtmPn0Vo7-Ny7i417aUimMJHuyZN_YgMrr1qQeJwhkjXmub9IeEFTYxQZ00y5=s0)

### Cuối cùng ta sẽ thay tên Model vào

> không bắt buộc

Ví dụ như mình ở đây thì sẽ thay tên là AMD Radeon R9 390.

![](https://lh4.googleusercontent.com/evM8t-31rWZzLZv9maC0Wee4_uWllNH4PrdlLN85lq-_K00trunv-3NqHluQNbpurJCl8NbpvM61A7Qw8iAxr6HY0-8mYv0mE0OJ16zPrhzKO6VbVraWm38DWf_KiaUkHz7GY-6Y=s0)

B5: Sau khi làm xong các bạn chuyển về file .aml và bỏ ào EFI ==> OC ==> ACPI hoặc EFI ==> CLOVER ==> ACPI ==> PATCHED.

## Cách 2: Add vào DeviceProperties

> Yêu cầu:
> 
> - Sử dụng whatevergreen version 1.5.2+

### Xác định ID PCI

B1: Truy cập vào trang sau để tìm Device ID [PCI Devices (ucw.cz)](https://pci-ids.ucw.cz/read/PC/1002) các bạn lướt xuống mục device để tìm id đúng với tên GPU của các bạn 

> 1 số GPU sẽ không có trong danh sách này các bạn dùng app [gpuz](https://taimienphi.vn/download-gpu-z-1325) để check mã của gpu vd : R7 240 sẽ có mã là oland thì bạn sẽ fake thành dòng R7 240 cũng có mã là Oland

![](https://lh5.googleusercontent.com/xgWM0VoFZ2eyVxk0q0NJ4g--t9wVvAWJEUWLXFRCvyHEyz8GwwCw7H9fcmMdx-z6UA00el2_3AeG3yYWSKyFECZkxywuDgxN_atOXfgFHC9Y-8f_YdnvTrvO_3tJsHFD6BgXT2nj=s0)

B2: Ấn vào mục ID tương ứng với tên dGPU 

> VD mình cần fake dòng R9 390 thì dòng gần với nó nhất là dòng R9 390X

![](https://lh3.googleusercontent.com/0VhOXv0_OBKkXWjvmleouDsVwFJTGtAQQw7wA0UMQF81I9zKwMAVj3t9QzOvSwTgbe_yUniosxX9qQ3OsWRlfaW4FAV41rF_vVho_0fvXL5hvpsJFIFYKcTvetfSVTKnr6QB2inC=s0)

> Ta có Device ID của dòng này sẽ là 67b0.

B3: Tách Device ID thành như sau:

```
// Thêm các chữ số 0 vào trước device-id sao cho device-id có 8 chữ số

67b0 ==> 000067b0

// Tách device-id thành các cặp 2 chứ số

000067b0 ==> 00 00 67 b0

// Đảo ngước thứ tự các cặp từ phải sang trái

00 00 67 b0 ==> b0 67 00 00

// Viết lại và sử dụng 

b0 67 00 00 ==> b0670000
```

### Xác định Location Patch GPU

B1: Mở Hackintool lên và chọn Tab PCle.

![](https://imgur.com/YkU9VHs.png)

B2: Các bạn cần chú ý đến phần Device Name và Device Patch.

![](https://imgur.com/qJpwyzP.png)

B3: Các bạn tìm đến tên VGA trong tab Device Name và nhìn sang Tab Device Patch chuột phải chọn Copy Patch

![](https://imgur.com/7kX3P11.png)

### Add vào Device Properties

B1: Các bạn mở config vào mục DeviceProperties nếu ở OpenCore và Device ==> Properties nếu ở Clover.

![](https://imgur.com/CfrLJUP.png)

B2: Các bạn Add dòng Device Patch vừa lấy được vào chọn kiểu dữ liệu là Dictionary.

B3: Add Device-ID vào như sau:

```
device-id|data|< giá trị id>
//ví dụ 
device | data | b0670000
```

![](https://imgur.com/m696bn3.png)

Cuối cùng Save lại và Reboot thôi.

## Bảng tóm tắt thông tin Card AMD

| Graphics Card       | OOB | min. OSX | DevID  | Reference Port Layout | Framebuffer | Lưu ý           |
| ------------------- | --- | -------- | ------ | --------------------- | ----------- | --------------- |
| HD 5770             | yes | 10.6.8   | 0x68B8 | 2x DVI, HDMI, DP      | Vervet      | none            |
| HD 5770 Mac Edition | yes | 10.6.8   | 0x68B8 | 2x mDP, DVI           | Hoolock     | none            |
| HD 5850             | yes | 10.6.8   | 0x6899 | 2x DVI, HDMI, DP      | Uakari      | none            |
| HD 5870             | yes | 10.6.8   | 0x6898 | 2x DVI, HDMI, DP      | Uakari      | none            |
| HD 5870 Mac Edition | yes | 10.6.8   | 0x6898 | 2x mDP, DVI           | Langur      | none            |
| HD 5870 Eyefinity 6 | yes | 10.6.8   | 0x6898 | 6x mDP, DVI           | Zonalis     | *chưa xác định* |

**AMD5000Controller**

| Graphics Card | OOB | min. OSX | DevID  | Reference Port Layout | Framebuffer | Lưu ý                     |
| ------------- | --- | -------- | ------ | --------------------- | ----------- | ------------------------- |
| HD 6790       | No  | ?        | 0x673E | ?                     | ?           | thiếu DevID               |
| HD 6850       | No  | ?        | 0x6739 | ?                     | ?           | thiếu DevID               |
| HD 6870       | Yes | ?        | 0x6738 | 2x mDP, HDMI, 2x DVI  | Duckweed    | không có tác dụng 2nd DVI |
| HD 69×0       | No  | ?        | –      | –                     | –           | rất nhiều lỗi             |

**AMD6000Controller**

| Graphics Card     | OOB | min. OSX | DevID  | Reference Port Layout | Framebuffer | Lưu ý                  |
| ----------------- | --- | -------- | ------ | --------------------- | ----------- | ---------------------- |
| HD 7750           | Yes | 10.8.3   | 0x683F | ?                     | ?           | lỗi đen màn từ 10.10.3 |
| HD 7770 / R7 250X | Yes | 10.8.3   | 0x683D | 2x mDP, HDMI, DVI     | Dashimaki   | lỗi đen màn từ 10.10.3 |
| HD 7850           | Yes | 10.8.3   | 0x6819 | ?                     | ?           | none                   |
| HD 7870 / R9 270X | Yes | 10.8.3   | 0x6810 | 2x mDP, HDMI, DVI     | Futomaki    | none                   |
| R9 270 / R7 370   | No  | 10.8.3   | 0x6811 | ?                     | ?           | thiếu DevID            |
| HD 7950 / R9 280  | Yes | 10.8.3   | 0x679A | 2x mDP, HDMI, DVI     | Hamachi     | none                   |
| HD 7970 / R9 280X | Yes | 10.8.3   | 0x6798 | 2x mDP, HDMI, DVI     | Hamachi     | none                   |

**AMD7000Controller**

| Graphics Card     | OOB | min. OSX | DevID  | Reference Port Layout | Framebuffer | lưu ý         |
| ----------------- | --- | -------- | ------ | --------------------- | ----------- | ------------- |
| HD 7790 / R7 260  | Yes | 10.10?   | 0x665C | none                  | none        | rất nhiều lỗi |
| R7 260X / R7 360  | No  | 10.10?   | 0x6658 | none                  | none        | “             |
| R9 290 / R9 390   | No  | 10.10?   | 0x67B1 | none                  | none        | “             |
| R9 290X / R9 390X | Yes | 10.10?   | 0x67B0 | none                  | none        | “             |

**AMD8000Controller**

| Graphics Card   | OOB | min. OSX | DevID  | Reference Port Layout | Framebuffer | Known Issues                     |
| --------------- | --- | -------- | ------ | --------------------- | ----------- | -------------------------------- |
| R9 285 / R9 380 | Yes | 10.10    | 0x6939 | DP, HDMI, 2x DVI      | Lagotto     | Needs RadeonDeInit (since 10.11) |
| R9 380X         | Yes | 10.10    | 0x6938 | DP, HDMI, 2x DVI      | Lagotto     | “                                |
| Fury / Fury X   | No  | 10.12    | 0x7300 | none                  | none        | Needs RadeonDeInit               |

**AMD9000Controller**

| Graphics Card   | OOB | min. OSX | DevID  | Reference Port Layout | Framebuffer | Known Issues     |
| --------------- | --- | -------- | ------ | --------------------- | ----------- | ---------------- |
| RX 460 / RX 560 | Yes | 10.12    | 0x67EF | DP, HDMI, DVI         | Acre        | cần RadeonDeInit |
| RX 470 / RX 570 | Yes | 10.12.6  | 0x67DF | 2x DP, HDMI, 2x DVI   | Orinoco     | cần RadeonDeInit |
| RX 480 / RX 580 | Yes | 10.12.6  | 0x67DF | 2x DP, HDMI, 2x DVI   | Orinoco     | cần RadeonDeInit |

**AMD9500Controller**

| Graphics Card | OOB | min. OSX | DevID  | Reference Port Layout | Framebuffer       | Known Issues                     |
| ------------- | --- | -------- | ------ | --------------------- | ----------------- | -------------------------------- |
| RX Vega56     | Yes | 10.12.6  | 0x687F | 3x DP, HDMI           | Kamarang / Iriri? | cần RadeonDeInit, sớm có drivers |
| RX Vega64     | Yes | 10.12.6  | 0x687F | 3x DP, HDMI           | Kamarang / Iriri? | cần RadeonDeInit, sớm có drivers |

> Chú thích:
> 
> - OOB: no có nghĩa là VGA cần phải được fake device-id.
> - Min OS: bản macOS thấp nhất được support.

**Source tham khảo:** [**Renaming GPUs (SSDT-GPU-SPOOF) Getting Started With ACPI (dortania.github.io)**](https://dortania.github.io/Getting-Started-With-ACPI/Universal/spoof.html) | [Releases · a](https://github.com/acidanthera/whatevergreen/releases)**[cidanthera/WhateverGreen (github.com)](https://github.com/acidanthera/whatevergreen/releases) | [Radeon Compatibility Guide – ATI/AMD Graphics Cards | tonymacx86.com](https://www.tonymacx86.com/threads/radeon-compatibility-guide-ati-amd-graphics-cards.171291/)**

## Lưu ý

Đối với card RX 550 bản `lexa` thì các bạn có thể thử device-id là `FF67`

Đối với card HD 7730 các bạn có thể thử device-id là `3F68`

Đối với Big Sur: đối với 1 số máy sẽ gặp bug `DeviceProperties injection failing` để kiểm tra các bạn sẽ dùng [IOReg](https://github.com/khronokernel/IORegistryClone/blob/master/ioreg-210.zip) vào đường dẫn của VGA rời tìm mục ACPI-Patch.

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/acpi-path.e72cc3e9.png)

Nếu không có thì các đang gặp lỗi này khi này các bạn sẽ không thể fake được device-id để fix các bạn sẽ dùng [SSDT-BRG0](https://github.com/acidanthera/OpenCorePkg/tree/master/Docs/AcpiSamples/Source/SSDT-BRG0.dsl)

Các bạn sẽ thay các mục như ảnh:

![](https://imgur.com/PXZHhAd.png)

Thay bằng đường dẫn như của phần ***Xác định path ACPI dGPU.***

![](https://imgur.com/abp0C0b.png)
