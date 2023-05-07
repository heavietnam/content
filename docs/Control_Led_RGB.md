# Control Led RGB

B1: Các bạn mở Terminal và dán dòng sau vào:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

B2: Đợi khi Brew được tải xong các bạn nhập lệnh sau vào Terminal:

```
brew install liquidctl
```

Hoặc:

```
brew install liquidctl --HEAD ( khuyến khích )
```

B3: Tới đây là xong rồi bước này mình sẽ giới thiệu 1 vài dòng lệnh để thiết lập cơ bản:

- Liệt kê và chọn thiết bị (liquidctl list).
- Thiết lập cài đặt (liquidctl [options] initialize | thay thế option bằng các thiết bị của bạn).
- Thiết lập thủ công:

```
liquidctl [options] set <channel> color <mode> [<color>]
```

**Lưu ý: Các bạn có thể xem thêm các lệnh [tại đây](https://github.com/liquidctl/liquidctl#installing-on-macos)**.
