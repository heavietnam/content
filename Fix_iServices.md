# Fix iServices

> Mình sẽ chia phần này làm các phần nhỏ như là Fix ROM, thay SMBIOS, Fix en0. 

## ***Thay SMBIOS:***

## OpenCore

**B1**: Các bạn tải file Gen SMBIOS về theo link sau [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS).

**B2**: Mở phần mềm lên và nhấn phím 1.

![](https://lh6.googleusercontent.com/MMEOpBo_14ZaT4gmCXghkly6gAGbia2sNF0vR2NQpDAKJwEhuqttkHcol-dblU-UJPxm7h3WcMx6QO0PSyZcP_Ib-lbUFjPq_rJ_mFtfM_izBrw9E1QFru8i9TuA0Lp6MuVhfD4l=s0)

     **B3**: Chọn 3 và gõ tên SMBIOS cần Gen để phần mềm Gen các SMBIOS ra. ![](https://lh3.googleusercontent.com/pxM6pg3_hGmBhBb7TLqQ7uFUTv9McupFgRsTMY2CcCeC11L_c9vI_a7fsWkmiKOC5Dp-MCBIoLyjpA5jsw3UaoBlWAwQGTKGr7TZH4KePti_CN7pd654Ub7S1AHEHH-o8zOl-ucl=s0)![](https://lh6.googleusercontent.com/BMnHbKRR7rAu6ijbXyDOFy7ImeQn1dOJZENenwC9BCbiraiA-bGBNBinULXpi9TIAeApF3IbAQEQLUkfkPXcwL14G7QSonSy1SeHutNgvHMarfUqI_HrIPJIIn9REtQyG6xf4w7w=s0)

**B4**: Các bạn vào trang [Check Your Service and Support Coverage – Apple Support](https://checkcoverage.apple.com/) dán số Serial vào và nhập mã xác thực vào là done khi check bạn sẽ nhận được ba kết quả:

![](https://lh5.googleusercontent.com/A-XhI47bvntyDBbL8qRVha9fIB0CLX-ZNZemdU5DpjYeYEgKnmV-Etc7rW9hb9Uad2syOdhXiN9WQx1IwoRGleCN7IEzG7i5VIKUYntPEFZSNd-c1pNiCGTnni2GLjsbCWro4a_k=s0)

![](https://lh4.googleusercontent.com/q2q-mgBk6THBT9_DfFa4vaM-xP57wfcMja1RHimDB-U5eqrLDe82eZ71_M-pXOvlQuwrcW2APm0AQraZELrLwf0Qqe0E4KGTqdhhCnAG7YKgTXKtvzDOhfdH2N11w6Q4TkpgEvAF=s0)

![](https://lh4.googleusercontent.com/YupQDvkVnTW-M1WufJmCmgPdJOUMixDEA81mUMS5Hdh2dBynzvX6lD6B92bRKuC1UVwpR4IXHCBpUH0v1crlcidOHs6vmQingHxMZGD00Dc7ymVqVkQaVxZWKs_Gh4jef3T5Wfh5=s0)

> Nếu sau khi check bạn nhận được `We’re sorry, but this serial number isn’t valid.` thì bạn có thể chuyển sang bước tiếp theo nếu khi check bạn được dòng `Valid Purchase date` thì hãy gen lại 1 số serial khác còn nếu khi check bạn nhận được kết quả là `Purchase Date not Validated` thì xin chúc mừng SMBIOS của bạn đã rất tốt rồi. 

**B5**: Các bạn thay SMBIOS vào config.plist -> PlatformInfo -> Generic theo các phần tương ứng: 

- Type = SystemProductName
- Serial = SystemSerialNumber
- Board Serial = MLB
- SmUUID = SystemUUID

**B6**: Save lại và Reboot thôi. 

**Lưu ý: Khi thay SMBIOS mà các bạn bị như ảnh** (thường xảy ra với Laptop Dell)

![](https://lh5.googleusercontent.com/xUCfpvh_EqAhVtrfEf7h3sps49QGxqITt9rOTTZMIOXw87hmUTUYQS31kEZpSOSETEHxjqzZqouwXDb-rPjoqGsy8EoE5TPq4_8j27S5jeXfS2pf_hqQuVdFdADRwDSrJ5Oc4JD2=s0)

Thì chỉnh config lại như sau: 

- `Kernel ⇒ Custom SMBIOS Guide: True`
- `Platform Info ⇒ Update SMBIOS Mode: Custom`
- `Platform Info ⇒ Spoof Vender: True`

> Lưu ý: Khi chỉnh như vậy sẽ không thể dùng BootCamp.

## Clover

B1: Các bạn Gen SMBIOS như OpenCore.

B2: Check SMBIOS như ở OpenCore.

B3: Các bạn vào mục SMBIOS và chọn dấu

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.40.52.png?w=66)

sau đó chọn SMBIOS dúng với SMBIOS vừa Gen.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.39.40.png?w=1024)

Sau đó thay các mục:

- Serial
- System UUID
- MLB (chỉnh ở mục RT Variables)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.45.55.png?w=1024)

## **Fix en0**

**B1**: Các bạn mở [Hackintool]([headkaze/Hackintool: The Swiss army knife of vanilla Hackintoshing (github.com)](https://github.com/headkaze/Hackintool)) vào tab `System -> Peripherals` xem đã có tick en0 ngay phần Card `Ethernet/Wifi` chưa. 

**B2**: Nếu vẫn chưa tick thì các bạn chạy dòng code sau trong Terminal:  

```bash
sudo rm /Library/Preferences/SystemConfiguration/NetworkInterfaces.plist

sudo rm /Library/Preferences/SystemConfiguration/preferences.plist
```

**B3**: Tiếp các bạn vào tap PCl chọn ![](https://lh4.googleusercontent.com/2YxQuyuyzmvMfcfCFB9p8oWesnCvp3lUyn9uGPwnNB_jUXXkFmrU486siFkZqDwhFb_D8c2Mam97RCOdEcNHZHvSeUVjqWeGiJrO75Fv9FTvZ0gul7UpzkABSAWg3AEMm6X5VUnO=s0) và mở file.plist lên bằng ProperTree và tìm đến dòng `Ethernet Controler` và copy tên của phần chứa nó như của mình sẽ là `PciRoot(0x0)/Pci(0x1f,0x6)`.

> link tải ở trên | các bạn có thể dùng plist edit pro hoặc xcode v.v 

![](https://lh6.googleusercontent.com/ZjieM0MgxX65MsZUg59PmNSDKm59iHacPjf9Lccr0Cy5ODUeDijNQ6-w_UiusX34wKDUJzP9pFkzetd_6FWw9weOMupCwz5PhftY_Y81cOxb7B0PQAoWaDWKmKLuiYqCSwSyilWO=s0)

**B4**: Create thêm 1 dòng ở mục `config.plist -> DeviceProperties` với tên là dòng bạn mới copy như của mình sẽ là `PciRoot(0x0)/Pci(0x1f,0x6) `với định dạng là `Dictionary`. 

**B5**: Add thêm 1 dòng vừa add có tên là 

`built-in|data|01` 

**B6**: Save lại và Reboot thôi. 

> Lưu ý: Nếu bạn đã làm tất cả mà en0 vẫn chưa được tick thì bạn hãy thêm SSDT và Kext sau  [NullEthernet.kext](https://bitbucket.org/RehabMan/os-x-null-ethernet/downloads/) , [ssdt-rmne.aml](https://github.com/RehabMan/OS-X-Null-Ethernet/blob/master/ssdt-rmne.aml) và Snapshot lại như thế là done. 

## F**ix** ROM

**B1**: Vào System Preferences tiếp vào mục Network chọn Advance Copy đĩa chỉ MAC của bạn. 

![](https://lh4.googleusercontent.com/VDCSpZNhJu7rsCVgAgHo216AQwco580jCYzzwADUn5kV38zjg1mc5zsrl-rqI0BdYOQw4SXkFsXwOxzQVMZA-NcPLWk7cTdD_Gc17Rmxr6xicjI7iz3NNN3xWyzG9OWR1i8YTs-M=s0)

**B2**: Các bạn vào config mục PlatformInfo ⇒  GENERIC ⇒  ROM paste mục MAC Address vào rồi xóa các dấu “:” đi như của mình sẽ là (ở Clover các bạn chỉnh ROM ở mục RT Variables).

![](https://lh5.googleusercontent.com/faNLTOdVbOatEM2APvn1nB6KS0l_ZBXTRg1RbhzTXaQKevyP_-IqWUl1yHwKpOr2JeddUcXEoT2W4588P6I5Bmpd5KF85VjCnyLCOqR8HYlHUHlNVrG_x2Z1U4TKsDv_ZNnYoPMe=s0)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-13.37.44.png?w=1024)

**B3**: Save và Reboot

***Lưu ý: Nếu bạn đã làm tất cả nhưng iMessage vẫn không hoạt động thì hãy Call Apple nhờ giúp đỡ theo số 18001127 hoặc Chat với Apple theo links sau*** [***Apple – Support – Solutions***](https://getsupport.apple.com/) ***(bạn phải để vùng là United States thì mới xuất hiện ô Chat | main source :*** [***Fixing iMessage and other services with OpenCore | OpenCore Post-Install (dortania.github.io)***](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html) ***| nhớ là phải nói là RealMac nhé không thì sẽ bị liệt vào blacklist đấy)***.

## ***Tips hướng dẫn ẩn tất cả các file Hackintosh đi***

Phần này dành cho những bạn muốn Call với Apple. 

B1: Nhấn tổ hợp phím Shift + Command + “.” 

B2: Tạo 1 Folder để chứa tất cả các File Hackintosh. 

B3: Rename Folder với bất kỳ tên gì mà bạn muốn nhưng lưu ý là thêm 1 dấu “.” vào trước tên mà bạn muốn đổi như mình muốn đổi tên nó thành “hackintosh” thì mình sẽ Rename lại là “.hackintosh”. 

B4: Nhấn lại tổ hợp phím Shift + Command + “.”
