# Cách làm EFI Opencore No ACPI

B1: Download bản opencore no ACPI [tại đây](https://tinyurl.com/2p9bw3th)

B2: Tiến hành thay thế các file sau:

- Boot
- Drivers
- OpenCore.efi
- Resources
- Tools

B3: Mở config bằng Propertree và chỉnh các dòng như sau

- ACPI –> Quirks –> EnableForAll: false
- Booster –> Quirks –> EnableForAll: false

B4: Tiến hành snapshot

B5: Save lại và restart

**Source tham khảo: [OpenCore_NO_ACPI – Opencore with additional features/changes implemented – How to use this fork – Guides and Tutorials – Olarila.com | The Real Vanilla Hackintosh](https://www.olarila.com/topic/24542-opencore_no_acpi-opencore-with-additional-featureschanges-implemented-how-to-use-this-fork/)**
