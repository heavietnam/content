# Hướng dẫn Dual Boot

## Tạo usb Winpe trên mac

### Cách 1: dùng command terminal

**B1**: Các bạn Format USB theo định dạng FAT32 với tên `WINDOWS10` tiếp các bạn tải file ISO Wi‌nPE Khuyến khích dùng bộ [nhvboot](https://nhvboot.com/)

**B2**: Các bạn Mount file ISO vừa tải về. 

**B3**: Gọi Terminal lên và gõ lệnh

```
cp -rp kéo ổ iso vừa mount vào /* /Volumes/WINDOWS10 

Vd: cp -rp /Volumes/CCCOMA_X64FRE_EN-GB_DV9/* /Volumes/WINDOWS10
```

**B4**: Boot vào WinPE và cài thôi (các bạn có thể dùng CMD gõ lệnh setup.exe hoặc winNTsetup hoặc wintohdd v.v)

### Cách 2: dùng unetbootin

B1: Tải unetbootin [tại đây](https://unetbootin.github.io/)

B2: Chọn xuống diskimage sau đó tiến hành chọn file iso và usb

![](https://i.imgur.com/xWSYYNP.png)

B3: nhấn ok và boot thôi

> Lưu ý: các bạn vẫn có thể dùng cả 2 cách để tạo bộ cài linux thay vì windows

## OpenCore:

> OpenCore không chuyên về Dual Boot nhưng các bạn vẫn có thể thực hiện Dual Boot với OpenCore sau đây là hướng dẫn mình sẽ chia phần này làm 2 phần nhỏ là tạo USB Boot ngay trên macOS và hướng dẫn Dual Boot với OpenCore và rEFInd.

### Chỉnh sửa config

B1: mở config bằng propertree

B2: Tiến hành chỉnh sửa những phần sau

- Bật `CustomSMBIOSGuid`.  
- `UpdateSMBIOSMode` set là `Custom`.

> Mục đích: là “chống inject” thông tin SMBIOS sang các OS khác (ví dụ MacBookPro15,1; iMac20,1;…). Nếu muốn kiểm tra máy có bị inject hay không thì chỉ cần chạy dxdiag ở Run trên Windows là được.

> Hãy nhớ rằng opencore inject smbios sang những os khác là để dùng bootcamp. Nên khi bạn chỉnh như phần này sẽ không dùng được bootcamp

> Đối với 1 số model thì opencore không thể win được phân vùng windows khi này các bạn sẽ add config theo sau
> 
> `Misc -> BlessOverride -> \EFI\Microsoft\Boot\bootmgfw.efi`

### Chỉnh sửa ACPI

> Đối với phần này cũng như phần trên để dùng bootcamp nên opencore đã cho acpi injet qua các os khác. Để ngăn chạn việc này đòi hiẻu bạn phải có kỹ năng chỉnh sửa SSDT, DSDT xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/)

B1: Mở file SSDT hoặc DSDT cần sửa bằng [maciasl](https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/)

B2: Các bạn sẽ tiến hành add 1 dòng  `If (_OSI ("Darwin"))`

```
//Add dưới method cần patch
            If (_OSI ("Darwin"))
            {
               Giá trị cần patch 
            }
            Else
            {
                Giá trị ban đầu
            }
```

- Giá trị ban đầu: tức là giá trị trước khi patch. Vậy lấy nó ở đâu, Các bạn sẽ lấy nó ở DSDT gốc máy
  
  - Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)

- Vậy ý nghĩa của nó là gì?
  
  - Các bạn càn biết là win cũng cần DSDT để chạy. Nhưng DSDT của mac và win khác nhau.
  - Do đó các bạn cần sửa giá trị của DSDT ở win cho phù hợp đẻ chạy ở mac
  - Nhưng do opencore cho phép inject ACPI sang os khác do đó giá trị ban đầu của DSDT sẽ bị ghi đè bởi DSDT đã sửa đổi dẫn đến lỗi win
  - Do đó khi bạn add dòng if này có nghĩa là
    - `If (_OSI ("Darwin"))`: tức là macos (nhân của macos là `Darwin`)
      - thì sẽ chạy giá trị đã sửa
    - `Else`: tức là những os khác
      - thì sẽ chạy những gì trị ban đầu

- Ví dụ:

```
        Method (_STA, 0, NotSerialized)  // _STA: Status
        {
            If (_OSI ("Darwin"))
            {
                Return (0x0F)
            }
            Else
            {
                Return (Zero)
            }
       }
```

Chuyển về file `.aml` để sử dụng 

> Update: đã có một bản opencore no acpi giúp acpi ko bị inject sang os khác. giúp cho các bạn ko có kiến thức về SSDT vẫn sử dụng được
> 
> Build opencore mod theo hướng dẫn [tại đây]([Cách làm EFI Opencore No ACPI - Heavietnam August 2022](https://heavietnam.ga/2022/08/02/cach-lam-efi-opencore-no-acpi/))

## rEFInd

B1: Các bạn down refind theo hướng dẫn [tại đây](https://sourceforge.net/projects/refind/)

B2: vào folder `EFI --> Boot` Copy `Bootx64.efi` vào `EFI --> OC` và đổi tên lại là `BOOT-origx64.efi`

B3: Copy `refind_x64.efi` vào `EFI --> boot` và đổi tên lại là `Bootx64.efi`

B4: copy các mục `drivers, icons, tools, refind.conf-sample` vào `EFI --> Boot`

B5: Đổi tên `refind.conf-sample` thành `refind.conf`

B6: Xoá hết các arg trong `refind.conf` đi sau đó add các arg sau

```
timeout 10 //thời gian đếm ngược ở menu boot

resolution max //độ phân giải màn hình

use_nvram true //sử dụng rest nvram

scanfor manual,external //chỉ cho phép quét đối với manual và external
```

Chi tiết các arg của refind xem [tại đây](https://www.rodsbooks.com/refind/configfile.html)

B7: tạo thư mục `themes` tại EFI --> Boot

B8: Tải theme refind [tại đây](https://github.com/topics/refind-theme)

B9: copy theme vào thư mục themes vừa tạo

B10: copy các arg trong file `theme.conf` vào file `refind.conf`

B11: Tiến hành add các đường dẫn boot theo sau

```
menuentry "My macOS" {
    icon \EFI\refind\icons\os_mac.png
    loader  \EFI\OC\Opencore.efi
}

menuentry "Windows" {
    icon \EFI\Boot\themes\rEFInd-minimal\icons\os_win.png
    loader \EFI\Microsoft\Boot\bootmgfw.efi
}

menuentry "Ubuntu" {
    loader /EFI/ubuntu/grubx64.efi
    icon /EFI/refind/icons/os_linux.png
}

menuentry "Arch Linux" {
    icon     /EFI/refind/icons/os_arch.png
    volume   "Arch Linux"
    loader   /boot/vmlinuz-linux
    initrd   /boot/initramfs-linux.img
    options  "root=PARTUUID=5028fa50-0079-4c40-b240-abfaf28693ea rw add_efi_memmap"
    submenuentry "Boot using fallback initramfs" {
        initrd /boot/initramfs-linux-fallback.img
    }
    submenuentry "Boot to terminal" {
        add_options "systemd.unit=multi-user.target"
    }
}
```

![](https://i.imgur.com/V48dfEK.png)

Như vậy là xong rồi

> Đối với các bạn lười. Có thể lấy bản refind mình đã build sẵn [tại đây](https://za7o7cw6-my.sharepoint.com/:u:/g/personal/hoanglong_coursedeals_org/EcsvkpyK7z1OqOVjeMBtn1UBDmeHhbXge5qFvsYC5W_ZXg?e=1KhHcf)

## Sử dụng clover để boot OC

Nghe có vẻ lạ nhưng đây hoàn toàn là điều khả thi. Bởi bạn thân clover là một bootloder rất mạnh nên nó hoàn toàn có thể boot OC

B1: Tải bản clover mình đã chỉnh sửa [tại đây](https://za7o7cw6-my.sharepoint.com/:u:/g/personal/hoanglong_coursedeals_org/EY4_mDGDF3pGvLT3_ItFoXgB7F-n4WkOmkigK2n5GIrefw?e=lFiLSO)

B2: Tải clover configurator và mở file config của clover bằng propertree sau đó làm theo video

B3: tiến hành boot thôi

## Clover

Đối với clover như đã nói ở trên trình dual boot của clover đã rất mạnh. Nên cứ như vậy thì boot thôi. Tuy nhiên trường hợp các bạn muốn add thêm bootoption làm theo hướng dẫn tại đây

B1: Các bạn mở config bằng Clover Configuration. 

B2: Các bạn chuyển tới mục Gui. 

B3: Bấm vào nút ![](https://lh3.googleusercontent.com/wSD14bkNpGgSKgrgTgdAQaEB8dTMo5VY9W-DTxpxmUaxRUlG9nJwjJd2hNlKzxaWV-kYoySoFA6ec-eGR55G8EHwtc437IS_Ad423vrNkCJFWwECwb2AHVeKta5RSldLYqONolJ4=s0) ở mục Custom Entries. 

B4: Sau khi làm xong các bạn sẽ được như hình:

![](https://lh4.googleusercontent.com/RYp9C1SDmcMm4yQj4yjmrYlU-GNFn_npMZyjhv7RdPzhf_HDWPtQBQMryoUVpkT5D2NUmO8UQ_2odV1-rMU9IBQmxY95bl75wLjXVAz-PvjTTkB8-OSPNnh3Ct2FUyzN-trQa_HM=s0)

B5: Các bạn nhấn đúp và chọn ![](https://lh6.googleusercontent.com/s4dUG0sC-D-aHSuVtdvu_GZxRvASSYwUg0zOiD2A22Dj8OPle1uaCltKlJqZupfuXkh6BhYC3pDLQYJHAL5GfgA1RDSloM1NjHCF6dbwnOMqBt_-5MWYvFYRgPeUEWhCAjLOkA8J=s0) (sẽ được như hình).

![](https://lh5.googleusercontent.com/CmZ1A4P6GG_0lvy2TiIttwbkY6oKS924oSUbwctk3PLrb029rr9YjAijw93VxhTiAOK8-U8fSf0QPHA028S5hLQH_61lvS0h5pQPvRQ1CtqEKCLj824E4-_u4HjUMGkfKWSnz1pg=s0)

B6: Add Boot Option cho OpenCore các bạn chỉnh như sau (nếu các bạn xoá mục Drivers thì ở đây các bạn phải add đường dẫn cho os mà các bạn muốn Dualboot).

- volume: Chọn partition chứa file boot cần add
- path: chọn đến đường dẫn cần add
- Title / full title: tên tùy ý
- Type: Other.
- Volume type: internal. 

B7: Save lại. 

B8: Reboot và tận hưởng thôi.
