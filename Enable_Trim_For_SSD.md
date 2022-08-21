# Enable Trim For SSD

> Trim là một công nghệ cải tiến trên những dòng ổ cứng SSD. Chúng hỗ trợ ổ cứng SSD trong việc tăng tốc độ ghi, đọc và xử dữ liệu trên ổ cứng. Đây chính là yếu tố khiến ổ cứng SSD được đánh giá cao hơn những ổ cứng thông thường hiện nay.

B1: các bạn download hackintool [tại đây](https://github.com/headkaze/Hackintool)

B2: Mở hackintool ở tab `Boot`

![](https://i.imgur.com/jzqlZMU.png)

B3: Chọn vào dòng Enable Trim for SSD

![](https://i.imgur.com/pH47r8n.png)

B4: Ấn vào biểu tưởng Apply Bootloader Patches

![](https://i.imgur.com/4mHRSOE.png)

B5: Ấn OK

![](https://i.imgur.com/egjSMvi.png)

B6: Chọn nơi lưu ở Desktop

![](https://i.imgur.com/x0NvCPb.png)

B7: Ấn OK

![](https://i.imgur.com/wPIuSQZ.png)

B8: Mở file config vừa tạo ra file config đang sử dụng

![](https://i.imgur.com/ahmyClE.png)

B9: Copy patch `0` ở file config vừa tạo vào file config đang sử dụng theo đường dẫn sau `Root --> Kernel --> Patch`

![](https://i.imgur.com/ljsK1mR.png)

B10: Add thêm dòng `EnableTRIM|Data|74727565` ở `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82`

![](https://i.imgur.com/IjZ0CKC.png)

B11: Save lại và reboot

B12: Tiến hành check trim Truy cập vào system report theo đường dẫn `SATA/SATA Express --> TRIM Support`

![](https://i.imgur.com/EIqlqZJ.png)
