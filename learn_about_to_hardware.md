# Tìm hiểu chung về phần cứng

## CPU

- Các CPU AMD Bulldozer (15h), Jaguar (16h) and Ryzen (17h) CPUs chỉ được hỗ trợ cho desktop, laptop ko đc hỗ trợ.

- Mọi CPU Intel từ đời Yonah đều được hỗ trợ.

- CPU 32bit được hỗ trợ từ 10.4.1 --> 10.6.8

- CPU 64bit được hỗ trợ từ 10.4.1+

- SEE hỗ trợ:
  
  - SSE3 : hỗ trợ tất cả các version OS X/macOS.
  - SSSE3: hỗ trợ tất cả các version 64 bit OS X/macOS.
  - SSE4: hỗ trợ 10.12+
  - SSE4.2: hỗ trợ 10.14+

- Firmware hỗ trợ:
  
  - 10.4.1-10.4.7 yêu cầu EFI32 (IA32)
  - 10.4.8-10.7.5 hỗ trợ cả 32 bit lẫn 64 bit.
  - 10.8+ yêu cầu EFI64 (X64).
  - 10.7-10.9 yêu cầu OpenPartitionDxe.efi để có thể boot được vào phần vùng Recovery.

- Yêu cầu của kernel:
  
  - 10.4-10.5 yêu cầu kext 32-bit do chỉ support 32-bit kernelspace.
  - 10.6-10.7 support cả 32-bit và 64-bit.
  - 10.8+ yêu cầu kext 64-bit do chỉ support 64-bit kernelspace.

- Nhân và luồng:
  
  - Từ 10.10 có thể không boot được với 24 luồng nó sẽ gặp lỗi `mp_cpus_call_wait() timeout` panic.
  - Từ 10.11+ bị giới hạn 64 luồng.
  - cpus=1 có thể là giải pháp giúp disable hyperthreading.

- Lưu ý:
  
  - Lilu yêu cầu version 10.8+ (đối với OS X thì nên dùng FakeSMC)
  - Đối với version 10.6 và cũ hơn sẽ yêu cầu RebuildAppleMemoryMap.

- Bảng tóm tắt các version hỗ trợ cho các đời CPU và CPUID Intel (bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support) | thank you)

| CPU-GEN                        | MIN VERSION               | MAX VERSION                   | GHI CHÚ               | CPUID       |
| ------------------------------ | ------------------------- | ----------------------------- | --------------------- | ----------- |
| Pentium 4                      | 10.4.1                    | 10.5.8                        | Only used in dev kits | 0x0F41      |
| Yonah                          | 10.4.4                    | 10.6.8                        | 32-Bit                | 0x0006E6    |
| Conroe, Merom                  | 10.4.7                    | 10.11.6                       | No SSE4               | 0x0006F2    |
| Penryn                         | 10.4.10                   | 10.13.6                       | No SSE4.2             | 0x010676    |
| Nehalem                        | 10.5.6                    | Current                       | N/A                   | 0x0106A2    |
| Lynnfield                      | 10.6.3                    | No iGPU support 10.14+        | 0x0106E0              |             |
| Westmere, Clarkdale, Arrandale | 10.6.7                    | 0x0206A0(M/H)                 |                       |             |
| Sandybirdge                    | 10.6.7                    | 0x0206A0(M/H)                 |                       |             |
| Ivy Bridge                     | 10.7.3                    | No iGPU support 11+           | 0x0306A0(M/H/G)       |             |
| Ivy Bridge-E5                  | 10.9.2                    | N/A                           | 0x0306E0              |             |
| Haswell                        | 10.8.5                    | 0x0306C0(S)                   |                       |             |
| Broadwell                      | 10.10.0                   | 0x0306D4(U/Y)                 |                       |             |
| Skylake                        | 10.11.0                   | 0x0506e3(H/S) 0x0406E3(U/Y)   |                       |             |
| kaby Lake                      | 10.12.4                   | 0x0906E9(H/S/G) 0x0806E9(U/Y) |                       |             |
| coffee Lake                    | 10.12.6                   | 0x0906EA(S/H/E) 0x0806EA(U)   |                       |             |
| Amber Whiskey Comet Lake       | 10.14.1                   | 0x0806E0(U/Y)                 |                       |             |
| Comet Lake                     | 10.15.4                   | 0x0906E0(S/H)                 |                       |             |
| Ice Lake                       | 0x0706E5(U)               |                               |                       |             |
| Rocket Lake                    | Requires Comet Lake CPUID | 0x0A0671                      |                       |             |
| Tiger Lake                     | N/A                       | N/A                           | Untested              | 0x0806C0(U) |

- Nhiều tính năng trên macos hoàn toàn ko hoạt động và 1 số bị lỗi trên CPU AMD bao gồm:
  - Ảo hóa dựa trên Apple HV (VMWare, Parallels, Docker, Android Studio,….) bị vô hiệu hóa VirtualBox được hỗ trợ.
  - Các phần mềm Adobe.
  - Phần mềm 32-bit.
  - Và 1 vài app audio.

## GPU

Các GPU AMD nhân GCN được hỗ trợ trên các phiên bản mới nhất.

- Các APU AMD không được hỗ trợ.

- Các GPU AMD nhân Lexa không được hỗ trợ chính thức.

- GeForce 900 series (Maxwell 9XX), GeForce 10 series (Pascal 10XX) được hỗ trợ giới hạn đến macOS 10.13 (High Sierra)

- GeForce 20 series, GeForce 16 series không được hỗ trợ.

- GeForce 30 series cũng không được hỗ trợ.

- GeForce 600 series, GeForce 700 series (Kepler) vẫn đang được hỗ trợ hiện tại là Monterey beta 9 (từ Monterey phải dùng tool Geforce-Kepler-patcher để patch không native, xem hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxiipatch-card-do-hoa-nvidia/))

- iGPU GT2 được hỗ trợ.

- GMA series được hỗ trợ 1 số dòng, xem chi tiết [tại đây](https://heavietnam.ga/2021/10/22/patch-gma-gpu/).

- iGPU GT1 trên pentium, Celerons và Atoms không được hỗ trợ.

- Bảng tóm tắt các version hỗ trợ cho các đời iGPU Intel (bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support) | thank you)

| GPU                    | MIN VERSION | MAX VERSION                                      | GHI CHÚ                                                  |
| ---------------------- | ----------- | ------------------------------------------------ | -------------------------------------------------------- |
| 3rd Gen GMA            | 10.4.1      | 10.7.5                                           | Requires 32-bit kernel and patches                       |
| 4th Gen GMA            | 10.5.0      |                                                  |                                                          |
| Arrandale(HD Graphics) | 10.6.4      | 10.13.6                                          | Only LVDS is supported, eDP and external outputs are not |
| Sandy Bridge(HD 3000)  | 10.6.7      | N/A                                              |                                                          |
| Ivy Bridge(HD 4000)    | 10.7.3      | 11.6.1                                           |                                                          |
| Haswell(HD 4XXX, 5XXX) | 10.8.5      | Current                                          |                                                          |
| Broadwell(5XXX, 6XXX)  | 10.10.0     |                                                  |                                                          |
| Skylake(HD 5XX)        | 10.11.0     |                                                  |                                                          |
| Kaby Lake(HD 6XX)      | 10.12.4     |                                                  |                                                          |
| Coffee Lake(UHD 6XX)   | 10.13.6     |                                                  |                                                          |
| Comet Lake(UHD 6XX)    | 10.15.4     |                                                  |                                                          |
| Ice Lake(Gx)           | 10.15.4     | Requires `-igfxcdc` and `-igfxdvmt` in boot-args |                                                          |
| iger Lake(Xe)          | N/A         | N/A                                              | No drivers available                                     |
| Rocket Lake            | N/A         | N/A                                              | No drivers available                                     |

- Bảng tóm tắt các phiên bản macOS hỗ trợ cho các đời GPU AMD (bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support) | thank you)

| GPU           | MIN VERSION | MAX VERSION                             | GHI CHÚ               |
| ------------- | ----------- | --------------------------------------- | --------------------- |
| X800          | 10.3.x      | 10.7.5                                  | yêu cầu kernel 32 bit |
| X1000         |             |                                         |                       |
| TeraScale     | 10.4.x      | 10.13.6                                 |                       |
| TeraScale 2/3 | 10.6.x      |                                         |                       |
| GCN 1         | 10.8.3      | Current                                 |                       |
| GCN 2/3       | 10.10.x     |                                         |                       |
| Polaris 10,20 | 10.12.1     |                                         |                       |
| Vega 10       | 10.12.6     |                                         |                       |
| Vega 20       | 10.14.5     |                                         |                       |
| Navi 10       | 10.15.1     | yêu cầu `agdpmod=pikera` trong boot-arg |                       |
| Navi 20       | 11.4        | Hiện tại 1 vài navi 21 vẫn hoạt động    |                       |

- Bảng tóm tắt các version hỗ trợ cho các đời dGPU Intel (bảng này dựa trên [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support) | thank you)

| GPU       | MIN VERSION | MAX VERSION                                                                       | GHI CHÚ                                                                                                   |
| --------- | ----------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| GeForce 6 | 10.2.x      | 10.7.5                                                                            | yêu cầu kernel 32 bit và [NVCAP patching](https://heavietnam.ga/2021/09/29/xiii-fix-connect-hdmi/)        |
| GeForce 7 | 10.4.x      | [yêu cầu NVCAP patching](https://heavietnam.ga/2021/09/29/xiii-fix-connect-hdmi/) |                                                                                                           |
| Tesla     | 10.4.x      | 10.13.6                                                                           |                                                                                                           |
| Tesla v2  | 10.5.x      |                                                                                   |                                                                                                           |
| Fermi     | 10.7.x      |                                                                                   |                                                                                                           |
| Kepler    | 10.7.x      | current                                                                           | [macos 12 yêu cầu Geforce-Kepler-patcher](https://heavietnam.ga/2021/09/29/xxiipatch-card-do-hoa-nvidia/) |
| Kepler v2 | 10.8.x      |                                                                                   |                                                                                                           |
| Maxwell   | 10.10.x     | 10.13.6                                                                           | [yêu cầu NVIDIA Web Drivers](https://heavietnam.ga/2021/09/29/xxiipatch-card-do-hoa-nvidia/)              |
| Pascal    | 10.12.4     |                                                                                   |                                                                                                           |
| Turing    | N/A         | N/A                                                                               | No drivers available                                                                                      |
| Ampere    |             |                                                                                   |                                                                                                           |

## Bo mạch chủ (Motherboard)

Hầu hết các bo mạch chủ đều được hỗ trợ miễn là CPU được hồ trợ. Hồi trước các main B550 gặp 1 vài vấn đề, tuy nhiên hiện nay các bạn chỉ cần thêm [SSDT-CPUR](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-CPUR.aml) và SetupVirtualMap=false.

## Ổ cứng

Phần lớn các ổ cứng đều được hỗ trợ ngoại trừ một số dòng sau đây:

- Các ổ Samsung PM981, PM991 and Micron 2200S NVMe SSDs hỗ trợ không tốt do đó bạn cần [NVMeFix.kext](https://github.com/acidanthera/NVMeFix/releases) trong EFI ==> OC ==> Kext và snaps hoặc EFI ==> Clover ==> Kext ==> other

- Tuy nhiên ổ Samsung 970 EVO Plus NVMe SSDs trước đó gặp 1 vài vấn đề nhưng ở bản FIRMWARE mới nhất đã được fix các bạn có thể tải [ở đây](https://www.samsung.com/semiconductor/minisite/ssd/download/tools/).

- Intel 600p hỗ trợ không tốt nó có rất nhiều bug (khuyến khích nên tránh | cần [NVMeFix](https://github.com/acidanthera/nvmefix) để khởi động).

## NIC (Card mạng LAN)

Hầu hết đều được hỗ trợ tốt (xem bài patch LAN [ở đây](https://heavietnam.ga/2021/09/29/viii-fix-ethernet/))

Tuy nhiên 1 số card vẫn gặp lỗi, ví dụ:

- Intel I225 2.5Gb NIC trên các dòng Desktop cao cấp Comet Lake các bạn có thể tham khảo cách fix [tại đây](https://www.hackintosh-forum.de/forum/thread/48568-i9-10900k-gigabyte-z490-vision-d-er-l%C3%A4uft/?postID=606059#post606059).
- Intel I350 1Gb server NIC được tìm thấy trên các dòng intel và Supermicro server và đây là cách fix các bạn add mục:

```
PciRoot(0x0)/Pci(0x1,0x1)/Pci(0x0,0x0) | dictionray -> device-id | data | 33150000
```

Đôi khi các bạn sẽ gặp kernel panic xem cách fix chi tiết [tại đây](https://github.com/dortania/bugtracker/issues/213).

- Intel 10Gb server NICs cách fix cho các dòng X520 and X540 chipsets [tại đây](https://www.tonymacx86.com/threads/how-to-build-your-own-imac-pro-successful-build-extended-guide.229353/).

## WiFi và Bluetooth

Hầu hết đều được hỗ trợ bao gồm các card:

- Intel
- Broadcom
- Atheros

Xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/vii-fix-wifi-va-bluetooth/).

**Source tham khảo: [Hardware Limitations | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#miscellaneous)**
