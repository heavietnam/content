# Fix Shut down

## ***Lỗi Shut down Restart***

> Đối với 1 số máy khi Shut down thì tự động Restart lại bắt buộc phải Shut down bằng phím cứng thì có thể thử cách sau đây:

B1: Down file từ nguồn [sau đây](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/FixShutdown-USB-SSDT.dsl).

B2: Chuyển file từ dạng file .dsl sang dạng file .aml theo hướng dẫn ở Mục patch DSDT phần 1

B3: Bỏ file vào mục `EFI ⇒ ACPI `sau đó snaps lại (đối với Clover các bạn bỏ file ssdt vào `EFI ==> ACPI ==> Patched`)

B4: Các bạn Add vào config các Patch theo links sau [OpenCore-Post-Install/FixShutdown-Patch.plist at master · dortania/OpenCore-Post-Install · GitHub](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/FixShutdown-Patch.plist) 

B5: Save lại. 

B6: Reboot và tận hưởng thôi. 

***Lưu ý: Source tham khảo*** [***Fixing Shutdown/Restart | OpenCore Post-Install (dortania.github.io)***](https://dortania.github.io/OpenCore-Post-Install/usb/misc/shutdown.html)

**Lưu ý 2: Đối với các bạn đã làm theo những vẫn bị lỗi thì các bạn cóthể thử Apply Patch sau vào DSDT** [**Laptop-DSDT-Patch/system_Shutdown_restart.txt at master · RehabMan/Laptop-DSDT-Patch (github.com)**](https://github.com/RehabMan/Laptop-DSDT-Patch/blob/master/system/system_Shutdown_restart.txt) **sau đó trích ra SSDT để sử dụng (xem ở mục [XXVIII](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/) | hãy chắc rằng cách này hoạt động trên DSDT và lưu ý nếu như sử dụng DSDT bạn cần Rename EC đọc ở phần lưu ý mục [XXVII](https://heavietnam.ga/2021/09/29/xxvii-patch-dsdt-phan-2/) để biết cách Rename EC)**

## Lỗi Shut down do NVRAM:

B1: Mở Terminal và gõ các lệnh sau: 

```
sudo -s

nvram -c

nvram myvar=test

exit
```

B2: Restart máy và mở Terminal gõ lệnh sau:

`nvram -p | grep -i myvar`

Nếu không có giá trị trả về thì bạn hãy bỏ [SSDT-PMC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PMC.aml) vào 

> do NVRAM không hoạt động

nếu có giá trị trả về `myvar` test thì các bạn chuyển sang phần tiếp theo 

> đối với OC nhớ Snapshot

**Lưu ý: Source tham khảo ​​**[**Emulated NVRAM | OpenCore Post-Install (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/misc/nvram.html#enabling-emulated-nvram-with-a-nvram-plist).

## Fix Shut down do DSDT bị lỗi

B1: Các bạn dump DSDT như hướng dẫn ở mục [XXVI](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/). 

B2: Các bạn biên dịch DSDT thành file .dsl như hướng dẫn ở mục XXVII.2 

B3: Apply các patch sau [shutdown v1](https://github.com/RehabMan/Laptop-DSDT-Patch/blob/master/system/system_Shutdown.txt) [shutdown v2](https://github.com/RehabMan/Laptop-DSDT-Patch/blob/master/system/system_Shutdown2.txt) (không apply cả 2 patch cùng lúc thử apply Shut down v1 trước nếu không được thì xóa patch Shut down v1 rồi mới apply Shut down v2 | apply patch theo hướng dẫn [XXVII](https://heavietnam.ga/2021/09/29/xxvii-patch-dsdt-phan-2/)).

B4: Trích SSDT-Shutdown từ DSDT theo hướng dẫn ở mục [XXVIII](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/) (hãy chắc rằng các patch đã apply vào DSDT hoạt động trước khi trích ra SSDT).

B5: Bỏ SSDT vừa trích vào EFI ⇒ ACPI và Snapshot lại.

B6: Save và Restart.
