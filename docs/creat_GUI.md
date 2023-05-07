# Tạo GUI

## OpenCore

### Chuẩn bị

B1: Các bạn tải Open Canopy (phải đúng với phiên bản OpenCore).

B2: Tải file Resource theo nguồn này [GitHub – acidanthera/OcBinaryData](https://github.com/acidanthera/OcBinaryData) 

### Tiến hành

B1: Các bạn set config theo sau:

- Misc ⇒ Boot ⇒ PickerMode: `external`
- Misc ⇒ Boot ⇒ PickerAttributes : `1` (nếu muốn sử dụng chuột thì có thể đổi value thành `17`)
- Misc -> Boot -> PickerVariant : Chọn tùy theo sở thích 
- Auto — Tự động chọn một bộ biểu tượng dựa trên màu DefaultBackground.
- `Acidanthera\Syrah` — Bộ biểu tượng bình thường
- `Acidanthera\GoldenGate`— Bộ biểu tượng Nouveau
- `Acidanthera\Chardonnay`— Bộ biểu tượng vintage 

Hoặc các bạn có thể set theo hình sau: 

![](https://lh4.googleusercontent.com/8eDFICes8A5dCEjhR7g0v-7Nynz2bRsBa4xhHzwgWsDAtXTrrYCoi3oxggB2trleF-aWqq4NyinhXnAvjb5FikZQgtkztOPBKiamf4buDSkoKyH6IW2jSdrR1LOyBreOZ5TLyos6=s0)

B2: Thêm OpenCanopy vào `EFI ⇒ Drivers`

## Custom GUI

B1: Các bạn có thể tải các theme mà các bạn thích ở đây mình gợi là nên dùng app [GitHub – LAbyOne/OpenCore-Themes-Downloader: Little App to download my OpenCore Themes](https://github.com/LAbyOne/OpenCore-Themes-Downloader) 

B2: Bỏ theme vừa tải vào thư mục Acidanthera lưu ý chỉ có 1 folder sau đó phải tới các icons VD khi các bạn tải theme từ app về theme sẽ nằm trong mục “oc themes” nó sẽ có các mục ⇒ opencore ⇒ icons thì thư mục icons chính là thư mục chứa các tệp ảnh nên khi copy vào resource các bạn chỉ copy thư mục icons. 

B3: Set config như trên nhưng phần Misc -> Boot -> PickerVariant: thì các bạn thay đổi vualt thành tên của thư mục chứa tệp ảnh ( sau khi đổi xong các bạn sẽ được như hình 

![](https://lh6.googleusercontent.com/_B-s4wEICtcFJMHROmQJevqbn-PUSbT7qSKGqX532l388WfKrOy8rw8YceC2YJ1DgIZ94vRGy0sY-SZnELbpuNVSUQmN1d4Ugko1JHrWbYLjC4PIUp0kU4Vs94u5DTlEK3GgBN0n=s0)

> Lưu ý: Cách này chỉ áp dụng đối với phiên bản OpenCore > 0.5.7 nên bạn nào có phiên bản OpenCore < 0.5.7 thì nên Update OpenCore theo bài hướng dẫn ở trên nếu các bạn đã làm nhưng vẫn không nhận GUI thì các bạn nên xem lại phiên bản OpenCanopy có đúng với phiên bản OpenCore đang sử dụng hay không.

## Clover

B1: Tải theme [từ nguồn](https://github.com/CloverHackyColor/CloverThemes) hoặc bất cứ nguồn nào mà bạn thích.

B2: Bỏ theme vào mục Themes.

B3: Mở config vào mục Gui.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-09.31.43.png?w=1024)

B4: Gõ têm Theme vào mục Theme.

B5: Save lại và Restart.

## Thay đổi ảnh nền cho GUI

B1: Chọn 1 bức ảnh mà các bạn thích để làm ảnh nền cho Gui 

B2: Rename ảnh lại thành định dạng .png.

B3: Tải app sau về [Releases · chris1111/Icnspack-Builder (github.com)](https://github.com/chris1111/Icnspack-Builder/releases)

B4: Mở app lên các bạn sẽ được như hình:

![](https://lh6.googleusercontent.com/WhVfA06Ma8-fTqCYzv3raB_oYpOWxQB8AHVRLnwjxFE8gkQax2yD6GpO7VCQJRw6lBqXvDyrYSrlDKVjki89pw19bviN1KGYnjTw_4d-6uF8wY9wk5KRm7_7M6OczWtSM_n-VvpY=s0)

B5: Bấm vào ![](https://lh5.googleusercontent.com/dXIZ6F2jfhyYimw5zddIlJUSx9JzDwkr3mxHVXEp9VLwtt-1kw2PJDNHQD-AYshX1ztF3gYySOQ7q5ZPCHvzMKhdgaEd5Y8Wx_5BYNgBABM7HTqorszuJraa3r1nANNrWQaEx93W=s0)

B6: Kéo file .png vào. 

B7: Bấm save và chọn nơi save.

B8: Sau khi hoàn thành tất cả các bước trên các bạn sẽ được file ![](https://lh3.googleusercontent.com/7uhnV1xO601Efd0FROepj8RdRlQZi1MxzT3CesAtSZ99zpuK2B1Nc1dq5Egah5ssLXmIhpxPT05FGdfZRRhOsSwSsfcy5OvZtGGJ3yTuX0QqdiV61KtIeCUzFpBQcY6N7GWg2S5F=s0) 

B9: Giải nén file ra và các bạn copy vào theme đang ùng hiện tại và rename lại thành “Background.icns” như ảnh.

B10: Reboot và tận hưởng thôi.
