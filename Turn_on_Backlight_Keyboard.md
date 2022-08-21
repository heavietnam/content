# Turn on Backlight Keyboard

Đối với 1 số bàn phím cơ khi Hackintosh sẽ bị mất led keyboard để khắc phục điều này chúng ta sẽ làm như sau:

B1: Download [Releases · damieng/setledsmac (github.com)](https://github.com/damieng/setledsmac/releases).

![](https://i.imgur.com/7woq4J6.png)

B2: Download [Keyboard_Backlit_Files.zip](https://www.mediafire.com/file/iaghltseh3xtu6j/Keyboard_Backlit_Files.zip/file).

B3: Giải nén cả 2 tệp.

B4: Mở Finder nhấn tổ hợp phím `Shift + Command + G` và gõ `/Library`.

B5: Tạo 1 folder có tên `Keyboard Backlit`.

![](https://i.imgur.com/Jc5XOHp.png)

B6: Copy file `setleds` vào `Keyboard Backlit`.

![](https://i.imgur.com/iNsB6iW.png)

B7: Copy K`eyboardBacklit.sh` vào `Keyboard Backlit`.

![](https://i.imgur.com/K14wnjo.png)

B8: Nhấn tổ hợp phím S`hift + Command + G` và gõ đường dẫn `/Library/LaunchAgents/`

![](https://i.imgur.com/6gxZuSQ.png)

B9: Copy file `com.keyboardbacklit.init.plist` vào đây.

![](https://i.imgur.com/upK20yp.png)

B10: Mở Terminal và gõ các lệnh sau:

```
sudo chown root /Library/LaunchAgents/com.keyboardbacklit.init.plist

sudo chmod -R 755 /Library/Keyboard\ Backlit/

sudo launchctl load -w /Library/LaunchAgents/com.keyboardbacklit.init.plist
```

![](https://i.imgur.com/va47hy3.png)

**Source tham khảo: [How To Turn On Keyboard Backlit Through Scroll Lock Led On macOS | tonymacx86.com](https://www.tonymacx86.com/threads/how-to-turn-on-keyboard-backlit-through-scroll-lock-led-on-macos.251506/?fbclid=IwAR2Qr1ELm77UKdxtc8BFSM2F-IlmM1DI5sbN77kX-UQNcwGb_Fd0T4qrs70)**
