# Boot Arguments

## Boot Arguments là gì?

Boot Arguments hay Boot-arg là các tham số khởi động có thể sẽ giúp ích cho bạn trong một số trường hợp. Đôi khi, nó có thể giúp bạn tránh những lỗi không đáng có khi cài Hackintosh. Tuỳ vào các kext mà các bạn sử dụng thì số lượng boot-arg có thể sử dụng cũng sẽ thay đổi

## Bảng các boot-arg thường dùng

| BOOT-ARGS            | MÔ TẢ                                                                                                                                                                                                        |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **-v**               | Enable verbose giúp bạn có thể xem các big ngay trên màn hình                                                                                                                                                |
| **debug=0x100**      | Nó giúp disable watchdog trên macos. Giúp không bị reboot khi gặp kernel panic giúp bạn có thể đọc lỗi dễ dàng hơn                                                                                           |
| **keepsyms=1**       | Dùng chung với debug=0x100 để giúp bạn có thể dễ dàng đọc các lỗi kernel panic                                                                                                                               |
| **alcid=1**          | dùng để fix audio bằng apple alc xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/)                                                                   |
| **-no_compat_check** | dùng để bỏ qua phần check smbios giúp bạn có thể dễ dàng sử dụng các smbios ko support trên macos version cao                                                                                                |
| **agdpmod=pikera**   | Sử dụng để tắt board ID checks trên Navi GPUs (RX 5000 & 6000 series) nếu không sử dụng bạn sẽ nhận được 1 màn hình đen và chẳng có gì khác ngoài nó<br>Không sử dụng nó nếu gpu của bạn không phải Navi GPU |
| **nvda_drv_vrl=1**   | Để enable web driver trên highsierra                                                                                                                                                                         |
| **-wegnoegpu**<br>   | dùng để disable tất cả các gpu trừ igpu xem chi tiết các disable gpu [tại đây](https://heavietnam.ga/2022/06/13/disable-dgpu-laptop/)                                                                        |
| **+x**               | Tiến vào chế độ safe mode giúp bạn gỡ lỗi                                                                                                                                                                    |
| **shikigva=40**      | đổi boardID với iMacPro1,1. Cho phép các GPU Polaris, Vega and Navi GPUs xửa lý được tất cả các loại render. Hữu ích cho các bạn sử dụng smbios hỗ trợ igpu                                                  |
| **radpg=15**         | Fix initialization cho HD 7730/7750/7770/R7 250/R7 250X                                                                                                                                                      |
| **-raddvi**          | Fix connector type DVI cho 290X, 370,…                                                                                                                                                                       |
| **-radvesa**         | Bắt các GPU amd vào chế độ VESA mode (bỏ qua tăng tốc GPU) Giúp ích cho việc gỡ lỗi                                                                                                                          |
| **agdpmod=vit9696**  | Disable board-id check có thể giúp bạn enable external display                                                                                                                                               |
| **shikigva=1**       | Cho phép igpu xuất màn khi bạn đang sử dụng dgpu cho phép iGPU xử lý giải mã phần cứng ngay cả khi không sử framebuffer                                                                                      |
| **shikigva=4**       | cần thiết cho hỗ trợ tăng tốc phần cứng và giải mã video. Trên các hệ thống mới hơn haswell cần boot-arg `shikigva=12`                                                                                       |
| **-wegnoegpu**       | Disable tất cả GPU trừ IGPU                                                                                                                                                                                  |
| **-igfxnohdmi**      | Tắt chuyển đổi audio hdmi to DP                                                                                                                                                                              |
| **-cdfon**           | Thực hiện nhiều patch để bật hdmi 2.0                                                                                                                                                                        |
| **-igfxvesa**        | Buộc igpu vào VESA mode (bỏ qua tăng tốc GPU) Hữu ích cho việc gỡ lỗi                                                                                                                                        |
| **igfxonln=1**       | Thực hiện force-online giúp giải quyết vấn đề về wake screen trên Coffee and Comet Lake                                                                                                                      |
| **igfxfw=2**         | Cho phép tải chương trình cơ sở GUC của Apple IGPU yêu cầu chipset 9 series+                                                                                                                                 |
| **nv_disable=1**     | Buộc GPU NVIDIA vào VESA mode (bỏ qua tăng tốc GPU) Hữu ích cho việc gỡ lỗi                                                                                                                                  |
| **-igfxblr**         | Khắc phục lỗi mất backlight trên các IGPU UHD                                                                                                                                                                |
| **uia_exclude**      | Dùng để map usb với hackintool. Chỉ định những porrt bị loại bỏ                                                                                                                                              |
| **uia_include**      | dùng để map usb với hackintool. Chỉ định những port được load                                                                                                                                                |
| **darkwake=0**       | dùng để fix lỗi darkwake                                                                                                                                                                                     |
