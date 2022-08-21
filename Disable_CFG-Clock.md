# Disable CFG-Clock

## Kiểm tra xem Bios của bạn có CFG Không

B1: Bạn tạo 1 bộ EFI debug theo hướng dẫn chi tiết [ở đây](https://heavietnam.ga/2022/04/09/opencore-debug/)

B2: Chỉnh target là 67

B3: Các bạn tiến hành boot vào opencore debug

B4: mở file log nhận được trong EFI

![](https://i.imgur.com/sfGRatl.png)

B5: search `OCCPU: EIST CFG Lock`

B6: Chú ý vào kết quả hiện ra nếu giá trị trả về là 1 thì tức `CFG Clock` đang enable và ngược lại nếu kết quả trả về 0 tức đang disable

![](https://i.imgur.com/0AXHI5R.png)

## Cách 1: Disable trong config

Nhược điểm sẽ bị mất 1 ít hiệu năng.

B1: Mở config băng [ProperTree](https://github.com/corpnewt/ProperTree).

B2: Các bạn Enable AppleCpuPmCfgLock và AppleXcpmCfgLock ở phần Kernel -> Quirks.

- Gen 3 và 2: dùng AppleCpuPmCfgLock
- Gen 4+: Dùng AppleXcpmCfgLock

## Cách 2: Dùng Tool Grub-Mod-Setup

B1: Các bạn Download Tool [tại đây](https://github.com/datasone/grub-mod-setup_var/releases).

B2: Các bạn Download UEFITool [tại đây](https://github.com/LongSoft/UEFITool/releases).

B3: Các bạn Download Universal-IFR-Extractor [tại đây](https://github.com/LongSoft/Universal-IFR-Extractor/releases).

B4: Các bạn Download mã Bin BIOS của các bạn (cái này mình không hướng dẫn được các bạn lên Google search model + Bin BIOS ).

B5: Các bạn kéo File Bin BIOS vào UEFI Tool sau đó ấn Command + F và gõ `CFG Lock`

![](https://imgur.com/Wn0iuT4.png)

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/uefi-tool.5f61054a.png)

B6: Các bạn click chuột phải vào Setup và nhấn `Export as is`.

![](https://imgur.com/gaOOJ8o.png)

Sau đó Save dưới dạng `.bin` hoặc `.sct`.

> Các bạn tìm thấy CFG clock ở đâu thì export nó ngay dòng đó. Nhưng nếu bạn export nó ra và extra bị lỗi Protocol: Unknown thì các bạn sẽ tiến hành thử export lần lượt các dòng thuộc cùng 1 đơn vị với dòng tìm đuợc CFG Clock

B7: Các bạn mở Terminal lên

Và kéo file ifrextract đã tải ở trên vào sau đó ấn dấu cách tiếp tục kéo File vừa xuất tiếp theo bạn gõ đường dẫn File .txt muốn xuất. Ví dụ:

`path/to/ifrextract path/to/Setup.bin path/to/Setup.txt`

![](https://imgur.com/rNX7OS4.png)

B8: Sau đó mở File vừa xuất lên và search `CFG Lock, VarStoreInfo (VarOffset/VarName):` để tìm OffSet.

![](https://imgur.com/rN8cSzN.png)

Như ở đây là 0x54.

B9: Reboot và chọn GRUB Shell sau đó gõ lệnh:

```
setup_var 0x5A4 // thay 0x54 thành offset bạn tìm được
```

Nếu gặp lỗi `Error: offset is out of range` thì các bạn gõ dòng sau:

```
setup_var2 0x5A4
```

Nếu vẫn gặp lỗi `Error: offset is out of range` thì các bạn gõ dòng sau:

```
setup_var_3 0x5A4
```

Nếu không còn gặp lỗi đó nữa các bạn gõ lệnh:

```
setup_var_3 0x5A4 0x00
```

Sau đó gõ:

```
reboot
```

Và như vậy là xong bạn có thể tắt đi `Kernel -> Quirks -> AppleCpuPmCfgLock` và `Kernel -> Quirks -> AppleXcpmCfgLock`.

> Đối với lệnh được dùng ở bài viết này thì đã cũ nên đối với 1 số model sẽ báo lỗi các bạn sẽ cần tham khảo lệnh mới tại bài viết [này](https://heavietnam.ga/2022/07/08/cach-mod-bios/)

Source tham khảo: [Fixing CFG Lock | OpenCore Post-Install (dortania.github.io)](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#turning-off-cfg-lock-manually)
