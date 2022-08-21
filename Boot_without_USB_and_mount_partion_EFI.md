# Boot không cần USB và mount phân vùng EFI

## ***P1: Mount efi là gì ?***

Trước tiên ta phải biết phân vùng “EFI”  (Giao diện chương trình cơ sở mở rộng phân vùng hệ thống) hay có tên gọi khác là “ESP” là gì ?  Ta cứ hiểu đơn giản EFI là 1 phân vùng dùng để boot vào các hệ điều hành.  Thế tại sao ta lại cần mount phân vùng EFI?  Như đã nói ở trên, trước khi làm việc gì liên quan đến hệ điều hành điu phải thông qua EFI nên bắt buộc phải mount phân vùng EFI.

## ***P2: Tiến hành***

**B1**: Mount USB:

Mount USB bằng Hackintool ( ESP Mount Pro, OpenCore Configuratior,……v.v | ở đây mình chỉ hướng dẫn mount EFI bằng Hackintool )

Tiến hành các bạn mở Hackintool lên sau đó vào mục Disk như hình sau đó bấm vào  dấu ![](https://lh3.googleusercontent.com/wD_Gsvy3fLiUnYSBFW6AAlj6TOLpn4cdIxdRip0RDxQkroiBPTBZrE0qM5sIOcWdNl-yKMBplxu0zLyRFybikTKRf9XJoomwCZxCysXpo7mE6V05A_aQhM_epBws77cfnbDBruSf=s0) ngay tên ổ đĩa và USB

Tiếp đến các bạn sẽ bấm vào dấu ![](https://lh6.googleusercontent.com/7gi904ZcnjeCyfMgppVcjitylfagySC8rDgAEuBI_O2rLRVLReWoKIxxIfu0EICNIZVSadJkmnEsN9RqQEOL5tPtOGparIrG2qz_iwTZNdylp0n8B93_TCFgUzWiBaIwuV1lsgpp=s0) để thấy EFI của máy và USB trong Finder ![](https://lh4.googleusercontent.com/9f_Smi9dd5UE6hS0lYppcAPgzc5NH4ZOmL9BcLObKmJRQFTLow15h8r6RJfY6tw6MUGigGglhoUxVFI-QvzVJaylMbbNo9AXFeOFyn0R4uEk6G61Edq6sk6pastoetxgng80T1pc=s0) **B2**: Các bạn mở EFI của USB ra. Lấy hai mục OC ( hoặc clover ) và Boot trong USB và copy nó vào folder EFI của máy 

> làm như vậy cho các bạn đang có win mà muốn dual với win còn đối với các bạn không có win hoặc đã xóa win trước đó các bạn có thể replace thư mục efi của ổ cứng 

**B3**: kích hoạt launch option lên (xem chi tiết [tại đây](https://heavietnam.ga/2021/09/29/ix-cach-them-boot-vao-bios/)) đối với opencore  hoặc  Add boot vào BIOS theo đường dẫn sau: `EFI\OC\opencore.efi`  đối với clover các bạn add theo đường dẫn sau `EFI/CLOVER/CLOVERX64.efi `
