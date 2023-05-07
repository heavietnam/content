# Fix trackpad

## ******Tìm hiểu****** chung

Đâu tiên ta phải biết, giao thức trackpad là gì? Giao thức trackpad chính là cách trackpad được kết nối với phần còn lại của máy tính, hiện tại có 2 giao thức chính là giao thức `PS2` và giao thức `I2C`

- Giao thức `PS2` thường được dùng cho các máy đời cũ từ Intel 6th gen trở xuống, patch khá đơn giản chỉ việc cài kext vào thôi.
- Giao thức `I2C` thường được dùng cho các máy đời mới hơn từ Intel 6th gen trở lên, hỗ trợ nhiều thao tác hơn nhưng đòi hỏi kỹ năng patch DSDT
- Tiếp theo các bạn cần xác định `Satelite type` của trackpad bằng [AIDA64](https://www.aida64.com/downloads) hướng dẫn chi tiết [ở đây](https://heavietnam.ga/2022/04/29/cach-xac-dinh-phan-cung/)

> Vẫn có một số máy gen 8 dùng ps2, ở trên chỉ mang tính chất khái quát

## ***PS2 trackpad:***

> Các bạn hãy [patch pin](https://heavietnam.ga/2021/09/29/iv-patch-pin/) trước khi làm bất cứ điều gì theo guide này

### **Synaptics PS2 :**

**B1**: Download kext `VoodooPS2controller.kext` theo link sau: [Releases](https://github.com/acidanthera/VoodooPS2/releases)

**B2**: Download kext `VoodooRMI.kext` the‌o link sau: [Releases](https://github.com/VoodooSMBus/VoodooRMI/releases)

**B3**: Vào folder `Contents/Plugins` của `VoodooPS2Controller.kext`, xóa `VoodooPS2Mouse` và xóa  `VoodooPS2Input`, thêm kext `VoodooSMBus.kext` (nếu cần mới thêm, lấy th‌eo link sau: [Releases](https://github.com/VoodooSMBus/VoodooSMBus/releases))   
![](https://lh5.googleusercontent.com/Hsgs69vD1_IFsbLa6kfDW1qt87WUuo4q38szDy_wfwPdzUB8FIvsVN1wkWQv83j0nX55VuY3xdP6mjZOh2tVen0McFiMjUH_Oy1G1xnLvd5zN_IT5t2LCwNzrcARQB7n0WGJL2z6=s0)

**B4**: Dùng [ProperTree](https://github.com/corpnewt/ProperTree) để mở config.plist và chọn `OC Snapshot` (hoặc `Clean Snapshot`)

**B5**: Kéo xuống phần `Kernel` để tìm và `Enable` (bật True) `VoodooInput` của `VoodooRMI.kext` và `RMI SMBus` (có thể xóa `VoodooRMI I2C` nếu các bạn không cần).

**B6**: Khởi động lại máy.

### Elan PS2:

**B1**: Tải xuống kext từ nguồn [tại đây](https://github.com/BAndysc/VoodooPS2/releases/tag/elanps2.v1)

**B2**: Snap‌hot config bằng [ProperTree](https://github.com/corpnewt/ProperTree) 

**B3**: Restart máy 

> Hầu hết các giao thức khác bạn chỉ việc patch cho hiển thị phần trăm pin và thêm kext [voodoops2](https://github.com/acidanthera/VoodooPS2/releases) là sẽ lên được

## ***Fix i2c trackpad:***

### Tìm Hiểu Chung:

I2c là 1 giao thức trackpad mới đầy mạnh mẽ và tiềm năng. Tuy nhiên để patch được multi gesture trên nó đòi hỏi 1 kỹ năng patch DSDT. Tuy nhiên nó chỉ đang chú ý đối với những service hackintosher. Bởi lẽ hầu hết các i2c trackpad chỉ cần gắn GPI0 là sẽ hoạt động. Vậy GPI0 là gì và để enable được multi gesture trên i2c chúng ta cần phải làm những công đoạn gì?

- Gắp GPI0
- Gắn ACPI ID
- Pin List

À còn 1 phần nữa đó chế độ chạy của i2c gồm 2 chế độ:

- Polling mode: loại sử dụng về phần cứng nên nó sẽ ăn tài nguyên nhiều hơn
- Interrupt mode: loại sử dụng về phần cứng nên nó ăn tài nguyên ít hơn

Nhiều hackintosher thích interrupt hơn polling nên ở cuối bài viết mình sẽ hướng dẫn cách chuyển từ polling mode sang Interrupt mode các bạn kiên nhẫn đọc hết bài từ trên xuống dưới để hiểu hết nội dung nhé.

Tiếp đó là Satelite type của i2c bao gồm các loại:

- Synaptics
- HID
- FTE
- ELAN
- AtmelMXT

Trong đó HID là loại phổ biến nhất. Bạn cần xác định Satelite type chính xác để sử dụng kext voodooi2c thích hợp

### Chuẩn bị:

B1: Các bạn cần xác định xem i2c của bạn đang thuộc Satelite type nào xem hướng dẫn chi tiết [tại đây](https://heavietnam.ga/2022/04/29/cach-xac-dinh-phan-cung/)

B2: Tải voodooi2c [tại đây](https://github.com/VoodooI2C/VoodooI2C/releases)

B3: Các bạn sẽ tiến hành thêm kext những kext sau vào EFI ==> OC ==> Kext (hoặc EFI ==> Clover ==> Kext ==> Other)

- VoodooI2C.kext
- VoodooI2cSatelite.kext
  - Những kext voodoo i2c có tên trùng với Satelite type của bạn

B4: Snapshot (Nếu OC) xem hướng dẫn snapshot [tại đây](https://heavietnam.ga/2021/09/29/tim-hieu-ve-hackintosh/)

B5: reboot

### Gắn GPI0

B1: Tải ioreg [tại đây](https://github.com/vulgo/IORegistryExplorer)

B2: Mở Ioreg lên và tiến hành Search `GPIO`

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/gpio-enabled.c84356b7.png)

Như này là GPIO đã được gắn

Nếu `GPIO` chưa được gắn các bạn sẽ tiến hành làm như sau:

B2: Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)

B3: Search `Device (GPI0)`

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/gpi0-2.8c3726d3.png)

Ở đây mình sẽ chia làm 2 mục nhỏ là đối với những bạn hot patch và static patch

- Static Patch

các bạn sẽ tiến hành apply patch sau

```
into method label _STA parent_label GPI0 replace_content begin
Return (0x0F)
end;

into_all method code_regex If\s+\([\\]?_OSI\s+\(\"Windows\s2015\"\)\) replace_matched begin If(LOr(_OSI("Darwin"),_OSI("Windows 2015"))) end;
```

- HotPatch

B1: các bạn sẽ cần quan sát `method _sta` ở `device GPIO` trong DSDT

```
Method (_STA, 0, NotSerialized)
{
    If ((SBRG == Zero))
    {
        Return (Zero)
    }

    If ((GPEN == Zero))
    {
        Return (Zero)
    }

    Return (0x0F)
}
```

Ở đây ta sẽ thấy có 2 method là `GPEN` VÀ `SBGR` ta chỉ cần sửa giá trị của 1 trong 2 method này thì Method `_Sta` sẽ hoạt động. Bởi ta cần Method `_sta` trả về giá trị là 0x0f nhưng nhìn vào đoạn ví dụ trên ta sẽ thấy

- Nếu `SBRG=zero` thì `method _Sta` sẽ trả về giá trị là `zero`
- Nếu `GPEN=zero` thì `method _Sta` sẽ trả về giá trị là `Zero`

Do đó để Method `_Sta` trả về `0x0f` thì ta phải chỉnh sửa giá trị của method `SBRG` và `GPEN` Tuy nhiên để đảm bảo `GPIO` hoạt động bình thường ta sẽ không nên đụng chạm vào `SBRG` vậy ta sẽ tiến hành chỉnh sửa giá trị của `GPEN` cụ thể ở đây là ta sẽ chỉnh cho giá trị `GPEN` thành 1 thì method `_sta` sẽ trả về là `0x0f`

Nhưng không phải máy nào cũng vậy. Để cho các bạn hình dung rõ hơn mình sẽ cho thêm 1 ví dụ nữa

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/gpi0.b0e0b8d8.png)

```
Method (_STA, 0, NotSerialized)
{
    If ((GPHD == One))
    {
        Return (0x03)
    }

    Return (0x0F)
}
```

Ở đây ta sẽ thấy sự khác biệt

- Nếu method `GPHD` trả về giá trị là 1 thì method `_sta` sẽ trả về là 0x03 nhưng giá trị để enable là `0x0f`
- Vậy ta sẽ cần chỉnh method `GPHD` về `zero` thì method `_sta` sẽ trả về là `0x0f`

B2: Ta sẽ edit [SSDT-GPIO](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/decompiled/SSDT-GPI0.dsl)

![](https://i.imgur.com/bManIm6.png)

```
// Source: https://github.com/daliansky/OC-little
DefinitionBlock("", "SSDT", 2, "DRTNIA", "GPI0", 0)
{
    External(\GPEN, FieldUnitObj)

    If (_OSI("Darwin"))
    {
        \GPEN = One
    }
}
```

Ở đây ta sẽ cần phải chỉnh mục này

```
If (_OSI ("Darwin"))
{
    \GPEN = One
}
```

- Nếu như method _sta call qua 1 method khác không phải `GPEN` thì các bạn sẽ tiến hành đổi tên thành method đấy
- Tiếp theo các bạn sẽ cần đọc DSDT để chỉnh giá trị tương ứng
  - như ở ví dụ 1 ta sẽ set là `one`
  - Ví dụ 2 sẽ set là `zero`

```
If (_OSI ("Darwin"))
{
    \GPHD = Zero
}
```

B3: Thông thường các thiết bị i2c sẽ kiểm tra chúng có đang chạy trong Windows hay không trước khi tự kích hoạt. Tương tự như GPIO ở đây chúng ta cũng sẽ làm việc trên method `_sta`

ví dụ _sta

Các tốt nhất cũng như nhanh nhất cho việc giả lập này đó chính là sử dụng `SSDT-XOSI` kèm patch rename:

- `_OSI to XOSI` method
  - B1: Các bạn down [SSDT-XOSI.dsl](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/decompiled/SSDT-XOSI.dsl)
  - B2: apply patch rename [OSI to XOSI](https://za7o7cw6-my.sharepoint.com/:u:/g/personal/hoanglong_coursedeals_org/Ea8SlxMbxlVHtrpzqXk-S9gBW2mS-50nx2H0PGeKE4GB-g?e=xkbO1Z) sau vào file config.
    - Nếu là máy dell thì các bạn sẽ cần add patch rename [OSID to XSID](https://za7o7cw6-my.sharepoint.com/:u:/g/personal/hoanglong_coursedeals_org/EZjpfVdbTV5BiFSlQaRWdLIBKICA1k4rGZ1CBK3F-zyH9A?e=SlTDpl) trước patch rename `OSI to XOSI` để tránh rename nhầm method `backlight keys`

> Còn 1 các nữa là `Create OSYS Variable Under I2C Scope` nhưng mình thấy không cần thiết nên mình sẽ không hướng dẫn cách này

> 1 lưu ý nữa là nếu các bạn đã làm đúng mà GPIO vẫn ko được gắn thì các bạn có thể thử add boot-arg sau vào `-vi2c-force-polling` nhằm force polling mode khi dùng arg này bạn sẽ không thể chuyển quan `Interrupt mode`

### Gắn ACPI ID i2c device

B1: các bạn sẽ cần boot vào win tìm đến mục `Device Manager ==> Human Interfaces Device ==> I2C HID` device để xác định ACPI ID

![](https://i.imgur.com/VM2IjPW.png)

Ở đây ta có ACPI ID là TPL0

```
// Các ACPI ID phổ biến:
    1. Touchpads - TPDX, ELAN, SYNA, CYPR
    2. Touchscreen - TPLX, ELAN, ETPD, SYNA, ATML
    3. Sensor Hubs - SHUB
```

Thông thường ACPI ID sẽ được tự động gắn nhưng cũng có 1 số trường hợp ngoại lệ là ACPI ID không được gắn. Để khắc phục chúng bạn sẽ cần kiểm tra [ioreg](https://github.com/vulgo/IORegistryExplorer)

Nếu bạn tìm thấy ACPI ID của mình trong ioreg thì hãy chuyển tới phần tiếp theo nếu không tìm thấy nó chứng tỏ rằng ACPI ID i2c device chưa đước gắn ta sẽ tiến hành gắn ACPI ID i2c device vào

![](https://i.imgur.com/Q015Plo.png)

Đây là hình ảnh ACPI ID đã gắn

> Để gắn ACPI ID xem tiếp bước 2

B2: Các bạn mở DSDT ra tìm đến ACPI ID i2c device của các bạn và chú ý vào `method _sta` chúng ta sẽ tiến hành xoá các dòng `if` để method `_sta` luôn trả về `0x0f` tức là luôn bật

![](https://i.imgur.com/CfoUzBF.png)

> Thông thường khi làm như trên các bạn đã có thể gắn ACPI ID nếu vẫn chưa gắn được ACPI ID thì các bạn bắt buộc phải tự đọc DSDT và tự tìm hiểu

### Cách xác định i2c mode

Như đã nói ở trên i2c có 2 loại là Polling và Interrupt vậy làm sao đê biết thiết bị của bạn đang ở loại nào

#### Dùng Ioreg

B1: Mở ioreg ra

B2: Tìm đến ACPI ID i2c device đã xác định ở trên

B3: Chú ý vào mục interupt mode và để ý giá trị của nó bạn sẽ xác định được trackpad mình đang ở loại nào

![](https://i.imgur.com/QLug2pG.png)

polling mode

![](https://i.imgur.com/m3JfTry.png)

Interrupt mode

### Dùng Gen i2c

B1 Tải app Gen i2c trong mục tool (trên thanh menu của web)

B2: mở app ra và chú ý vào mục mode

![](https://i.imgur.com/jeMRQiO.png)

Polling mode

![](https://i.imgur.com/UI3M1gD.png)

Interrupt mode

### Pin List

> Phần này rất hiếm người cần dùng đến. Tuy nhiên nếu bạn cần dùng thì hãy đọc thật chậm và kỹ lưỡng nhé

> Nếu bạn đọc phần này tức là thiết bị của bạn đang là interrupt

B1: Check xem `Pin Number` của bạn. Mở ioreg và tìm đến ACPI-ID của các bạn, chú ý vào dòng `IOInterruptSpecifiers`. Nếu như không có dòng đó hoặc giá trị nhỏ hơn `0X2F` (47) thì không cần làm gì nữa

![](https://i.imgur.com/2VkEyv9.png)

Như ở đây ta có `Pin number` là `0x33`. Tại sao là `0x33` vì ta chỉ lấy hai chữ số đầu của dãy số. `0x33` lớn hơn `0x27` nên tả sẽ phải tiến hành patch pin list cho nó.

![](https://i.imgur.com/nJ4vewI.png)

Việc đầu tiên cần làm là xác định `name SBFX` mà như hình là `SBFG` ta có 2 loại pin 1 là `pin crs` hai là `pin root`:

- `Pin root`: `name SBFX` được tìm thấy ở ngay ACPI-ID
- `Pin CRS`: `name SBFX` được tìm thấy ở method crs

ở đây ta sẽ chú ý vào dòng note `//pin list` nếu giá trị khác `0x0000` thì thiết bị của bạn đang được pin tốt. Tuy nhiên nếu là `0x0000` thì đừng vội lo lắng bạn hãy tiến hành kiểm tra dòng return của method crs nếu nó có call tới method crs thì thiết bị của bạn vẫn đang được pin tốt ở đây ta có

![](https://i.imgur.com/xSPozfw.png)

```
                If (LEqual (TPDM, Zero))
                {
                    Return (ConcatenateResTemplate (SBFB, SBFG))
                }

                Return (ConcatenateResTemplate (SBFB, SBFI))
```

2 dòng return ta có:

- `SBFG` có nghĩa là `interrupt mode`
- `SBFI` có nghĩa là `polling mode`

Mà `GPIO` pin patch đòi hỏi bạn phải ở `interrupt mode` ta sẽ tieén hành edit method crs để cho nó luôn trả về là `interrupt mode`

```
Return (ConcatenateResTemplate (SBFB, SBFG))
```

![](https://i.imgur.com/RoATCT2.png)

> Ở đây ta có `//pin list` nếu là `0x00000` thì các bạn làm như vầy để cho nó trở thành pin tốt nếu khác `0x0000` thì các bạn cũng cần làm bước này để cho nó luôn trả về `interrupt`

Nhưng nếu bạn không tìm thấy `name SBFX` ở cả ACPI ID i2c device và method `crs` thì có nghĩa là i2c device của bạn chưa được pin. Nếu như vậy bạn sẽ cần phải tiến hành gắn pin thủ công xem ở bước 2

B2: Các bạn sẽ tiến hành add `Name SBFX` vào ACPI id i2c device (đây gọi là gắn pin thủ công chỉ khi thiết bị của bạn không được pịn mới cần làm)

```
    Name (SBFG, ResourceTemplate ()
    {
        GpioInt (Level, ActiveLow, ExclusiveAndWake, PullDefault, 0x0000,
            "\\_SB.PCI0.GPI0", 0x00, ResourceConsumer, ,
            )
            {   // Pin list
                0x0000
            }
    })
```

![](https://imgur.com/7rm6D78.png)

B3: các bạn sẽ cần mở ioreg lên và search `GPIO`

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/gpio-enabled.c84356b7.png)

Ta có thể thấy ở đây là `CannonLakeH`, tiến hành tra nó theo bảng sau

| *text 1*                                                                                                                    | *text 2*                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| [*Sunrise Point*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/skylake/include/soc/gpio_defs.h)           | [*Sunrise Point*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/skylake/include/soc/gpio_soc_defs.h)*,*        |
| [*Cannon Point-LP*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_defs.h)      | [*Cannon Point-LP*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_soc_defs.h)      |
| [*Cannon Point-H*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_defs_cnp_h.h) | [*Cannon Point-H*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/cannonlake/include/soc/gpio_soc_defs_cnp_h.h) |
| [*Ice Lake*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/icelake/include/soc/gpio_defs.h)                | [*Ice Lake*](https://github.com/coreboot/coreboot/blob/master/src/soc/intel/icelake/include/soc/gpio_soc_defs.h)                |

> `CannonLakeH` có thể dùng của `Cannon Point-H`

B4: mở file text bên text 1 và search `Pin number` đã xác định được ở ioreg

![](https://i.imgur.com/dc6RSlA.png)

Ở đây ta sẽ copy mục name và search bên text 2

![](https://i.imgur.com/uTofIiJ.png)

Ta sẽ copy giá trị nhận được là 43 và dùng hackintool `convert decimal to hex`

![](https://i.imgur.com/bidpOFh.png)

Ta có `GPIO number` ở đây là 0x2B

Ta tiến hành edit DSDT như sau

![](https://i.imgur.com/PbH4pfF.png)

Ngoài ra khi `Pin number` của bạn có nhiều giá trị chân GPIO bạn sẽ cần phải thử từng cái một

![](https://i.imgur.com/L3p8R9n.png)

Đối với các dòng `Cannon Point` trở lên có sự không khớp giữa `GPIO Pin number` và `hardware pin number`. Nếu bạn sử dụng loại phần cúng này thì bạn sẽ cần convert `hardware pin number` thành `GPIO number` 

> Nếu bạn không biết mình có đang sử dụng phần cứng này không thì bạn cứ thử từng cái thử cách bình thường trước

- B1: mở file text1 ra và search Pin number nhận được ở ioreg sau đó copy phần name tương ứng như hướng dẫn ở trên

| Text 2′                                                                                                                           |
| --------------------------------------------------------------------------------------------------------------------------------- |
| [*Cannon Point-LP*](https://github.com/VoodooI2C/VoodooGPIO/blob/master/VoodooGPIO/CannonLake-LP/VoodooGPIOCannonLakeLP.hpp#L366) |
| [*Cannon Point-H*](https://github.com/VoodooI2C/VoodooGPIO/blob/master/VoodooGPIO/CannonLake-H/VoodooGPIOCannonLakeH.hpp#L414)    |
| [*Ice Lake-LP*](https://github.com/VoodooI2C/VoodooGPIO/blob/master/VoodooGPIO/IceLake-LP/VoodooGPIOIceLakeLP.hpp#L358)           |

- B2: mở file text 2′ lên và xem phần tôi khoanh đỏ

![](https://i.imgur.com/jBpL3qN.png)

Ở đây ta có thể sẽ thấy được ý nghĩa các dòng trong file text

- B3: search name đã lấy được ở file 1 trong text 2′
- B4: ta sẽ lấy `Pin number` lấy được ở ioreg (ở decimal) trừ đi cho `base` cộng cho `gpio_base`

```
Tức là Pin number - s + g
```

- B5: convert `GPIO number` vừa nhận được thành hex
- B6: edit DSDT theo hướng dẫn ở trên

> Chú ý vị trị nằm của r s e g

## Force Polling mode

Đối với các máy ở chế độ polling mode mà không native thì các bạn có thể tiến hành force `polling mode`

### Edit DSDT

B1: Mở DSDT lên và tìm đến mục ACPI ID ở DSDT sau đó chú ý vào method `crs` ta có:

- `SBFI`: là `polling mode`
- `SBFG`: là `interrupt`

Do đó việc bạn cần làm chỉ đơn giản là xoá return của `SBFG` đi và chỉ để lại return của `SBFI`

![](https://i.imgur.com/0gUlJah.png)

> Lưu ý ngoài name `SBFI` ra thì bạn cũng có thể sử dụng name `SBFB`
> 
> Khi dòng return có name khác đứng cùng name `SBFI` thì sẽ tiến hành rename nó thành `SBFB`

### Add boot-arg

B1: mở config

B2: thêm boot-arg `-vi2c-force-polling`

B3: save lại và reboot

## Force interrupt mode

B1: Các bạn tìm ACPI-id của các bạn trong DSDT sau đó kéo lên mục `scope` bạn sẽ có thể thấy name `SBFB` và một name `SBFX` nào đó theo sau nó như hình

![](https://i.imgur.com/EBeqO8Z.png)

B2: Chúng ta sẽ cần xem name `SBFX` đó có dòng nào đại loại như

```
                Interrupt (ResourceConsumer, Level, ActiveLow, ExclusiveAndWake, ,, _Y3A)
```

Thì tiến hành xoá cả phần đó đi

Ngoài ra có trường hợp bạn sẽ không tìm thấy name `SBFB` mà sẽ tìm thấy 1 name `SPFX` đại loại như là

```
    Method (_CRS, 0, Serialized)  // _CRS: Current Resource Settings
                {
                    Name (SBFI, ResourceTemplate ()
                    {
                        I2cSerialBusV2 (0x0015, ControllerInitiated, 0x00061A80,
                            AddressingMode7Bit, "\\_SB.PCI0.I2C1",
                            0x00, ResourceConsumer, , Exclusive,
                            )
                        Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive, ,, )
                        {
                            0x0000006D,
                        }
                    })
                    Return (SBFI)
                }
```

B3: các bạn cần chuyển `SBFX to SBFB`

B4: xóa dòng này đi nếu có 

```
       Interrupt (ResourceConsumer, Level, ActiveLow, Exclusive, ,, )
    {
        0x0000006D,
    }
```

B5: Nếu bạn đang ở `polling mode` muốn force sang `interrupt mode` thì các bạn cần xem DSDT đã có pin chưa và nếu đã có pin thì chú ý vào name của pin đó nếu là `SBFB` hoặc `SBFG` thì bỏ qua nếu là `SBFI` hoặc các `SBFX` khác thì rename nó sang `SBFG`

B6: Các bạn sẽ tiến hành chỉnh method `CRS`. xoá hết các dòng return đi chỉ chừa lại dòng có name `SBFG`. Nếu name khai báo cùng `SBFG` không phải `SBFB` thì bạn sẽ cần rename nó là `SBFB`

![](https://i.imgur.com/Eeh6tEE.png)

![](https://i.imgur.com/HIRgnzO.pnghttps://i.imgur.com/HIRgnzO.pnghttps://i.imgur.com/HIRgnzO.png)

> Chú ý rằng khi force từ `polling mode` sang `interrupt mode` thì bạn có thể sẽ cần phải pin list

> đối với các bạn lười không làm theo những phương pháp trên được vẫn còn 1 biện pháp cho bạn đó là các bạn hãy tải kext apple ps2 smart touch pad và đặc biệt lưu ý sau khi các bạn bỏ kext vào sẽ nhận trackpad trong setting nhưng sẽ không chỉnh gì được các thao tác (tùy máy) do đó các bạn nên lưu ý khi dùng (links tải file [CÁC TOOL HACKINTOSH](https://drive.google.com/drive/folders/17n9x1wcqMZSE5unmNs9KmkGCq7WvJ6iN?usp=sharing\) do đây là 1 kext khá củ nên mình quên mất nguồn rồi nên mình sẽ để nó vào trong thư mục [CÁC TOOL HACKINTOSH](https://drive.google.com/drive/folders/17n9x1wcqMZSE5unmNs9KmkGCq7WvJ6iN?usp=sharing))

**Source tham khảo: [GPIO Pinning (voodooi2c.github.io)](https://voodooi2c.github.io/#GPIO%20Pinning/GPIO%20Pinning) | [Trackpad on Hackintosh với giao thức I2C – Hackintosh Vietnam – Chuyên trang cung cấp hướng dẫn cài hackintosh](https://hackintosh.vn/trackpad-on-hackintosh-voi-giao-thu-i2c) | [Fixing Trackpads: Manual | Getting Started With ACPI (dortania.github.io)](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/trackpad-methods/manual.html#create-osys-variable-under-i2c-scope) | [Polling Mode (voodooi2c.github.io)](https://voodooi2c.github.io/#Polling%20Mode/Polling%20Mode) (thanks you)**
