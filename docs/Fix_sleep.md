# Fix sleep

## **Xác định phần cần** sửa

B1: Nhập code sau vào terminal 

```
pmset -g log | grep -e "Sleep.*due to" -e "Wake.*due to"
```

B2: Xem bảng sau để biết cần phần cần fix

| Wake [CDNVA] due to GLAN: Using AC                                   | Vào cài đặt firmware tắt mục Wake on LAN đi                    |
| -------------------------------------------------------------------- | -------------------------------------------------------------- |
| Wake [CDNVA] due to HDEF: Using AC                                   | Vào cài đặt firmware tắt mục Wake on LAN đi                    |
| Wake [CDNVA] due to XHC: Using AC                                    | Vào cài đặt firmware tắt Wake on USB và patch GPRW, xem ở dưới |
| DarkWake from Normal Sleep [CDNPB] : due to RTC/Maintenance Using AC | Lỗi này thường gặp do power snaps                              |
| Wake reason: RTC (Alarm)                                             | Lỗi này thường do 1 ứng dụng, hãy thoát nó trước khi bạn sleep |

## ***Chỉnh mục Power Management trong Hackintool: Mở Hackintool, vào tab Power***![](https://lh5.googleusercontent.com/Ud3dJFNTR_eNGjR3y-Ya-mS4McG8iZX7HTF1wBPl32kUruuvBx3LBf76VkUCcv4auAveok1t9GqJTkVdGo4ryqa-HVZgVOImociGIjgdJwPdCL7C5Q0nU9uuho3v52fbiPEyIZkk=s0)

  + Nhấn vào dấu ![](https://lh6.googleusercontent.com/ssk0miuDyfm8Pdy47a5ngMW9M6i1XOzt2J45qVvd9TweppzXHBw72h1Wc2TW5AhPUiTXvnkqMXP6Xvzw24EAMKVoRrx1_N694RNho2P3UIoybeTz7UYeUbH9oDu4nIDrJvdroId-=s0) để sửa các mục trong Power Management thành `0`, oặc dùng kext [hypernation fixup](https://github.com/acidanthera/HibernationFixup/releases)

## Tiến hành patch

- [Map USB](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/)
- [Fix GPRW/UPRW/LANC](https://heavietnam.ga/2021/09/29/vi-2fix-gprw-uprw-lanc/)
- [Fix power management](https://everythingforhackintosher.wordpress.com/2021/09/10/157/)
- [Fix wake keyboard](https://heavietnam.ga/2021/09/29/vi-5fix-wake-keyboard/)
- [Fix dark wake](https://heavietnam.ga/2021/09/29/vi-4fix-darkwake/)
- [Fix PTS wake TTS](https://heavietnam.ga/2021/09/29/vi-6hotpatch-pts-wake-tts/)
