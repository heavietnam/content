# Add device-id vào amd kext

B1: Các bạn mở terminal lên và gõ `kextstat | grep AMD`

B2: Các bạn mở file `info.plist` các kext được liệt kê sau khi gõ lệnh trên và sửa lại như sau ( ở S/L/E )

![](https://imgur.com/qz9Zpxx.png)

![](https://imgur.com/qz9Zpxx.png)

Add device-id của các bạn vào mục IOPCIMatch

B3: các bạn chỉnh version cao hơn version gốc ( nếu ở cata thì không cần chỉnh )

![](https://imgur.com/XXTRdil.png)

tiếp theo chỉnh cả file version.plist ( nếu cata thì không cần )

![](https://imgur.com/DAlmIHD.png)

B4: Các bạn dùng hackintool để bỏ kext và L/E ( hoặc nếu ở cata thì S/L/E )

![](https://imgur.com/Yu6BbPL.png)

Lưu ý: nếu ở cata thì các bạn tiến hành xóa các kext đã xác định ở bước 1 đi và tiến hành bỏ  đã patch vào S/L/E và tiến hành rebuild kext cache

![](https://imgur.com/ILPsvvF.png)

Source tham khảo: [howtohackintosh.top](https://howtophackintosh.top/)
