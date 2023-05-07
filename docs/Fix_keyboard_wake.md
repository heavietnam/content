# Fix keyboard wake

> Đối với 1 số máy khi wake cần nhấn phím nguồn hoặc nhấn phím nhiều lần, ở đây mình sẽ hướng dẫn cách bạn chỉ cần nhấn phím 1 lần thôi là đã wake lên.

## Cách 1

### Chuẩn bị:

B1: Tải [gfxutil](https://github.com/acidanthera/gfxutil/releases) 

B2: Tải [ProperTree](https://github.com/corpnewt/ProperTree)

### Tiến hành:

B1: Chạy gfxutil lên nếu các bạn nào bị trường hợp chạy lên và tắt thì các bạn vào setting terminal chỉnh lại theo ảnh

![](https://lh5.googleusercontent.com/PyXzSHa0ZXnWT9Nm7JQ74QTxd2WYGqxkkH9NBwTt4haPkw7q-vaNO9yQF9DPJ7lHm-7cRsxWg6tvyigRCbVD0NqB0uLcLoVeRuw7eqiDHmMNZKFdPoygA87QqSnKH_O2ODwtQmSi=s0)

![](https://lh5.googleusercontent.com/dpQSRkek8DWUfScbj6ytf8aJ_9n29HZzuA4qfaeFzPy3lbANYZGiXVzgICHBgLxvrkEwXDUmMAyFcPQCWqUPXuP7utVD0VrYBm6CNOKEbFIoQ0LvhNrxd2QKCcALUtJM9h4LQ1Ts=s0)

B2: Tìm đến dòng có tên là `XHCI (XHC1/XHC)`

B3: Thêm 1 dòng vào `DeviceProperties` với tên và đoạn code vừa xác định 

B4: `acpi-wake-type | Data | <01>` thêm đoạn này vào config dưới dòng vừa thêm  

> sau khi làm xong sẽ được như hình

![](https://lh5.googleusercontent.com/7fQ2QPEYVztxe_oXS8BGD7XTf-Rnk8k-Oy_r_wXERhV1BRIkf8VtRFE76YfuHOhenaS1SKM4SUrRkc931yr7fz6MQ6WSIa45wJANM68dwIzZzVBjScEcihxnuJivJANyGFzD28b7=s0)

> Lưu ý: Nếu không được hãy chuyển sang cách 2**

## ***Cách 2:***

B1: Tải [Keyboard Wake Issues | OpenCore Post-Install (dortania.github.io)](https://dortania.github.io/OpenCore-Post-Install/usb/misc/keyboard.html#method-1-add-wake-type-property-recommended) và [SSDT-USBW.dsl](https://github.com/osy86/USBWakeFixup/blob/master/SSDT-USBW.dsl)

B2: Bỏ `kext` vừa tải vào folder kext và snapshot lại config 

B3: mở `SSDT` vừa tải bằng [MaciASL](https://github.com/acidanthera/MaciASL/releases)

B4: các bạn chuyển đoạn mã vừa lấy ở cách 1 thành `/PC00@0/XHCX@14 -> \_SB.PC00.XHCX` 

> thay hoặc bỏ X thành đoạn mã mà bạn thu được

![](https://lh3.googleusercontent.com/MdXvHr5gwTd97t_L09r40Y7Wag2IYGURnK7zT8yTpiUItrRkBbCipKIXr10kb8HYDg2zY4FxyjfU_FmppKLmv2KOorO7ZBlMXQs2wxCD1C-GDF5gdTjExWJPlNFouMNEzOFeJ02c=s0)

B5: Save lại và bỏ vô `EFI ⇒ OC ⇒ ACPI` (hoặc `EFI ==> Clover ==> ACPI ==> Patched`)

B6: Snapshot config và reboot

**Lưu ý : Nguồn:** [**Keyboard Wake Issues | OpenCore Post-Install (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/usb/misc/keyboard.html#method-1-add-wake-type-property-recommended) 

**Lưu ý chung : nếu các bạn có card rời thì phải disable card rời đi**

**Nguồn tham khảo:** [**Các vấn đề về đánh thức bàn phím | OpenCore Sau cài đặt (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/usb/misc/keyboard.html#method-3-configuring-darkwake)


