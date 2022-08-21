# Scan policy

Đôi khi cài dual boot xong cac bạn sẽ cam thấy bực bội với 1 đống boot option trong giao diện picker của opencore từ windows đến recovery boot windows,… Thì scan policy sinh ra để giúp bạn giải quyết vấn đề này

- **B1**: các bạn truy cập vào trang web [sau](https://oc-scanpolicy.vercel.app/)

- **B2**: Mình sẽ giải thích cho bạn từng dòng trọng các option nhiệm vụ của bạn là dự vào đó rồi tính toán ra cho mình một giá trị thích hợp
  
  | Mã hex                | Tên                           | Chức năng                                                                                                                                              |
  | --------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
  | `0x00000001` (bit 0)  | OC_SCAN_FILE_SYSTEM_LOCK      | Hạn chế (ngừng) Việc quét các file hệ thống trong phân vùng để tạo boot option trong picker opencore<br>Dùng kết hợp với OC_SCAN_ALLOW_FS_             |
  | `0x00000002` (bit 1)  | OC_SCAN_DEVICE_LOCK           | Dùng để hạn chế (ngừng) quét các device (ổ cứng,ổ đĩa, usb,…) để làm mất các option đó trong picker opencore<br>Dùng kết hợp với OC_SCAN_ALLOW_DEVICE_ |
  | `0x00000100` (bit 8)  | OC_SCAN_ALLOW_FS_APFS         | Cho phép quét các file hệ thống APFS                                                                                                                   |
  | `0x00000200` (bit 9)  | OC_SCAN_ALLOW_FS_HFS          | Cho phép quét các file hệ thống HFS                                                                                                                    |
  | `0x00000400` (bit 10) | OC_SCAN_ALLOW_FS_ESP          | Cho phép quét các file hệ thống thuộc efi partion                                                                                                      |
  | `0x00000800` (bit 11) | OC_SCAN_ALLOW_FS_NTFS         | Cho phép quét các file hệ thống NTFS (thường là windows)                                                                                               |
  | `0x00001000` (bit 12) | OC_SCAN_ALLOW_FS_LINUX_ROOT   | Cho phép quét các file hệ thống thuộc liux root partion                                                                                                |
  | `0x00002000` (bit 13) | OC_SCAN_ALLOW_FS_LINUX_DATA   | Cho phép quét các file hệ thống thuộc linux data partion                                                                                               |
  | `0x00004000` (bit 14) | OC_SCAN_ALLOW_FS_XBOOTLDR     | Cho phép quét các file thuộc Extended Boot Loader Partition được xác định bởi Boot Loader Specification                                                |
  | `0x00010000` (bit 16) | OC_SCAN_ALLOW_DEVICE_SATA     | Cho phép quét các thiết bị Sata                                                                                                                        |
  | `0x00020000` (bit 17) | OC_SCAN_ALLOW_DEVICE_SASEX    | Cho phép quét các thiết bị SAS và Mac Nvme                                                                                                             |
  | `0x00040000` (bit 18) | OC_SCAN_ALLOW_DEVICE_SCSI     | Cho phép quét các thiết bị SCSI                                                                                                                        |
  | `0x00080000` (bit 19) | OC_SCAN_ALLOW_DEVICE_NVME     | Cho phép quét các thiết bị NVME                                                                                                                        |
  | `0x00100000` (bit 20) | OC_SCAN_ALLOW_DEVICE_ATAPI    | Cho phép quét các thiết bị CD/DVD và các device sata cũ                                                                                                |
  | `0x00200000` (bit 21) | OC_SCAN_ALLOW_DEVICE_USB      | Cho phép quét các thiết bị USB                                                                                                                         |
  | `0x00400000` (bit 22) | OC_SCAN_ALLOW_DEVICE_FIREWIRE | Cho phép quét các thiết bị FireWire                                                                                                                    |
  | `0x00800000` (bit 23) | OC_SCAN_ALLOW_DEVICE_SDCARD   | Cho phép quét các thiết bi SD card                                                                                                                     |
  | `0x01000000` (bit 24) | OC_SCAN_ALLOW_DEVICE_PCI      | Cho phép quét các thị bị connect trực tiếp tới PCI bus (vd: VIRTIO)                                                                                    |

- **B3**: các bạn tính toán xong tiến hành tick vào các ô trong trang web trên. Một gợi ý cho các bạn là như sau
  
  ![](https://i.imgur.com/7kr0bha.png)

- **B4:** tiến hành copy giá trị scan policy
  
  ![](https://i.imgur.com/FNCJgaq.png)

- **B5**: Paste giá trị đó vào mục scan policy trong config
  
  ![](https://i.imgur.com/Mzs6Biw.png)
