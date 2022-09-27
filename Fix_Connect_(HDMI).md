# Fix Connect (HDMI)

## ***Patch Busid***

### ***Cách 1: Patch thủ công***

Dành cho các PC không thể vào đc macOS 

> bị đen màn không cổng nào hoạt động.

#### Lấy Busid

B1: Các bạn truy cập vào bài [Patch iGPU](https://heavietnam.ga/2021/09/29/xii-patch-igpu/) để lấy code.

B2: Chọn Spoiler để lấy Busid. 

B3: Chọn phần có ID trùng với appl-id của bạn như id của mình là 0x01660003.

B4: Chú ý như sau:

![](https://lh4.googleusercontent.com/HW8iwjE1sZ80wXdwuDV43GnRrr5dly_BcRgtmQr8x9qUFOqhtLxsOzFMYFL6leMokdyhG2GpRcEW2b36mPxTskWCozYsq0ynOrp7CPLY0gRWHqsGhSGXyOdVypmlcCkTZ2WnOpAY=s0)

B5: Ta có các dòng ở vị trí t1 sẽ tương ứng con0 và tiếp tục như vậy 

#### Tiến hành

B1: Chỉnh về loại type mà các bạn muốn ta có như sau: 

`05        03 0000 02000000 30000000`

`index    | busid |    type     |     flag     |`

Ở đây ta sẽ chú ý đến busid, type.

B2: Chuyển type code của dòng code mà bạn lấy: 

` 05030000 02000000 30000000 ⇒ 05030000 **00080000** 30000000`

> ở đây mình chuyển về port hdmi

![](https://lh3.googleusercontent.com/7cfbHLsGXNp7fOG0Gb4_NTzKdO8J-YV9kzuxKRO7p_5i89G8KP4UODVVXtQyCQrynQ0_RyoPGl4wSyjKhuBLWU_r7AUZbePQVLPeByuLwDsfTTZFtSo_C_IYa2lYXjsZF7P9V2zy=s0)

B3: Thử các busid từ `01 ⇒ 06`

Nhưng hãy nhớ rằng 1 số busid sẽ không khả dụng với các loại connector type nhất định xem bản sau

- HDMI
  - 0x1
  - 0x2
  - 0x4
  - 0x5
  - 0x6
- DP/Vga
  - 0x2
  - 0x4
  - 0x5
  - 0x6
- DIVI
  - 0x1
  - 0x2
  - 0x4
  - 0x6

> Chú ý rằng không được phép sử dụng 2 busid giống nhau trên 2 con khác nhau trong cùng 1 thời điểm. Tức là các bạn có thể thử busid ở cả 3 con cùng lúc mình có để mẹo ở lưu ý 5

` 05030000 00080000 30000000 ⇒  05xx0000 00080000 30000000`

B4: Disable 2 port còn lại đi bằng cách thay busid của các port muốn disable thành 00 như ở đây của mình là: 

```
02050000 00040000 07040000 ⇒ 02**00**0000 00040000 07040000

03040000 00040000 81000000 ⇒ 03**00**0000 00040000 81000000

04060000 00040000 81000000 ⇒ 04**00**0000 00040000 81000000
```

Sau khi đổi ta được 

```
framebuffer-patch-enable | Data | 01000000

framebuffer-con0-enable  | Data | 01000000

framebuffer-con1-enable  | Data | 01000000

framebuffer-con2-enable  | Data | 01000000

framebuffer-con3-enable  | Data | 01000000

framebuffer-con0-alldata | Data | 05**01**0000 00080000 30000000

framebuffer-con1-alldata | Data | 02**00**0000 00080000 07040000

framebuffer-con2-alldata | Data | 03**00**0000 00080000 81000000

framebuffer-con3-alldata | Data |  04**00**0000 00080000 81000000
```

B5: Add các giá trị vào mục device properties ở dưới mục patch igpu 

> tức là `device properties ==> PciRoot(0x0)/Pci(0x2,0x0)` đối với oc còn với clover các bạn add vào mục `device ==> properties ==> PciRoot(0x0)/Pci(0x2,0x0)`

**Lưu ý: Nếu vẫn chưa thành công sau khi đổi các busid cho port 1 thì các bạn sẽ disable port 1 đi và enable port 2 sau đó đổi busid của port 2 thử từng busid 01 ⇒ 06 và cứ tiếp tục như vậy** 

```
05010000 02000000 30000000

02020000 00040000 07040000

03030000 00040000 81000000

04040000 00040000 81000000
```

***Lưu ý 2: Nếu cổng HDMI của bạn dính với Card rời thì hãy Injects NVCAP ( chỉ với những dGPU được support thôi nếu gặp lỗi thì mới patch và chỉ với Card Nvidia )***

> Vậy làm sao biết hdmi của bạn có nối với dgpu không, chúng ta sẽ tiến hành vào device properties --> monitor --> properties --> location

![](https://i.imgur.com/Lmglskp.png)

**Lưu ý 3: Nếu trong quá trình thử mà bạn đều bị treo táo thì hãy thử add code sau vào boot-arg “igfxonln=1” hoặc add đoạn patch sau:**

**force-online|data|01000000 vào dưới dòng patch HDMI của bạn** 

**Lưu ý 4: Đối với các bạn khi xuất màn bằng HDMI mà khi Full tab ra bị tắt màn hình thì các bạn vào setting chỉnh như hình**:

![](https://lh3.googleusercontent.com/6qEpENycEY_Fth17xf-S_ScLVnY8jledfr9D_dpx3A4G8CzIBauFqSx1YHOaKqMDQjGXYgYsgTwPo1_8CmJmXxhrlFxYT2gQe6iMohn9Dxg8vTPrri703KQ5MeABBOsf2I3yjK3Z=s0)

**Lưu ý 5: Ta có 1 mẹo nhỏ như sau ở đây mình sẽ ko sử dụng method all data nữa như đã biết ở trên ta có busid sẽ nhận từ `1-6` mà trung bình frambuffer thường sẽ có `3 connector` nên nếu là theo cách thông thường ta phải thử 18 lần nhưng ở đây ta có 1 thủ thuật là trong 1 lần ta sẽ thử cả 3 con ta có là `123,234,345,456,562,611` Và các bạn cũng set type theo bảng trên nhé (`hdmi=00080000`)**.

**Ta có như sau**:

![](https://imgur.com/lpXdYr4.png)

### ***Cách 2: Patch với Hackintool***

Dành cho Laptop hoặc 1 số PC có thể kết nối với 1 cổng xuất màn hình. 

B1: Tải [hackintool](https://github.com/headkaze/Hackintool/releases).

B2: Các bạn chọn như hình:

![](https://lh5.googleusercontent.com/oEKxqdbxYuz1AIg4N_vfXlidsO8lemdqn8CyAh2Y8QHxeYQfHIzno3bOdDusWNSdEfSDRH0au4g0Gojqyv6QLjY2yGRTM66FWiLaqP_nLyMMkTOVuANi-mEureBN7BFgSh8WtMQ_=s0) ⇒ ![](https://lh5.googleusercontent.com/R8mMQlKN3jbNgAuCwj9AbkqWFxCc7axxCYxTnAk3nAUH2y5EUDw4uK02zsg1UpcJ9sL4qQ-qe_EgHDQCQRTNJVVnbraVHh4OlVX7WJO79SseYa3PSwNDWarTjk_Ih_0RKyWaHMwP=s0) 

B3: Chỉnh platformid và Intel Gen (mình mặc định là các bạn đã patch iGPU trước đó | nếu bạn nào chưa patch thì xem lại mục XIII).

![](https://lh3.googleusercontent.com/8D5y1nX_WvX3-7tP2xI-JJycxondFaK-8tRmng_29ppVNnZ9G_Z8L--hnyDUF-STx_qfQIPMGuy_3X2qX45TJAynQnwND-sK4fqIg60GR8RfN9NU-IHwOAT2nDcC2LyDhNWH-1W-=s0)

B4: Chuyển qua mục connect 

B5: Cắm các dây xuất màn hoạt động vào (ở đây mình chỉ có 1 cổng internal là hoạt động | thử từng dây nếu máy có 4 port xuất màn trong đó có 1 port HDMI thì các bạn cắm 3 port còn lại nếu nhận thì port ko tick đỏ chính là port HDMI)

![](https://lh3.googleusercontent.com/z6J58xFf2OpK2OEptol8M3ST3YLVHE2TY9nLynYF4tZEb5fmF5QUXxbHxyIRDPRHJw9fUHjFQIvU0GpXIYd2aJAEORBnM3f-buiBU3whXx7XAHjuppHiYA7HG8FJdfhckjhKV_vf=s0)

B6: Nhấn chuột phải vào mục busid của các port ko chuyển màu và thử từ 0x01 ⇒ 0x06 sau đó chỉnh type lại (đổi từng port 1 không đổi cùng lúc nhiều port).

![](https://lh6.googleusercontent.com/vQ5z3kDtC3fVV1LagrR7UlOJvFh-iduFu_QHK4TwQ51L7Gv5BfzN7ZniNcR5LUfCVT4O2mWetfP-BBjdfkzQ0vFIqd7zZsj2kqLSFiLjLzjHYjVhUJm71TwJRl3UB-cURNGiqMFb=s0)

B7: Chuyển tới tab Patch ⇒ General ⇒ Patch ⇒ Advance và chỉnh như hình. ![](https://lh3.googleusercontent.com/ngXjTRcyWifdejgBt0-enjCtd1ZyMwSlmkAXsL2bP5KnIY8QVo5Nltaeyifz3tuNctzg1eTgbkppyFl5Tf4nrvlObXlU9tjHiQvP4K3XM_kbj0fHON79E4oO0j5MhCXKHnxHdZna=s0)

![](https://lh6.googleusercontent.com/mOA7PtB8VYJV4nFHgGhjeb8jRmCZvpjYkx05uvqSZBs4VQ4gTTW1XXLMac6bfLnT0q1piDsrfKTppEseysjESoJPQSMYshk7dPm32jirUvHolUhzF_yjxYocGrfdnjWf1jo3DyUg=s0)

> mục spoof device id các bạn có thể xem ở phần patch iGPU

B8: Chọn mục Generate Patch. 

![](https://lh4.googleusercontent.com/x8r64qnxhQirdMNV7D3QWiaMol2tV9R9-N1ABUoFdCmsmWMc5RITdME-yDQbgUIlM-NOes-X6wcbS_bSgLfAKdZjLCTRvdNVLs-JoCye6X2PGMZHTs4c2YNpv6MXwoOCUxv9aJIQ=s0)

B9: Chọn file ⇒ export ⇒ bootloader config.plist

![](https://lh6.googleusercontent.com/1i14my5VMsxi9eQESo1D3VeinbcitRHPq5M9t9WaxIzHGj81TfCud_NrZpQZyoRvgH-kRXobUX8y6II-lXq1lZk4mrK1nnsYCvqJURRu591t34Gnvi6TPIaW3QxJD421mOWONUoh=s0)⇒ ![](https://lh5.googleusercontent.com/IxQEK4ag6cSuk6NhXN4Y4uliFLWXuFHVXHoPOOgg4oUAAp6f0F-folzRDEitDIwddLsSAFR6Vxlcxkd-rt8BwjycT9k1VpoIG8Sn2m1GIObBsS6cb72SRm4R0peHlBAb3Ord4iOO=s0)

B10: Mở cả 2 file là config.plist và file vừa tạo copy các mục trong Device Properties của file vừa tạo qua config sau đó Restart máy 

> đối với Clover thì copy như hình

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-11.22.51.png?w=1024)

**Lưu ý main source:** [**[HƯỚNG DẪN] Hướng dẫn vá lỗi bộ đệm khung chung (Sự cố màn hình đen HDMI) | tonymacx86.com**](https://www.tonymacx86.com/threads/guide-general-framebuffer-patching-guide-hdmi-black-screen-problem.269149/)

**Lưu ý 2: Khi patch bằng hackintool (nếu các bạn chỉ muốn patch hdmi thôi) thì nên disable các port chx patch và thử giống như  patch busid thủ công (như hình)** 

![](https://lh3.googleusercontent.com/iJTUumC_N7sfPSuKeAFeRcHrVVWuSqaFXjhUvX8T1VQ968vsxz-VT9KctQEc_8_ydu7rEFBWHBUOa5P69tgzsagNbQ0CYFTvTgKw3faN2vFxBgr1tzlB7JqqvdAZBzn_cSaUNv0z=s0)

**Lưu ý 3: Đối với 1 số dòng main của pc thì đã có các patch sẵn các bạn chỉ cần add vào thôi** 

- **Vào tab patch ⇒ system configs trên menu bar**
- **Khi này các setting sẽ được tự động detec công việc còn lại chỉ là generate patch thôi (làm như phần trên)**

### Enable lspcon driver support

Gần đây các nhà sản xuất đã trang bị cổng hdmi 2.0 cho các thiết bị KBL hoặc CFL+ nhưng chip intel UHD không hỗ trợ cổng chuyển hdmi 2.0 do đó các nhà sản xuất đã thêm một phần cứng được đặt tên là LSPCON ở trên mainboard để chuyển DP thành HDMI 2.0

Tiếp đó ta cần biết LSPCON có 2 chế độ chuyển đổi 1 là LS (tức dp to hdmi 1.4) và 2 là PCON tức (dp to hdmi 2.0) theo mặc định firmware thì LSPCON sẽ luôn chạy ở chế độ LS do đó nó sẽ dẫn đến lỗi màn hình đen khi các bạn xuất màn qua hdmi 2.0

Vậy những thiết bị nào cần patch LSPCON. Như đã nói ở trên các thiết bị có hdmi 2.0 chạy trên uhd thì sẽ cần patch hdmi 2.0 thường sẽ là các dòng SKL, KBL, CFL+

Vậy làm sau để khắc phục tình trạng đen màn

- B1: Add `enable-lspcon-support|Data|01000000` to igpu properties hoặc add arg -igfxlspcon
- B2: add `framebuffer-conX-has-lspcon|Data|01000000` thay thế X với con hdmi của các bạn (thử từng con 1 hoặc thử cùng lúc 3 con)
- Một tuỳ chọn thêm dành cho các bạn `framebuffer-conX-preferred-lspcon-mode` để có thể tuỳ chỉnh đàu ra hdmi của các bạn với type là `DATA` và các giá trị `01000000` (PCON, DP to HDMI 2.0) và `00000000` (LS, DP to HDMI 1.4). Mọi giá trị khác đều được coi là chế độ PCON và khi không add properties này thì chế độ PCON sẽ là chế độ được ưu tiên

## Patch NVCAP

Dành cho các dgpu hỗ trợ nhưng không xuất đc màn ngoài do injects thông tin chưa đúng

### Chuẩn bị

B1: Tải [gfxutil](https://github.com/acidanthera/gfxutil/releases) 

Sau đó vào terminal gõ lệnh path/to/gfxutil -f display sau khi gõ lệnh sẽ được như hình 

![](https://lh6.googleusercontent.com/uolq_0Y6aYU7cwXKWnJMtjf2yrS_HEXMLmEfIuiOFmtju4TQoWs9unrqBicydyhknj-Ftikuf7meq8VenVfzL2xNPq8ZaWI5hITpetbLO8sMfUn-nEZCYxt4GamuhOVpiZD_EiBo=s0)

Các bạn chú ý đến dòng PciRoot(0x2)/Pci(0x0,0x0)/Pci(0x0,0x0).

Và add vào mục 

c`onfig.plist ⇒ root ⇒ device properties ⇒ add ⇒PciRoot(0x2)/Pci(0x0,0x0)/Pci(0x0,0x0) | dictionary `

B3: Tải file vbios phù hợp với cấu hình theo link sau [VGA Bios Collection | TechPowerUp](https://www.techpowerup.com/vgabios/).

B4: Tải [Node JS’s site](https://nodejs.org/en/).

### Xác định cấu trúc cần Patch

Các bạn xác định theo bản sau cần chú ý đến các mục như sau: 

![](https://lh5.googleusercontent.com/jY2l3xbQ8ygBZlx-y-o5ce_gI7IvxgGSYAnCBzFTOW5FHMtEUncOQNpiRJy_bkcrE5ffQo4Qw7WfWAcpUnkRyOnbl_59coLPYU2gTJpNUYn-xcoxNztsDGpcu4IXSfLHGeQHYSur=s0)

### Tiến hành

B1: Xác định model phần này bạn sẽ chọn vualt là tên Card rời. 

B2: Xác định VRAM: 

- Chọn số vram bạn muốn set như ở đây mình muốn set là 1024 thì sẽ nhập như sau vào terminal 
- echo "số ram muốn set"  `1024 * 1024 * 1024' | bc` (`echo 1024 * 1024 * 1024'| bc `)

![](https://lh3.googleusercontent.com/oRnvz0P63NvEUQxFngLxUhAeXoI9bRw-uHmz4pgvlU1atBV2dp352kdo66cY4T3EpwkuvZcgyDOA0x0d0MxhJdSn1Xevf8_5Ax5hyxEOsLDSMg-ir8e69Q3nVbYTLBD21bmbzYVo=s0)

- `echo ‘obase=16; ibase=10; giá trị vừa có ở mục trên’ | bc `

vd: ` echo ‘obase=16; ibase=10; 1073741824’ | bc `![](https://lh6.googleusercontent.com/XeCXFaVgs2UYyaVj0SV4JkPi7jPSGsOfWH3SrYA7bOT1hnl7u4-1vZGSim85SZMIKZiaO2pUb4zsyjSDjdVXkGgiGeBrj62XnXgxpTnbVbPVV9HF0ZiHptZp9CRjTLdQzZTZqdUV=s0)

- Các bạn tách thành các cặp như sau `40000000 ⇒ 40 00 00 00`
- Đảo ngược thứ tự các cập `40 00 00 00 ⇒ 00 00 00 40 `
- Thế 8 số 00 vào cuối dãy dữ liệu 

`00 00 00 40 ⇒ 00 00 00 40 00 00 00 00`

- Cuối cùng kết nối các dữ liệu lại ta được `0000004000000000`

B3: Patch rom-revision.

Ở phần này rom-revision có thể là bất cứ giá trị nào nhưng bắt buộc phải có vd mình sẽ lấy giá trị là king-dragon man thì nó sẽ có dạng: 

`Rom-revision | string | king-dragon man `

B4: Xác định NVCAP. 

- Các bạn nhập các dòng sau vào Terminal:

```
git clone https://github.com/1Revenger1/NVCAP-Calculator

cd NVCAP-Calculator

npm install

npm run run
```

> sau khi chạy lên sẽ được như hình

![](https://lh3.googleusercontent.com/r6evqRixOO9zUGZ8tjIL-V_Win0bd_nktgEdToNSZmJ95lbK9xZE38H6o5mxa9VtEhz41CuyH0e9zNBjbIK6cXXXcLYbFoHhLKXaF8KUT2ZBoaPB-o2o0LbVRbiNrxGLQsJU85wY=s0)

- Nhấn 1 và kéo VBIOS vào sau đó Enter
- Nhấn 3 sẽ được như hình: 

![](https://lh3.googleusercontent.com/o4afN88_OlTHaL-tuO0O8QOcSspPoFk3wSVCfc0IiSi1chqUWPORF8ak8YBweFA1cIYYsdu_ocLv9Y5XKZoOORmdwmu98VwLMfniE8qph3desR3ONwtpSMCKNcN2MpaOt4Y3weEa=s0)

- Gắn display với heads bằng cách nhập số display trước heads sau (ví dụ 1 1 sẽ được như hình).

![](https://lh5.googleusercontent.com/n53-MkskCxE6o5nfBnwi2boP1DUKmjwQgceWKpxqTATVtfZBCQAYiIPxKdCNSyLhOTUkX35RFLmfX3rXS_jora7GairjQEO_yO1kx1QmHsxefP6e35PcxNSZaZ5rAvOTqPwCXjBm=s0)

> Các bạn nhấn lại 1 lần nữa để huỷ bỏ liên kết vd 1 1 sẽ được như hình.

![](https://lh3.googleusercontent.com/o4afN88_OlTHaL-tuO0O8QOcSspPoFk3wSVCfc0IiSi1chqUWPORF8ak8YBweFA1cIYYsdu_ocLv9Y5XKZoOORmdwmu98VwLMfniE8qph3desR3ONwtpSMCKNcN2MpaOt4Y3weEa=s0)

- Tiếp các bạn đặt các giá trị NVCAP theo bảng sau: 

![](https://lh4.googleusercontent.com/gYPv3aIHfTvDVjfXrR3z2oTeUd9vW4Tsk1FX2A1ExDuoX-KR0QV4zxzKJ4nxgdMuqSCnXnF1suVXWzOlGFyAafuBXBFsszMYGrASMmC4TCm0QnGX4sgFYNwgMcQR-C_wpnREYxHo=s0)

- Sau khi hoàn thành các bạn nhấn phím “C” sẽ được như hình

![](https://lh4.googleusercontent.com/O6pGEPXYcpnPOXZpmatAN9aOnOwlbXxIbcWA4DlCFV01hYkgzr0paCB7ZkhT04U2Xqhh2xqZM8dU2qwz61PFOqZlsqmd72yg8V787hB-UmNfj5Ch8rHF8W_UjaepVsxYuil1wuEP=s0)

- Tại đây ta sẽ được giá trị NVCAP
- như sau NVCAP: `05000000 00000300 0c000000 0000000f 00000000`

**Lưu ý: Mỗi heads chỉ có thể xuất ra 1 màn hình 1 lúc nên  bạn phải cẩn thận khi chọn heads VD bạn có 2 cổng HDMI muốn xuất ra 2 màn hình thì bạn phải để 2 display kết nối với 2 hex khác nhau.**

B4: Sau khi làm xong các bước trên các bạn sẽ được như hình: 

![](https://lh4.googleusercontent.com/asDfe549pD-DfkqlWuRygYcE8qL-kPOKNhcixk22_zYc2gaxZFPU9IVV3dYpK7xREwMBPhM9jgMZbz3RSrqkz0GbtUkJJm8TSEHZAA02o7wC2MgoaXhtN0DogE4IxjmCK6nsbP2u=s0)

B5: Add các giá trị sau vào mục Config.plist ⇒ Root ⇒ DeviceProperties ⇒ Add ⇒ dòng vừa add ở bước chuẩn bị sau khi add xong sẽ được như hình: 

![](https://lh6.googleusercontent.com/-2YX_JkvArxiF5nJG1CfXlWPFg5ridVTg9vEvJD_WIOeyEREfEEaqvHgRUycgLMt4Ra9CUiPp8i9rMutYNsFE2688O6C-tLpNcRCzL9j-9tM9kbJu-if3Ii08Xu96bWAv-Y9H7EL=s0)

**Lưu ý: Main source** [**Legacy Nvidia Patching | OpenCore Post-Install (dortania.github.io)**](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/nvidia-patching/)

**Lưu ý 2: Nếu các bạn dùng Cáp chuyển thì set type theo đầu cắm vào Main của máy VD HDMI to DP set type là** `HDMI`.

## Clover

Đối với Clover các bạn chỉ cần bật mục `injects nvidia` lên là được.

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-11.24.50.png?w=1024)

Tuy nhiên vẫn có 1 số trường hợp khi bật lên rồi nhưng vẫn gặp lỗi thì các bạn tính toán NVCAP như OpenCore sau đó bỏ vào mục NVCAP như hình:

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-11.26.10.png?w=1024)
