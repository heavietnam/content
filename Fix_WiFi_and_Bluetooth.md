# Fix WiFi và Bluetooth

## ***Wifi và Bluetooth các dòng arXXX:***

**B1**: Đối với Mojave, Catalina và Big Sur (10.14, 10.15, 11), tải xuống kext `AirPortATH40` và` HS80211Family` và `Ath3kBT` theo link: [Kexts](https://drive.google.com/drive/folders/1BVXdIqBJ2I1vYW0rUc_-40mXDcMiVSuo?usp=sharing) (chỉ lấy `HS80211Family.kext`)

Tải các kext sau:

- [AR9565](https://www.insanelymac.com/applications/core/interface/file/attachment.php?id=358450)
- [AR9462](https://www.insanelymac.com/applications/core/interface/file/attachment.php?id=358451)
- [AR9463](https://www.insanelymac.com/applications/core/interface/file/attachment.php?id=358452)
- [AR9485](https://www.insanelymac.com/applications/core/interface/file/attachment.php?id=358453)

Và bỏ vào EFI, snapshot config và save lại.

Đối với Catalina trở xuống, tải các kext sau:

- [AR 9462](https://www.mediafire.com/file/u5j19uuavpxzn6u/AR9462.zip/file)
- [AR9463](https://www.mediafire.com/file/s0i1tasojzn83ur/AR9463.zip/file)
- [AR9485](https://www.mediafire.com/file/4umyyowc1bpnn70/AR9485%25281%2529.zip/file)
- [AR9565](https://www.mediafire.com/file/izsscv0z9f1c90e/AR9565.zip/file)

Và bỏ vào `S/L/E` bằng [kext droplet](https://github.com/chris1111/Kext-Droplet)

Để fix Bluetooth, tải kext sau về:

- [Big Sur (11)](https://github.com/zxystd/AthBluetoothFirmware/) (bỏ vào `EFI`)
- [Catalina (10.15)](https://www.mediafire.com/file/ltd2y9v3v2co3js/IOath3kfrmwr.kext.zip/file) (bỏ vào `S/L/E`)

**B2:** Add vào boot-arg dòng sau `-athXXX` (thay `XXX` thành mã của card wifi vd như card mình là `AR9485` mình sẽ add là `-ath9485` )

## ***Fix WiFi, Bluetooth cho các card Intel:***

### Các dòng card được hỗ trợ:

- 1000 Series
  
  - Intel(R) Centrino(R) Wireless-N 1000 BGN
  - Intel(R) Centrino(R) Wireless-N 1000 BG
  - Intel(R) Centrino(R) Wireless-N 100 BGN
  - Intel(R) Centrino(R) Wireless-N 100 BG

- 2000 Series
  
  - Intel(R) Centrino(R) Wireless-N 2200 BGN
  - Intel(R) Centrino(R) Wireless-N 2200D BGN
  - Intel(R) Centrino(R) Wireless-N 2230 BGN
  - Intel(R) Centrino(R) Wireless-N 105 BGN
  - Intel(R) Centrino(R) Wireless-N 105D BGN
  - Intel(R) Centrino(R) Wireless-N 135 BGN

- 5000 Series
  
  - Intel(R) Ultimate N WiFi Link 5300 AGN
  - Intel(R) WiFi Link 5100 BGN
  - Intel(R) WiFi Link 5100 ABG
  - Intel(R) WiFi Link 5100 AGN
  - Intel(R) WiMAX/WiFi Link 5350 AGN
  - Intel(R) WiMAX/WiFi Link 5150 AGN
  - Intel(R) WiMAX/WiFi Link 5150 ABG

- 6000 Series
  
  - Intel(R) Centrino(R) Advanced-N 6205 AGN
  - Intel(R) Centrino(R) Advanced-N 6205 ABG
  - Intel(R) Centrino(R) Advanced-N 6205 BG
  - Intel(R) Centrino(R) Advanced-N 6205S AGN
  - Intel(R) Centrino(R) Advanced-N 6205D AGN
  - Intel(R) Centrino(R) Advanced-N 6206 AGN
  - Intel(R) Centrino(R) Advanced-N 6207 AGN
  - Intel(R) Centrino(R) Advanced-N 6230 AGN
  - Intel(R) Centrino(R) Advanced-N 6230 ABG
  - Intel(R) Centrino(R) Advanced-N 6230 BGN
  - Intel(R) Centrino(R) Advanced-N 6230 BG
  - Intel(R) Centrino(R) Advanced-N 6235 AGN
  - Intel(R) Centrino(R) Ultimate-N 6235 AGN
  - Intel(R) Centrino(R) Wireless-N 1030 BGN
  - Intel(R) Centrino(R) Wireless-N 1030 BG
  - Intel(R) Centrino(R) Wireless-N 130 BGN
  - Intel(R) Centrino(R) Wireless-N 130 BG
  - Intel(R) Centrino(R) Advanced-N 6200 AGN
  - Intel(R) Centrino(R) Advanced-N 6200 ABG
  - Intel(R) Centrino(R) Advanced-N 6200 BG
  - Intel(R) Centrino(R) Advanced-N + WiMAX 6250 AGN
  - Intel(R) Centrino(R) Advanced-N + WiMAX 6250 ABG
  - Intel(R) Centrino(R) Wireless-N + WiMAX 6150 BGN
  - Intel(R) Centrino(R) Wireless-N + WiMAX 6150 BG
  - Intel(R) Centrino(R) Ultimate-N 6300 AGN

- 7000 Series
  
  - Intel(R) Dual Band Wireless AC 7260
  - Intel(R) Dual Band Wireless N 7260
  - Intel(R) Wireless N 7260
  - Intel(R) Dual Band Wireless AC 3160
  - Intel(R) Dual Band Wireless N 3160
  - Intel(R) Wireless N 3160
  - Intel(R) Dual Band Wireless AC 3165
  - Intel(R) Dual Band Wireless AC 3168
  - Intel(R) Dual Band Wireless AC 7265
  - Intel(R) Dual Band Wireless N 7265
  - Intel(R) Wireless N 7265
  - Intel(R) Dual Band Wireless AC 7265
  - Intel(R) Dual Band Wireless N 7265
  - Intel(R) Wireless N 7265

- 8000 Series
  
  - Intel(R) Dual Band Wireless N 8260
  - Intel(R) Dual Band Wireless AC 8260
  - Intel(R) Dual Band Wireless AC 8265
  - Intel(R) Dual Band Wireless AC 8275
  - Intel(R) Dual Band Wireless AC 4165

- 9000 Series
  
  - Intel(R) Wireless-AC 9162
  - Intel(R) Wireless-AC 9260
  - Intel(R) Wireless-AC 9260-1
  - Intel(R) Wireless-AC 9270
  - Intel(R) Wireless-AC 9461
  - Intel(R) Wireless-AC 9462
  - Intel(R) Wireless-AC 9560

- 22000 Series
  
  - Intel(R) Wireless-AC 9162 160MHz
  
  - Intel(R) Wireless-AC 9260 160MHz
  
  - Intel(R) Wireless-AC 9270 160MHz
  
  - Intel(R) Wireless-AC 9461 160MHz
  
  - Intel(R) Wireless-AC 9462 160MHz
  
  - Intel(R) Wireless-AC 9560 160MHz
  
  - Killer (R) Wireless-AC 1550 Wireless Network Adapter (9260NGW)
  
  - Killer (R) Wireless-AC 1550i Wireless Network Adapter (9560NGW)
  
  - Killer (R) Wireless-AC 1550s Wireless Network Adapter (9560NGW)
  
  - Intel(R) Wi-Fi 6 AX101
  
  - Intel(R) Wi-Fi 6 AX200 160MHz
  
  - Intel(R) Wi-Fi 6 AX201 160MHz
  
  - Intel(R) Wi-Fi 6 AX211 160MHz
  
  - Intel(R) Wi-Fi 6 AX411 160MHz
  
  - Intel(R) Wi-Fi 6
  
  - Killer(R) Wi-Fi 6 AX1650w 160MHz Wireless Network Adapter (200D2W)
  
  - Killer(R) Wi-Fi 6 AX1650x 160MHz Wireless Network Adapter (200NGW)
  
  - Killer(R) Wi-Fi 6 AX1650s 160MHz Wireless Network Adapter (201D2W)
  
  - Killer(R) Wi-Fi 6 AX1650i 160MHz Wireless Network Adapter (201NGW)

### Các dòng card không được hỗ trợ:

- 22000 Series
  - Intel(R) Wireless-AC 9560 160MHz
  - Intel(R) Wi-Fi 6 AX210 160MHz

### Cài đặt:

#### Cách 1: dùng ItIwm + HeliPort:

B1: Tải ItIwm [tại đây](https://github.com/OpenIntelWireless/itlwm/releases)

B2: Thêm kext vào `EFI ==> OC ==> Kexts` và snapshot config (nếu dùng OpenCore) hoặc bỏ kext vào `EFI ==> Clover ==> Kext ==> Other` ( nếu dùng Clover )

B3: Reboot

B4: Tải HeliPort [tại đây](https://github.com/OpenIntelWireless/HeliPort/releases)

B5: Nhấn giữ phím `Option` và mở app `HeliPort` lên, sau đó kết nối WiFi

#### Cách 2: Dùng AirportItIwm

B1: Tải `AirportItIwm` [tại đây](https://github.com/OpenIntelWireless/itlwm/releases)

[tại đây](b2: Th%C3%AAm kext v%C3%A0o EFI ==> OC ==> Kexts v%C3%A0 snapshot config (n%E1%BA%BFu d%C3%B9ng OpenCore) ho%E1%BA%B7c b%E1%BB%8F kext v%C3%A0o EFI ==> Clover ==> Kext ==> Other ( n%E1%BA%BFu d%C3%B9ng Clover )%E2%80%9D>B2: Th%C3%AAm kext v%C3%A0o EFI ==> OC ==> Kexts v%C3%A0 snapshot config (n%E1%BA%BFu d%C3%B9ng OpenCore) ho%E1%BA%B7c b%E1%BB%8F kext v%C3%A0o EFI ==> Clover ==> Kext ==> Other ( n%E1%BA%BFu d%C3%B9ng Clover )</a></p><p>B3: Reboot</p><p>N%E1%BA%BFu nh%C6%B0 l%C3%A0m nh%C6%B0 tr%C3%AAn m%C3%A0 v%E1%BA%ABn ko nh%E1%BA%ADn th%C3%AC c%C3%B3 th%E1%BB%83 l%C3%A0m th%C3%AAm 1 s%E1%BB%91 b%C6%B0%E1%BB%9Bc nh%C6%B0 sau:</p><ul><li>Enable Apple Secure Boot, chi ti%E1%BA%BFt <a href=)

- Force load kext `IO80211Family` như sau:
  - B1: Mở config lên vào mục `Kernel -> Force`
  - B2: Chỉnh config như hình (đối với những bạn dùng clover có thể convert ngược các patch này, chi tiết [tại đây](https://heavietnam.ga/2020/12/24/convert-bootloader/))

![force-io80211.2e4f9bcd.png](https://raw.githubusercontent.com/king-dragon/image/main/2022/08/21-15-30-03-force-io80211.2e4f9bcd.png)

- Chuột phải vào `IO80211Family`, chọn `Show package contents` và cho kext `AirportItlwm` vào làm plugin, sau đó bỏ kext `IO80211Family` vừa chỉnh vào EFI

**Lưu ý: không bao giờ load chung 2 kext ItIwm và AirportItwIm**

#### Bluetooth:

B1: Tải kext IntelBluetoothFirmware [tại đây](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)

B2: Thêm kext vào `EFI ==> OC ==> Kexts` và snapshot config (nếu dùng OpenCore) hoặc bỏ kext vào `EFI ==> Clover ==> Kext ==> Other` ( nếu dùng Clover )

B3: Reboot

**Lưu ý đối với Monterey (12):**

- **Đảm bảo rằng kext `IntelBluetoothFirmware.kext` ở version 1.1.3+**
- **Disable kext `IntelBluetoothInjector.kext` trong config**
- **Thêm kext `BlueToolFixup.kext` có trong `BrcmPatchRAM.kext`, tải xuống [tại đây](https://github.com/acidanthera/BrcmPatchRAM)**

## ***Fix WiFi và Bluetooth cho các card Broadcom:***

**B1**: Tải xuống kext [tại đây](https://github.com/acidanthera/AirportBrcmFixup/releases) và Bcrmpatchram [tại đây](https://github.com/acidanthera/BrcmPatchRAM)

B2: Thêm kext vào `EFI ==> OC ==> Kexts` và snapshot config (nếu dùng OpenCore) hoặc bỏ kext vào `EFI ==> Clover ==> Kext ==> Other` ( nếu dùng Clover )

B3: Reboot

## Fix bluetooth BTA-403 trên monterey

B1: map usb xem hướng dẫn tại bài [này](https://heavietnam.ga/2021/2021/09/29/vi-1map-usb-intel-and-amd/index.html)

B2: các bạn thêm kext `bluetoothtoolfixup` [tại đây](https://github.com/acidanthera/BrcmPatchRAM/releases)

B3: snaps và restart

## Fix usb wifi in bigsur+

### Các dòng card support

- ASUS_USB-N10E_92CU

- ASUS_USB-N13_92CU

- ASUS_USB-N10_92CU

- ASUS_1870_8812BU

- ASUS_USB-N10E_92CU

- ASUS_USB-N10_92CU

- ASUS_USB-N13_92CU

- ASUS_USB-AC53_8812BU

- ASUS_USB-AC55B1_8812BU

- ASUS_USB-AC56_8812AU

- ASUS_USB-AC55_8812BU

- ASUS_USB-AC68ALL_8814AU

- ASUS_USB-AC68CE_8814AU

- ASUS_USB-AC68FCC_8814AU

- AboCom_8178_92CU

- AboCom_0811_8811AU

- AboCom_8189_92CU

- AboCom_92EU

- AboCom_88EU

- AboCom_AC_8812AU

- AboCom_AC_8812AU

- Actiontec_8811AU

- AirTies_Air2520_8811AU

- AirTies_Air2525_8811AU

- AboCom_8178_92CU

- AboCom_8189_92CU

- Actiontec_8105_SingleBand_8811AU

- Actiontec_8108_DualBand_8811AU

- Amigo_92CU

- Amigo_92CU

- AzureWave_92CU

- Belkin_1004_92CU

- Belkin_1102_92CU

- Belkin_2102_92CU

- Belkin_2103_92CU

- Belkin_92DUVS_1105

- Belkin_92DUVS_110A

- Belkin_92DUVS_120A

- Belkin_F9L1106_v2_8812AU

- Belkin_F9L1106v2_8812AU

- Buffallo_25D_8812AU

- Buffallo_433DM_8811AU

- Buffallo_WI_U2_433DHP_8811AU

- Buffallo_WLP_U2_433DHP_8811AU

- Compare-8010_92CU

- Compare-8011_92CU

- Corega_92CU

- DLink_DWA121_92CU

- DLink_DWA123_92CU

- DLink_DWA131B1_92CU

- DLink_DWA132_92CU

- DLink_DWA133_92CU

- DLink_DWA123_88EU

- DLink_DWA125_88EU

- DLink_DWA131C1_92EU

- DLink_DWA131E_92EU

- DLink_DWA171_8812AU

- DLink_DWA182B1_8812AU

- DLink_DWA182_8812AU

- DLink_DWA192_8814AU

- DLink_GO_USB_N150_88EU

- ELECOM_WDC300SU2S_92CU

- ELECOM_8811AU

- ELECOM_WDB433SU2M_8811AU

- ELECOM_WDC1300DU3_8814AU

- ELECOM_WDC1300SU3_8814AU

- ELECOM_WDC150SU2M_88EU

- ELECOM_WDC433DU2_8812AU

- ELECOM_WDC433SU2M2_8811AU

- EDIMAX- EW-7722UTn V2

- EDIMAX N300

- EDIMAX EW-7811Un

- Edimax_AC1750_8814AU

- Edimax_AC1750_A834_8814AU

- Edimax_AC600_8812AU

- Edimax_EW-7611ULB_8723BU

- Edimax_EW-7811UAC_8812AU

- Edimax_EW-7822UAC_8812AU

- Edimax_EW-7822ULC_8812AU

- Edimax_GLP_8812AU

- Edimax_7811_92CU

- Edimax_7822_92CU

- Feixun_90_92CU

- Feixun_91_92CU

- EnGenius_AC_8812AU

- HP_92CU

- Hawking_HWDN3_92CU

- Hawking_HWUN4_92CU

- Hercules_HWUm300_92CU

- Hercules_HWUp150_92CU

- Hawking_8812AU

- Hawking_HW7ACU_8812AU

- IO_DATA_AC433UM_8812AU

- O_DATA_WN-AC867U_8812AU

- Infocus_INA-LCKEY_8812AU

- IO_DATA_92CU

- Linksys_WUSB6300_8812AU

- Logitec_92CU

- Loopcomm_ACA1_8812AU

- Netgear_A7000

- Netgear_N300MA_92CU

- Netgear_WNA1000M_92CU

- Netgear_WNA3100M_92CU

- Netgear_A6100_8812AU

- Netgear_A6200v2_8812AU

- PCI_BT-Micro3H2X_92CU

- PCI_GW_USEco300_92CU

- PCI_GW_USLight_92CU

- PCI_GW_USNano2_92CU

- PCI_GW_USValue_EZ_92CU

- PCI_SW_WF02-AD15_92CU

- PCI_GW-300S_92EU

- PCI_GW-450S_8812AU

- PCI_GW-900D_8812AU

- Proxim_USB-9100_8812AU

- RTL8188CTV

- RTL8188CTV_0A8A

- RTL8188CTV_8011

- RTL8188CU

- RTL8188CUS

- RTL8188CUS_1E1E

- RTL8188CUS_2E2E

- RTL8188CUS_5088

- RTL8188CUS_Combo

- RTL8188CUS_Combo_AFF8

- RTL8188CUS_Combo_AFFB

- RTL8188CUS_Combo_AFFC

- RTL8188CUS_Solo

- RTL8188CUS_VL

- RTL8188CUS_solo_AFF7

- RTL8188CUS_solo_AFF9

- RTL8188CUS_solo_AFFA

- RTL8188RU

- RTL8188RU_Netcore

- RTL8192CU

- RTL8192CU_8177

- RTL8192CU_8178

- RTL8192DU_VS

- RTL8188EU

- RTL8188EUS

- RTL8188EU_ETV

- RTL8188EU_VAU

- RTL8192EU

- RTL8192EU-2

- RTL8811AU

- RTL8812AU

- RTL8812BU

- RTL8812AU-VL

- RTL8812AU-VN

- RTL8812AU-VS

- RTL8814AU

- Sitecom_WL365_92CU

- Sitecom_WLA1001v1_92CU

- Sitecom_WLA2102_92CU

- Sitecom_WLA4001_92CU

- Sitecom_WLA1100_88EU

- Sitecom_WLA2104_8812AU

- Sitecom_WLA7100_8812AU

- Sitecom_WLA8100_8814AU

- Tenda U3 Mini

- TPLink-Archer_T2U_NANO

- TL-WN823Nv3

- TL-WN725Nv3

- TL-WN723Nv3

- TL-WN723Nv2

- TL-WN722Nv3

- TL-WN722Nv2

- TL-WN821Nv6

- TPLink_92CU

- TPLink_821v5_92EU

- TPLink_822v4_92EU

- TPLink_823v2_92EU

- TPLink_8812AU_1

- TPLink_8812AU_2

- TPLink_8812AU_3

- TPLink_88EUSU

- TPLink_T4UH_8812AU

- TPLink_T4U_8812AU

- TPLink_T9UH_8814AU

- TRENDnet N150 Micro

- Trendnet_624D_92CU

- Trendnet_648B_92CU

- Trendnet_92DUVS

- TrendNet_TEW804B_8812AU

- TrendNet_TEW805B_8812AU

- TrendNet_TEW809UB_8814AU

- Western_AC_8812AU

- ZyXEL_AC_8812AU

- ZyXEL_92CU

### Tiến hành

B1: Down file pkg [tại đây]([Release Wireless USB Big Sur Adapter-V13 · chris1111/Wireless-USB-Big-Sur-Adapter (github.com)](https://github.com/chris1111/Wireless-USB-Big-Sur-Adapter/releases/tag/V13))

B2: tắt sip và gate keeper

B3: Tiến hành chạy file pkg

B4: restart

## Lưu ý

**Nếu bạn đã làm theo như trên nhưng card vẫn không nhận thì làm như sau:** 

- **Vào plugin của airportbcrm fixup và xoá kext sau đi `AirPortBrcm4360_Injector.kext` sau đó snapshot lại config và save** 
- **Reboot**

Nếu bạn đang boot monterey thì hãy xoá các kext injector đi

- IntelBluetoothInjector.kext
- BrcmBluetoothInjector.kext

**Khi làm theo cách trên đồng thời các bạn cần map USB theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/vi-1map-usb-intel-and-amd/)**


