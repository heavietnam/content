# OpenCore Legacy

## Tạo USB Boot

Đối với các máy Legacy thì chắc hẳn phần cứng đã cũ rồi đúng không nào. Do đó các bác cũng sẽ cần các phiên bản macOS phù hợp thì hãy làm theo sau:

- Offline (10.10-10.12)
  - B1: Các bạn truy cập vào trang [sau](https://support.apple.com/en-us/HT211683) để tải bộ cài.
  - B2: Các bạn chạy file PKG lên và cài đặt. Nếu gặp lỗi sau: ![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/unsupported.679e01e6.png)

Các bạn mount file dmg ra và mở terminal lên sau đó gõ các lệnh sau:

```
cd ~/Desktop
mkdir MacInstall && cd MacInstall
```

Tiếp theo bạn sẽ tiến hành giải nén file installer (mở terminal lên và gõ)

```
// 10.11 và cũ hơn

xar -xf /Volumes/Install\ OS\ X/InstallMacOSX.pkg

// 10.12

xar -xf /Volumes/Install\ macOS/InstallOS.pkg
```

Tiếp theo hãy chạy các lệnh sau (chạy từng lệnh một)

```
//Yosemite
cd InstallMacOSX.pkg
tar xvzf Payload
mv InstallESD.dmg Install\ OS\ X\ Yosemite.app/Contents/SharedSupport/
mv Install\ OS\ X\ Yosemite.app /Applications

//El Capitan
cd InstallMacOSX.pkg
tar xvzf Payload
mv InstallESD.dmg Install\ OS\ X\ El\ Capitan.app/Contents/SharedSupport/
mv Install\ OS\ X\ El\ Capitan.app /Applications

//Sierra
cd InstallOS.pkg
tar xvzf Payload
mv InstallESD.dmg Install\ macOS\ Sierra.app/Contents/SharedSupport/
mv Install\ macOS\ Sierra.app /Applications
```

- B3: Các bạn chạy lệnh cài đặt theo trang [sau](https://heavietnam.ga/2022/2021/09/29/xxxi-cac-tao-bo-cai-offline-khi-khong-co-usb/index.html)

## Setting Up

### macOS

- Chuẩn bị:
  - BootInstall_IA32.tool or BootInstall_X64.tool trong thư mục  `/Utilties/LegacyBoot/`(File này ở trong  thư mục [opencore pkg](https://github.com/acidanthera/OpenCorePkg/releases))
  - Install USB (tạo theo link [sau](https://heavietnam.ga/2022/2020/12/23/cach-tao-bo-cai-online/index.html) hoặc theo link [sau](https://heavietnam.ga/2022/2020/12/23/cach-tao-bo-cai-offline/index.html))
  - Sau đó mở terminal lên và nhập lệnh sau:

`sudo ~/Downloads/OpenCore/Utilities/legacyBoot/BootInstall_X64.tool` Nếu là 32 bit thì thay `X64` thành `IA32` Sau đó các bạn sẽ nhận được 1 phân vùng EFI có 1 folder EFI và 1 file `bootx64.efi` hoặc `bootia32.efi`

### Windows

- Chuẩn bị:
  - [7-Zip](https://www.7-zip.org/)
  - [BOOTICE](https://www.majorgeeks.com/files/details/bootice_64_bit.html)
  - [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg/releases)
- Tiến hành:
  - B1: Các bạn mở Bootice lên và chọn đúng ổ đĩa:

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/bootice.f83b0859.png)

- B2: Ấn Process MBR và chọn Restore MBR tiếp theo bạn chọn file boot0 trong Utilities/LegacyBoot/ ở OpenCore PKG.

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/restore-mbr.8e5164a9.png)

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/restore-mbr-file.a0daa24a.png)

- B3: Các bạn trở về menu chính và ấn Process PBR sau đó ấn Restore PBR và chọn file boot1f32 từ Utilities/LegacyBoot/ ở OpenCorePkg

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/restore-pbr.2635de6c.png)

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/restore-pbr-file.cbf5dcf4.png)

- B4: Mở USB ra rồi copy file bootx64 và bootia32  từ Utilities/LegacyBoot/ và đổi tên chúng lại thành boot

![](https://dortania.github.io/OpenCore-Install-Guide/assets/img/final-boot-file.a45bcbb9.png)

**Source tham khảo: [Making the installer in Windows | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#making-the-installer)**
