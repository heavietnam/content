# Fix Ethernet

> Phần này mình làm dành cho các bạn dùng Desktop và 1 số bạn dùng Laptop.

## Card Ethernet Intel

- Đa phần các Card Intel đều dùng Kext này [IntelMausi](https://github.com/acidanthera/IntelMausi/releases).

- Dùng Kext này nếu Card Intel của bạn dựa trên tiến trình I211 Nics [SmallTreeIntel82576](https://github.com/khronokernel/SmallTree-I211-AT-patch/releases).
  
  ### Card Ethernet Atheros

- Đối với các bạn dùng Card này thì chỉ cần thêm Kext này vào thôi [AtherosE2200Ethernet](https://github.com/Mieze/AtherosE2200Ethernet/releases).
  
  ## Card Ethernet Realtek

- Đối với các bạn dùng Gigabit Ethernet của Realtek thì dùng Kext này [RealtekRTL8111](https://github.com/Mieze/RTL8111_driver_for_OS_X/releases).

- Đối với các bạn dùng 2.5Gb Ethernet của Realtek hoặc Intel’s I225-V NICs hoặc Intel’s I350 NICs (nói rõ hơn thì đối với các bạn dùng Desktop Comet Lake hoặc HDET Sandy Bridge hoặc HDET Ivy Birdge) thì dùng Kext này [LucyRTL8125Ethernet](https://www.insanelymac.com/forum/files/file/1004-lucyrtl8125ethernet/).
