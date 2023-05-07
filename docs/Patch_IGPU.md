# Patch IGPU

## **Chuẩn bị**

B1: Chỉnh DVMT trong bios thành 64 MB (DVMT Pre-Allocated)

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/bios.png)

B2: Thêm [lilu](https://github.com/vit9696/Lilu/releases) và [whatvergreen](https://github.com/acidanthera/WhateverGreen/releases) Kext vào `EFI ==> OC ==> kext` sau đó snapshot nếu dùng OpenCore hoặc bỏ kext vào `EFI ==> Clover ==> kext ==> other` nếu dùng Clover.

B3: Xóa hết các Kext sau (nếu có dùng).

- IntelGraphicsFixup.kext
- NvidiaGraphicsFixup.kext
- CoreDisplayFixup.kext
- Shiki.kext
- IntelGraphicsDVMTFixup.kext
- AzulPatcher4600.kext
- AppleBacklightFixup.kext
- FakePCIID_Intel_HD_Graphics.kext
- FakePCIID_Intel_HDMI_Audio.kext
- FakePCIID.kext (Nếu không có bất kì kext PCIID nào khác)

B4: Tắt tất cả Graphic Injects của Clover.

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/Clover1.png)

B5: Tắt những DSDT sau của Clover đi

- AddHDMI
- FixDisplay
- FixIntelGfx
- AddIMEI
- FixHDA
- AddPNLF

B6: Tắt các tính năng ở sau ở Clover

- UseIntelHDMI
- SetIntelBacklight
- SetIntelMaxBacklight

B7: Tắt tính năng `Devices ==> Inject` ở Clover

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/Clover2.png)

B8: Xóa các mục sau ở trong boot-arg (boot arguments).

- -disablegfxfirmware
- -igfxnohdmi

B9: Xóa các patch `fake-id` cho `IMEI` và `IntelGFX`.

B10: Xóa các patch ig-platform-id ở Clover.

![](https://imgur.com/g90qPT5.png)

B11: Xóa hết các patch về IGPU, IMEI, HDEF và HDMI audio trong SSDT hoặc DSDT.

B12: Xóa hết các Patch rename sau:

- GFX0 to IGPU
- PEGP to GFX0
- HECI to IMEI
- MEI to IMEI
- HDAS to HDEF
- B0D3 to HDAU

## Sơ lược:

Đầu tiên ta phải biết để inject properties ở OpenCore ta sẽ inject vào `Devices` ==> `Properties` ở Clover ta sẽ inject vào `DeviceProperties` trong `config.plist`

1 số Properties thường dùng:

- `AAPL,ig-platform-id` và `AAPL,snb-platform-id` patch framebuffer.
- `device-id` dùng để fake id của các device.
- `layout-id` cho `HDEF` để inject các layout cho AppleALC.

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/basic.png)

Ngoài ra bạn còn có thể dùng boot-arg để inject framebuffer vd: `igfxframe=0x0166000B`

**Lưu ý:**

- **Nếu framebuffer không được sử dụng thì sẽ tự inject frambuffer mặc định**
- **Nếu có dgpu và framebuffer cũng không được inject thì sẽ inject 1 framebuffer rỗng**

## Cách chọn framebuffer và 1 số lưu ý đặc biệt

### Gen 1 (Arrandale) :

Được support ở OS X 10.6.4 đến macOS 10.13.6

Hầu hết đều yêu cầu Cách patch sau: `framebuffer-patch-enable` và `framebuffer-singlelink`. `AAPL,ig-platform-id` thường không yêu cầu.

Lưu ý: 1 số tình trạng gặp lỗi về display hoặc là wake thì các bạn có thể add `framebuffer-fbccontrol-*` and `framebuffer-featurecontrol-*` nó có thể sẽ fix được lỗi hoặc các bạn có thể thử. `AppleIntelHDGraphicsFB.kext`

#### **Khuyến khích:**

- *framebuffer-patch-enable (enable patching)*
- *framebuffer-linkwidth (dùng để đặt link width mặc định để là 1)*
- *framebuffer-singlelink (enable single link mode)*

#### **FBCControl:**

- *framebuffer-fbccontrol-allzero (đặt tất cả các properties thành 0, các*properties* bên dưới sẽ ghi đè)*
- *framebuffer-fbccontrol-compressio*

#### **FeatureControl:**

- *framebuffer-featurecontrol-allzero (*đặt tất cả các properties thành 0, các*properties* bên dưới sẽ ghi đè*)*
- *framebuffer-featurecontrol-fbc*
- *framebuffer-featurecontrol-gpuinterrupthandling*
- *framebuffer-featurecontrol-gamma*
- *framebuffer-featurecontrol-maximumselfrefreshlevel*
- *framebuffer-featurecontrol-powerstates*
- *framebuffer-featurecontrol-rstimertest*
- *framebuffer-featurecontrol-renderstandby*
- *framebuffer-featurecontrol-watermarks*

#### Device-id:

- 0x0042
- 0x0046

### Gen 2 (2000/3000 | Sandy Bridge):

Được support từ OS X 10.7.x đến macOS 10.13.6

#### Spoiler: SNB connectors

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelSNBGraphicsFB.kext

ID: SNB0 0x10000, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 1 MB, MAX STOLEN: 0 bytes, MAX OVERALL: 1 MB (1064960 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 2, PortCount: 4, FBMemoryCount: 0
[5] busId: 0x03, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000009 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000400, flags: 0x00000009 - ConnectorDP
05030000 02000000 30000000
02050000 00040000 07000000
03040000 00040000 09000000
04060000 00040000 09000000

ID: SNB1 0x20000, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 1 MB, MAX STOLEN: 0 bytes, MAX OVERALL: 1 MB (1052672 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 2, PortCount: 1, FBMemoryCount: 0
[5] busId: 0x03, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
05030000 02000000 30000000

ID: SNB2 0x30010 or 0x30020, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 1 MB, MAX STOLEN: 0 bytes, MAX OVERALL: 1 MB (1060864 bytes)
Camellia: CamelliaUnsupported (255), Freq: 0 Hz, FreqMax: -1 Hz
Mobile: 0, PipeCount: 2, PortCount: 3, FBMemoryCount: 0
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000009 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
02050000 00040000 07000000
03040000 00040000 09000000
04060000 00080000 06000000

ID: SNB3 0x30030, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 0 bytes, MAX STOLEN: 0 bytes, MAX OVERALL: 0 bytes
Camellia: CamelliaUnsupported (255), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: SNB4 0x40000, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 1 MB, MAX STOLEN: 0 bytes, MAX OVERALL: 1 MB (1060864 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 2, PortCount: 3, FBMemoryCount: 0
[1] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000009 - ConnectorDP
01000000 02000000 30000000
02050000 00040000 07000000
03040000 00040000 09000000

ID: SNB5 0x50000, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 0 bytes, MAX STOLEN: 0 bytes, MAX OVERALL: 0 bytes
Camellia: CamelliaUnsupported (255), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: SNB6 Not addressible, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 512 KB, MAX STOLEN: 0 bytes, MAX OVERALL: 512 KB
Camellia: CamelliaUnsupported (255), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 1, PortCount: 0, FBMemoryCount: 0

ID: SNB7 Not addressible, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 0 bytes, MAX OVERALL: 1 MB (1589248 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 3, PortCount: 4, FBMemoryCount: 0
[1] busId: 0x00, pipe: 0, type: 0x00000400, flags: 0x00000030 - ConnectorDP
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000009 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
01000000 00040000 30000000
02050000 00040000 07000000
03040000 00040000 09000000
04060000 00080000 06000000

Default SNB, DVMT: 0 bytes, FBMEM: 0 bytes, VRAM: 0 bytes, Flags: 0x00000000
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 3, PortCount: 4, FBMemoryCount: 0
[1] busId: 0x00, pipe: 0, type: 0x00000400, flags: 0x00000030 - ConnectorDP
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000009 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
01000000 00040000 30000000
02050000 00040000 07000000
03040000 00040000 09000000
04060000 00080000 06000000

// khi không apply các patch framebuffer thì mặc định sẽ lấy framebuffer từ smbios 

Mac-94245B3640C91C81 -> SNB0 (MacBookPro8,1)
Mac-94245AF5819B141B -> SNB0
Mac-94245A3940C91C80 -> SNB0 (MacBookPro8,2)
Mac-942459F5819B171B -> SNB0 (MacBookPro8,3)
Mac-8ED6AF5B48C039E1 -> SNB2 (Macmini5,1)
Mac-7BA5B2794B2CDB12 -> SNB2 (Macmini5,3)
Mac-4BC72D62AD45599E -> SNB3 (Macmini5,2) -> no ports
Mac-742912EFDBEE19B3 -> SNB4 (MacBookAir4,2)
Mac-C08A6BB70A942AC2 -> SNB4 (MacBookAir4,1)
Mac-942B5BF58194151B -> SNB5 (iMac12,1) -> no ports
Mac-942B5B3A40C91381 -> SNB5 -> no ports
Mac-942B59F58194171B -> SNB5 (iMac12,2) -> no ports
```

#### Device-id:

- 0x0106
- 0x1106
- 0x1601
- 0x0116
- 0x0126
- 0x0102

#### Framebuffer:

- Desktop:
  - 0x00030010 (default)
- Laptop:
  - 0x00010000 (default)
- Empty Framebuffer:
  - 0x00050000 (default)

**Lưu ý: Empty Framebuffer nó vẫn là 1 framebuffer nhưng nó ko đc ùng để xuất màn hình tức là không có bất kì màn nào cắm vào iGPU**.

**Lưu ý 2: Intel HD2000 không thể xuất màn được. Vì vậy bạn phải dùng empty frambuffer để kích hoạt iGPU này. Dòng Sandy Bridge chỉ có HD3000 mới có khả năng xuất màn**.

**Lưu ý 3: Sandy Bridge thường không cần inject framebuffer mà sẽ tự lấy ở SMBIOS**.

**Lưu ý 4: Desktop yêu cầu fake device-id bạn có thể thử device-id này `26010000` cho iGPU**

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/snb_igpu.png)

**Lưu ý 5: Đối với những bạn dùng empty framebuffer thì các bạn cần pahỉ fake device-id**.

**Lưu ý 6: Đối với những bạn sử dụng main series 7 thì cần add device-id `3A1C0000` và [SSDT-IMEI](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-IMEI.dsl)**

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/snb_imei.png)

### Gen 3 (2500/4000 | Ivy Bridge):

Được hỗ trợ từ OS X 10.8.x đến macOS 11.x ở bản macOS 12 các bạn vẫn có thể dùng OpenCore Leagcy Patcher để patch.

#### **Spoiler: Capri Connectors**

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelFramebufferCapri.kext

ID: 01660000, STOLEN: 96 MB, FBMEM: 24 MB, VRAM: 1024 MB, Flags: 0x00000000
TOTAL STOLEN: 24 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 72 MB, MAX OVERALL: 73 MB (77086720 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 0, PipeCount: 3, PortCount: 4, FBMemoryCount: 3
[1] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000400, flags: 0x00000007 - ConnectorDP
01000000 02000000 30000000
02050000 00040000 07000000
03040000 00040000 07000000
04060000 00040000 07000000

ID: 01620006, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 256 MB, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 0 bytes, MAX STOLEN: 0 bytes, MAX OVERALL: 0 bytes
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 01620007, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 256 MB, Flags: 0x00000000
TOTAL STOLEN: 0 bytes, TOTAL CURSOR: 0 bytes, MAX STOLEN: 0 bytes, MAX OVERALL: 0 bytes
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 01620005, STOLEN: 32 MB, FBMEM: 16 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 16 MB, TOTAL CURSOR: 1 MB, MAX STOLEN: 32 MB, MAX OVERALL: 33 MB (34615296 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 0, PipeCount: 2, PortCount: 3, FBMemoryCount: 2
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000011 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
02050000 00040000 11000000
03040000 00040000 07010000
04060000 00040000 07010000

ID: 01660001, STOLEN: 96 MB, FBMEM: 24 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 24 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 72 MB, MAX OVERALL: 73 MB (77086720 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 3, PortCount: 4, FBMemoryCount: 3
[1] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[2] busId: 0x05, pipe: 0, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
01000000 02000000 30000000
02050000 00080000 06000000
03040000 00040000 07010000
04060000 00040000 07010000

ID: 01660002, STOLEN: 64 MB, FBMEM: 24 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 24 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 24 MB, MAX OVERALL: 25 MB (26742784 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 3, PortCount: 1, FBMemoryCount: 1
[1] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
01000000 02000000 30000000

ID: 01660008, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 16 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 48 MB, MAX OVERALL: 49 MB (51916800 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[1] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
01000000 02000000 30000000
02050000 00040000 07010000
03040000 00040000 07010000

ID: 01660009, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 16 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 48 MB, MAX OVERALL: 49 MB (51916800 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[1] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
01000000 02000000 30000000
02050000 00040000 07010000
03040000 00040000 07010000

ID: 01660003, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 16 MB, TOTAL CURSOR: 1 MB, MAX STOLEN: 32 MB, MAX OVERALL: 33 MB (34619392 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 2, PortCount: 4, FBMemoryCount: 2
[5] busId: 0x03, pipe: 0, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000407 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000081 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000400, flags: 0x00000081 - ConnectorDP
05030000 02000000 30000000
02050000 00040000 07040000
03040000 00040000 81000000
04060000 00040000 81000000

ID: 01660004, STOLEN: 32 MB, FBMEM: 16 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 16 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 16 MB, MAX OVERALL: 17 MB (18354176 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 1, PipeCount: 3, PortCount: 1, FBMemoryCount: 1
[5] busId: 0x03, pipe: 0, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
05030000 02000000 30020000

ID: 0166000A, STOLEN: 32 MB, FBMEM: 16 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 16 MB, TOTAL CURSOR: 1 MB, MAX STOLEN: 32 MB, MAX OVERALL: 33 MB (34615296 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 0, PipeCount: 2, PortCount: 3, FBMemoryCount: 2
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
02050000 00040000 07010000
03040000 00040000 07010000
04060000 00080000 06000000

ID: 0166000B, STOLEN: 32 MB, FBMEM: 16 MB, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 16 MB, TOTAL CURSOR: 1 MB, MAX STOLEN: 32 MB, MAX OVERALL: 33 MB (34615296 bytes)
Camellia: CamelliaUnsupported (255), Freq: 1808 Hz, FreqMax: 1808 Hz
Mobile: 0, PipeCount: 2, PortCount: 3, FBMemoryCount: 2
[2] busId: 0x05, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[3] busId: 0x04, pipe: 0, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[4] busId: 0x06, pipe: 0, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
02050000 00040000 07010000
03040000 00040000 07010000
04060000 00080000 06000000
```

#### **Device-id:**

- 0x0152
- 0x0156
- 0x0162
- 0x0166

#### **framebuffers :**

- Desktop:
  - 0x0166000A (default)
  - 0x01620005
- Laptop:
  - 0x01660003 (default)
  - 0x01660009
  - 0x01660004
- Empty Framebuffer:
  - 0x01620007 (default)

**HD2500 không thể xuất được màn hình do đó bạn vẫn phải dùng empty framebuffer. Nếu máy bạn có main là 6 series thì cần fake imei `device-id` `3A1E0000` và add [ssdt-imei](https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-IMEI.dsl). Nếu dử dụng empty framebuffer thì bạn phải fake device-id**.

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/ivy_imei.png)

### Gen 4 (4200-5200 | Haswell):

Hỗ trợ từ OS X 10.9.x+

#### Spoiler: Azul connectors

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelFramebufferAzul.kext

ID: 0C060000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 0C160000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 0C260000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 04060000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 04160000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 04260000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 0D260000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 0A160000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 2777 Hz, FreqMax: 2777 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 0A260000, STOLEN: 64 MB, FBMEM: 16 MB, VRAM: 1024 MB, Flags: 0x00000004
TOTAL STOLEN: 209 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 209 MB, MAX OVERALL: 210 MB (220737536 bytes)
Camellia: CamelliaDisabled (0), Freq: 2777 Hz, FreqMax: 2777 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30000000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 0A260005, STOLEN: 32 MB, FBMEM: 19 MB, VRAM: 1536 MB, Flags: 0x0000000F
TOTAL STOLEN: 52 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 116 MB, MAX OVERALL: 117 MB (123219968 bytes)
Camellia: CamelliaDisabled (0), Freq: 2777 Hz, FreqMax: 2777 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000087 - ConnectorDP
[2] busId: 0x04, pipe: 9, type: 0x00000400, flags: 0x00000087 - ConnectorDP
00000800 02000000 30000000
01050900 00040000 87000000
02040900 00040000 87000000

ID: 0A260006, STOLEN: 32 MB, FBMEM: 19 MB, VRAM: 1536 MB, Flags: 0x0000000F
TOTAL STOLEN: 52 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 116 MB, MAX OVERALL: 117 MB (123219968 bytes)
Camellia: CamelliaDisabled (0), Freq: 2777 Hz, FreqMax: 2777 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000087 - ConnectorDP
[2] busId: 0x04, pipe: 9, type: 0x00000400, flags: 0x00000087 - ConnectorDP
00000800 02000000 30000000
01050900 00040000 87000000
02040900 00040000 87000000

ID: 0A2E0008, STOLEN: 64 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000021E
TOTAL STOLEN: 99 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 227 MB, MAX OVERALL: 228 MB (239611904 bytes)
Camellia: CamelliaV1 (1), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000107 - ConnectorDP
00000800 02000000 30000000
01050900 00040000 07010000
02040A00 00040000 07010000

ID: 0A16000C, STOLEN: 64 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000001E
TOTAL STOLEN: 99 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 227 MB, MAX OVERALL: 228 MB (239611904 bytes)
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000107 - ConnectorDP
00000800 02000000 30000000
01050900 00040000 07010000
02040A00 00040000 07010000

ID: 0D260007, STOLEN: 64 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000031E
TOTAL STOLEN: 99 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 227 MB, MAX OVERALL: 228 MB (239616000 bytes)
Camellia: CamelliaDisabled (0), Freq: 1953 Hz, FreqMax: 1953 Hz
Mobile: 1, PipeCount: 3, PortCount: 4, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 11, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[2] busId: 0x04, pipe: 11, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[3] busId: 0x06, pipe: 3, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
00000800 02000000 30000000
01050B00 00040000 07010000
02040B00 00040000 07010000
03060300 00080000 06000000

ID: 0D220003, STOLEN: 32 MB, FBMEM: 19 MB, VRAM: 1536 MB, Flags: 0x00000402
TOTAL STOLEN: 52 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 116 MB, MAX OVERALL: 117 MB (123219968 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000087 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000087 - ConnectorDP
[3] busId: 0x06, pipe: 8, type: 0x00000400, flags: 0x00000011 - ConnectorDP
01050900 00040000 87000000
02040A00 00040000 87000000
03060800 00040000 11000000

ID: 0A2E000A, STOLEN: 32 MB, FBMEM: 19 MB, VRAM: 1536 MB, Flags: 0x000000D6
TOTAL STOLEN: 52 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 116 MB, MAX OVERALL: 117 MB (123219968 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000011 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000087 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000087 - ConnectorDP
00000800 02000000 11000000
01050900 00040000 87000000
02040A00 00040000 87000000

ID: 0A26000A, STOLEN: 32 MB, FBMEM: 19 MB, VRAM: 1536 MB, Flags: 0x000000D6
TOTAL STOLEN: 52 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 116 MB, MAX OVERALL: 117 MB (123219968 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000011 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000087 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000087 - ConnectorDP
00000800 02000000 11000000
01050900 00040000 87000000
02040A00 00040000 87000000

ID: 0A2E000D, STOLEN: 96 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000040E
TOTAL STOLEN: 131 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 227 MB, MAX OVERALL: 228 MB (239607808 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 2, FBMemoryCount: 2
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000107 - ConnectorDP
01050900 00040000 07010000
02040A00 00040000 07010000

ID: 0A26000D, STOLEN: 96 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000040E
TOTAL STOLEN: 131 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 227 MB, MAX OVERALL: 228 MB (239607808 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 2, FBMemoryCount: 2
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000107 - ConnectorDP
01050900 00040000 07010000
02040A00 00040000 07010000

ID: 04120004, STOLEN: 32 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 0412000B, STOLEN: 32 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00000000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 0D260009, STOLEN: 64 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000001E TOTAL STOLEN: 99 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 99 MB, MAX OVERALL: 100 MB (105385984 bytes)
Camellia: CamelliaDisabled (0), Freq: 1953 Hz, FreqMax: 1953 Hz
Mobile: 1, PipeCount: 3, PortCount: 1, FBMemoryCount: 1
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
00000800 02000000 30000000

ID: 0D26000E, STOLEN: 96 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000031E
TOTAL STOLEN: 131 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 323 MB, MAX OVERALL: 324 MB (340279296 bytes)
Camellia: CamelliaV2 (2), Freq: 1953 Hz, FreqMax: 1953 Hz
Mobile: 1, PipeCount: 3, PortCount: 4, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
[1] busId: 0x05, pipe: 11, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[2] busId: 0x04, pipe: 11, type: 0x00000400, flags: 0x00000107 - ConnectorDP
[3] busId: 0x06, pipe: 3, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
00000800 02000000 30000000
01050B00 00040000 07010000
02040B00 00040000 07010000
03060300 00080000 06000000

ID: 0D26000F, STOLEN: 96 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x0000001E
TOTAL STOLEN: 131 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 131 MB, MAX OVERALL: 132 MB (138940416 bytes)
Camellia: CamelliaV2 (2), Freq: 1953 Hz, FreqMax: 1953 Hz
Mobile: 1, PipeCount: 3, PortCount: 1, FBMemoryCount: 1
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000030 - ConnectorLVDS
00000800 02000000 30000000
```

#### Device-id:

- 0x0d26
- 0x0a26
- 0x0a2e
- 0x0d22
- 0x0412

#### **framebuffers:**

- Desktop:
  - 0x0D220003 (default)
- Laptop:
  - 0x0A160000 (default)
  - 0x0A260005 (recommended)
  - 0x0A260006 (recommended)
- Empty Framebuffer:
  - 0x04120004 (default)

**Đối với desktop HD4400 and mobile HD4200/HD4400/HD4600 cần fake `device-id` `12040000.`**

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/hsw_igpu.png)

### Gen 5 (5300-6300 | Broadwell):

Được support từ OS X 10.10.2+

#### Spoiler: BDW connectors

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelBDWGraphicsFramebuffer.kext

ID: 16060000, STOLEN: 16 MB, FBMEM: 15 MB, VRAM: 1024 MB, Flags: 0x00000B06
TOTAL STOLEN: 32 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 64 MB, MAX OVERALL: 65 MB (68694016 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30020000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 160E0000, STOLEN: 16 MB, FBMEM: 15 MB, VRAM: 1024 MB, Flags: 0x00000706
TOTAL STOLEN: 32 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 64 MB, MAX OVERALL: 65 MB (68694016 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30020000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 16160000, STOLEN: 16 MB, FBMEM: 15 MB, VRAM: 1024 MB, Flags: 0x00000B06
TOTAL STOLEN: 32 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 64 MB, MAX OVERALL: 65 MB (68694016 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30020000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 161E0000, STOLEN: 16 MB, FBMEM: 15 MB, VRAM: 1024 MB, Flags: 0x00000716
TOTAL STOLEN: 32 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 64 MB, MAX OVERALL: 65 MB (68694016 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30020000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 16260000, STOLEN: 16 MB, FBMEM: 15 MB, VRAM: 1024 MB, Flags: 0x00000B06
TOTAL STOLEN: 32 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 64 MB, MAX OVERALL: 65 MB (68694016 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30020000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 162B0000, STOLEN: 16 MB, FBMEM: 15 MB, VRAM: 1024 MB, Flags: 0x00000B06
TOTAL STOLEN: 32 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 64 MB, MAX OVERALL: 65 MB (68694016 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30020000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 16220000, STOLEN: 16 MB, FBMEM: 15 MB, VRAM: 1024 MB, Flags: 0x0000110E
TOTAL STOLEN: 32 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 64 MB, MAX OVERALL: 65 MB (68694016 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000004, flags: 0x00000004 - ConnectorDigitalDVI
[2] busId: 0x04, pipe: 9, type: 0x00000800, flags: 0x00000082 - ConnectorHDMI
00000800 02000000 30020000
01050900 04000000 04000000
02040900 00080000 82000000

ID: 160E0001, STOLEN: 38 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00000702
TOTAL STOLEN: 60 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 136 MB, MAX OVERALL: 137 MB (144191488 bytes)
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00001001 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00003001 - ConnectorDP
00000800 02000000 30020000
01050900 00040000 01100000
02040A00 00040000 01300000

ID: 161E0001, STOLEN: 38 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00000702
TOTAL STOLEN: 60 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 136 MB, MAX OVERALL: 137 MB (144191488 bytes)
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00001001 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00003001 - ConnectorDP
00000800 02000000 30020000
01050900 00040000 01100000
02040A00 00040000 01300000

ID: 16060002, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00004B02
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 30020000
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 16160002, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00004B02
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 30020000
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 16260002, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00004B0A
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 30020000
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 16220002, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00004B0A
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 30020000
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 162B0002, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00004B0A
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 30020000
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 16120003, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00001306
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131612672 bytes)
Camellia: CamelliaV1 (1), Freq: 1953 Hz, FreqMax: 1953 Hz
Mobile: 1, PipeCount: 3, PortCount: 4, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 11, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 11, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[3] busId: 0x06, pipe: 3, type: 0x00000800, flags: 0x00000006 - ConnectorHDMI
00000800 02000000 30020000
01050B00 00040000 07050000
02040B00 00040000 07050000
03060300 00080000 06000000

ID: 162B0004, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00040B46
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000211 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 11020000
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 16260004, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00040B46
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000211 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 11020000
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 16220007, STOLEN: 38 MB, FBMEM: 38 MB, VRAM: 1536 MB, Flags: 0x000BB306
TOTAL STOLEN: 77 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 153 MB, MAX OVERALL: 154 MB (162017280 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[3] busId: 0x06, pipe: 8, type: 0x00000400, flags: 0x00000011 - ConnectorDP
01050900 00040000 07050000
02040A00 00040000 07050000
03060800 00040000 11000000

ID: 16260005, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00000B0B
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaDisabled (0), Freq: 2777 Hz, FreqMax: 2777 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 11, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 11, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 30020000
01050B00 00040000 07050000
02040B00 00040000 07050000

ID: 16260006, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00000B0B
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Camellia: CamelliaDisabled (0), Freq: 2777 Hz, FreqMax: 2777 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000230 - ConnectorLVDS
[1] busId: 0x05, pipe: 11, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 11, type: 0x00000400, flags: 0x00000507 - ConnectorDP
00000800 02000000 30020000
01050B00 00040000 07050000
02040B00 00040000 07050000

ID: 162B0008, STOLEN: 34 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x00002B0E
TOTAL STOLEN: 69 MB, TOTAL CURSOR: 1 MB, MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109060096 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 2, PortCount: 2, FBMemoryCount: 2
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
01050900 00040000 07050000
02040A00 00040000 07050000

ID: 16260008, STOLEN: 34 MB, FBMEM: 34 MB, VRAM: 1536 MB, Flags: 0x00002B0E
TOTAL STOLEN: 69 MB, TOTAL CURSOR: 1 MB, MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109060096 bytes)
Camellia: CamelliaDisabled (0), Freq: 5273 Hz, FreqMax: 5273 Hz
Mobile: 0, PipeCount: 2, PortCount: 2, FBMemoryCount: 2
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000507 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000507 - ConnectorDP
01050900 00040000 07050000
02040A00 00040000 07050000
```

#### Device-id:

- 0x0BD1
- 0x0BD2
- 0x0BD3
- 0x1606
- 0x160e
- 0x1616
- 0x161e
- 0x1626
- 0x1622
- 0x1612
- 0x162b

#### **framebuffers:**

- Desktop:
  - 0x16220007 (default)
- Laptop:
  - 0x16260006 (default)

### Gen 6 (510-580 | Skylake):

Được support từ OS X 10.11.4+

#### Spoiler: SKL connectors

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelSKLGraphicsFramebuffer.kext

ID: 191E0000, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x0000050F
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Model name: Intel HD Graphics SKL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 19160000, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x0000090F
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Model name: Intel HD Graphics SKL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 19260000, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x0000090F
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Model name: Intel HD Graphics SKL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 19270000, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x0000090F
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Model name: Intel HD Graphics SKL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 191B0000, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x0000110F
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Model name: Intel HD Graphics SKL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 193B0000, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00001187
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Model name: Intel HD Graphics SKL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[2] busId: 0x04, pipe: 10, type: 0x00000800, flags: 0x00000187 - ConnectorHDMI
[3] busId: 0x06, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
02040A00 00080000 87010000
03060A00 00040000 87010000

ID: 19120000, STOLEN: 34 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x0000110F
TOTAL STOLEN: 56 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 124 MB, MAX OVERALL: 125 MB (131608576 bytes)
Model name: Intel HD Graphics SKL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[255] busId: 0x00, pipe: 0, type: 0x00000001, flags: 0x00000020 - ConnectorDummy
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
FF000000 01000000 20000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 19020001, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00040800
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics SKL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 19170001, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00040800
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics SKL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 19120001, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00040800
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics SKL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 19320001, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00040800
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics SKL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 19160002, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00830B02
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Graphics 540
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 19260002, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00E30B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Graphics 540
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 191E0003, STOLEN: 40 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x002B0702
TOTAL STOLEN: 41 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 121 MB, MAX OVERALL: 122 MB (128462848 bytes)
Model name: Intel HD Graphics 515
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000181 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000181 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 81010000
02040A00 00040000 81010000

ID: 19260004, STOLEN: 34 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00030B0A
TOTAL STOLEN: 35 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109588480 bytes)
Model name: Intel Iris Graphics 550
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7010000
02040A00 00040000 C7010000

ID: 19270004, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00E30B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Graphics 550
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 193B0005, STOLEN: 34 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0023130A
TOTAL STOLEN: 35 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 137 MB, MAX OVERALL: 138 MB (145244160 bytes)
Model name: Intel Iris Pro Graphics 580
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 4, FBMemoryCount: 4
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
[3] busId: 0x06, pipe: 10, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7010000
02040A00 00040000 C7010000
03060A00 00040000 C7010000

ID: 191B0006, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00131302
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 512 KB, MAX STOLEN: 39 MB, MAX OVERALL: 39 MB (41422848 bytes)
Model name: Intel HD Graphics 530
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 1, PortCount: 1, FBMemoryCount: 1
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
00000800 02000000 98040000

ID: 19260007, STOLEN: 34 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00031302
TOTAL STOLEN: 35 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109588480 bytes)
Model name: Intel Iris Pro Graphics 580
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7010000
02040A00 00040000 C7010000

// khi không add framebuffer thì mặc định sẽ tự inject framebuffer 19120000
```

#### **Device-id:**

- 0x1916
- 0x191E
- 0x1926
- 0x1927
- 0x1912
- 0x1932
- 0x1902
- 0x1917
- 0x193B
- 0x191B

#### **framebuffers:**

- Desktop:
  - 0x19120000 (default)
- Laptop:
  - 0x19160000 (default)
- Empty Framebuffer:
  - 0x19120001 (default)

### Gen 7-8 (610-650 | Kaby Lake và Amber Lake Y)

Kaby Lake support từ macOS 10.12.6+ vàAmber Lake Y support từ macOS 10.14.1

#### **Spoiler: KBL/ABL connectors**

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelKBLGraphicsFramebuffer.kext

ID: 591E0000, STOLEN: 34 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000078B
TOTAL STOLEN: 35 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109588480 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 87C00000, STOLEN: 34 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000078B
TOTAL STOLEN: 35 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109588480 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 59160000, STOLEN: 34 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00000B0B
TOTAL STOLEN: 35 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 103 MB, MAX OVERALL: 104 MB (109588480 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000800, flags: 0x00000187 - ConnectorHDMI
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00080000 87010000

ID: 59230000, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00030B8B
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 115 MB, MAX OVERALL: 116 MB (122171392 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 59260000, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00030B8B
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 115 MB, MAX OVERALL: 116 MB (122171392 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 59270000, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00030B8B
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 115 MB, MAX OVERALL: 116 MB (122171392 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 59270009, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00830B0A
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 115 MB, MAX OVERALL: 116 MB (122171392 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7010000
02040A00 00040000 C7010000

ID: 59160009, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00830B0A
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 115 MB, MAX OVERALL: 116 MB (122171392 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7010000
02040A00 00040000 C7010000

ID: 59120000, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000110B
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 115 MB, MAX OVERALL: 116 MB (122171392 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[3] busId: 0x06, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
01050900 00040000 87010000
02040A00 00040000 87010000
03060A00 00040000 87010000

ID: 591B0000, STOLEN: 38 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x0000130B
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 136 MB, MAX OVERALL: 137 MB (144191488 bytes)
Model name: Intel HD Graphics KBL CRB
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[2] busId: 0x04, pipe: 10, type: 0x00000800, flags: 0x00000187 - ConnectorHDMI
[3] busId: 0x06, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
02040A00 00080000 87010000
03060A00 00040000 87010000

ID: 591E0001, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x002B0702
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 115 MB, MAX OVERALL: 116 MB (122171392 bytes)
Model name: Intel HD Graphics 615
Camellia: CamelliaV2 (2), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000181 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000181 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 81010000
02040A00 00040000 81010000

ID: 59180002, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics KBL
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 59120003, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics KBL
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 59260007, STOLEN: 57 MB, FBMEM: 21 MB, VRAM: 1536 MB, Flags: 0x00830B0E
TOTAL STOLEN: 79 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203960320 bytes)
Model name: Intel Iris Plus Graphics 640
Camellia: CamelliaDisabled (0), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 59270004, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00E30B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Plus Graphics 650
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 59260002, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00E30B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Plus Graphics 640
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 87C00005, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00A30702
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel UHD Graphics 617
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 591C0005, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00A30702
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel UHD Graphics 617
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 591B0006, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00031302
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 512 KB, MAX STOLEN: 39 MB, MAX OVERALL: 39 MB (41422848 bytes)
Model name: Intel HD Graphics 630
Camellia: CamelliaV3 (3), Freq: 1388 Hz, FreqMax: 1388 Hz
Mobile: 1, PipeCount: 1, PortCount: 1, FBMemoryCount: 1
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
00000800 02000000 98040000

// khi không add framebuffer thì mặc định sẽ tự inject framebuffer 59160000
```

#### **Device-id**:

- KBL:
  - 0x5912
  - 0x5916
  - 0x591B
  - 0x591C
  - 0x591E
  - 0x5926
  - 0x5927
  - 0x5923
- ABL:
  - 0x87C0

#### **framebuffers:**

- Desktop:
  - 0x59160000 (default)
  - 0x59120000 (recommended)
- Laptop:
  - 0x591B0000 (default)
- Empty Framebuffer:
  - 0x59120003 (default)

### Gen 8-9 (UHD 610-655 | Coffee Lake và Comet Lake):

Được hỗ trợ từ macOS 10.14+ (UHD630 củaComet Lake được hỗ trợ từ macOS 10.15.4)

#### Spoiler: CFL/CML connectors

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelCFLGraphicsFramebuffer.kext
ID: 3EA50009, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00830B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel HD Graphics CFL CRB
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000001C7 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 C7010000
02040A00 00040000 C7010000

ID: 3E920009, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0083130A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel HD Graphics CFL CRB
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[255] busId: 0x00, pipe: 0, type: 0x00000001, flags: 0x00000020 - ConnectorDummy
[255] busId: 0x00, pipe: 0, type: 0x00000001, flags: 0x00000020 - ConnectorDummy
00000800 02000000 98000000
FF000000 01000000 20000000
FF000000 01000000 20000000

ID: 3E9B0009, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0083130A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel HD Graphics CFL CRB
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 3EA50000, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00030B0B
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel HD Graphics CFL CRB
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 3E920000, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000130B
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel HD Graphics CFL CRB
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 3E000000, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000130B
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel HD Graphics CFL CRB
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 3E9B0000, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000130B
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel HD Graphics CFL CRB
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x00000187 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x00000187 - ConnectorDP
00000800 02000000 98000000
01050900 00040000 87010000
02040A00 00040000 87010000

ID: 3EA50004, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00E30B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Plus Graphics 655
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 3EA50005, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00E30B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Plus Graphics 655
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 3EA60005, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00E30B0A
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel Iris Plus Graphics 645
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
00000800 02000000 98040000
01050900 00040000 C7030000
02040A00 00040000 C7030000

ID: 3E9B0006, STOLEN: 38 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00131302
TOTAL STOLEN: 39 MB, TOTAL CURSOR: 512 KB, MAX STOLEN: 39 MB, MAX OVERALL: 39 MB (41422848 bytes)
Model name: Intel UHD Graphics 630
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 1, PortCount: 1, FBMemoryCount: 1
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000498 - ConnectorLVDS
00000800 02000000 98040000

ID: 3E9B0008, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00031302
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 512 KB, MAX STOLEN: 58 MB, MAX OVERALL: 58 MB (61345792 bytes)
Model name: Intel UHD Graphics 630
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 1, PortCount: 1, FBMemoryCount: 1
[0] busId: 0x00, pipe: 8, type: 0x00000002, flags: 0x00000098 - ConnectorLVDS
00000800 02000000 98000000

ID: 3E9B0007, STOLEN: 57 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00801302
TOTAL STOLEN: 58 MB, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 172 MB, MAX OVERALL: 173 MB (181940224 bytes)
Model name: Intel UHD Graphics 630
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[1] busId: 0x05, pipe: 9, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[2] busId: 0x04, pipe: 10, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
[3] busId: 0x06, pipe: 8, type: 0x00000400, flags: 0x000003C7 - ConnectorDP
01050900 00040000 C7030000
02040A00 00040000 C7030000
03060800 00040000 C7030000

ID: 3E920003, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics CFL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 3E910003, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics CFL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 3E980003, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics CFL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 9BC80003, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics CFL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 9BC50003, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics CFL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

ID: 9BC40003, STOLEN: 0 bytes, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00001000
TOTAL STOLEN: 1 MB, TOTAL CURSOR: 0 bytes, MAX STOLEN: 1 MB, MAX OVERALL: 1 MB
Model name: Intel HD Graphics CFL
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 0, PipeCount: 0, PortCount: 0, FBMemoryCount: 0

// khi không add framebuffer thì mặc định sẽ tụe inject frambuffer 3EA50000
```

#### **Device-id**:

- CFL:
  - 0x3E9B
  - 0x3EA5
  - 0x3EA6
  - 0x3E92
  - 0x3E91
  - 0x3E98
- CML:
  - 0x9BC8
  - 0x9BC5
  - 0x9BC4

#### **Framebuffers:**

- Desktop:
  - 0x3EA50000 (default)
  - 0x3E9B0007 (recommended)
- Laptop:
  - 0x3EA50009 (default)
- Empty framebuffer (CFL):
  - 0x3E910003 (default)
- Empty framebuffer (CML):
  - 0x9BC80003 (default)

**Đối với Gen 9 Coffee Lake cần add device-id `device-id` `923E0000` . Nhưng đối với macos 10.14.4+ thì ko cần thiết nữa.**

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/cfl-r_igpu.png)

Đối với uhd 620 Whiskey Lake cần fake `device-id` `A53E0000`

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/WhiskeyLake.png)

Lưu ý: Nếu bạn cài macOS 10.13 trên Coffee Lake thì nó sẽ không hoạt động. Tuy nhiên vẫn có 1 phiên bản macOS High Sierra 10.13.6 (17G2208) có hỗ trợ Coffee Lake nhưng bạn không thể sử dụng empty framebuffer. Vì chỉ có AppleIntelCFLGraphicsFramebuffer.kext từ 10.14+ mới hỗ trợ empty framebuffer và ở bản 10.13 cx ko có hỗ trọ device id `0x3E91` . Nhưng bạn vẫn có thể enable UHD630 ở 10.13 bằng việc fake device-id về Kaby Lake HD630 và sử dụng kabylake framebuffer HD630.

![](https://github.com/acidanthera/WhateverGreen/raw/master/Manual/Img/kbl.png)

### Gen 10 (Iris Plus Graphics | Ice Lake):

Hỗ trợ từ macOS 10.15.4+

#### Spoiler: ICL connectors

```
// khi thêm các patch framebuffer vào thì properties sẽ được thêm vào AppleIntelICLLPGraphicsFramebuffer.kext

ID: FF050000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00000300
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203960320 bytes)
Model name: Intel HD Graphics ICL SIM
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000201 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000201 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 01020000
02000000 09000000 01000000 01000000 00040000 01020000

ID: 8A710000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008301
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A700000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00018301
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A510000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008305
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A5C0000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008305
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A5D0000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008301
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A520000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008305
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A530000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008305
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A5A0000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008305
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A5B0000, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x00008301
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203972608 bytes)
Model name: Intel HD Graphics ICL RVP
Camellia: CamelliaDisabled (0), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 6, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[1] busId: 0x02, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x00000281 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
01000000 02000000 01000000 00000000 00040000 81020000
02000000 09000000 01000000 01000000 00040000 81020000
03000000 0A000000 01000000 01000000 00040000 81020000
04000000 0B000000 01000000 01000000 00040000 81020000
05000000 0C000000 01000000 01000000 00040000 81020000

ID: 8A710001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000A300
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

ID: 8A700001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0001A304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

ID: 8A510001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000E304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203960320 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000

ID: 8A5C0001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000A304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203960320 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000

ID: 8A5D0001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000A300
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203960320 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000

ID: 8A520001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000E304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000118 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18010000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

ID: 8A530001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000E304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000118 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18010000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

ID: 8A5A0001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000A304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000118 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18010000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

ID: 8A5B0001, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000A300
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel HD Graphics ICL RVP BigSur
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000118 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18010000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

ID: 8A510002, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000E304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203960320 bytes)
Model name: Intel Iris Plus Graphics
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000

ID: 8A5C0002, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000E304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203960320 bytes)
Model name: Intel Iris Plus Graphics
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 3, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000018 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18000000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000

ID: 8A520002, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000E304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel Iris Plus Graphics
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000118 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18010000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

ID: 8A530002, STOLEN: 64 MB, FBMEM: 0 bytes, VRAM: 1536 MB, Flags: 0x0000E304
TOTAL STOLEN: 193 MB?, TOTAL CURSOR: 1 MB (1572864 bytes), MAX STOLEN: 193 MB, MAX OVERALL: 194 MB (203968512 bytes)
Model name: Intel Iris Plus Graphics
Camellia: CamelliaV3 (3), Freq: 0 Hz, FreqMax: 0 Hz
Mobile: 1, PipeCount: 3, PortCount: 5, FBMemoryCount: 3
[0] busId: 0x00, pipe: 0, type: 0x00000002, flags: 0x00000118 - ConnectorLVDS
[2] busId: 0x09, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[3] busId: 0x0A, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[4] busId: 0x0B, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
[5] busId: 0x0C, pipe: 1, type: 0x00000400, flags: 0x000002C1 - ConnectorDP
00000000 00000000 00000000 00000000 02000000 18010000
02000000 09000000 01000000 01000000 00040000 C1020000
03000000 0A000000 01000000 01000000 00040000 C1020000
04000000 0B000000 01000000 01000000 00040000 C1020000
05000000 0C000000 01000000 01000000 00040000 C1020000

// khi bạn không inject framebuffer thì sẽ inject framebufer theo id thực 8A520000
// khi không inject framebuffer thì  sẽ tự inject framebufer theo id ảo FF050000
```

#### **Device-id:**

- 0xff05
- 0x8A70
- 0x8A71
- 0x8A51
- 0x8A5C
- 0x8A5D
- 0x8A52
- 0x8A53
- 0x8A5A
- 0x8A5B

#### **framebuffers:**

- Laptop:
  - 0x8A520000 (default)

**Lưu ý: đối với các iGPU UHD G1 các bạn có thể fake device-id thành UHD630 hoặc G4, G7 và sử dụng framebuffer mặc định `0x8A52000`**

> Lưu ý đối với GPU g1 các bạn cần fake deivce id thành 0x8A5C và platform id khuyến khích là 01005C8A

## Phương Pháp:

### ***Cách 1: Patch thủ công***

#### ***Sửa AAPL, ig-platform-id :***

##### **OpenCore**

**B1:** Các bạn tiến hành lấy framebuffer thích hợp ở phần trên (chọn framebuffer):

- device-id: các bạn lấy device-id ở đây
- Framebuffer: các bạn lấy appl,ig-platform-id tại đây

![](https://imgur.com/U5A4pqG.png)

**B2:** Bây giờ chúng ta sẽ chuyển đổi id vừa lấy được để sử dụng:

- Đầu tiên chúng ta bỏ ký tự “0x” ở đâu mỗi id và tách thành từng cập như của mình sẽ là 0x01660003 -> 01 66 00 03.
- Sau đó các bạn đảo ngược thứ tự các cập của mình sẽ là: 

03 00 66 01 

- Cuối cùng bạn tái cấu trúc nó lại thành id của mình sẽ là: 

03006601 

**B3:** Các bạn hãy thêm 1 dòng PciRoot(0x0)/Pci(0x2,0x0) trong DeviceProperties tiếp các bạn add dòng AAPL,ig-platform-id dưới định dạng là data với giá trị là là id vừa tạo như của mình là 03006601 như hình (do id này mình chỉ lấy làm demo nên sẽ không khớp với id trong hình mong các bạn thông cảm)

![](https://lh3.googleusercontent.com/lA4LQjawD-skc_JTsL3XIgZf8La2o6-TA-W0d92HQmGdffXK-qdqMKSMIGlaoFGvbC94sz64sSDd85uYOWexIAKRilVxIGhVWh90UBOqct1anBlmCzl67dLgXxmAhKjfU1-yo5o-=s0)

##### Clover

B1: Tải [Clover Configurator](https://mackie100projects.altervista.org/download-clover-configurator/).

B2: Mở đến tab Device ==> Properties

B3: Ấn vào dấu `+`

![](https://imgur.com/YToHwKG.png)

B4: Nhấn đúp vào tên của Properties vừa add và đổi thành `PciRoot(0x0)/Pci(0x2,0x0)`.

B5: Add các Framebuffer như ở OpenCore.

![](https://imgur.com/fOqet1r.png)

#### **Sửa Device Id**

- Truy cập trang [WhateverGreen/FAQ.IntelHD.en.md at master · acidanthera/WhateverGreen · GitHub](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md)  để lấy device id nó sẽ có dạng:  

![](https://lh5.googleusercontent.com/dVGDCnAJ6CMseO70t0ujH1zNFoJcY2CFDE6k9sboLvuAhsb2X_c4t1HYdKZ2DD-9srsVzCPB3PPzvntHNx_S3zrk4OrvSkKb0YlmAjC1FeFWmmLcYuOAAQw0hOUHsEu28KVuMZMj=s0)

- Đầu tiên ta xóa “0x” và biến dãy số đó thành 1 dãy có 8 chữ số bằng cách thêm cách số 0 vào đằng trước dãy số và tách id thành từng cặp của mình sẽ là:

 0x0152 -> 00 00 01 52

- Sau đó các bạn đảo ngược thứ tự từng cặp: 

 00 00 01 52 -> 52 01 00 00

- Cuối cùng bạn tái cấu trúc nó lại thành id của mình sẽ là:

52010000 

- Sau đó bạn add dòng device-id với định dạng là data vào dưới dòng AAPL, ig-platform-id với giá trị là id vừa sửa sau khi làm xong nó sẽ có dạng (do id chỉ là Demo nên id trong hình và id trên đây  sẽ không khớp) đối với Clover thì add vào đây:

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-10-at-19.20.08.png?w=1024)

![](https://lh4.googleusercontent.com/OPjIScFOMbjg9-p5CsLCWwX8Y4hSm0mOEw5y8RnU-asJe0Ka6hwN3p8KoqXjT650ltw27YGbWf0kXZiIU2WRX7Ca2QT9e9WhzHHtlx2-5ys681SUWqezNWVx-dBAuVesDq6hN5PK=s0)

add dưới dòng này

Tiếp theo Restart và tận hưởng thôi.

#### ***Các kiểm tra xem iGPU có hoạt động hay không:***

**B1**: Các bạn mở Hackintool (link tải ở trên)

**B2**: Các bạn mở tab System xuống phần VDA Decoder nếu nó hiện thị là fully supported như hình thì iGPU đã hoạt động.

![](https://lh4.googleusercontent.com/DpypfU7s_KJH0pws5KUdtYO9gN918uLVlSorH3tu4S3HDd6ScbsnqfNgc1lOUBWdYgrd191y7QBcozHXMOLjdQ11Au5h_CHV1lONAKZYzT1g_9P1ddkk6NXyah_WHTk3MDj7zpic=s0)

**Lưu ý: Cách ở trên mình chỉ hướng dẫn đối với các iGPU Gen 3 trở lên đối với những bạn Gen 3 trở xuống có thể tham khảo Cách 2 (hoặc đổi aapl-id thành AAPL,snb-platform-id)**

**Lưu ý 2: Các bạn lưu ý rằng trong quá trình patch iGPU mà bị đen màn (PC) thì các bạn Patch Busid trước nhé khả năng cao là máy bạn đã nhận iGPU rồi nhưng ko nhận Busid link hướng dẫn chi tiết [ở đây](../xiii-fix-connect-hdmi/index.html).**

### ***Cách 2: Sử dụng Hackintool để Patch iGPU:***

B1: Down Hackintool tại đây [Releases · headkaze/Hackintool · GitHub](https://github.com/headkaze/Hackintool/releases) 

B2: Mở sang Tab Patch (các bạn nhớ chỉnh mục frambuffer và patch đúng với phiên bản macOS hiện tại).

![](https://lh6.googleusercontent.com/FEWlsY6Ss6WEcee9_-llgq1QqdNyW5iIWw4Icet0PAmbEkenmIXawzBmYVk8z95phTSQJrvO38LgDo3POl2mX3lZzVCC2R8kgnch2zHLKfM_a_kujdH3X92ATTK0k3LnOzWb-NEp=s0)

B3: Các bạn chỉnh mục Intel Generation này lại thành Gen CPU của các bạn. 

![](https://lh3.googleusercontent.com/XMB1I0WkpQNsD7QcniKje2LdUV-wnFG3bUOtg0W2UIqt2FR3gDppWnwLF-szOB6E2NYjdiNoYqZePE7-E0hWLTT0VcJ8vQzGOkP6VVFJtlqzXaQ27M43On9-2jbd_OVlRQuXXje0=s0)

![](https://lh5.googleusercontent.com/U5RXUHBYumlFpw8RqKLx5sgTy88DwUT9Im75WnJqpd9CUjHCETzkGub2s71hPcqwDjeX1YnSLq9m_CXo5KfhDzwfmwRYTnJB-UX-rBZoVQKq08LezfeU9tBlCT0LmvezC2usKJia=s0)

B4: Mục PlatformID lại sao cho ở phần selected Framebuffer hiện đúng tên igpu của các bạn. 

B5: Mở qua mục Patch và chỉnh như hình: ![](https://lh6.googleusercontent.com/XpkHSy5PHErAfiIYKZ7fSsRZeSS-Xo7zcp-0fYn6_RSHx6GHUNzDlcWwE3XB4qey2ECSL7EUvsyoKcY8myXYg46H-QvXvN_WT_F90eIdk6cTsPjzOOlxrF9pBvL4HDrS5TrbqUJ-=s0)

B6: Mở tiếp qua tab Advanced và chỉnh như hình (Lưu ý : nếu các bạn tick mục Enable HDMI20 (4K) thì hãy thêm vào boot-arg “-cdfon” | Chỉ tick dòng GfxYTile Fix nếu cpu của bạn là Gen 6+) 

![](https://lh6.googleusercontent.com/sJes_HPbkUtopgpiHwujd2OBJVGOMGnH1ZF9VLevLdhuhZLA6tGhBW4MQXsPA-mU0a4W2LC1LcFP-KNPjKXkSJv1SAOWfU3gcijsciJx7gEdO4lISkF38u9i0nxDZNC2xePlyYC4=s0)

B7: Chọn dòng Generate Patch (sẽ được như hình).

![](https://lh6.googleusercontent.com/xI7W4qe9ZS1eAvgqgcSSJCRs8TukIYni26nRpD-n-LmFlmZWstV1ZPwyWBo0DP8Ha9bW0Wl9nuuPIT2bkDxXCByHhWJahHvHdTcbDHn4S0iTH3m_T4OPq-cP7ej7LLCksGiIrS2x=s0)

B8: Các bạn  Menu ⇒  File ⇒  Export ⇒  Bootloader config.plist

B9: Mở file config vừa tạo và copy mục Devices Properties qua file config trong EFI (lưu ý chỉ copy mục PciRoot(0x0)/Pci(0x2,0x0) nếu mục này của các bạn có những patch khác)

B10: Save lại và Reboot (và đây là thành quả)![](https://lh5.googleusercontent.com/hmINLk--jPXLwpdOKOYFdiLau5JvxHbsBfwoU1HpZduaAgxeQVDZwFPnf8F-MYaDD7MqKc1FGVHVq9fbAbPLD3RDvrYDRxae39En2wIoK9e50btSNProPtVy9lQ1CLTe-g5d7xtZ=s0)

**Lưu ý:  Đối với những bạn nào muốn VRAM nhận là 3gb ( mặc định sau khi làm xong sẽ là 2gb ) thì chỉnh lại mục framebuffer-unifiedmem này thành “000000C0”**

**Lưu ý 2: Quy luật nhận VRAM khi dùng framebuffer-unifiedmem là mặc định sẽ là “0000000” ta sẽ chú ý 2 chữ số cuối nếu là 10 ⇒  80 mỗi lần sẽ tăng 255MB (ở hàng số thì vault 80 là cao nhất) từ 80 ⇒ A0 ⇒ F0 mỗi lần tăng 255MB (ở hàng chữ thì F0 là cao nhất) tiếp sau khi full chữ kết cuối tức là “F0” thì các bạn đổi tiếp chứ số kế nó từ là F1 ⇒ FF sau khi full chữ cuối cùng ta sẽ đổi tiếp tới chữ số ở trên 2 chữ số vừa full tức 1FF ⇒ FFF như vậy nếu muốn tăng VRAM lên 4GB ta sẽ đổi như sau “00000FFF” (max là 4GB)**

**Lưu ý 3: Đối với các bạn Desktop khi bị tạch iGPU mà vẫn muốn vào macOS trước thì các bạn dán code sau vào boot-arg  “-igfxvesa”**

**Lưu ý 4: Code “-cdfon” chỉ khi nào các bạn đã patch thành công iGPU thì mới dùng nếu chưa thành công mà dùng sẽ có thể ko boot vào được hoặc mất độ sáng thì tự chịu nhé .**

**Lưu ý 5: Các bạn phải đổi SMBIOS cho đúng với model.**

**Lưu ý 6: nếu các bạn dùng iGPU cần fake device-id thì có thể dùng Hackintool để fake thành 1 dòng cụ thể như sau:**

- **Các bạn hãy chọn appl-id chính xác với máy của mình (các làm như ở trên nên phần này mình sẽ không nói kỹ)**.
- **Tiếp các bạn chọn đến Hackintool ⇒ Patch ⇒ Patch ⇒ Advanced (sẽ được như hình)**.

![](https://lh5.googleusercontent.com/TnVtBuMXB97KybXJPAIogSpeuUwsG3HpCTiSFrya3T69KCWrnlmZRTqBhJ9R4B4gSEkM2BAGx__MAlN6r3cM1-VPgHwwvgxQJWTlEWR6KktB83kBaLOX9XYSdKyvzgjHcJdxNYBb=s0)

- **Các bạn chú ý nhớ tick và mục Spoof Video Device ID .**
- **Sau đó ấn vào (sẽ được như hình)**.

![](https://lh5.googleusercontent.com/9BiyvHV7aIb0-joqshz66EgSf0W6S5igIrAqFT77pmeCvElDG9xsHop3haKLsARtlM-h22_HMJ1FU1h8pqyqleXQgfEiu46bUEPvZMZQbcwMtsSbAJ2yFkx4cRZL_8HQ95LkwaQG=s0)

- **Các bạn sẽ chọn iGPU muốn fake .**
- **Sau đó generate patch thôi**.
- **Sau khi xong các bạn chọn export để xuất ra file config (làm như ở trên)**.
- **Sau khi copy xong các bạn save lại và reboot**.

## **Cách tăng VRAM để patch iGPU cho 1 số BIOS không cho tăng VRAM:**

### **P1: Xác định giá trị của các Patch**

Các bạn vào trang này để lấy các thông tin cần thiết [whateverGreen](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md)  (Lưu ý là phải nhấn vào nút spoiler: Azul connectors | lấy phần thông tin có id khớp với id định patch igpu | ở đây mình lấy vd là igpu của Haswell desktop).

![](https://lh3.googleusercontent.com/IA1VcySu9BNtlwbJL9JWLww-lO-qcCBszvXy9Wks6kw00gM6MaWyrvQmPT8ZpEk8G9OfRlTI8SAM5BUAdsyvWhr7JUSZZqhAja4qLk3Li1v79TSEhGR4Id-yeOn5bq9RwjPr1_xo=s0)

 Chúng ta sẽ cần chú ý đến 2 dòng đầu. 

 Các mục chính các bạn cần quan tâm trong 2 dòng này là: 

![](https://lh5.googleusercontent.com/Nrx_04Oiv-kDSb7bZQIBS-rnArarHAuhAPcwRPV8EPqfDiHi3flBmtX3fGiF28rrpKM1wJ39hLqcSClgdIJaIDK1u8EQrkTp3K6Xhpul5Sasc9iVnV3UBUMPabEac_BOk_vPbCcD=s0)

Dựa vào bảng trên ta có thể hình dung được những thứ cần patch:

![](https://lh3.googleusercontent.com/m0TJ_i20IEZld35glYFKdoW8YG1XNTQWyMK5O42S_7wP6aD5Er7IOuext_3bgcPzhzpNuphiQ6PJlZyKXjD_ng2cELL2C1rtnUDjZ2IKGGGGl1JzQwq8u3a9Joxfz4ry3xEfcNfB=s0)

### ***P2: Tiến hành Patch***

B1:  Xác định STOLEN FBMEM :

- Các bạn cần chọn số MB cần cho STOLEN, FBMEM (Lưu ý là nhỏ hơn số MB tối đa như ở đây số MB tối đa của mình là 32 và 19) nên mình sẽ lấy là 19 và 9 (khuyến khích các dòng máy lấy data này)

B2:  Chuyển đổi MB sang Bytes. 

- Dán lệnh này vào Terminal 

echo ‘số mb cần chuyển  ** 1024 ** 1024′ (như của mình sẽ là echo ‘9 ** 1024 ** 1024′ | bc)![](https://lh3.googleusercontent.com/jaIzUfSGInQoH7MhtXBSUCgvs-7Q3IT8-_n4xVr4j_90NB8PrE54hXcDbZlo1sOyEJq1O0t-kcgXFLi8CJ-I9jQfobHSPnfPtBEBDv1tB_JhQ4L7XvlDexcyco4Zkk2U09_gJQl6=s0)

B3: Chuyển đổi từ thập phân sang thập lục phân:

- Các ban dán lệnh sau vào Terminal (hoặc dùng Hackintool).

 echo ‘obase=16; ibase=10; kết quả của B1’ | bc (như của mình sẽ 

echo ‘obase=16; ibase=10; 9437184’ | bc)

![](https://lh3.googleusercontent.com/K92nWEE5_6I3nLVKvbFMnfUgaHxjnrtxM6-pG0whyfdTnMV3TN_EMea9U4xAAXLuXMUvUDHmr_2yAvpcSXt_CAdB9ZMkEdyFzzsjj4dkU76nCGqRHZK71AA1RFPHseCyc9H7tmuj=s0)

![](https://lh4.googleusercontent.com/TX8Dx5pcY-8g057NinHt4i6CKxMnxAWPpPQv5l5riDysNx1hsY1MeB4I7HE9bKv85jPkEpe1zrfgAVhVAoayrRPxmAIgfXRxY4xPDhEhMp6DgTz0N0DeJva-9DngYaAlvCmsLkzk=s0)

B4: Các bạn tách thành từng cặp và đảo ngược thứ tự các cặp (Lưu ý ở đây là đảo ngược thứ tự các cập nên phải giữ đúng thứ tự các chữ số trong từng cập).

900000 -> 90 00 00 -> 00 00 90 

B5: Các bạn  viết thêm 2 chữ số 0 vào cuối dãy số vừa chuyển. 

00 00 90 -> 00 00 90 00 -> 00009000  

***Lưu ý: Làm tương tự với giá trị của stolenmem và đảm bảo giá trị cuối cùng gồm 8 chữ số (như Patch Stolenmem thì chỉ thêm 1 số 0 ở đằng sau cùng để đảm bảo giá trị cuối cùng nhận được là 8 chữ số và lưu ý là tách từ dưới lên trên và làm đúng quy trình)***.

### ***P3: Áp dụng các bản vá lỗi vào config***

B1: Xác định các Patch đúng với các giá trị:

<img src="https://lh4.googleusercontent.com/jFnw5BwHpfw3tsSPDszFV4jfDnLzPTRyEFGI8jOA0zS7a50OiQsGtiXfXBuOZh1XEavS9yhultX-Fv6vxcgCdWns30jklka2z4_jjyO0fuG6nBiF79Tom9oymshrKq12SrZRJYbN=s0" title="" alt="" data-align="inline">

Sau khi làm xong các phần trên ta sẽ có được các giá trị ở các patch sau (giá trị ở framebuffer-patch-enable đơn giản chỉ là để enable các patch mà thôi).

B2: Add các patch vào config theo đường dẫn sau DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) (đối với clover thì các bạn cũng add tương tự nhưng ở device ==> properties ==> PciRoot(0x0)/Pci(0x2,0x0 )

## 1 số properties thường dùng và công dụng

1. hda-gfx: patch audio ( thường là hdmi audio )
2. framebuffer-conx-type : dùng để set type cho port  ( thường là hdmi, DP )
3. framebuffer-patch enable: dùng để enable các patch framebuffer
4. Appl, ig-platform-id: dùng để inject frambuffer
5. device-id: dùng để inject device-id
6. model: set model cho device
7. framebuffer-stolenmem: để tăng vram
8. framebuffer-fbmem: để tăng vram
9. framebuffer-unifiedmem: dùng để tăng vram ( không khuyến khích )
10. framebuffer-flags: dùng để inject flag cho framebuffer
11. framebuffer-conX-enable: enable port framebuffer ( dùng để patch connect )
12. framebuffer-conX-index: inject index cho frambuffer ( dùng để patch connect )
13. framebuffer-conX-busid: inject busid cho frambuffer ( dùng để patch connect )
14. framebuffer-conX-pipe: inject pie cho frambuffer ( dùng để patch connect )
15. framebuffer-conX-type: inject type cho framebuffer ( dùng để patch connect )
16. framebuffer-conX-flags: inject flag cho frambuffer ( dùng để patch connect )
17. framebuffer-conX-alldata: inject data cho frambuffer bằng method all data ( dùng để patch connect )
18. AAPL00,override-no-connect: để inject edid
19. enable-hdmi20: để add thuộc tính hdmi cho igpu
20. disable-external-gpu: disable vga rời
21. enable-lspcon-support: enable LSPCON adapter cho igpu
22. framebuffer-conX-has-lspcon: inject frambuffer cho LSPCON adapter
23. framebuffer-conX-preferred-lspcon-mode: inject model cho LSPCON adapter
24. AAPL01,DualLink: inject duallink chi igpu
25. AAPL00,DualLink: inject duallink chi igpu

Dưới đây là 1 số properties thường dùng mà mình tổng hợp đc để hiểu thêm chi tiết các bạn có thể đọc thêm bài [patch busid](../../../2021/09/29/xiii-fix-connect-hdmi/index.html) và [patch connect type, inject edid](../../../2021/09/29/xiv-patch-connect-type-force-rgb-injects-edid/index.html)

## Một số lưu ý đặc biệt cho HD3000:

Lưu ý : 1 số dòng HD3000 khi boot sẽ gặp tình trang sau:

![](https://imgur.com/I4IMC3w.png)

Cách fix: các bạn add đoạn code sau vào DeviceProperties -> Add -> PciRoot(0x0)/Pci(0x2,0x0) -> AAPL00,DualLink =00000000 (hoặc bằng 01000000 nếu màn hình của bạn có độ phân giải lớn hơn 1366×768)

Lưu ý 2: Đối với những bạn dùng HD3000 bị lag hoặc đứng có thể thêm 2 kext sau [AppleIntelHD3000Graphics.kext](https://github.com/ipang-dwi/ihd3000/releases/tag/v2gb) và [AppleIntelSNBGraphicsFB.kext](https://github.com/ipang-dwi/ihd3000/releases/tag/v2gb) Bỏ vào S/L/E bằng [kext droplet](https://github.com/chris1111/Kext-Droplet)

**Source tham khảo: [WhateverGreen/FAQ.IntelHD.en.md at master · acidanthera/WhateverGreen (github.com)](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md?fbclid=IwAR1dvRq8GHggWnRt36_FPYJUOlE2J3mCwGsGVNhFdEzMYWhGUEtPzFgXMBk) | [[GUIDE] General Framebuffer Patching Guide (HDMI Black Screen Problem) | tonymacx86.com](https://www.tonymacx86.com/threads/guide-general-framebuffer-patching-guide-hdmi-black-screen-problem.269149/) | [Intel iGPU Patching | OpenCore Post-Install (dortania.github.io)](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/intel-patching/)** | [***Patching VRAM | OpenCore Post-Install (dortania.github.io)***](https://dortania.github.io/OpenCore-Post-Install/gpu-patching/intel-patching/vram.html#creating-our-patch)
