# SSDT recomend

Đối với hầu hết các model chỉ cần [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-DESKTOP.aml) là đã có thể boot lên. Tuy nhiên để sử được đầy đủ chức năng thì các bạn sẽ cần add đầy đủ các SSDT. Sau đây là 1 sô ssdt khuyến khích nên có:

## Desktop

### Penryn/Lynnfield/Clarkdale

- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### SandyBridge/Ivy Bridge

- [CPU-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/)
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### Haswell/Broadwell

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### Skylake/Kaby Lake

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)

### Coffee Lake

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
- [SSDT-AWAC](https://heavietnam.ga/2021/09/29/xxix-dung-ssdt-time-de-prebuilt-ssdt/) ( dùng ssdt-time để prebuilt ssdt )
- [SSDT-PMC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PMC.aml)

### Comet Lake

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
- [SSDT-AWAC](https://heavietnam.ga/2021/09/29/xxix-dung-ssdt-time-de-prebuilt-ssdt/) ( dùng ssdt-time để prebuilt ssdt )
- [SSDT-RHUB](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-RHUB.aml)

### AMD (17/19h)

- [SSDT-CPUR](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-CPUR.aml)

## High End Desktop

### Nehalem and Westmere

- [](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)[SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)

### Sandy Bridge-E/Ivy Bridge-E

- [](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)[SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-DESKTOP.aml)
- [SSDT-UNC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-UNC.aml)

### Haswell-E/Broadwell-E

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
- [SSDT-RTC0-RANGE](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-RTC0-RANGE-HEDT.aml)
- [SSDT-UNC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-UNC.aml)

### Skylake-X

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-DESKTOP.aml)
- [SSDT-RTC0-RANGE](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-RTC0-RANGE-HEDT.aml)

## Laptop

### Clarksfield and Arrandale

- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-LAPTOP.aml)
- [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml)
- [SSDT-IRQ](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/)

### SandyBridge/Ivy Bridge

- [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/)
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-LAPTOP.aml)
- [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml)
- [SSDT-IRQ](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/)
- [SSDT-IMEI](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-IMEI.aml)

### Haswell/Broadwell

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-LAPTOP.aml)
- [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml)
- [SSDT-IRQ](https://heavietnam.ga/2021/09/29/ii-patch-am-thanh-voi-apple-alc-with-patch-hpet/)
- [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://heavietnam.ga/2021/09/29/v-fix-trackpad/) )

### Skylake/Kaby Lake

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-LAPTOP.aml)
- [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml)
- [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://heavietnam.ga/2021/09/29/v-fix-trackpad/) )

### Coffee Lake/Whiskey Lake/Comet Lake

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-LAPTOP.aml)
- [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml)
- [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://heavietnam.ga/2021/09/29/v-fix-trackpad/) )
- [SSDT-AWAC](https://heavietnam.ga/2021/09/29/xxix-dung-ssdt-time-de-prebuilt-ssdt/) ( dùng ssdt-time để prebuilt ssdt )
- [SSDT-PMC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PMC.aml) ( chỉ sử dụng cho coffelake gen 9 )

### Ice Lake

- [SSDT-PLUG](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PLUG-DRTNIA.aml) ( hoặc [SSDT-PM](https://heavietnam.ga/2021/09/29/vi-3fix-power-manager/) )
- [SSDT-EC](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-EC-USBX-LAPTOP.aml)
- [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml)
- [SSDT-XOSI](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-XOSI.aml) ( hoặc [SSDT-GPI0](https://heavietnam.ga/2021/09/29/v-fix-trackpad/) )
- [SSDT-AWAC](https://heavietnam.ga/2021/09/29/xxix-dung-ssdt-time-de-prebuilt-ssdt/) ( dùng ssdt-time để prebuilt ssdt )
- [SSDT-RHUB](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-RHUB.aml)

**Lưu ý: đối với ssdt-awac các bạn có thay thế bằng cách sử dụng [fix rtc](https://heavietnam.ga/2021/10/14/fix-rtc/)**

**Main source: [Gathering files | OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#ssdts)**
