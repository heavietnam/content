# Patch DSDT phần 4

Hiện nay mình thấy nhiều bạn có xu hướng sài lại static patch. Trong khi các SSDT đã được prebuilt thì có 1 cách để apply các SSDT ấy vào DSDT giúp bạn hoàn thiện chức năng của DSDT.

## Apply trực tiếp

## Chuẩn bị

B1: Tải maciasl [tại đây](https://github.com/acidanthera/MaciASL/releases).

B2: Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

## Tiến hành

B1: Các bạn mở SSDT muốn apply ra và chú ý vào phần method.

![](https://i.imgur.com/OB0Yxlh.png)

B2: Các bạn tiến hành search method đó trong DSDT.

![](https://i.imgur.com/um520h8.png)

B3: Các bạn tiến hành thay method đó bằng method trong DSDT.

![](https://i.imgur.com/xhtV1gc.png)

B4: Ấn `compile`.

![](https://i.imgur.com/1hpVivE.png)

B5: Ở đây bạn thấy sẽ có 1 `error` báo lỗi do `method XPRW` vừa được add vào DSDT vẫn chưa được `import` các bạn sẽ tiến hành `import` nó vào.

![](https://i.imgur.com/dqjcDIB.png)

Ở đây nếu bạn nào không biết cách import có thể thao khảo [tại đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/)

B6: Ấn `compile` để check lỗi.

![](https://i.imgur.com/gekpIt0.png)

Ở đây ta đã thấy DSDT không còn error nữa.

B7: Save DSDT.

B8: Reboot.

## Dùng kèm SSDT

> Nhiều bạn dùng kèm ssdt với dsdt cách làm này rất tốt. Tuy nhiên xảy ra 1 vấn đề là các SSDT cần patch rename như GPRW thì sẽ không thể chạy được. Do khi bạn rename qua bootloader mà dùng DSDT thì các patch rename sẽ không chạy. Chúng ta có 1 cách khắc phục

B1: Tải SSDT bạn cần dùng cho vào `EFI --> OC --> ACPI`

B2: Tiến hành snapshot

B3: Mở DSDT bằng maciasl

B4: nhấn tổ hợp phím `command + F` và tick vào nút replace

![](https://i.imgur.com/uk8tYrF.png)

B5: Search từ khoá mà bạn cần rename vào ô find

B6: gõ từ khoá bạn cần rename vào ô replace

B7: ấn nút All

![](https://i.imgur.com/2L6nvGg.png)

B9: nhấn nút complie

![](https://i.imgur.com/KCpEwOh.png)

B10: Nếu không có error thì các bạn ấn save

> Và như vậy là done. Cách này chúng ta sẽ rename trực tiếp vào DSDT chúc các bạn thành công
