# Patch DSDT Phần 2

## ***Làm quen với MaciASL***

B1: Thiết lập MaciASL về 5.0 hoặc cao hơn. 

![](https://lh6.googleusercontent.com/F04DBx2fmlW8trALW90inBbb8t3UXrhKRYyyumCkPCyZaMlNT2p-MLE4dD9I2rFrzxzQp5Xus_od5UKy-iQJTHj7bogrKiuWZnWuzMqBy1hnGlLf3iCMunC1S-qkM17ssDVBkAHo=s0)

B2: Thêm các Source hữu ích: 

- Vào Setting của MaciASL (Command + “,”).

![](https://lh4.googleusercontent.com/DPRPDwh2g5Hld1fV3SGBQDjkZU4V3_VftBHgS74WU8_HPwSZR2IGTkSMt3Op9ZHlaQ3ivqAKW1x6aKTnXEg7RzdyLI3Sui1xEgcGGXhtQ0VUSVKvdxMyItrud7iNAxzPsv7_95mn=s0)

- Bấm dấu “+” để thêm Source. 

![](https://lh3.googleusercontent.com/EVcqFbg-RLgfUmQeVTyX4wqGRw2RPUMo-FxPE2PuZfOJSOFXDMQ-7RpEUwy67YrJzes-L1G1QUtstiEv0vORd5KUFj3X4ygmngfph7z-0nQC5P1cxYOKM5YoWjNwhpNPNcq56DIx=s0)

- Các Source hữu ích 

- **Rehabman DSDT Patches**:  
  http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master

- **HP Probook patch**:  
  http://raw.github.com/RehabMan/HP-ProBook-4x30s-DSDT-Patch/master

- **PCbeta dxxs dsdt Patches**:  
  http://raw.github.com/Yuki-Judai/dxxs-DSDT-Patch/master

- **MacMan Gigabyte**:  
  http://www.tonymac86.com/DSDT/

- **Toleda Audio HDMI HD4600/Haswell/8 Series**:  
  http://raw.github.com/toleda/audio_hdmi_8series/master

- **Toleda Airport PCle Half Mini**:  
  http://raw.github.com/toleda/audio_ALCInjection/master

- **Toleda Audio Realtek ALC injection**http://raw.github.com/toleda/audio_ALCInjection/master

- **Toleda Audio HDMI UEFI Audio dsdt edits – Desktop/Laptop/Intel NUC**http://raw.github.com/toleda/audio_hdmi_uefi/master

- **Toleda Audio HDMI HD4000/Ivy Bridge/7 Series**http://raw.github.com/toleda/audio_hdmi_hd4000/master

- **Toleda Audio HDMI HD3000/Sandy Bridge/6 Series**http://raw.github.com/toleda/audio_hdmi_hd3000/master 

- **Toleda Audio HDMI 5 Series**http://raw.github.com/toleda/audio_hdmi_5series/master

- **ASUS:** **All-in-one patches for ASUS motherboards**http://maciasl.sourceforge.net/pjalm/asus/

- **MSI:** **All-in-one patches for MSI motherboards**http://maciasl.sourceforge.net/pjalm/msi/

- **Zotac:** **All-in-one patches for Zotac motherboards**http://maciasl.sourceforge.net/pjalm/zotac/

- **Gigabyte:** **All-in-one patches for Gigabyte motherboards**http://maciasl.sourceforge.net/pjalm/gigabyte/

- **ASRock:** **All-in-one patches for ASRock motherboards**http://maciasl.sourceforge.net/pjalm/asrock/

- **Graphics:** **Patches for Intel HD and AMD/nVidia graphic cards**http://maciasl.sourceforge.net/pjalm/graphics/

- **Intel Series 6:** **Intel Series 6 Patches for SATA, USB, SMBUS, IGPU, GbE and general fixes**http://maciasl.sourceforge.net/pjalm/intel6/

- **Intel Series 7:** **Intel Series 7 Patches for SATA, USB, SMBUS, IGPU, GbE and general fixes**http://maciasl.sourceforge.net/pjalm/intel7/

- **Intel Series 8:** **Intel Series 8 Patches for SATA, USB and general fixes**http://maciasl.sourceforge.net/pjalm/intel8/

- **Intel Series 9:** **Intel Series 9 Patches for SATA, USB and general fixes**http://maciasl.sourceforge.net/pjalm/intel9/

- **General:** **General patches for Shutdown, HDEF, USB3, SATA and LAN**http://maciasl.sourceforge.net/pjalm/general/

B3: Apply các Patch Online như hình: ![](https://lh6.googleusercontent.com/Q3WNU_soF5eKLmr7K0P-Nns8By13aW_6eg7y2lW1N5_9cnTybAKbdOOUqimed4OpuRDW5v9wl8rKksZJhhohmzRzo9Vxe7t6yjvJR9vIv98KnqYMHzf7KesR_TcfbWE7l5izMIFL=s0)

![](https://lh4.googleusercontent.com/T7tl4_1XDJ9M-eBrqpxOfiUo-mr8zxx9BZzNP7MSLVDcJdhc8iiTy78Wqe815EzLZCFKFSPWhsSMk76-Wa9WRBywooHfI-aklTNOEkwSoSYDDEIr1V_SpJmu_VqNB4hfr8yWGb3X=s0)

Các bạn chọn các Patch ở mục 1 rồi Apply vào nút ở mục 2

B4: Apply các patch offline: 

- Vào trang offline của các patch muốn apply ( vd mình muốn apply patch battery  )

- Vào patch muốn down chọn như hình 

- Nhấn tổ hợp phím Command + A và Command + C 

- Tải [sublime text](https://www.sublimetext.com/) 

- Nhấn tổ hợp phím Command + V 

- Và Command + S và save các bạn đặt tên tùy ý ở cuối tên các bạn để là .txt 

- Các bạn chọn như hình:

![](https://lh3.googleusercontent.com/GU4JHoCFRE0oDWOFALFp5c8mAkxzjdaMr-FGXmQqYssiW7mZ4HF-RBHBlOTieeHzVeRiCMSxUeG09sjZSXVOnKFGREM-JAA_gwXSlRukO5MBsroR5X5h6zRSEXBJskYUExf7iTPR=s0)

![](https://lh5.googleusercontent.com/t9RJSSlPZ27aTGC_J-yveH2L4wkyMa1qSN5oRu9-u1jZ5IUbN2mQ-Cn4w3Uq3mc22J3WYHrEV5iIJKQ3_c3H5Xa5LvSqB3s8MfcIF33kXYGacYjzLD7iYdkgynzghhqGC4_DBxdh=s0)

![](https://lh6.googleusercontent.com/OxS54qXXtyi1KFAAXqNA-nHsKiz-g0_GvXsR6jUWNvw__LdTlhG_afL676EmWr-2TVRXHntovUkltJYb_GwXp7by171_GYQPCOVcCjBNOaQljRZOIPFIE0ycdWnNnMIjxh5_gKgx=s0)

**Lưu ý : Thông thường khi dùng DSDT 90% các bạn sẽ bị treo táo do DSDT chưa Rename EC thì các bạn hãy làm theo sau:**

**B1: Mở DSDT với MaciASL bấm tổ hợp phím Command + F và tìm từ khóa sau PNP0C09**.

**B2: Nhìn đến mục Device để xác định Device cần đổi tên thường sẽ là các tên sau EC0, H_EC, ECDV,…..**

**B3: Vào nút**  

**B4: Nhận vào dòng replace “EC” và nhấn “ALL”** 

![](https://lh5.googleusercontent.com/oabS33LjsoOuvpLLpohUN00-IQLKAe_IaNLM_DyMVJKEoxf19gps8UBd8GVARPKTOUuZZVJ8i36hgW6Pev3PwNGSMIjy7zEngPYX2z14JvsbrUHF-IKKwkK7G5EHr2sEwsQK5OaO=s0)

## ***XXVII.4 : Fix Error***

Đa số các DSDT Native và được biên dịch đúng thường sẽ không có Error nhưng vẫn có 1 số trường hợp ngoại lệ là vẫn có từ 1-3 Error thì các bạn sẽ fix như sau:

B1: Bấm vào nút ![](https://lh3.googleusercontent.com/G2z06nvwh0acmhihJFr76tlu9LiO8EDZJaFCEgcjleE48z-7F_7_j971PCE7KxA3UeSWoRlULkN1fkBbwCRzcPaclPEGx92rMmmF7uugBpsR-cfJBPqOT89EgRmIsq6M7HK5xV2Y=s0) để hiển thị lỗi và fix lỗi như hình: 

![](https://lh6.googleusercontent.com/mGGzpYDNegFFpNGwSY8X-WHPVj-fOjtzVqSe1e9II6T5hwvprDMnvQ8C6cmuMPbHV7PP17U2Z6YwVxNJDRERQPzCI7TMAVCrev_ueZsXvB2ljU5RddBZhLTtW6aTGPZ5KKmIAe8E=s0)

1: Số dòng lỗi.

2: Thứ tự code bị lỗi. 

3: Nội dung lỗi. 

B2: Add source của Rehabman vào [http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master](https://raw.github.com/RehabMan/Laptop-DSDT-Patch/master) hoặc add các patch offline theo hướng dẫn ở trên từ link sau đây: 

https://github.com/RehabMan/Laptop-DSDT-Patch

B3: Apply các patch vá lỗi của Rehabman (thường sẽ có các ký tự đầu là “syn”).

**Lưu ý : Các bạn nên Remove các _DSM methods của dsdt trước khi Apply các bản vá lỗi của Rehabman các Remove như sau các bạn làm như hình**:

![](https://lh4.googleusercontent.com/ryb4stc6eHWDke87B_y7z_uWmIQWSjr2rJATbBed0BgEo4LrA-LTcT6qg3YkCxIgpImXJf_IodkgCs9joju-lovo9tJeU6YEegEBh6_M5r3Va8MvRjC3WMvFYG3Jx0QV_UF4a7-g=s0)

## 1 số lỗi phổ biến

Lỗi bm6h khi gặp lỗi như hình:

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-19.17.57.png?w=1024)

Khi gặp lỗi này các bạn thay đoạn code sau vào chỗ bị lỗi:

```

```

**Lưu ý 2 : Luôn phải Fix Error trước khi Apply các bản vá vào**.

**Lưu ý 3: Các nguồn tham khảo** [**Getting a copy of your DSDT | Getting Started With ACPI (dortania.github.io)**](https://dortania.github.io/Getting-Started-With-ACPI/Manual/dump.html#from-windows) **|** [**[Guide] Patching LAPTOP DSDT/SSDTs | tonymacx86.com**](https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/) **|** [**[Guide] Patch DSDT cho máy Hackintosh (Phần 5) – UEFI & OS (niemtin007.blogspot.com)**](https://niemtin007.blogspot.com/2015/05/guide-huong-dan-patch-dsdt-cho-may.html) **|** [**DSDT, SSDT: Những kiến thức cơ bản | Lập Trình TV (laptrinhtv.blogspot.com)**](https://laptrinhtv.blogspot.com/2014/03/dsdt-ssdt-nhung-kien-thuc-co-ban.html#.YRzAS9Tiu00)

**Lưu ý 4: Đối với những máy dùng Patchmatic để Dump DSDT mà gặp Error thì xui cho các bạn rồi các bạn bắt buộc phải dùng DSDT gốc máy nếu dùng DSDT gốc máy thì trong ACPI các bạn phải xoá hết (bao gồm các patch rename) đi chỉ để mỗi DSDT thôi rồi add các patch vào từ từ.** 

**Lưu ý 5: Đối với Clover các bạn cần phải bật Drop OEM lên nếu muốn Load được DSDT**

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-19.46.50.png?w=874)
