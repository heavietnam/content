# Patch audio với VoodooHDA

## Cài đặt

### Từ Catalina (10.15) trở xuống

**B1**: Tải xuống kext từ nguồn [sau](https://sourceforge.net/projects/voodoohda/)

**B2**: Thêm kext vào folder trong EFI sau đó snapshot lại config

**B3**: Restart và tận hưởng 

### Từ Big Sur (11) trở lên

**B1**: Tải xuống file từ nguồn [sau](https://github.com/chris1111/VoodooHDA-OC) 

> đối với clover các bạn tải kext từ nguồn [sau](https://github.com/chris1111/VoodooHDA-2.9.2-Clover)

**B2**: Mở file `.dmg` và bỏ app vào desktop 

> Đối với Clover chạy file `pkg` lên

**B3**: Chạy app và kéo phân vùng ổ cứng vào 

**B4**: Bỏ kext vào `L/E` bằng [kext droplet v2](https://github.com/chris1111/Kext-Droplet) 

**B5**: Xóa kext ở `bootloader` và `restart` 

> Đối với Opencore
> 
> Lưu ý ở bigsur+ các bạn bắt buộc phải disable sip thì mới có thể tiếp tục

## Tinh chỉnh :

**B1**: Tải xuống file `get_dump` từ nguồn sau về [tại đây](https://sourceforge.net/projects/voodoohda/files/getdump.zip/download)

**B2 :** Chạy file getdump lên 

**B3**: Các bạn copy hết "đóng" code vừa dum và copy vào 1 file `.docx ` hoặc `.txt`

**B4**: Bấm Command+f để tìm từ khóa patched pin configration

![](https://lh6.googleusercontent.com/ipXQ3WMTKJ6jTdAaslFAJ6sX9n_E7ifo04oC9MBt78WQJDIdJCu3kvgCDouf-RsJa_Pgumfd9Vk2wWu46a1Vk9eZA3l9o4NyODs768Ymi54oFjXuHgFI9z-GH1OIjgaemtU4Q_3E=s0)

**B5**: Copy mục patch `pin` ra 1 file .`docx` khác ( cho dễ phân biệt còn đối với các bạn "super" thì bỏ qu‌a bước này và bước 6 cũng được )

**B6**: Copy các dòng không chữ `disabled` ra 1 phần riêng

![](https://lh5.googleusercontent.com/x27aOFoID00SFT10Aaqgo3hJu3JLrI23GrFZaqch4GgfQduiZZgNjBSYkIcHYQjtXNuRD2pVxuJ9Rcd2tGgB_wSZ3NJFoV3dLEfXUjnpw6pCPFrr_L0znvdPihj4CfFEQYfm2R_e=s0)

**B8**: Mở file `info.plist` và file [`tmp info.plist`](https://za7o7cw6-my.sharepoint.com/:u:/g/personal/hoanglong_coursedeals_org/ESpaXYe4xtJAuX_poDrJ6QYB3W9NCcw3BhFK07LTH4PrKQ?e=dLgUoZ) 

> file info.plist nằm ở trong voodoohda --> content --> info.plist

**B9**: Tìm phần `nodes to pad` và xóa các `node` trong đấy đi và copy mục 0 của file `tmp info.plist` trong mục  `nodes to pad` qua file `info.plist` 

> mục 0 dùng để `inject` các `codec` thực

**B10**: Các bạn clone mục 0 thành nhiều mục 

> số mục tương ứng với các mục của của codec thực ở đây của mình là 3

![](https://lh4.googleusercontent.com/hW9_o1g7U0-pqvJlQ2rZ6jGriyErLQ1PzKay2LQUv0vBhbevUq7vrkIlEisSRvqqEpNPce23AH2pAh3e2IZBfhqEJEueRZAwwgfZqnTiXA9Y8BCyI0E_ysuB1l0Kn-eBAlxW9fdf=s0)

Sửa dòng node của file `info.plist` theo các `codec` thực  

```
nid 23
nid 26
nid 33
```

> Như ở đây của mình là `23,26,33`

thì sau khi mình sửa nó sẽ là 

![](https://lh4.googleusercontent.com/5TuMCD2HoGhmOOMkNVzTEPfNLQjI3yNrbnTRAF2dKwOrt-mUj7nB7BLq-XrW0rT3txpnnVeSDhNupR35dvqJdG8CxeY8IMclbB_T7NubSlTVAQIC_xktqRbH76QsqZQGPJL56z2k=s0)

**B11**: Copy các mục config sau như hình vào mục `nodes to pad` trong dòng `config`  của file `info.plist`

```c
#như ở đây của mình sẽ là 

0x90170110
0x02a11030
0x02211020
```

Và đây là của mình sau khi làm xong

![](https://lh4.googleusercontent.com/0ayQJyft58Gp4SLJ3JftktMomdLqXJbc1vDDaE_QTaZ-iJOqbiJbHGvBqBX1RW7iJwO4-pMV6CYALXqhMyqXcuxhLnd_jk5Nf3X2d4WaaRm-NbeevSwU5BFY3BiyfRQEllUVGcaP=s0)

> Nhưng khi bạn cắm headphone vào nó sẽ hiện cả 2 tùy chọn là `speaker` và `headphones` nếu các bạn muốn chỉ hiện 1 tùy chọn thì sửa dữ liệu của phần `config` ở mục `jack` lại như sau 2 chữ số cuối cùng của phần `config` ở mục jack các bạn xóa đi và thêm là `1f`  của mình sau khi làm xong nó sẽ là 

![](https://lh4.googleusercontent.com/sB4fGVrxH5Low8OVUJIGZURCPqyqD0pmtluTnRoD4zOF3pviSOaerhYS4oN3bFcq1AoxgSVrW8yePfIPA0TU5WXWIIkB3MRSzgbWkXfz5ggyhR6Pen9qIO6ykPO52aLv8EbBL-j5=s0)

   **B12**: Ở phần codec các bạn để là 0 như mặc định số 0 này là của phần  hda codec của các bạn như của mình là

![](https://lh4.googleusercontent.com/GCKyOTQ4bfQdx5vYdnIsByZ5QgooobtI-90vqUCc3OCjVQpymw8IJ8h5nbmI52Z8zQhb1OO0FTADJfYFC42zLSyylDZh7qHdt6tb62SNEvmmpfi1-JdICRKzY9yG5JgF_iPK6Jy7=s0)

> Bây giờ sẽ chuyển tới mục `disable` các codec ảo

**B13**: Bây giờ các bạn copy mục 1 ở file `tmp info.plist` phần `nodes to pad` sang file `info.plist` và `clone` ra tương ứng bằng số `codec` ảo của bạn 

**B14**: tiếp đến các bạn làm như trên copy phần `nod` của các codec ảo sang phần `nodes to pad --> node` của file `info.plist`  như mình sau khi copy xong sẽ là 

![](https://lh5.googleusercontent.com/Tg6ywaJzcJlWkzWizaIJsHB2SdwCOpX2bEy8NKdWVRwLR213xmYg00sIU6f_YpchZb_0i3nfdM6qZb6iw47qJ_2yFZEA3ctjo_aaSMMil8tpSxD09YcnB899OkwIu3FDkv_RkfX0=s0)

**B15**: Tiếp theo copy toàn bộ mục `mixer` của file `tmp info.plist` sang file `info.plist` 

> các thứ có sẵn trong mục mixer các bạn xóa hết đi 
> 
> của mình sau khi làm xong sẽ là

![](https://lh3.googleusercontent.com/f_z9fI83bqmZuef7hMrqWIb45cWDHPjJSXCk6lO3j8mjC3w0sotcfKesnX54Tmyz01FMRRyUNFG6O12F7-rpNKNgTbSlML0RuceyI5iTIRQuaX0Sl49qRaHLLx9oS_Z-Qj25q904=s0)

> Sau khi làm xong bước này là gần như âm thanh của bạn sẽ không còn rè nữa nhưng để loại bỏ triệt để chúng ta hãy đến phần kế tiếp 

 **B14**: Các bạn chỉnh mục `noise` trong file `info.plist` về `5` 

![](https://lh5.googleusercontent.com/Ta6iSDkDgBxsPWCKgY7GICBqpLEUA3ubvgZdOJNA2Ks7ITuhF1EspPvjmAZbMYNOlLI0nq2Fxkcsi43-t9ppsXHdWaQJXYasyLwTt8EHmaOQVvl7gpvk219rk3lSBO6Kh1mby1Ld=s0)

> Như thế là đã loại bỏ triệt để rè nhưng nếu các bạn hơi khó tính thì nên bỏ thêm kext codec commander theo nguồn [sau](https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads/)  vào mục `kext` và `snaps` lại 

**B15**: Tiếp theo chỉnh các mục fix theo file `tmp info.plist` đây là của mình sau khi chỉnh xong 

![](https://lh6.googleusercontent.com/1zlbAQXkBVfI5qQK5UZEckbo-_0lSfA6G2q6PgnTcKP-MNbvgpANuZTEKcvexWJ7d4WIcoEgkzUjRy6zYOvaRRt4qqHJ3DtNIj-Y0DRRvsUG3a8wlBy9aJGBLggdvVAaRaOg-XCF=s0)

> Đến đây là gần như xong xuôi 

**B16**: Save lại và copy file `VoodooHDA` vừa chỉnh vào mục `kext` và xóa file cũ đi sau đó `snapshot` lại `config` và restart tận hưởng thành quả thôi

> Nếu bạn nào thấy hơi rối thì đây là video cho các bạn [Hướng dẫn chi tiết cài đặt âm thanh cho Hackintosh với VoodooHDA](https://youtu.be/TMjlI79f4KU) 

> Đây là đoạn âm thanh sau khi đã tinh chỉnh voodoohda [Audio recording 2018-03-02 00-05-57.wav](https://drive.google.com/file/d/1zxraP_Aq65pbEp6AZxLfez5j37TtsS_W/view) 
