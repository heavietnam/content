# Cách thêm Boot vào BIOS

## Cách 1 : Add đường dẫn vào BIOS

### Add đường dẫn vào BIOS bằng Bootice

#### Chuẩn bị :

- 1. WinPE hoặc dual boot với Windows.
- 2. App Bootice. 
- 3. Tắt Secure Boot trong BIOS.

#### Tiến hành:

- Đầu tiên các bạn vào mục EFI trong Bootice và add tên đường dẫn là OpenCore BootManager và chọn đường dẫn theo `EFI\OC\opencore.efi (hoặc EFI\OC\OpenCore.efi`)
- Sau đó bấm Open và Save sau đó Restart máy và tận hưởng thành quả.

### Add đường dẫn vào BIOS bằng EasyUEFI

#### Chuẩn bị:

- 1. WinPE hoặc Windows.
- 2. EasyUEFI.
- 3. Tắt Secure Boot trong BIOS.

#### Các bước tiến hành:

- B1: Các bạn bấm vào ![](https://lh3.googleusercontent.com/m3DFWg5B7gbYxI2PETS0uW7mZpXngen5URbjj-tKqwsBzuBl9qfh989NX0u1A-_uGQyPyDGergssXSZyDQRQtD0KDcacQDpSetTRDSox7gpCOHrTwmyVx6_e1eUikGJtjA-OBP5Y=s0) để quản lý các Boot Option của mình 
- B2: Các bạn chọn ![](https://lh4.googleusercontent.com/h2b097gYHZkj42UROgC8HBcIvRMYsiJteX6ZE4gJK3hT-ELdlhVViBzNXLZrx53Mb9IHCE-_WlledAc1PAHDD57EGUHpVK5lRBL2hPsDzP4Ho7baUYQ4aTTI5Q_gb4lWfYRsAmOH=s0) để tiến hành Add Boot Option.
- B3: Các bạn chọn 
  - Type : Linux or other OS.
  - Description: 
    
    >  tên Boot Option các bạn tự đặt (ở đây mình đặt là OpenCore).
  - Please select the target partition
    
    > chọn ổ EFI của bạn.
  - File path: Chọn Browse và dẫn nó theo đường dẫn `\EFI\OC\OpenCore.efi hoặc \EFI\OC\opencore.efi` 
  - > tuỳ thuộc của các bạn.

![](https://lh3.googleusercontent.com/Wy1ZmJhl2zHCaL1pPvy9IzQkMvP600YdFm3jb3AZdCVFfRswYEw0IPBy4Erktd-OHxTsA6duEL62MQEEPIHMpRFhsej-fV8eqcn9Gc_kQE3NxSqyh8mpb1gOWKftWGB1huG-X2B2=s0)

Lúc này Boot Option của bạn vừa mới thêm vào nó sẽ nằm ở cuối cùng. 

![](https://lh4.googleusercontent.com/7e2jhql24pbf8V4AVzImMNWU18FNaISWJ_Je7VgHIIfH7RVSKQLoSg9DRl40fy8vuaRy7y-uAYA1nRdXL33pQjlAlkrzGFUfKBBDbgk4ZXFm4dzD8DLaYu4bKPxf2NeZ-oPh-Wok=s0)

- B4: Để Boot Option bạn vừa mới thêm vào lên đầu thì các bạn bấm ![](https://lh3.googleusercontent.com/W04saciKSBfuzQRyJK_I6-j021qPgHu2ehYgbpS50De1AxaoXv7bQ7U0S-VkOL5MREmAPIjii9vm_cHPUwtzurzcIpCmgXsZ38JToIPy8Azm7gTZnhnJu1DTaYTCh9-88_TsnYav=s0) đến khi nào nó lên phía đầu thì thôi.

![](https://lh3.googleusercontent.com/gPMPhDv7lkkkWgH5ZisTcNRxmCRC_JVSXewqWliPUYJuYT638NOwVhHpTODJuPLmjT6aaRohBfoTbNdT2Mx9Qk8pQwmFK2TG_dnq9uz9pokGJUn2xWkKP_NK_auNmbpSyOqof0Fp=s0)

B5: Tắt App Restart lại máy và tận hưởng thôi. 

**Lưu ý: Khi dùng cách này khi các bạn Reset NVRam nó sẽ tự mất Boot Option và các bạn phải Add lại .**

> nếu muốn không mất Boot Option thì các bạn làm theo cách 2 sử dụng Launch Option nhé

## Cách 2 : sử dụng launch option ( oc > 0.6 )

B1: mở config bằng propertree links ở trên 

B2: tìm đến mục `Root ⇒ Misc ⇒ Boot ⇒ launch option | string | full`

B3: save lại và reboot

**Lưu ý : nếu chưa xuất hiện boot-option thì các bạn restart nvram lại nhé**

**Lưu ý 2 : nếu khi boot vô menu khởi động opencore mà mất quá nhiều thời gian ( 3’- 4’ ) thì các bạn sửa config lại như sau :**

**`Root ⇒ Misc ⇒ Boot ⇒ launch option | string | Short` ( nhớ restart nvram lại nhé )**

**Source tham khảo: [dortania.github.io](https://dortania.github.io/OpenCore-Post-Install/multiboot/bootstrap.html#prerequisites)**
