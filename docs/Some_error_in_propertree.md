# 1 số lỗi ProperTree

# Monterey:

Nếu các bạn đã lên Monterey thì có thể gặp những lỗi sau khi sử dụng ProperTree:

1. Black screen ProperTree:
   - Nguyên nhân là do gói tk đi kèm theo python bạn cài chưa tương thích với macOS Monterey
   - Cách khác phục là các bạn tải gói python 3.9.7 (không phải bản universal) từ trang [python.org](https://python.org/) hoặc tải trực tiếp [tại đây](https://www.python.org/ftp/python/3.9.7/python-3.9.7-macosx10.9.pkg). Sau đó tiến hành cài và tải bản ProperTree mới nhất [tại đây](https://github.com/corpnewt/ProperTree). Và chạy tập lệnh `buildapp-select.command` sau đó trong folder `Scripts` của ProperTree (chọn mục build app với python3)
2. Không thể save và mở file plist:
   - Nguyên nhân cũng tương tự như lỗi 1 đây là do sử dụng gói universal. 
   - Cách khắc phục là các bạn tải bản ProperTree 3.9.7 (intel only) tại trang [python.org](https://python.org/) hoặc tải [tại đây](https://www.python.org/ftp/python/3.9.7/python-3.9.7-macosx10.9.pkg) sau đó chạy tập lệnh `buildapp-select.command` trong folder `Scripts` (nhớ chọn build app with python3).

# Ubuntu:

1. Khi run ProperTree gặp lỗi **`[ModuleNotFoundError: No module name 'tkinter']`**
   - Nguyên nhân là do tk chưa được cài đặt.
   - Cách khắc phục là chạy lệnh sau trong terminal `sudo apt-get install python3-tk -y`
2. ProperTree không chạy vì không đủ quyền:
   - Nguyên nhân do ProperTree không có quyền để chạy
   - Cách khắc phục các bạn gõ lệnh sau vào terminal `chmod +x ProperTree.command`

Và như vậy là xong nếu có lỗi gì mình sẽ tiếp tục cập nhật. Follow this forum để nhận được nhưng tin tức mới liên tục.

Source tham khảo: [corpnewt/ProperTree: Cross platform GUI plist editor written in python. (github.com)](https://github.com/corpnewt/ProperTree)
