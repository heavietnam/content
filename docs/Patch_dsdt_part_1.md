# Patch dsdt phần 1

## ***Dump dsdt***

### ***Hệ điều hành Windowns (khuyến khích)***

     **B1**: Down ssdt-time từ nguồn sau [GitHub - corpnewt/SSDTTime: SSDT/DSDT hotpatch attempts.](https://github.com/corpnewt/SSDTTime)

     **B2**: Bấm phím 8 sau đó enter 

     **B3**: Lấy dsdt từ thư mục của ssdt-time

![](https://lh5.googleusercontent.com/3YtJQMBP_rs6jRtn-PsyRTOe29lq8xdNBXqRjngYHXNuEJKuZfUAmbtIPA9lQZKMMXix-f2XnuZyyxnhnXS9xxunyg4g4Ucvj7xOZaWsNx_KwpuEUkudAqZWXOsw4Gr88S1EStDv=s0)

**Lưu ý : Nếu các bạn dual boot với win qua opencore thì dsdt sau khi dump sẽ mất đi sự thuần khiết (sinh ra nhiều lỗi hơn)**

### winpe

#### rw everything

B1: Tải [rw-evrything](http://rweverything.com/download/)

B2: Các bạn tiến hành cài đặt và mở app 

B3: Sau đó các bạn chọn vào menu Access ⇒ ACPI Tables ⇒ dsdt

B4: Sau đó các bạn bấm save và chọn nơi lưu 

B5: Tiếp các bạn đổi tên file từ đuôi .bin ⇒ .aml 

B6: Sau đó boot về mac và tận hưởng thôi 

#### acpidump.exe

B1: Tải [acpidump.exe](https://acpica.org/downloads/binary-tools) 

B2 : Mở cmd ra 

B3: [ kéo acpidump.exe vào ] -b -n DSDT -z

B4: Đổi extension của dsdt từ .dat ⇒ .aml 

## Hệ điều hành macos

### With opencore

#### Cách 1 : Dump bằng acpidump.efi

**B1**: Tải acpidum.efi từ nguồn [OpenCore-Install-Guide/acpidump.efi.zip at master · dortania/OpenCore-Install-Guide · GitHub](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/acpidump.efi.zip) sau đó bỏ nó vào mục EFI-tool rồi snapshot config

**B2**: Tại giao diện boot chọn acpidum.efi  

**B3**: Dùng open‌sell.efi  dán đoạn sau vào openshell.efi ( viết từng dòng không dán toàn bộ)

```
fs0: // replace with proper drive

dir  // to verify this is the right directory

cd EFI\OC\Tools

EFI\OC\Tools> acpidump.efi -b -n DSDT -z
```

Khi dán toàn bộ vào nó sẽ có dạng là

```
shell> fs0: // replace with proper drive

fs0:\> dir  // to verify this is the right directory

   Directory of fs0:\

   01/01/01 3:30p EFI

fs0:\> cd EFI\OC\Tools

fs0:\EFI\OC\Tools> acpidump.efi -b -n DSDT -z
```

**Lưu ý : Sau khi dump xong nó sẽ nằm ở mục EFI ⇒ OC ⇒ tool dưới dạng file .dat hay rename nó lại thành file .aml**

#### Cách 2 : SysReport Quirk ( khuyến khích

B1: Các bạn cần tạo 1 bộ efi debug như sau :

- Tải bộ [opencore pkg debug](https://github.com/acidanthera/opencorepkg/releases)

- Thay thế các mục sau vào efi đang sử dụng 

- bootX64.efi

- drivers

- opencore.efi

B2: Mở config lên snaps lại và tìm như hình 

![](https://lh6.googleusercontent.com/k_3zv83oQuGu2EEU1i-xk6Aqn_vlW7HtIDJz8f-9X6GUdoN-hoRga1NEvRAv91Wcu5BAnFV1r2EcLUgd5n4zftJRSaMc1l-tzg_GJA85cFFX6xPrr4YhczrnElJWIb-ZTlWEZWy9=s0)

B3: Boot lại vào ổ cứng 

B4: Các bạn sẽ thấy xuất hiện mục SysReport ⇒ ACPI

![](https://lh5.googleusercontent.com/DHiHPSsiG_klXenIfZgwXllxnEXwh3RIgAIu1FSSMM-760pSubbLzerXAqD0cQPsKV7RXEuCVd2uk_KJ-8UPQS8ag5AynOMdzAuU_otPyzBF_ccSdSefxapZx3PNDL_nxp0un1rz=s0)

![](https://lh3.googleusercontent.com/3z0J2PxCdKzjDtR7usSNQ3a7dUn-qgvYymJjQgSmMGdB6_0jDiM-4o3Jg03LmKV56HJ58mVYMq4Hu5rU4pmZEUIddR5VbWVRVcH7GLkdaB3iPQCKAh0zc0P_PTVWHCKbeMTZKH2Y=s0)

Sau khi làm xong các bạn sẽ được như hình 

![](https://lh4.googleusercontent.com/H5ZqduhdBqrFczNnkQq_sNicD-S-pvJs2RvqynIdm8wEAUbnCTdPLzp-qLqGZBHI3YFqWk-6EqEIz4444bJIw91hwx5wafulgjPVcv3rElPRr_9pznrzrskqPnEl9J6WeU8k99Lt=s0)

### with hackintool

B1:  Các bạn mở hackintool mục utilities 

B2: Các bạn ấn ![](https://lh3.googleusercontent.com/dfguZY9Ik7T8V-Asyqw-qrxS4FpJWYIDMe6Z49WQJErhryBwbJGxnoaeHl3tHW5ty5DhK0KruPMwtyH9h2B5xY4vnhyhXWU120_8sk4gt80Mtb2jNWpgJLUq26TvCRXh45h3pCUz=s0) 

B3: Chọn nơi lưu ( sau khi chọn xong sẽ được như hình )

![](https://lh3.googleusercontent.com/aHTbga7EWfVBM11xNzpn71A5QRfHaeI0YBm30OSX26M8ZBsVL3_HDsM8plK0nA2Cv4RFOaOaTJi_NI0jhpHEfs8Aafe6OHg2A6-QRwRm21LBBrTq98qYZ7RN4DcuLo2HDeakUrDw=s0)

B4: Mở folder chọn lưu ở bước 2 ra và copy file dsdt.aml ra nơi mà bạn muốn như của mình nó sẽ là ở desktop  

![](https://lh6.googleusercontent.com/Y1q-FciNEJhvvtsgnHbz_5c9JMvt9gcn5WvxXpEW1SglbqPZmUvNXp8bHIt0abbM_EwNBN6xFuB7NhV6Ozy7HUhLQQWc2x-rqpwK32xOUoEmxA0RL994Vt0LeNcr0SSBiShtzMQ1=s0)

### with patchmatic

B1: Tải [patchmatic](https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/)

B2: Nhập lệnh sau vào terminal 

cd + [ kéo folder muốn lưu ssdt vào ] [kéo patchmatic vào ] -extract

Sau khi làm xong các bạn sẽ được như hình 

![](https://lh3.googleusercontent.com/gDaVFvNeT_OTDv0TFpUUKDEdQICsaSd0g6nwlGEHXF0NLAHnXkPVAtkIFa8EhwMED3hF-mVcQ7q1aLRfH3dfdqtU4ZXr5_g_7VDmncKMiWaE5XBW0EAoUh8r4u4oeihgg40TcIf6=s0)

B3: Mở folder vừa chọn ở bước 2 sẽ được như hình 

![](https://lh4.googleusercontent.com/Q3eNYJzyRnQfazI_M-NulPKHe_KapLfzbNZITawpAeeGsHLu1gJ4A5Wo2k2rAxCwKCJ46LzzgWTf_yzSha0oYocqZR0O5uutL2QKEvVVllvyr10PAAx69xcdr2JcGiV-QL6p09tH=s0)

### with clover (khuyến khích)

B1: Tải efi clover [ở đây](https://drive.google.com/drive/folders/1fh5ZKcshCjLAp5SMH200Snd0DAvTmIDL?usp=sharing) 

B2: Cho efi vừa tải vào usb hoặc 1 phân vùng trên ổ cứng ( được format  với chuẩn fat32 )

B3: Boot vào efi vừa tải 

B4: Ngay giao diện boot các bạn ấn f4 ( hoặc fn +f4 )

B5: Boot lại vào macos các bạn vào efi clover mục ACPI ⇒ origin 

B6: Các bạn sẽ được như hình 

![](https://lh5.googleusercontent.com/Q7IceDENPqbwTFkXgEN9871vRGFDqBh-fLAiFZ9A5FrNJ4i3TbrCZ8sHb37862jHjxblYbWm1WkwcSh_G1e4d8A0Y7UUwP_PwkC6riuPRuUpV-E5cmBECDEO1h10PSQ5RIWMAWVD=s0)

**Lưu ý: Ở macos mình đưa ra khá nhiều cách dump nhưng theo cảm nhận của mình thì  cách dump bằng clover là chuẩn và trực quan nhất** 

## Hệ điều hành linux

### Cách 1 : with terminal

B1: Mở terminal ( nhấn alt + T )

B2: Gõ lệnh 

Cd + [ tên nơi các bạn muốn lưu file ]

sudo apt update

sudo apt install acpidump

sudo acpidump > acpidump.out

sudo acpixtract -a acpidump.out

B3: Sau khi nhập các lệnh trên thì terminal sẽ dump tất cả các file acpi ra ngoài nơi mà các bạn chỉ định ở đây các bạn sẽ tìm file dsdt và đổi tên file từ đuôi .dat ⇒ .aml

### with ssdt-time

B1: Tải [ssdt-time](https://github.com/corpnewt/SSDTTime)

B2: Chọn phím 4 

B3: ssdt sẽ được dump tự động vào mục Results![](https://lh6.googleusercontent.com/Li54WCXY7_Y15H-HtXL_Ur9rFkXlMTvni_MQnWltCHKUwpS1Tx_InJ0mYwVZ8XtIdMJlwOOPC6_L1sAZCh5zoUuHNfDH2XIpfOTwZr0b-uqL0okK76PAMFF_HRFnDMn61D3G12D8=s0)

**Lưu ý: Do ở đây mình mod giao diện giống mac** 

**Lưu ý 2: Mình khuyến khích các bạn dùng sysreport và tính năng dump của clover ( vậy khi nào thì dùng patchmatic ? Dùng patchmatic sẽ apply tất cả các hotpatch vào dsdt vì vậy sẽ mất đi bản chất gốc của dsdt nó sẽ sinh ra 1 số lỗi còn nếu các máy dùng patchmatic để dump dsdt mà không có error thì cứ dùng patchmatic để dump )**

## ***Biên dịch dsdt***

### AML to DSL

B1: Tải [maciasl](https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/)

B2: Chọn show package contents ⇒ contents  ⇒ macos  ( như hình )

![](https://lh3.googleusercontent.com/vxlfGSeeSldAmAeiA2hzupFHa_XavD3Krv6p6JDQY81YBZh0_Xw-MVX_0A8SWENZSTQ6bd_EYAc1Rqy9-FqxRiflBbrtabaZElvOSUQa10NbPXsHdF_AvaiUSdq_NB05dRTcudTk=s0)

![](https://lh6.googleusercontent.com/Tuu-JT2IjfcstvBIeMuONaTTrdRDUj4sK64zEN5zaatiuNop5d99FcQFN5sj41ZAqNPcip4A_7AyJEugwvAR6Ft6fFxBSswPC_msFKn3in-jbO4mlZi-GLMMfkbhh1N8ertqgRTM=s0)

B3: Copy file iasl62 vào thư mục lưu dsdt ( như của mình là extract )

![](https://lh5.googleusercontent.com/OEkCeGPzc5pyTlTG93A3SBZZzX_vRSbBZhicHJNDvGVPj1SyZEHc9ob4X8YSAh3F-Oj_DY3UpNyUODpmSLZLPDr8uxpXGrVe0z-CfLOAJIaiUir1jik7IS2DaiPZIsGCBdw_fbh4=s0)

B4: Copy đoạn code sau vào bộ nhớ tạm ( command +c )

```
External(MDBG, MethodObj, 1)

External(_GPE.MMTB, MethodObj, 0)

External(_SB.PCI0.LPCB.H_EC.ECWT, MethodObj, 2)

External(_SB.PCI0.LPCB.H_EC.ECRD, MethodObj, 1)

External(_SB.PCI0.LPCB.H_EC.ECMD, MethodObj, 1)

External(_SB.PCI0.PEG0.PEGP.SGPO, MethodObj, 2)

External(_SB.PCI0.GFX0.DD02._BCM, MethodObj, 1)

External(_SB.PCI0.SAT0.SDSM, MethodObj, 4)

External(_GPE.VHOV, MethodObj, 3)

External(_SB.PCI0.XHC.RHUB.TPLD, MethodObj, 2)
```

B5: Gõ lệnh sau và terminal ( sau khi làm xong sẽ được như hình )

cd + [ kéo folder lưu dsdt vào ]

pbpaste>refs.txt

![](https://lh6.googleusercontent.com/28fTnQzt65XKqbMNVcI3h3RHKBc5jjWgNUeuOEINLNTlx782-Q-4cHF8MnxYYy4Y7RmculCSmGSLRp4UGDJ6Ytop22mFoWW4GG_I_0BRXPp_rpl7X3RxzQRXU3gGyMsg8VhmLYUJ=s0)

B6: Gõ lệnh sau vào terminal 

cd + [ kéo file chứ dsdt vào ] [ kéo file iasl62 vào ] -da -dl -fe refs.txt DSDT.aml SSDT*.aml

**Lưu ý: Ở đây mình hướng dẫn biên dịch file dsdt với refs.txt sẽ giúp giảm các lỗi phổ biến . nếu bạn nào cảm thấy không thích refs.txt thì các bạn nhập code sau vào terminal** 

**[ kéo file iasl62 vào ]** **-da -dl *.aml**

**Lưu ý 2: Nếu các bạn cần biên dịch 1 file ssdt nào đó hoặc dsdt mà muốn dùng refs.txt thì dùng code trên sẽ ta sẽ bị lỗi vì code trên áp dụng khi bạn có 2 file dsdt và ssdt cần biên dịch nếu chỉ biên dịch 1 file ta nhập code sau ( ơ đây đang nói là dùng phương pháp refs.txt )**

- **Ssdt : `[ kéo file iasl62 vào ] -da -dl -fe refs.txt SSDT*.aml`**
- **Dsdt : `[ kéo file iasl62 vào ] -da -dl -fe refs.txt DSDT.aml`** 

### DSL to AML

B1: ấn comlie

![](https://i.imgur.com/mlAru41.png)

B2: chọn `File -> Save as`

![](https://i.imgur.com/ftj2NmU.png)

B3: Sửa file formart lại thành `ACPI Machine language Binary`

![](https://i.imgur.com/dpMrREJ.png)

B4: Ấn save
