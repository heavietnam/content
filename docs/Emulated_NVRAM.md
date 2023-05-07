# Emulated NVRAM

Hầu hết các model đều native nhận NVRAM nhưng đối với các chip set X99 và X299 series thì bạn cần phải patch NVRAM.

Đối với B360, B365, H310, H370, Z390 thì bạn có thể sử dụng [SSDT-PMC](https://heavietnam.ga/2022/03/10/ssdt-recomend/) để kích hoạt được NVRAM. Gen 10 thì không cần SSDT này.

## Cleaning out the Clover gunk

Có thể bạn không biết nhưng Clover có thêm các tệp RC vào macOC để mô phỏng nvram điều này sẽ xung đột với phương pháp mô phỏng của OpenCore. Để khác phục các bạn sẽ xóa các file sau:

- `/Volumes/EFI/EFI/CLOVER/drivers64UEFI/EmuVariableUefi-64.efi`

- `/Volumes/EFI/nvram.plist`

- `/etc/rc.clover.lib`

- `/etc/rc.boot.d/10.save_and_rotate_boot_log.local`

- `/etc/rc.boot.d/20.mount_ESP.local`

- `/etc/rc.boot.d/70.disable_sleep_proxy_client.local.disabled`

- `/etc/rc.shutdown.d/80.save_nvram_plist.local​`

- `/etc/rc.boot.d`

- `/etc/rc.shutdown.d​`

Nếu các thư mục trống các bạn vẫn phải xóa chúng.

## Kiểm tra xem NVRAM của bạn có hoạt động hay không

```
// dán từng dòng code sau vào

sudo -s
nvram -c
nvram myvar=test
exit

// reboot và chạy tiếp dòng sau

nvram -p | grep -i myvar

// nếu không có gì trả về thì nvram bạn không hoạt động. Nếu như nó trả về 1 dòng có từ khóa myvar test thì nvram của bạn hoạt động

// lệnh nvram -c yêu cầu tắt sip. Tuy nhiên bạn có thể thay thế dòng này bằng cách reset nvram ở menu boot (bật Misc -> Security -> AllowNvramReset -> YES)
```

## Enabling emulated NVRAM

B1:Chỉnh config theo sau:

- Booter:
  - DisableVariableWrite: NO
- Misc -> Security:
  - ExposeSensitiveData: 0x3
- NVRAM:
  - LegacyEnable: YES
  - LegacyOverwrite: YES
  - LegacySchema: được OpenCore set mặc định
  - WriteFlash: YES

![](https://dortania.github.io/OpenCore-Post-Install/assets/img/nvram.c97ef040.png)

Bây giờ bạn sẽ chạy [LogoutHook.command](https://github.com/acidanthera/OpenCorePkg/releases) (trong /Utilities/LogoutHook/).

```
// chạy đoạn code sau 
sudo defaults write com.apple.loginwindow LogoutHook
```

B2: Bạn sẽ kéo file LogoutHook.command vào terminal và nhấn enter

Lưu ý: Boot-arg `-x` yêu cầu NVRAM để hoạt động. với những macOS 10.12- thì điều này sẽ không khả dụng. Bạn sẽ cần copy file `nvram.mojave` vào cùng folder với `LogoutHook.command`. Nó sẽ gọi nvram.mojave thay vì gọi NVRAM hệ thống.

**Source: [Emulated NVRAM | OpenCore Post-Install (dortania.github.io)](https://dortania.github.io/OpenCore-Post-Install/misc/nvram.html#enabling-emulated-nvram-with-a-nvram-plist)**
