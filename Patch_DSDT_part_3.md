# Patch DSDT Phần 3

## ***Cách tạo Patch Rename***

B1: Xác định Method cần Rename.

- Tuỳ vào mỗi SSDT thì các bạn sẽ cần add patch rename tương ứng
  - Lý do cho điều này là vì khi dùng SSDT thì các bản và trong SSDT sẽ không thể ghi đè lên DSDT. Do đó khi thì các bản patch lỗi sẽ không được áp dụng
  - Để khắc phục điều này các bạn sẽ tiến hành rename các method trong DSDT khi này thì SSDT đã có thể ghi đè lên DSDT. Tức các bản patch lỗi sẽ được thực hiện

> Nếu SSDT của bạn sử dụng 1 method không có trong DSDT (thêm method) thì các bạn không cần rename

Các Patch Rename thông dụng: 

- GFX0 -> IGPU : Dùng để Enable GPU khi các bạn dùng với SSDT-iGPU.
- SAT0 -> SATA : Kích hoạt Sata Controller với những cái không hỗ trợ dùng kèm với SSDT SATA.
- EHC1 -> EH01 : Để Hotpatch các cổng USB.
- EHC2 -> EH02 : Để Patch các cổng USB.
- XHCI -> XHC : Để Enable các cổng USB 3.0
- HECI -> IMEI : Để Fake IMEI.
- MEI -> IMEI : Để Fake IMEI.

B2: Các bạn Dump DSDT như hướng dẫn [ở đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)

B3: Vào terminal gõ lệnh: 

`cd [kéo thư mục chứa file DSDT vào]`

`[kéo file iasl62 vào] -l -dl DSDT.aml`

B4: Mở file DSDT vừa biên dịch và nhấn tổ hợp phím `Command +F` sau đó gõ tên Method vào. Ví dụ `FBST` và cần Replace thành `XBST` các bạn sẽ được đoạn code như sau: 

```
           Method (FBST, 4, NotSerialized)

            {

                And (Arg1, 0xFFFF, Local1)

                Store (Zero, Local0)

  FF74: 14 43 12 46 42 53 54 04 7B 69 0B FF FF 61 70 00  // .C.FBST.{i…ap.

  FF84: 60
```

B5: Chuyển `FBST` thành dạng thập lục phân :

- Mở Hackintool lên và gõ `FBST` vào ô ASCII.
- Sau đó gõ cụm `FBST` vào và quan sát ô Hex. 

![](https://lh5.googleusercontent.com/i9zxuhKJARx9e22CpTxldE5Sc1lAYDy64uIEK1rorCSKIF-MMy4Ck3cTU9H240Esg_rjERoPSmUNle7Qt7MVcp-N5nPioyGuv8pIoyDK6y2JREtamDBnIQxToncFvF2RQXHA2uQz=s0)

(ta có `FBST= 46 42 53 54`)

B6: Vào lại đoạn code vừa lấy ở bước 4 và lấy cụm số vừa nhận được ở bước 5 lấy thêm 2 chữ số sau 2 chữ số cuối của bước 5 các bạn sẽ được `46 42 53 54 04` (tại sao lại có `04` là vì để không injects toàn bộ method mà chỉ injects phần cần thiết | nếu các bạn cần injects toàn bộ thì chỉ cần chuyển tên cần Rename về thập lục phân là được không cần tìm trong DSDT).

B7: Kiểm tra lại bằng cách nhập vào ô Hex dãy số vừa nhận được ở bước 6 và quan sát ô ASCII (sau đó chuyển tương tự với phần replace ta được `58 42 53 54 04 | X=58`).

![](https://lh6.googleusercontent.com/h0pf2kEIE_7WUzOebKMeKXuLgDateFQze7jf7WMdKLgxOM-wZT1U4e08BQ4eAEM2DesleaazyRnjaVSh1_5RaXVi_W1Bke9gwRn5bstOlpgSL9uyhrvFXU4jsWNsWSicREvceJ4e=s0)

B8: Add Patch vào config ⇒ ACPI ⇒ Patch 

```
comment | string | chang FBST to XBST 

Enable | bootclean | true 

Count | number | 0

Limit | number | 0

Find | data | 46425354 04

Replace | data | 58425354 04
```

**Lưu ý : Các nguồn tham khảo** [**[Guide] Using Clover to “hotpatch” ACPI | tonymacx86.com**](https://www.tonymacx86.com/threads/guide-using-clover-to-hotpatch-acpi.200137/) **|** [**Hot patch Clover, chỉnh độ sáng màn hình, tinh chỉnh hệ thống hackintosh**](https://hackintosh.vn/patch-dsdt)

## ***Cách tạo*** SSDT

### 1 số SSDT thường dùng:

- SSDT-RMCF.dsl: Dùng để cung cấp thông tin cho SSDT khác bổ trợ là chính.
- SSDT-RMDT.dsl: Được dùng với acpidebug.kext để hiện thị các log thông qua console (không khả dụng ở Big Sur).
- SSDT-XOSI.dsl: Được dùng để fix Trackpad I2C và giả lập Boot OC như Boot Windows (thường được dùng kèm patch rename _OSI ==> XOSI).
- SSDT-IGPU.dsl: Dùng để injects thông tin iGPU thông qua SSDT thường dùng kèm với SSDT-RMCF.dsl và SSDT-SKLSPF.dsl (nó mặc định iGPU được đặt là iGPU trong 1 vài trường hợp bạn cần rename gfx0 ==> igpu).
- SSDT-SKLSPF.dsl: Được dùng kèm với SSDT-IGPU.dsl để injects thông tin id iGPU.
- SSDT-IMEI.dsl: Được dùng để Fake IMEI trên iGPU HD4000 7 Series.
- SSDT-PNLF.dsl: Được dùng để Fix Backlight thường được dùng kèm với Kext SMCLightSensor.
- SSDT-LPC: Dùng để buộc apple LPC tải các id cho LPC (nó mặc định LPC được đặt thành LPCB).
- SSDT-SATA.dsl: Để Enable bộ điểu khiển Sata.
- SSDT-DDGPU.dsl: Dùng để disable dGPU không hỗ trợ.
- SSDT-SMBUS.dsl: Dùng để injects DVL0 trong các CPU Gen 2 và Gen 3 (Sandy Birdge, Ivy Birdge).
- SSDT-GPRW.dsl và SSDT-UPRW.dsl: Dùng để Fix Sleep (bị wake ngày sau khi vừa sleep | thường kèm theo các patch rename grw==> xprw, uprw ==> xprw).
- SSDT-LANC.dsl: Dùng để fix lỗi về Sleep (bị Wake ngay sau khi vừa Sleep do Ethernet).
- SSDT-PTSWAK.dsl: Dùng để fix lỗi Sleep (không thể Wake).
- SSDT-DEHCI.dsl: Dùng để vô hiệu hóa bộ điều khiển EHCI (khi đã đổi tên thành EH01 và EH02).
- SSDT-XCPM.dsl: Để set plugin type =1 giúp patch powermanagenment.
- SSDT-EH01.dsl: Dùng để injects USB 2.0
- SSDT-EH02.dsl: Dùng để injects USB 2.0
- SSDT-XHC.dsl: Dùng để injects USB 3.0
- SSDT-ALS0.dsl: Dùng để Fake ALS0 (cảm biến ánh sáng) để khắc phục sự cố ánh sáng không khôi phục sau khi khởi động.

### Xác định các phần thay đổi

B1: Các bạn Dump DSDT ra

B2: Apply các Patch vào (VD mình sẽ Apply Patch IMEI | nhớ chuyển về .dsl)

B3: Tải [DiffMerge.app](https://sourcegear.com/diffmerge/)

B4: Mở app lên và chọn nút ![](https://lh3.googleusercontent.com/QydjL452f2gXwfxE2K3BnXPBnof3CXqzeIitQ0sHBI4iQFwo6lRY63Ha-XXR06t5XPYAB7bEBQQcme8y4iZJ3pqZ8aVUBDhl-GOGrn96N5Q-z5kcsJrlCNWFWDmCIzWL-yegNhXe=s0)

B5: ![](https://lh4.googleusercontent.com/vMxYDfxhnz3SyqpIelGN69LDTDU8NNd8WcAsZABSoj0dBgnsuTT4VnHMnQhgtS5JmnCmJRuvfkCmYvGuc6Xm4MS2gSIaiAQYMW5kStqb7tu6kqhuCcl1IA5p4k7GT0qNCGnrKdU9=s0)

- Chọn mục 1: File gốc (DSDT gốc).
- Chọn mục 2: File đã sửa đổi (DSDT đã Apply Patch).

B6: Sau khi chọn xong ta sẽ có như sau:

![](https://lh3.googleusercontent.com/hcL5xw9J1lMdr1PxyPLAcNFkR50KTEbGn1DzAJydDwWKDji4_4ORwZainPbJMa8RhlR8aNpuRQOkScRjC1s2lta597I0PxO4_ho0coBy8C-9kzmzSceD5wccnJjNTCKrupq9z0TC=s0)

Mục 1: Các dòng thay đổi.

Mục 2: Nội dung thay đổi ở file gốc.

Mục 3: Nội dung thay đổi ở file đã sửa đổi.

### ***Tiến hành viết SSDT :***

B1: Mở Maciasl và bấn tổ hợp phím Command + N. 

B2: Tạo phần tiêu đề cho SSDT.

```
DefinitionBlock(“”, “SSDT”, 2, “hack”, “nhận tên ssdt (giới hạn 9 chữ số)”, 0)

{

}
```

![](https://lh3.googleusercontent.com/NGWkkbEOPNcNqq_gW5SQMOXMt4hGGNQi0bXNLvcY-cJlvQbuGygKpL0ntWE_RJV7eQM0Vs9w4Ms0S7KqrssS6ow-lfKt5RIYA_J8PMN-wZs9RcGdM7uAXJV4ykcmHUTyO0DQh1Ec=s0)

B2: Copy các Method và Device của những phần thay đổi vào SSDT vừa tạo (nên Copy Scope).

Ở bước này các bạn nhìn vào tree của DSDT để Copy ![](https://lh5.googleusercontent.com/LwrqhXZteHS5cTEAyQ69GguNfBHcyuPyueQs9whBbgeVgOy9-B4rmNt_Abi4ALyYq7Y9uD6_BAsN_DG1md8CSwoaRzl_AZedOF6kkj-CACidiK7Ya0JR3jQfoqdYuK9t4PYY3Sqo=s0)

![](https://lh5.googleusercontent.com/R4mvurflNTJilXMaZ8ZyR1qXegco1BcNg6mpBxZx5JCDJKQGY0B1SqLlUmLlHBZTZHSmlElrbU87I1aJQspyzp8l_YyQOod5o4yIsCKSq27un1pjSoU5TpBV8jAE4efSiGcnHCjl=s0)

B3: Check lỗi các bạn bấm vào dấu ![](https://lh4.googleusercontent.com/-dXDMALBFeF1yuJJ-SlQ6MoY_utqyxGiG7lcAF-CpJpl5Qutrf-HepeAYL4BtIL14hyaXtsmleDKio2iGd8b3CQb9cooZBbN3_gkY-4Sy6_GW_TVyXBTXNaX53U7_ugck6ody_1P=s0)  

Nếu như thế này thì đã có thể sử dụng. ![](https://lh6.googleusercontent.com/9e8wXwSYEMocRA5SeifpzbFUNrPQ6s1WttH3AqgsyPUBI7Z2moKRdqtiowaoIXXuTkLUvSuHbVjn3PiBZ4Z1WaRYPif5oX07o15Fsvkq-6O6jxUzDbobTeATfb4qIfVJsoTTxou2=s0)

Còn nếu sau khi check như hình dưới thì ta cần phải fix Error.

![](https://lh6.googleusercontent.com/5t1qdsw3C6fSeu7Qsu6rxMyCYMPJJ06uu80YGu5WRnW9bAU8MRYLKrCxAPskgpL8-gBmzhPapbRrOBbtUpadHETLSM1fTP8QZ_hOQg_aaDJUFAe1hELKybVZQsef-qAx54mt1RP1=s0)

B4: Khai báo các giá trị nếu gặp lỗi ở bước 3 

- Nhập cấu trúc như sau:

```
External (nhập đường dẫn giá trị cần khai báo, nhập các đối tượng khai báo).
```

![](https://lh5.googleusercontent.com/F6LTEpv5zcTH4FCmw0gtYsJK6QYoGGaXRt1SPNfOxUQqJdYJqAB8aHOjMpC86gW-cqGd8GCYFBTq-faEngqi1PbDv3UtmSQ9POtHEIbuR8vQC4J-BFmK7aHmUqA-CQug6-TV5zUz=s0)

- Bấm lại nút ![](https://lh5.googleusercontent.com/eTb45nzPOcH0NpdpyTGuaS2jTBF8RdEi9lzwr73605VtuymnzT5EAe-obocYFXK8FD4hZUefkg2mDrEQVkSFhHE19onKfynXFZQOpyfUm3Wsa5qf2K5bMLQGKFMmZ8JGWVmbK0ya=s0) để check lỗi.![](https://lh6.googleusercontent.com/jsPS7GLZlxBFPrFYhBG_iCzjBt8rl0ah9PG_4lfBQicT-DPrwCblf7j5u63iJtzevop3lvXVnhJGrUQc5g8Iv-G954Bcii01ZQN56oAKjTHEue2QRYBVRV8qlsuTI4mWxWuiSQbv=s0)
- Ta có các giá trị Objects:
- DeviceObj (thường để khai báo mục device và scope).
- MethodObj (thường để khai báo method và các mục nằm trong method đó).
- PkgObj (thường để khai báo các package).![](https://lh6.googleusercontent.com/Sqr8kbOKrJOHnOCmLWnwrGIuWPyuWcQgDuus8lcKiFvL7kBG_n5kYY1yD8JywRnHD39MvmgePNr_V8GsmqiMHe3kqPy2QW9yinkCO5ew6LnR7kVteyWY5GU2FnQ2dCvgTfiN3q-S=s0)
- FieldUnitObj (dùng để khai báo các biến lưu ram).

![](https://lh6.googleusercontent.com/WMd5TIhK8fY6RpcH4ZzAHUJt8D2acCVMisGTSQ_Lpd1-RqGPk_O7OAoI2xnr2l30AiOOKk3IN3znAmqEJXzWS2rIPMJe72wfcHrEJe5k8IbIA5Dc2m4loSnPhCYHlHi7x0OD7jW5=s0)

- 

- Đôi khi các bạn sẽ gặp dấu “^” này trong Method tên nó là Uper chức năng của nó là sẽ thay thế các giá trị ở trong scope vào method khi khi báo các giá trị này ta sẽ bỏ dấu “^” và thay bằng các mục Scope 1 dấu “^” là sẽ thay bằng 1 mục ở Scope VD. 

![](https://lh5.googleusercontent.com/vhEg3kfbrfK9NvYm-Fjz4btSFZMZpFvzF_tTLxLl1BbJgr2RNK8g9y6_B34bG5Z11PLSzrZ43ZuKTNLT_RHyT7uLCJWiXozu2Mt3oNVdlFQj78pQg3PF-b3pR5lLo-7b03wee5y1=s0)

Có Scope là ![](https://lh3.googleusercontent.com/KBCb1khXgcMEqMBuUubpjZP-ig9gk49Y78Jz48Im-l-g4SjHMy7SaTpo4ZXarDrMO1iQ10MW5xLgJVmovEuUmF5a-hnohPjPnbkhKLW1Cr5SYvxCukoWqnv-v_qno25sN0-GllXQ=s0)Thì khi khai báo ta sẽ có cấu trúc như sau

“^^” = “_SB.PCI0”

⇒ “^^LPCB.EC0.BATP” = “_SB.PCI0.EC0.BATP”

Đối với ví dụ này ta sẽ External vào như sau: 

External (_SB.PCI0.EC0.BATP,  MethodObj)

> Phần phân loại object chúng ta sẽ tìm hiểu sâu hơn ở phần dưới

## Quy tắc import object

Ta có các object được import bằng external theo quy tắc kế thừa có nghĩa là:

DeviceObj --> MethodObj/MutexObj --> PkgObj --> FieldObj/OpRegionObj --> FieldUnitObj/IntObj

Ngoài ra IntObj còn có thể trực thuộc ở root scope

Để tìm được kế thừa của 1 biến thì ta sẽ tiền hành search khai báo của biến đó:

- DeviceObj có khai báo là `Device (variable)`
  - vd: `Device (EC)`
- MethodObj sẽ có khai báo là `Method (variable,`
  - vd: `Method (AR02, 0, NotSerialized)`
- MutexObj sẽ có khai báo là `Mutex (variable,`
  - vd: `Mutex (MUTX, 0x00)`
- PkgObj sẽ có khai báo là `Name (variable, Package`
  - vd: `Name (BIXT, Package (0x14)`
- FieldObj có khai báo là `Field (variable,`
  - vd: `Field (SMB2, ByteAcc, NoLock, Preserve)`
- OpRegionObj sẽ có khai báo `OperationRegion (variable,`
  - vd: `OperationRegion (SMB2, EmbeddedControl, 0x40, 0x28)`
- FieldUnitObj sẽ có khai báo là `Field () {variable, }`
  - vd: `Field (SMB2, ByteAcc, NoLock, Preserve) {PRT2, 8,}`
    - PRT2 là biến thuộc FieldUnitObj
    - Có thể nói các biến “kế thừa” (thuộc) FieldObj sẽ được khai báo là FieldUnitObj
- IntObj sẽ có khai báo là `Name (variable, )`
  - vd: `Name (RCBT, 0x05)`
  - Ngoài ra các biến “kế thừa” (thuộc) FieldObj cũng có thể được khai báo là IntObj

Ở trên là 1 số loại object phổ biến còn có những loại object khác hiếm gặp nên mình không liệt kê vào

**Lưu ý: đối với IntObj và FieldUnitObj các bạn có thể sử dụng thay được cho nhau như biến STAS của method AWAC. Tuy nó nằm dưới Field nhưng SSDT-AWAC vẫn import nó vô là IntObj. Tuy nhiên bạn nên dùng FieldUnitObj cho biến nằm dưới Field thì sẽ hay hơn.**

**Lưu ý 2: Khi sử dụng SSDT thì các method của nó sẽ không thể ghi đè lên DSDT. Do đó ta cần add patch rename để đổi tên method trong DSDT. Khi này thì các method trong SSDT mới có thể hoạt động**

**Source tham khảo** [**[Guide] Using Clover to “hotpatch” ACPI | tonymacx86.com**](https://www.tonymacx86.com/threads/guide-using-clover-to-hotpatch-acpi.200137/)
