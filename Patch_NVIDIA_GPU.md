# Patch Card đồ họa NVIDIA

## Patch card đồ hoạ highsierra

B1: các bạn cần cài macos highsierra với build version là `17G14042` tức là version mới nhất

> Nếu chưa đúng build version thì các bạn sẽ tiến hành update lên macos 10.13.6 phiên bản cao nhất

```
// Các bạn dùng lệnh sau để kiểm tra

sw_vers 
```

![](https://i.imgur.com/JhDeZf7.png)

B2: Download bộ web driver sau về [tại đây](https://za7o7cw6-my.sharepoint.com/:u:/g/personal/hoanglong_coursedeals_org/EZwpS5tNKpJBsRhL3qfUClABZmB8WwKlpM1RR0htgVi89w?e=9VkkIX)

B3: tiến hành giải nén sau đó chạy file pkg lên

B4: các bạn tiến hành cài đặt app cuda

B5: add boot-arg sau vào `nvda_drv_vrl=1`

B6: reboot và tận hưởng thành quả

> Lưu ý: Đây là phiên bản web driver mới nhất từ nvidia các bạn không dùng các phiên bản cụ hơn vì những phiên bản đó đã hết chứng chỉ nên sẽ không thể enable được CUDA driver (web driver vẫn cài được)

> Lưu ý 2: Ở thời điểm hiện tại tool [nvidia-update](https://github.com/Benjamin-Dobell/nvidia-update) của [Benjamin-Dobell](https://github.com/Benjamin-Dobell) vẫn chưa update version mới nhất từ nvidia do đó các cuda driver vẫn đang hết hạn giấy phép. Có thể sau này sẽ update

```
// Nếu trong tương lai tool có update thì các bạn chỉ việc chạy lệnh sau để cài web driver

bash <(curl -s https://raw.githubusercontent.com/Benjamin-Dobell/nvidia-update/master/nvidia-update.sh)
```

## Patch card đồ hoạ trên bigsur và monterey

### **Các dòng card không được support chính thức**

#### Chuẩn bị

Ở thời điểm hiện tại tool chỉ mới là beta chỉ dành cho dev nên sẽ không full chức năng như ở highsierra

Nếu bạn nghĩ có thể dùng card rời để làm việc thì quên đi nhé vì nó sẽ không có Metal và Vulkan (Force OpenGL) nên chỉ có thể lướt web hoặc chơi game nhẹ nhàng

Tiếp đó tính đến thời điểm hiện tại thì các dòng RTX series, MX gen 4 series, GTX 16xx đều tạch. Còn lại được hỗ trợ. xem list support chi tiết [tại đây](https://github.com/dortania/OpenCore-Legacy-Patcher/blob/main/data/pci_data.py?fbclid=IwAR1UjBJoYhSbmccSU6jzmnaLQXuaO9ZeYnoM-1X1_TF-F5qkbnYwxTnDAUo)

Tiếp theo đối với laptop thì bắt buộc phải có `mux-switch` (cực kì quan trọng)

- Vậy làm sao để biết được laptop của bạn có `mux-switch` hay không
  - đừng check thong tin máy không có đâu. Vì nhà sản xuất không thích ghi
  - Các bạn vào bios để tìm mục `mux-switch`
    - hoặc mục nào đó dùng để switch giữa dgpu và igpu

> Không có lên mà lag thì đừng khóc

Cuối cùng nếu bios bạn có CSM thì enable lên nhé. Không có thì bỏ qua

Để tránh các lỗi không cần thiết các bạn sẽ cài python version 3.9.x (không phải 3.10.x)

#### Tiến hành

B1: add boot-arg sao vào config

```
amfi_get_out_of_my_way=0x1 ngfxcompat=1 ngfxgl=1 nvda_drv_vrl=1
```

B2: set `Csr-active-config` thành `030A0000`

B3: tải `OpenCore-Patcher-GUI.app` version mới nhất theo hướng dẫn [tại đây](https://github.com/dortania/OpenCore-Legacy-Patcher/releases/0.4.7)

B4: chỉnh `SecureBootModel` trong config thành `Disabled`

B5: reboot và restart nvram

B6: ấn vào `Post Install Root Patched`

![](https://i.imgur.com/bL2ybYT.png)

B7: ấn `Start Root Patching`

![](https://i.imgur.com/7aUAkEy.png)

B8: allow `unknown software and kernel` trong `Preferences -> Security and Privacy`

B9: reboot và restart nvram

Lưu ý: Nếu bạn nào cài lên rồi muốn gỡ ra có thể ấn vào nút `Revert Root Patches` trong `Post Install Root Patched`

### **Patch card kepler trên monterey**

Đối với Monterey Beta 9 + 1 số dòng Card Geforce Kepler sẽ không Native cách fix là các bạn sẽ dùng công cụ [Geforce-Kepler-patcher](https://github.com/chris1111/Geforce-Kepler-patcher) như sau:

B1: Tắt SIP Gatekeeper và Root SIP

- Chỉnh csr-active-config
  - `EF0F0000`: nếu dùng opencore
  - `0xFEF`: nếu dùng clover
    - ngoài ra clover còn có thể dùng `0x867`

B2: chỉnh `SecureBootModel` trong config thành `Disabled`

B3: reboot

B4: Các bạn tải công cụ Geforce-Kepler-patcher theo link trên.

B5: Giải nén ra sau đó App ra Desktop.



![CleanShot 2021-10-09 at 12.42.32.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-14-12-45-CleanShot%202021-10-09%20at%2012.42.32.png)

B6: Các bạn nhấn Run.

![CleanShot 2021-10-09 at 12.54.08.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-14-12-57-CleanShot%202021-10-09%20at%2012.54.08.png)



B7: Tiếp theo nhấn OK.

![CleanShot 2021-10-09 at 13.04.25.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-14-13-09-CleanShot%202021-10-09%20at%2013.04.25.png)



Lưu ý: Đối với những dòng card Nvidia hỗ trợ nhưng không nhận đủ VRAM các bạn thêm boot-arg sau vào : `agdpmod=vit9696` hoặc `agdpmod=pikera`.

Lưu ý 2: Đối với 1 số máy có dGPU không muốn dùng iGPU thì Disable như sau:

- B1: Add đoạn code sau vào boot-arg `-wegnoigpu`
- B2: Restart và tận hưởng thôi.

Lưu ý 3: Có 1 số Card không hỗ trợ ở bất kỳ phiên bản nào đó là các Card sau GeForce 20 Series, GeForce 16 Series và GeForce 30 Series (danh sách các Card hỗ trợ và các phiên bản hỗ trợ cho từng Card các bạn xem [tại đây](https://heavietnam.ga/2020/12/25/tim-hieu-phan-cung/)). 

> Hãy ghi nhớ rằng các card nvidia ở laptop để có thể chạy được cần phải có mux switch hoặc cổng xuất màn hình internal nằm ở dgpu để check xem cổng xuất màn hình internal có nối với dgpu không các bạn sẽ tiến hành vào device properties –> monitor –> properties –> location

![](https://i.imgur.com/d6gXaY9.png)

> Một lưu ý cuối cùng nhé là khi muốn sử dụng card rời trên laptop thì bắt buộc laptop đó phải có mux switch nếu không card rời sẽ không hoạt động.
> 
> Vì màn hình trong laptop nói với igpu và macos ko hỗ trợ chạy song song cả igpu và dgpu. Và đương nhiên bản vẫn có thể dùng dgpu được nếu bạn convert màn trong ra dgpu và đương nhiên khi này bạn sẽ không thể xuất màn rời
