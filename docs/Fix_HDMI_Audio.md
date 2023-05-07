# Fix HDMI Audio

B1: Mở config bằng [ProperTree](https://github.com/corpnewt/ProperTree)

B2: Tìm đến mục Root ⇒ acpi ⇒ patched 

Copy các patch [tại đây](https://za7o7cw6-my.sharepoint.com/:u:/g/personal/hoanglong_coursedeals_org/ESs52HLJV3FKr8T6M-mkCCEBPdOtSsZyweQ64Yi-ARIKYg?e=nFJxyt) vào config tương ứng các mục

> **Lưu ý : không cần add các patch rename sau nếu bạn sử dụng AppleALC**.

> **Lưu ý : chỉ add 1 là HECI hay là MEI không add cả hai .**

Cách để biết bạn dùng patch rename HECI hay MEI:

- Dump DSDT theo bài hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

- Mở DSDT bằng [MaciASL](https://github.com/acidanthera/MaciASL/releases).

- Search từ khoá sau `HECI` hoặc `MEI`

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.31.24.png?w=782)

-  Từ khoá nào có các bạn add patch rename đó. 

B3: Tải [gfxutil](https://github.com/acidanthera/gfxutil/releases) và nhập code sau vào terminal `patch to gfxutil -f HDEF` 

![](https://lh5.googleusercontent.com/2AnHbqNDrwEeOVMONErYSRehzJyZQvfb4bKd4DUobxhKvWsfb6MzL5qy3YIsYG5RLpJZsIldYy1NcEcbzhLKTN2avgVZdKgMfbkCWpDlbmz3iBUIBQaKxFp1XgJzFxNPFpU-KyZZ=s0)

B4: Lấy mục vừa đc dump từ Terminal, ở đây của mình là 

`PciRoot(0)/Pci(0x1f,3)` add vào mục `device properties` tiếp add các patch sau dưới dòng vừa tạo `hda-gfx | STRING | onboard-1` 

> đối với clover thì add như hình:

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-12.30.08.png?w=1024)

B5: Xác định `conX` liên kết với cổng hdmi (xem [tại đây](https://heavietnam.ga/2021/09/29/xiv-patch-connect-type-force-rgb-injects-edid/))

B6: Các bạn mở  `config.plist ⇒ Devices Properties ⇒ PciRoot(0)/Pci(0x02,0) `và add như sau: 

```
framebuffer-patch-enable | data | 01000000
framebuffer-conX-enable |data | 01000000
framebuffer-conX-type | data | 00080000
framebuffer-conX-pipe | data|12000000
```

> chỉ add patch `framebuffer-conX-pipe` nếu như  máy của bạn bị restart sau khi được cắm thiết bị `HDMI` vào máy, bạn cần thay đổi giá trị pipe của đầu nối HDMI thành `12` sẽ giúp bạn giải quyết vấn đề này

> Nếu các bạn dùng các cổng kết nối khác thì thay đổi vault của mục framebuffer-conX-type theo bảng sau

![Screen Shot 2021-08-02 at 10.58.50.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/09/03-11-02-35-Screen%20Shot%202021-08-02%20at%2010.58.50.png)

> Các bạn thay chữ X thành index của con liên kết với cổng kết nối của các bạn như của mình là `con1`

B7: Save lại và Reboot. 

> Các source tham khảo: [**Cài driver âm thanh, đừng để Hackintosh của bạn im lặng mãi!!!**](https://hackintosh.vn/driver-audio#hdmisound) **|** [**Fixing audio with AppleALC | OpenCore Post-Install (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/universal/audio.html#making-layout-id-more-permanent) **|** [**Các loại đầu nối vá lỗi | OpenCore Sau cài đặt (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/intel-patching/connector.html)
