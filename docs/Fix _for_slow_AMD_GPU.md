# Fix for slow AMD GPU

Ở macOS Monterey version 12.3+ các AMD GPU sẽ bị Apple bóp hiệu năng để fix điều này các bạn sẽ làm như sau:

B1: Tải [gfxutil](https://github.com/acidanthera/gfxutil/releases).

B2: Mở terminal lên kéo gfxutil vào Terminal và gõ `-f GFX0`

![](https://i.imgur.com/EVhS2mn.png)

B3: Bạn tải file sau về [fix.xml](https://www.mediafire.com/file/1wmh922goaj8khg/fix.xml/file) và mở bằng [ProperTree](https://github.com/corpnewt/ProperTree).

![](https://i.imgur.com/RaIquFF.png)

B4: Các bạn tiến hành thay:

- 1: Thay đường dẫn ở bước 2 vào.
- 2: Thay giá trị sau theo model:
  - Radeon 5500: `ATY,Python`
  - Radeon 5600 / 5700: `ATY,Adder`
  - Radeon 6600: `ATY,Deepbay`
  - Radeon 6800: `ATY,Belknap`
  - Radeon 6900: `ATY,Carswell`

B5: Save lại và Reboot.

**Source tham khảo: [Fix for slow AMD GPU after Monterey 12.3 update – Guides and Tutorials – Olarila](https://www.olarila.com/topic/25302-fix-for-slow-amd-gpu-after-monterey-123-update/?fbclid=IwAR39arh0lOj-WAyLGNPJ6n_oY7zvFG11fP2F5HGmqdn20E1-uICORbgIAXE) | [PSA: macOS Monterey 12.3 and AMD 5xxx and 6xxx GPU issues : hackintosh (reddit.com)](https://www.reddit.com/r/hackintosh/comments/ter3g2/psa_macos_monterey_123_and_amd_5xxx_and_6xxx_gpu/)**
