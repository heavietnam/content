# Cách tạo bộ cài online (recovery)

> Chỉ áp dụng với OpenCore

## Formart ổ USB

### Tối thiểu cần 4GB

B1: Nhấn chuột phải tại nút Start trên Windows

B2: Chọn Disk Management

B3: Nhấp chuột phải chọn `Forma`t, sử dụng định dạng `FAT32`

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-14-at-12.25.19-1.png?w=533)

### Ổ USB 16GB trở lên:

B1: Tải Rufus [tại đây](https://rufus.ie/en/)

B2: Chọn mục `Boot selection` là `Non bootable`

B3: Chọn type là `fat32`

B4: Click `Start`

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-14-at-12.37.44-2.png?w=471)

### Dành cho các máy Legacy:

B1: Nhấn tổ hợp phím `Windows + R`, gõ `diskpart` và nhấn enter

B2: Gõ `list disk`

B3: Nhìn vào tên ổ USB, xác định xem ở disk mấy sau đó gõ select disk # ( thay # thành số thứ tự của ổ USB. Ví dụ: `select disk 1` )

B4: Gõ `clean`

B5: Gõ `convert gpt`

B6: Gõ create `partition primary`

B7: Gõ `list partition`

B7: Gõ `select partition #` ( thay # thành số thứ tự của partion )

B8: Gõ `format fs=fat32 quick`

B9: Gõ `assign letter=#` ( thay # thành tên letter mà bạn muốn )

## Tải bộ cài

B1: Tải [OpenCorePkg](https://github.com/acidanthera/opencorepkg/releases)

B2: Tìm đến thư mục `Utilities/macrecovery/` và chọn mục copy path

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-14-at-18.55.12.png?w=898)

B3: Mở cmd và gõ lệnh `cd` + control v

B4: Gõ một tron các lệnh sau, tùy vào bản macOS mà bạn muốn cài:

```
# Lion(10.7):
python macrecovery.py -b Mac-2E6FAB96566FE58C -m 00000000000F25Y00 download
python macrecovery.py -b Mac-C3EC7CD22292981F -m 00000000000F0HM00 download

# Mountain Lion(10.8):
python macrecovery.py -b Mac-7DF2A3B5E5D671ED -m 00000000000F65100 download

# Mavericks(10.9):
python macrecovery.py -b Mac-F60DEB81FF30ACF6 -m 00000000000FNN100 download

# Yosemite(10.10):
python macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000GDVW00 download

# El Capitan(10.11):
python macrecovery.py -b Mac-FFE5EF870D7BA81A -m 00000000000GQRX00 download

# Sierra(10.12):
python macrecovery.py -b Mac-77F17D7DA9285301 -m 00000000000J0DX00 download

# High Sierra(10.13)
python macrecovery.py -b Mac-7BA5B2D9E42DDD94 -m 00000000000J80300 download
python macrecovery.py -b Mac-BE088AF8C5EB4FA2 -m 00000000000J80300 download

# Mojave(10.14)
python macrecovery.py -b Mac-7BA5B2DFE22DDD8C -m 00000000000KXPG00 download

# Catalina(10.15)
python macrecovery.py -b Mac-00BE6ED71E35EB86 -m 00000000000000000 download

# Big Sur(11)
python macrecovery.py -b Mac-42FD25EABCABB274 -m 00000000000000000 download

# Monterey(12)
python ./macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 download

#Latest version
./macrecovery.py -b Mac-E43C1C25D4880AD6 -m 00000000000000000 -os latest download
```

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-14-at-19.04.38-1.png?w=904)

B5: Bỏ 2 file `BaseSystem.dmg` và `BaseSystem.chunklist` vào thư mục `com.apple.recovery.boot` ( tạo thư mục này ở thư mục gốc của ổ USB )

B6: Các bạn tạo EFI theo hướng dẫn [ở đây](https://heavietnam.ga/2021/11/23/cach-tao-efi-cho-opencore/) sau đó bỏ folder EFI vào usb ( chung phân vùng với folder `com.apple.recovery.boot` )
