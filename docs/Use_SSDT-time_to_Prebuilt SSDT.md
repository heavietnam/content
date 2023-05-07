# Dùng SSDT-Time để Prebuilt SSDT

Phần này mình dành cho các bạn newbie, bạn nào biết rồi thì có thể bỏ qua nha.

B1: Tải [SSDT-Time](https://github.com/corpnewt/SSDTTime).

B2: Mở SSDT-Time lên (nếu các bạn gặp như hình)

![](https://lh5.googleusercontent.com/Q0SSC3nTTMN_8SjQMQmAnn4vrYK-kSQDE3M9HI6v6N9Lu2abv5y3ex--5GczffBD-ZOskBZDMJY9kiNuSeI12qwx8Nz8fbMuqjxS_yyf-OzLkq85d9w6ZE2KJHid8GOmO12Trkw8=s0)

Thì chờ một tí nhé (nếu các bạn chờ hoài vẫn không xong  tải iasl theo nguồn này bỏ vào mục SSDT-Time-master ⇒ Scripts sau đó chạy lại SSDT-Time | sẽ được như hình).

B3: Chọn D và kéo DSDT vào sau đó ấn Enter (nếu chưa có DSDT thì các bạn Dump theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)).

![](https://lh4.googleusercontent.com/wWAXc4iGlDU1P5r7wMG5eDNQOnTMuNmDEZX6jjrumTT5RJAhjUuwMIFTehBLGBY8QLVsMnYhIrVltAfsYe-rH8nEghZGGLixMb2yGP1LmT6vcQx8EmUGljKaJ9LzQ49gUykjcsk2=s0)

B4: Chọn các ssdt cần Prebuilt (VD 1,2,3 | 1 vài SSDT không cần nó sẽ báo no need).

![](https://lh5.googleusercontent.com/0fOw4vX7tICimfRGPZubP0Z70W2X7UC00W1AhKqDlyy2dVHGAdtpA3oIWB-XZ7obVw5-PwQi6uCSYXBAp0SMzHODtAH3FPGB3jZZVLdCjjy-RNEkKVJdKqUdDnDEhExdO-wTjRR3=s0)

B5: Sau đó bỏ nó vào EFI ⇒ OC ⇒ ACPI tiếp các bạn Snapshot.

B6: Reboot và tận hưởng thôi. 

**Lưu ý : Chỉ Build những SSDT nào thật sự cần thiết nếu không rất dễ Panic.**
