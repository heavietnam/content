# macOS 12: Monterey

## Tin tức mới trên Monterey

### SMBios:

Các SMBios sau sẽ ko còn support nữa:

- iMac15,x và cũ hơn.
- Macmini6,x và cũ hơn.
- MacBook8,1 và cũ hơn.
- MacBookAir6,x và cũ hơn.
- MacBookPro11,3 và cũ hơn.

Đối với những SMBios khác vẫn còn support.

### Hardware:

các GPU sau sẽ không được support nữa

- Ivy Bridge (HD 4000 and HD 2500 | bạn có thể dùng [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases) để add support và có thể dùng [HD4000 Patcher](https://github.com/chris1111/Patch-HD4000-Monterey) để add các kext và bundle của Big Sur)
- Nvidia Kepler (GTX 6xx/7xx Cards | bạn có thể dùng OpenCore Legacy Patcher để add support và có thể dùng [Kepler Patcher](https://github.com/chris1111/Geforce-Kepler-patcher) để add các kext và bundle của Big Sur)

### AMD Patcher:

Các bạn phải nhớ là Update Kernel Patcher nhé download [ở đây](https://github.com/AMD-OSX/AMD_Vanilla).

### Bluetooth:

Ở thời điểm hiện tại bluetooth vẫn còn 1 số lỗi hãy đợi cộng đồng update kext riêng các dòng card ar9XXX đã chính thức không còn support.

### OTA Update:

Nếu bạn đang sử dụng T2 SMBios thì phải boot với OpenCore 0.7.4+ và `SecureBootModel` set là `Default` nếu bạn không sử dụng T2 SMBios thì có thể set là `Default` hoặc `Disabled` xem chi tiết [tại đây](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html#apecid).

## 1 số app bị lỗi

Tổng hợp 1 số app bị lỗi trên monterey

1. **Illustrator 2021:** (kể cả bản quyền và thuốc): Chuột phải thanh Menu bị hiện ở góc dưới chứ không xuất hiện ngay chỗ click. Update: AI dùng bản quyền thì cập nhật lên Illustrator 2022 fix lỗi này rồi.
2. **Mamp Pro** bản thuốc ko chạy trên M1, Intel vẫn chạy.
3. **Cleanmymac:** ae cứ than ko chạy, nhưng maclife có hướng dẫn fix từ hồi còn Beta rồi.
4. **Wirecast:** Crash – Chưa có cách fix, chờ cập nhật thôi.
5. **Sketchup** (thuốc): Crash.
6. **Tg Pro** (bản thuốc): Crash: **Update:** Đã có bản 2.6.1 chạy được trên Monterey.
7. **Yoink**: Không chạy.
8. **Adobe Audition:** (bản thuốc) – Crash.
9. **On1 Photo Raw** – Crash.
10. **Smart Album** – Crash.
11. **TextSniper** – Crash.
12. **Total Finder** – Không ổn định.
13. **Airserver**.
14. **Sketch** – Lỗi trên Mac Intel, **M1 xài bình thường**.
15. **Photoscape x**.
16. **Fl Studio**.
17. **Itube Studio.**

## Fix lỗi không Mount Disk được

Hiện nay 1 số may xuất hiện tình trang không thể Mount được file dmg trên Monterey và đây là cách khắc mục:

B1: Mở terminal.

B2: Gõ dòng lệnh sau vào terminal.

`hdiutil attach [ khoảng trắng ] path\to\dmg file` 

![](https://imgur.com/xGfUsX3.png)

![](https://imgur.com/yKYs3DM.png)

B3: Các bạn sẽ thấy file dmg được mount ngoài desktop ấn vào là dùng được.

![](https://imgur.com/HTkzDvU.png)

**Lưu ý: Bài viết mang tính chất tạm thời tương lai có thể sẽ thay đổi thì mình sẽ cập nhật lại**

Source tham khảo: [maclife.vn](http://maclife.vn/)
