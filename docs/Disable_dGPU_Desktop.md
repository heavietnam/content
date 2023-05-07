# Disable dGPU Desktop

> Phần này dành cho các máy có Card rời không hỗ trợ khi Fix Sleep bắt buộc các bạn phải Disable Card rời đi (những dòng không hỗ trợ).

## Cách 1: Disable DGPU bằng boot flag

B1: mở config lên bằng proertree

B2: ấn `command + f` search `boot-arg`

B3: Thêm đoạn code sau vào boot-arg “`-wegnoegpu`” 

B4: reboot

> Đối với clover mục boot-arg tương ứng với mục `boot --> Arguments`

## **Cách 2: Disable Dgpu bằng device properties**

### OpenCore

B1: Tải [gfxutil](https://github.com/acidanthera/gfxutil/releases)

B2: Gõ lệnh sau vào Terminal 

- Kéo gfxutil vào Terminal nhận code sau` -f GFX0` (như hình).

![](https://lh5.googleusercontent.com/6tUCYgfFlIzsP2sR0ET-S0Fxv49LVbta9wpXpmOVx-GoxJ5bEzC0K7cU0rSstmWZz10oK15y8sYkLXcf99SblF9JljCOBspbi2yWx4YQ6z3K59HLgDq-MG-gtNqRUo1ZbEuXDEeQ=s0) 

(do ở đây mình không có card rời | nhưng vd giá trị dup ra sẽ là DevicePath = `PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)`)

B3: Thêm dòng vừa dump vào config như sau: 

`PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0) | dictionary`  

B4: Thêm các dòng sau dưới dòng vừa add: 

| Name       | data   | 23646973706C6179 |
| ---------- | ------ | ---------------- |
| IOName     | string | #display         |
| class-code | data   | FFFFFFFF         |

![](https://lh5.googleusercontent.com/pbACDJWPgu-fxjybli620i-RJNblggMHTL9BS-Wbq2oD6uCOLy-YgLM8FxklZoh7TsyLMJ8hNoVDdfKzhTMVatdBD_uSsy_WEC_VK1lzbsQ7sDLUzSO5mdJUUUk-uexOqCNS0FVh=s0)

B4: Save lại và Reboot.

### Clover

Đối với Clover các bạn làm tương tự nhưng thay vì Add vào DeviceProperties thì các bạn add vào Device ==> Properties

![](https://everythingforhackintosher.files.wordpress.com/2021/09/cleanshot-2021-09-11-at-13.49.37.png?w=1024)

## ***Cách 2: Biên dịch SSDT***

B1: Tải [SSDT-GPU-DISABLE](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/decompiled/SSDT-GPU-DISABLE.dsl.zip)

B2: boot vào windown hoặc win pe

B3: mở device manager và tìm đến đường dẫn sau

```
Device Manager -> Display Adapters -> dGPU -> Properties -> Details > Location Paths
```

![](https://dortania.github.io/Getting-Started-With-ACPI/assets/img/amd.acf5492b.png)

Xem cách lấy ACPI path từ location path theo hướng dẫn [tại đây](https://heavietnam.ga/2022/04/29/cach-xac-dinh-phan-cung/#Xac_dinh_location_path). Ta sẽ có được ACPI path là

```
\_SB_.PC02.BR2A.PEGP
```

B4: Sửa ACPI path của SSDT thành ACPI path vừa xác định ở trên cụ thể những phần cần phải sửa là

```
External (_SB_.PCI0.PEG0.PEGP, DeviceObj)
Method (_SB.PCI0.PEG0.PEGP._DSM, 4, NotSerialized)
```

![](https://i.imgur.com/kVZjEvp.png)

Ở ví dụ này ta sẽ tiến hành thay

- `PCI0` thành `PC02`
- `PEG0` thành `BR2A`

B5: biên dịch SSDT thành file aml theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)

B6: Bỏ SSDT vào `EFI –> OC –> ACPI` (hoặc `EFI –> Clover –> ACPI –> patched`| snapshot nếu là opencore)

**Lưu ý: Main source [Disabling GPU | OpenCore Install Guide (dortania.github.io)](https://dortania.github.io/OpenCore-Install-Guide/extras/spoof.html#windows-gpu-selection) | [Disabling desktops unsupported GPUs(SSDT-GPU-DISABLE) | Getting Started With ACPI (dortania.github.io)](https://dortania.github.io/Getting-Started-With-ACPI/Desktops/desktop-disable.html#finding-the-acpi-path-of-the-gpu)**

**Lưu ý 2: Do máy mình không có dGPU nên các hình có thể không khớp nhau mong các bạn thông cảm (đã test máy khác và thành công)**.

**Lưu ý 3: Nếu các bạn patch iGPU bằng Hackintool thì các bạn không cần làm phần này vì Hackintool đã Disable Card rời rồi.**
