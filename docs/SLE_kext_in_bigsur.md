# Cách sửa kext S/L/E trên bigsur

## Tìm hiểu chung

Đối với bigsur apple đã chặn tính năng write ở S/L/E (để biết S/L/E là gì bạn vui lòng xem lại thuật ngữ [ở đây](https://heavietnam.ga/2021/09/29/tim-hieu-ve-hackintosh/)). Tuy nhiên khi đã tìm hiểu sâu vào hackintosh bạn sẽ cần mod kext và cần cho nó vào S/L/E. Ta sẽ có 2 phương pháp cho trường hợp này:

- Snapshot: đây cũng là 1 cách cho phép bạn tạo 1 folder extension ở user và tiến hành snapshot nó với S/L/E
  - Không khuyến khích vì nó rất nguy hiểm chỉ cần 1 sơ xuất hoạt sử dụng ko đúng phiên bản macos có thể bay ngay lập tức
- Sử dụng L/E
  - đúng đối với cách này việc cần làm là bạn chỉ cần tăng version kext đã mod lên cao hơn version kext gốc và dùng kext-droplet cho nó vào L/E thì sẽ nhận kext

## Tiến hành

B1: mở kext ra

- Chuột phải chọn `show package contents`

![](https://i.imgur.com/bEAh6tw.png)

B2: sửa `CFBundleVersion` và `CFBundleShortVersionString` thành version cao hơn

![](https://i.imgur.com/oZmbOhs.png)

B3: mở file version.plist và chỉnh những mục tương tự

![](https://i.imgur.com/kdu6m4i.png)

B4: tải kext-droplet [tại đây](https://github.com/chris1111/Kext-Droplet-Big-Sur)

B5: tắt sip theo hướng dẫn [tại đây](https://maclife.vn/huong-dan-tat-system-integrity-protection-sip-de-cai-app.html)

B6: bỏ kext vào kext-droplet

**Source tham khảo: howtohackintosh.top**
