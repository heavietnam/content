# Fake CPU-ID

> Phần này dành cho những bạn có CPU Pentium hoặc Celeron hoặc Desktop Gen 11 và các CPU Gen10 muốn sử dụng High Sierra các bạn bắt buộc phải fake CPU-ID.

## Các CPU Pentium/Celeron/Xeon

Add các patch sau vào Kernel ⇒ Emulate (đối với Clover các bạn chỉ lấy mục cpuid1data thôi và add vào mục cpu-id và bấm vào dấu

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-17.11.59.png?w=150)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-17.10.55.png?w=1024)

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-12-at-11.36.19-1.png?w=874)

### Các CPU từ Haswell trở lên (Pentium/Celeron)

```
Cpuid1Data: A9 06 03 00 00 00 00 00 00 00 00 00 00 00 00 00

Cpuid1Mask: FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00
```

### Các CPU dòng HDET (Xeon)

- Haswell-E :

```
Cpuid1Data: C3 06 03 00 00 00 00 00 00 00 00 00 00 00 00 00

Cpuid1Mask: FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00
```

- Broadwell-E :

```
Cpuid1Data: D4 06 03 00 00 00 00 00 00 00 00 00 00 00 00 00

Cpuid1Mask: FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00
```

### Enable XCPM trên các dòng low-end (Pentium và Celeron)

Các bạn add các patch sau vào `Root ⇒ Kernel ⇒ Patch` (đối với Clover các bạn chuyển các mục patch sau theo quy tắt dưới vào mục kextopatch)

```
Comment: sử dụng cho cả OpenCore và Clover.
  
Disabled: tương ứng với mục Enable ben OpenCore.
  
MatchBuild: được thay thế bằng Minkernel và Maxkernel (các bạn dùng trang sau để search version Darwin [Darwin (operating system) - Wikipedia](https://en.wikipedia.org/wiki/Darwin_(operating_system))).
  
MatchOS: được thay thế bằng Minkernel và Maxkernel (các bạn dùng trang sau để search version Darwin [Darwin (operating system) - Wikipedia](https://en.wikipedia.org/wiki/Darwin_(operating_system))).
  
Find: có ở OpenCore và Clover.
  
Replace: có cả ở OpenCore và Clover.
  
MaskFind: OpenCore sử dụng Mask để thay thế.
  
Mask replace: có cả ở OpenCore và Clover.
  
Count, limit, skip thường được đặt thành 0.
  
Identifier thường được đặt thành kernel (trông 1 số trường hợp khi bạn patch kext nào thì sẽ để đường dẫn theo kext đó vd như com.apple.iokit.IOGraphicsFamily).
```

#### Pentium/Celeron (10.11 trở xuống)

```
Base: _xcpm_bootstrap

Comment: _xcpm_bootstrap (Haswell+ low-end Celeron/Pentium) 10.12

Count: 1

Enabled: YES

Find: C4830022

Identifier: kernel

Limit: 0

Mask: FFFF00FF

MatchKernel: 16.

Replace: C6830022

ReplaceMask: FFFF00FF

Skip: 0
```

#### Pentium/Celeron (10.13+)

```
Base: _xcpm_bootstrap

Comment: _xcpm_bootstrap (Haswell+ low-end Celeron/Pentium) 10.13+

Count: 1

Enabled: YES

Find: 00C43C22

Identifier: kernel

Limit: 0

Mask: 00FFFFFF

MatchKernel: 16.

Replace: 00C63C22

ReplaceMask: 00FFFFFF

Skip: 0
```

#### Pentium/Celeron dòng AVX (12+)

```
Base: [EMPTY]

Comment: Haswell+ low-end Celeron/Pentium cpuid_set_info_rdmsr (c) vit9696

Count: 1

Enabled: YES

Find: B9A00100000F32

Identifier: kernel

Limit: 0

Mask: [EMPTY]

MatchKernel: [EMPTY]

Replace: B9A001000031C0

ReplaceMask: [EMPTY]

Skip: 0
```

> Lưu ý: Sau khi các bạn add các patch trên các bạn hãy patch powermanagement

## CPU Gen 11 (Rocket Lake | dòng Core i)

> Hiện tại Gen 11 Desktop đã có thể Hackintosh mà không cần Fake CPU-ID nhưng nếu các bạn gặp 1 số lỗi thì có thể fake CPU-ID theo sau: 

```
Cpuid1Data: EB 06 09 00 00 00 00 00 00 00 00 00 00 00 00 00 

Cpuid1Mask: FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00
```

## CPU Gen 10 (dòng Core i)

> Đối với các dòng Gen 10 ko cần fake cpuid nhưng nếu các bạn muốn cài High Sierra (để sử dụng card rời thì fake theo đây) trong quá trình cài Web Driver các bạn có thể gặp lỗi tự bật CSM trong BIOS dẫn đến bị đen màn khi cài Web Driver.

```
Cpuid1Data: EC 06 08 00 00 00 00 00 00 00 00 00 00 00 00 00

Cpuid1Mask: FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00
```

> dành cho các dòng U

Hoặc 

```
Cpuid1Data: EB 06 08 00 00 00 00 00 00 00 00 00 00 00 00 00

Cpuid1Mask: FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00
```

Hoặc

```
Cpuid1Data to ED060900 00000000 00000000 00000000

Cpuid1Mask to FFFFFFFF 00000000 00000000 00000000
```

> đã thử nghiệm trên Mojave

## Quy tắc Fake CPU-ID

### Cách lấy CPU-ID:

#### Cách 1 Search Google

B1: Các bạn 1 vào 1 trình duyệt bất kỳ search cpuid + tên cpu cần fake (vd cpuid haswell)

B2: Nên vào trang wiki để tìm kết quả (cpuid sẽ có mã là 0306C3)

![](https://lh4.googleusercontent.com/uJP-9DYEuigV6TvVJZfn8BzHaszQIFD-1Z2Khd9BXPXbmCiLNscN4EaiQ_kQ7BkDjhwHuhBOjl_sUlBSlgy_Il6ZfIYjZV67aE5ckh95SCvVZiPn1AsYbBQF8US3xK3auczVswKD=s0)

#### Cách 2 dùng Clover Configurator

B1: Tải Clover Configurator [tại đây](https://mackie100projects.altervista.org/download-clover-configurator/). 

B2: Vào Tab Kernel and Kext Patches. 

![](https://lh6.googleusercontent.com/8pOnhOf5kCqzVd1lTF3voNQVVdltwZcpixUnpElH2iXjhlwK_hMXrzRwLorNzSjMcJP2A1YDROeuLOj0q-KhpEhtKvnK66eLniGxv1drhYNN5A_ZGh25PBqlC04CFSrJQ2evamhg=s0)

B3: Nhấn vào dấu ![](https://lh3.googleusercontent.com/7JTeJC23uPzXkOLr8T-Z_KXj3VQlBVGbMjCJI5hQ22S4O3FlwZxppwtmg0tVzM_dbsF5F-rlnVfwTWbPPIJH7yLqaCpvGu6fo-QyD3QcA7oINtLMFRF0fpM1XZlLJ0R1gSqOi3PF=s0) 

B4: Chọn CPU-ID phù hợp (cao nhất là kabylake như ở đây mình sẽ có là 0x0306C0 của Haswell)

- Cách 3:

B1: Vào trang Tìm hiểu phần cứng để lấy `cpuid`

### Quy tắc đổi CPU-ID

B1: Các bạn loại bỏ chữ số `0x` đi (`0x0306C0 ⇒ 0306C0`).

B2: Tách nó thành các cập rồi đổi thứ tự (`0306C0 ⇒ 03 06 C0 ⇒ C0 06 03`).

B3: Sau đó chuyển nó thành hệ thập lục phân(`C0 06 03 ⇒ C0 06 03 00 00 00 00 00 00 00 00 00 00 00 00 00`).

B4: Ta có (Clover chỉ lấy mục `CPU1` Data).

```
Cpuid1Data: C3 06 03 00 00 00 00 00 00 00 00 00 00 00 00 00

Cpuid1Mask: FF FF FF FF 00 00 00 00 00 00 00 00 00 00 00 00
```

### Cách Fake 1 CPU cụ thể

> Giải thích. 1 chút về phần này. Ở các phần trước thì mình chỉ các bạn cách Fake CPU-ID theo gen tức lấy 1 CPU-IDlàm đại diện để fake ở đây mình sẽ hướng dẫn các bạn cách Fake thành 1 CPU cụ thể VD cụ thể ở đây sẽ là `i9-9980HK`

B1: Các bạn truy cập vào trang cpuid world [tại đây](https://www.cpu-world.com/cgi-bin/CPUID.pl)

B2: Các bạn chọn cpu muốn fake 

> ở đây các bạn có thể dùng tính năng filter để lọc

![](https://imgur.com/h4S7cqY.png)

B3: Sau khi các bạn chọn được cpu muốn fake các bạn tiếp tục ấn tổ hợp phím `Command + F` hoặc `Ctrl + F` và gõ từ khóa `cpuid`

![](https://imgur.com/CeE6Ul1.png)

Các bạn chú ý đến dòng `CPUID signature` đây chính là cpuid các bạn cần tìm nhưng nó vẫn chưa thể dùng được các bạn cần biến đổi nó 1 tí

B4: Các bạn tách cpuid vừa nhận được thành các cập từ phải qua trái:

```
// ví dụ như cpuid của con i9-9980HK sẽ là

906ED ==> 9 06 ED
```

B5: Các bạn thêm số 0 vào trc dãy 6 đảm bảo dãy số có đủ 6 chữ số.

```
// thêm số vào trc dãy số đảm bảo có đủ 6 chữ số

9 06 ED ==> 09 06 ED
```

B6: Các bạn thực hiện quy tắc đổi cpuid như ở phần IV và viết lại như bình thường.

```
// đảo thứ tự các cập

09 06 ED ==> ED 06 09

// chuyển thành hệ thập lục phân

ED 06 09 ==> ED 06 09 00 00 00 00 00 00 00 00 00 00 00 00 00

// viết lại và sử dụng

ED 06 09 00 00 00 00 00 00 00 00 00 00 00 00 00 ==> ED060900000000000000000000000000
```

> Lưu ý : Sau khi làm xong thì cpu sẽ mất khả năng quản lý năng lượng các bạn có thể patch PowerManagement theo mục VI.2 nếu các bạn gặp lỗi khi Boot (chỉ với CPU dong low-end tức là Celeron hay Pentinum thì các bạn có thể sử dụng [**N**](https://drive.google.com/file/d/1tRgTaoZrQ6HB3NE_iOMS1UcxA-yfm4by/view?usp=sharing)[**ullCPUPowerManagement.kext**](https://drive.google.com/file/d/1tRgTaoZrQ6HB3NE_iOMS1UcxA-yfm4by/view?usp=sharing) hoặc bật `config ⇒ Kernel ⇒ Emulate ⇒ DummyPowerManagement`.

> Nếu sau khi đã Patch PowerManagement thì các bạn phải xóa `NullCPUPowerManagement.kext` đi hoặc tắt `DummyPowerManagement`.

**Các nguồn tham khảo** [**OcAppleKernelLib: Add builtin XCPM patches for Ivy Bridge and other unsupported CPUs · Issue #365 · acidanthera/bugtracker (github.com)**](https://github.com/acidanthera/bugtracker/issues/365) 


