# GPRW/UPRW/LANC

## Thêm SSDT:

B1: Dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/) 

B2: Mở file DSDT.dsl bằng [Releases · acidanthera/MaciASL · GitHub](https://github.com/acidanthera/MaciASL/releases)

B3: Nhấn tổ hợp phím Command+F hoặc nhấn nút lệnh search trên menu bar và search các từ khóa sau

`Method (GPRW, 2)` và `Method (UPRW, 2)` hoặc *`Device (LANC)`*

B4: Nếu bạn tìm được từ khóa:

- `Method (GPRW, 2)` thì hãy tải SSDT này về: [OpenCore-Post-Install/SSDT-GPRW.aml at master · dortania/OpenCore-Post-Install · GitHub](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/SSDT-GPRW.aml) 
- `Method (UPRW, 2)` thì hãy tải SDTS này về: [OpenCore-Post-Install/SSDT-UPRW.aml at master · dortania/OpenCore-Post-Install · GitHub](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/SSDT-UPRW.aml)
-  *`Device (LANC)`* thì hãy tải SSDT này về: [OpenCore-Post-Install/SSDT-LANC.aml at master · dortania/OpenCore-Post-Install · GitHub](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/SSDT-LANC.aml) 

B5: Sau khi tải các SSDT về, bỏ vào EFI/OC/ACPI và snapshot config.

B6: Add các patch rename sau vào config/ACPI/Patch:

- [OpenCore-Post-Install/GPRW-Patch.plist at master · dortania/OpenCore-Post-Install · GitHub](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/GPRW-Patch.plist)  add patch này nếu các bạn tìm được từ khóa `Method (GPRW, 2)`
- [OpenCore-Post-Install/UPRW-Patch.plist at master · dortania/OpenCore-Post-Install · GitHub](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/UPRW-Patch.plist) add patch này nếu các bạn tìm được từ khóa `Method (UPRW, 2)`
- [OpenCore-Post-Install/LANC-Patch.plist at master · dortania/OpenCore-Post-Install · GitHub](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/LANC-Patch.plist) add patch này nếu các bạn tìm được từ khóa `Device (LANC)`

B7: Save config và reboot thôi. 

## **Cách Add patch**

B1: Vào trang copy các dòng code 

B2: Tải [Sublime Text – Text Editing, Done Right](https://www.sublimetext.com/) ( thường thì sever down khá chậm nên các bạn hãy tải ở maclife tuy là links fshare nhưng ít nhất cũng đc 100-150kb còn hơn 56kb ở trang chủ nhé links đây nhé [Sublime Text 4 – Code Editor mạnh mẽ trên mac – MacLife – Everything for Mac Lovers](https://maclife.vn/sublime-text-4-code-editor-manh-me-tren-mac.html) ) 

B3: Mở lên và tải các gói mà nó yêu cầu 

B4: Dán code vào và lưu file dưới dạng file xml 

B5: Tải app plist edit pro theo links sau [PlistEdit Pro – Advanced Mac plist and JSON editor (fatcatsoftware.com)](https://www.fatcatsoftware.com/plisteditpro/), hoặc có thể mở bằng propertree sau khi mở lên các bạn sẽ được như hình:

![](https://lh5.googleusercontent.com/ki-UyieeeckKdzAamtGq_2iRFxI11JOnRSpLiRAHm9o9d5rLSDaTsEkBFT5jNNuP7mwnywRiwevdeh8xNv7S58vouQ3g_95KO6H4eZfe72YtaqhGE2tnKpslFia1WmBDa5dMPwjN=s0)

B6: Các bạn add mục vào mục `root ⇒ ACPI ⇒ Patch` của file `config.plist `

***Lưu ý đối với máy mình thì chỉ cần fix nhiêu đây thì đã có thể sleep/wake nếu các bạn vẫn chưa sleep/wake được thì có thể tham khảo links sau*** [***Fixing Sleep | OpenCore Post-Install (dortania.github.io)***](https://dortania.github.io/OpenCore-Post-Install/universal/sleep.html)

***Lưu ý 2 nếu như các bạn đã làm phần chuẩn bị nhưng vẫn bị lổi restart khi sleep thì các bạn có thể thử bỏ kext sau vào mục `EFI ⇒ kext`*** [***Releases · acidanthera/HibernationFixup · GitHub***](https://github.com/acidanthera/HibernationFixup/releases) ***nhớ snaps lại config***
