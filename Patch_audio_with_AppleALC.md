# Patch âm thanh với AppleALC

# **Patch âm thanh**

**B1:** Tải xuống kext `AppleALC` từ nguồn [sau](https://github.com/acidanthera/applealc/releases)

**B2**: Down `hackintool` từ nguồn [sau](https://github.com/headkaze/Hackintool/releases)

**B3**: Mở Hackintool, vào tab `Sound` vào mục `ALC Layout ID `chọn layout phù hợp và thay vào config mục `NVRAM ==> boot arg ==>alcid=xx` ( của mình sẽ là `alcid=3` )

![](https://lh4.googleusercontent.com/gjx8zKgIrbfDE0iaY5JkMcIiago9ZvPnUUivphi1fMeeJclBNWoMpRo_XiwioV__VNZFIhkE4o5IrrpvCLti-mcuEct8qcOlmhg0xhUJsfMDhCn9HALMT9pYcXHoOEZZ53PFby2A=s0)

**B4**: Snapshot config và restart máy 

## **LƯU Ý:**

> Nếu bạn đã làm và máy vẫn không nhận `mic` hoặc ko nhận `speaker` thì bạn thay tất cả `layout-id` khác

> Thử từng layout-id cho tới khi nhận đầy đủ

> Nếu phần hackintool mục sound của bạn không hiện gì cả (như ảnh) thì các bạn sẽ tiến hành patch hpet như hướng dẫn ở dưới

![](https://lh5.googleusercontent.com/VsBezumgDn0Kjraq1xpqjmzVLT3TaOEAPnCzIpRgMNa8E4bWhTCGZ2PeQfB_s9eiDyFNs9wbihtGbTkr-ibQ2CXACYhsVCwWoeElkPVjwqTkb7KC6nsIT6ZSq3F0iUpsDQigxiSB=s0)

> Nếu như đã patch hpet vẫn không hiện thì các bạn cứ patch bình thường  không ảnh hưởng lỗi thường  gặp ở gen 10 các bạn vào Linux để xác định codec (chỉ gen 9 + mới bị) 

* B1: vào terminal gõ lệnh sau  `cat /proc/asound/card0/codec#0 > ~/Desktop/codec_dump_0.txt`

![](https://lh5.googleusercontent.com/wqEGbGfcAtCuPunnfMGD36w3lXMZWtvBvjdijD7_nlohvhDgvs8mJa7WBUQ6tjDAsvBmKk8nDihwIUA9wxm_iDxJS5QW32fMmvV4ACdmQs2ebkiJs3_2FroH9y07okT8ZAMzyYYh=s0)

* B2: tìm alcid** [**tại đây**](https://github.com/acidanthera/AppleALC/wiki/Supported-codecs)

> một số bạn dùng efi prebuilt cũng có thể gặp lỗi này

> Đối với một số bạn thử fix `AppleHDA` không được và `AppleALC` bị lỗi thì các bạn có thể tham khảo cách cài cũng như tinh chỉnh VoodooHDA theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/iii-patch-voodoohda-khi-da-patch-thanh-cong-am-thanh-se-khong-thua-apple-alc/)

# **Patch Hpet, IRQ** **:**

> Lỗi này thường xảy ra đối với các máy Intel 5th gen trở xuống, khi gặp lỗi sẽ bị như ảnh 

![](https://lh5.googleusercontent.com/VsBezumgDn0Kjraq1xpqjmzVLT3TaOEAPnCzIpRgMNa8E4bWhTCGZ2PeQfB_s9eiDyFNs9wbihtGbTkr-ibQ2CXACYhsVCwWoeElkPVjwqTkb7KC6nsIT6ZSq3F0iUpsDQigxiSB=s0)

**B1**: Dump DSDT xem hướng dẫn ở mục [Patch DSDT phần 1](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)

**B2**: Gõ `D`  và kéo file DSDT của bạn vào tiếp nhấn enter

![](https://lh4.googleusercontent.com/v_l7aP6THGqwuhGA1OZEONrHCIgvf5c9KVKHrnU-XK8O26PoDtAOf7Aclv-Dek-kcq51LMdKfaUYjlGqEkOILrEG6vGf7rtv2Xq1E2Ad_2lbStIz4-F1QrjdWc8z4qR_obhaLgoj=s0)

**B3**: Chọn mục `patch hpet` chọn sau đó chọn `c` và enter 

> hoặc chọn phù hợp nhất cho máy của các bạn

**B4**: Lấy file `hpet.aml` vừa dum copy nó vào folder `ACPI` trong `EFI`

**B5**: Mở file `patches_OC.plist` vừa dump và copy mục patch qua mục patch của file `config` 

> nếu chưa chỉnh gì mục patch của file `config` thì bạn có thể thay thế nó bằng mục patch của file `patches_OC.plist` 

![](https://lh6.googleusercontent.com/Gbgd0KtHasm421xvq-9REljmGeEXYOlVGgNbdle0hUmAltAvPJGOei0fX0j-ifqbQ2XGTxgr0ZYsfHoiYZsN1PxT4OEIbzsuBVtqRLPMXJJr3qDXtyfBKCGVekAXkElQkJz8z1t7=s0)

**B6**: Snapshot và restart

> đối với clover thì copy file SSDT vào mục `EFI --> Clover --> ACPI --> patched `và copy file `patches_clover.plist ` vào config thay vì là file `patches_OC.plist`
