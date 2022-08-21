# Fix Backlight

## ***Cách 1: Fix bằng SSDT Prebuilt***

B1: Tải [SSDT-PNLF.aml](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml)

B2: Snapshot Config (oc only)

B3: Reboot và tận hưởng. 

> Lưu ý : Với các dòng uhd thì các bạn cần phải add code “-igfxblr” này vào boot-arg

> hoặc có thể dùng device properties
> 
> B1: Đi đến device properties ⇒ PciRoot(0x0)/Pci(0x2,0x0)
> 
> B2: Add enable-backlight-registers-fix | Data | 01000000

## ***Cách 2 : Fix thủ công***

B1: Vào Windows `Device Manager -> Display Adapters -> Properties -> Details > BIOS Device Name` để tìm thông tin về màn của các bạn. 

(sau khi làm xong sẽ được như hình | các bạn có thể  dùng WinPE để xem)

![](https://lh5.googleusercontent.com/Rs57hQBIeqWiEMr0aJ44wk-vcJH8Rfm8x8YTX34kS7ujbUalMZKsqjx1wcLIjTBS8a3evi-ulUvSQtHaXLSkwhXpPzf8WrS9YrGsRVMtgj7R4xNAagtcB07Zp3NcU5PPs--mllcR=s0)

B2: Tải `SSDT Prebuilt` ở trên. 

B3: Tải MaciASL để chỉnh sửa file SSDT vừa tải 

> lưu ý : chỉnh cho giống với BIOS Device Name của các bạn nếu là GFX0 thì không cần sửa ở đây mình chỉ demo là BIOS Device Name của các bạn là `PCI.GPU0` thôi.

![](https://lh4.googleusercontent.com/yCbpcUq86x-uK1vYLwUTf5y-UcY16jGuLh9O3X_iZM4x3E51lMPX24MWOH6AqZUWFy3-rnz1n1heKo0_nMrh0kKGdNr2uwHAOmiP6Aej6SQ4m5RoZCiPib-w6J4lzaoJsnBadKPQ=s0)

B4: Mở file .dsl và chỉnh như hình 

> đổi đường dẫn mặc định thành đường dẫn của bạn đổi `.GFX0 ⇒ .GPU0`  
> 
> sau khi rename sẽ được như hình

![](https://lh3.googleusercontent.com/ylB6HTaAStIDAkiLamvLp2eczCc8xcgOQmJDuTQK_rkpJ-5vsvqvJE_1KW3MF1XAkunhmnG-J72o2e6OIs3vq8noiHt9wS7334ZmJpM-ElgI9T-pGt4bnZzu6urkfCSkOLtwwAnc=s0)

B5: Bấm vào `complie` 

> nếu không có lỗi thì Save lại 

sau đó các bạn bỏ file SSDT vừa Save và chuyển định dạng vào `EFI ⇒ ACPI`(hoặc `EFI ==> ACPI ==> Patched`) 

> cách chuyển định dạng file .dsl ⇒ .aml xem [ở đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)).

B6: Snapshot và Reboot

> chỉ với OC

**Lưu ý: Nếu làm như trên vẫn chưa nhận độ sáng các bạn bỏ kext SMC Light Sensor vào rồi snaps lại nha ( source tham khảo:** [**Fixing Backlight: Manual | Getting Started With ACPI (dortania.github.io)**](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/backlight-methods/manual.html#edits-to-the-sample-ssdt) **).**
