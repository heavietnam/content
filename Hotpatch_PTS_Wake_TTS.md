# Hotpatch PTS Wake TTS

> Phần này dành cho các máy khi sleep thì shutdown máy hoặc wake ko lên

B1: Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/) 

B2: Sử dụng app MaciASL để mở file DSDT 

> link: [Releases · acidanthera/MaciASL · GitHub](https://github.com/acidanthera/MaciASL/releases))

B3: Search các từ khóa sau:

- `Method (_PTS, 1, NotSerialized)`
- `Method (_WAK, 1, NotSerialized)`
- `Method (_PTS, 1, Serialized)`
- `Method (_WAK, 1, Serialized)`
- `Method (_TTS, 1, NotSerialized)`
- `Method (_TTS, 1, Serialized)`

B4: các bạn tải file `.dsl` sau về: [OpenCore-HotPatching-Guide/SSDT-PTSWAKTTS.dsl at master · jsassu20/OpenCore-HotPatching-Guide · GitHub](https://github.com/jsassu20/OpenCore-HotPatching-Guide/blob/master/10-PTSWAK%20Comprehensive%20Extension%20Patch/SSDT-PTSWAKTTS.dsl) (Lưu ý: Nhớ chỉnh sửa file nếu không tìm thấy dòng của `tts` thì các bạn xóa `method tts` đi sau đó save lại nhớ chú ý cú pháp các dấu `{ }` phải tương ứng đúng dòng đúng hàng đúng khoảng cách)

B5: Compile file `.dsl` thành file `.aml`, mình có hướng dẫn cách chuyển ở dưới phần patch DSDT phần 1 sao đó các bạn save file lại bỏ vào `EFI ⇒ ACPI`và nhớ snapshot lại file config. 

B6: Add các patch sau vào config theo link sau: [OpenCore-HotPatching-Guide/Comprehensive patch rename.plist at master · jsassu20/OpenCore-HotPatching-Guide · GitHub](https://github.com/jsassu20/OpenCore-HotPatching-Guide/blob/master/10-PTSWAK%20Comprehensive%20Extension%20Patch/Comprehensive%20patch%20rename.plist) (khuyến khích chuyển về dạng file xml để dễ copy và đối chiếu mình có hướng dẫn ở trên ở phần [VI.2](https://heavietnam.ga/2021/09/29/vi-2fix-gprw-uprw-lanc/) | không add toàn bộ mà các bạn kiếm thấy mới add thôi)

B7: Save và reboot 

***Lưu ý: Nguồn:*** [***OpenCore-HotPatching-Guide/10-PTSWAK Comprehensive Extension Patch at master · jsassu20/OpenCore-HotPatching-Guide · GitHub***](https://github.com/jsassu20/OpenCore-HotPatching-Guide/tree/master/10-PTSWAK%20Comprehensive%20Extension%20Patch) ***(***

***1 số các SSDT mở rộng các bạn có thể tham khảo link sau:***  [***OpenCore-HotPatching-Guide/10-PTSWAK Comprehensive Extension Patch at master · jsassu20/OpenCore-HotPatching-Guide · GitHub***](https://github.com/jsassu20/OpenCore-HotPatching-Guide/tree/master/10-PTSWAK%20Comprehensive%20Extension%20Patch) ***)***

***Lưu ý 2 : chỉ khi thật sự cần thiết mới fix pts wake vì khi dùng không đúng nó có thể gây tác dụng phụ***
