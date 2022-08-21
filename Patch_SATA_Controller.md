# Patch SATA Controller

Trước tiên ta phải tìm hiểu Patch SATA Controller dùng để làm gì. Nó dùng để inject device id SATA Controller (AHCI Mode). Nếu Device ID không được inject thì các bạn sẽ không thể nhận ổ cứng hoặc gặp lỗi `still waiting for root device`.

## Cách 1: Dùng Kext

B1: Các bạn sẽ chọn Kext theo sau:

- [SATA-Unsupported.kext](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/SATA-unsupported.kext.zip): dùng để kích hoạt các bộ điều khiển SATA thường dùng với Laptop.

- [CtlnaAHCIPort.kext](https://github.com/dortania/OpenCore-Install-Guide/blob/master/extra-files/CtlnaAHCIPort.kext.zip): thường được sử dụng ở Big Sur thay cho nhiều bộ điều khiển bị loại bỏ.

- Legacy SATA Kexts:
  
  - [AHCIPortInjector.kext](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/AHCIPortInjector.kext.zip): sử dụng trên Legacy SATA/AHC thường cần ở Penryn trở về trước.
  - [ATAPortInjector.kext](https://github.com/khronokernel/Legacy-Kexts/blob/master/Injectors/Zip/ATAPortInjector.kext.zip): thường được dùng khi không có tùy chọn AHCI trong BIOS.

B2: Các bạn bỏ kext vào `EFI ==> OC ==> kext` và Snapshot lại config hoặc bỏ kext vào `EFI ==> Clover ==> kext ==> Other` nếu dùng Clover.

B3: Reboot.

**Lưu ý: 1 số trưởng hợp không kích hoạt được các bạn có thể thử bỏ vào L/E sử dụng [Kext DropletV2](https://github.com/chris1111/Kext-Droplet-Big-Sur/releases) hoặc S/L/E sử dụng [Kext Droplet](https://github.com/chris1111/Kext-Droplet/releases/tag/V2)**.

## Cách 2: Inject qua Device Properties

B1: Các bạn cần xác định series của SATA Controler bằng cách vào Device Manager ==> IDE ATA/ATAPI Controllers.



Đối với 1 số máy sẽ không thấy được series các bạn tiến hành cài driver cho nó link chi tiết [tại đây](https://tinhte.vn/thread/sua-loi-full-disk-100-thanh-cong-tren-windows-10-bang-driver-hardisk-controller.3238245/).

B2: Các bạn tiến hành tải App [aida64](https://www.hwinfo.com/download/) về sau đó tiến hành đổi chiếu để tìn ra model gần với model của bạn nhất. Nhưng đặc biệt là phải trùng series và vendor id của các bạn.

B3: Các bạn truy cập vào trang [sau](https://devicehunt.com/).

B4: tìm đến model các bạn cần fake nhập vendor id vào tìm đến model sát cấu hình nhất và lấy device id.

Ở đây mình được device-id là `1E03`.

![](https://imgur.com/ELQLcyW.png)

B5: Mở hackintool lên tìm ở phần device name là sata controller và nhìn vào tab device path.

![](https://imgur.com/DuQ2lxc.png)

B6: Mở config lên vào mục `device properties` hoặc `device ==> prperties` nếu ở clover add 1 key với kiểu dữ liệu là `dictionary` và tên là device path vừa xác định ở trên sao đó add `device-id` vào với kiểu dữ liệu là `data`.

Biến đổi device-id theo quy tắc sau:

```
// Thêm chữ số 0 vào trước dãy số để được 8 chứ số 

1e03 ==> 00001e03

// Tác thành từng cặp 2 chứ số 

00001e03 ==> 00 00 1e 03

// Đảo thứ tự các cặp

00 00 1e 03 ==> 00 00 1e 03

// Viết lại thành device-id

00 00 1e 03 ==> 00001e03
```

![](https://imgur.com/6AD50xQ.png)

B7: Save lại và Reboot.
