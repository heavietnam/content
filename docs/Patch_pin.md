# Patch pin

## ***Tìm hiểu thuật ngữ***

- Trước tiên ta phải hiểu nguyên tác hoạt động:

Các bản AppleACPIPlatform mới không thể nào truy cập chính xác vào trường EC (Embedded Controller) nên nó sẽ gây ra 1 số lỗi khi sử dụng các kext hiển thị phần trăm pin, bạn phải tiến hành sửa đổi DSDT để nó nhận chính xác mặc dù bạn có thể dùng các bản AppleACPIPlatform cũ hơn (Snow Leopard) nhưng vì các CPU đời Ivy Birdge + có hỗ trợ tính năng quản lý năng lượng nên mình khuyên các bạn nên dùng bản mới nhất. Để làm điều đó, chúng ta cần thay đổi các trường trong EC thành 8 bit để có thể truy cập cùng 1 lúc.

## ***Tiến hành***

### **Cách 1: Dùng kext `ECEnabler`**

Khi dùng cách này thì khả năng 90% là nhận pin

**B1:** Tải kext ECEnabler từ nguồn [Releases · 1Revenger1/Enabler · GitHub](https://github.com/1Revenger1/ECEnabler/releases)  và bỏ vào thư mục EFI ⇒ OC hoặc EFI ==> CLOVER ==> KEXT ==> OTHER

**B2** Snapshot config nếu dùng OpenCore.

**B3**: Tải SMCBattery.kext bỏ vô mục kext và snapshot lại (nếu dùng OpenCore) (tải ở đây nhé [Releases · acidanthera/VirtualSMC · GitHub](https://github.com/acidanthera/virtualsmc/releases))

**B4**: Reboot và tận hưởng.

### **Cách 2: Dùng các patch sẵn của RehabMan**

B2: Các bạn download patch phù hợp với máy của các bạn [tại đây](https://github.com/RehabMan/Laptop-DSDT-Patch/tree/master/battery).

Hầu hết các máy ASUS và Lenovo đều có patch.

B3: Xem patch nào hỗ trợ máy của bạn:

- Các bạn vào các patch phù hợp với model máy và tìm xuống phần `works for`

![](https://lh5.googleusercontent.com/3f9UFToVOlHMlbvcR3Dhw7mmzks3wz6spyFUlhVzf9MbUW20gUIkNc5SqjugE4iDEEDKxm00NB1KRlpLvMys_T08tuSU_ZAD7-wCh5zvTIriY0rL2O5zbI_34jE9XOviY81apmgo=s0)

B4: Tải patch và MaciASL [tại đây](https://bitbucket.org/RehabMan/os-x-maciasl-patchmatic/downloads/).

B5: Dump DSDT và compile theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

B6: Áp dụng các patch theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvii-patch-dsdt-phan-2/).

**Lưu ý : Khi làm theo cách này thì lúc các bạn update firmware sẽ phải làm lại, nếu không muốn làm lại thì các bạn có thể trích DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/)**.

### **Cách 3: Static patch**

#### P1: Xác định phần cần patch:

B1: Các bạn dump DSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

B2: Biên dịch DSDT thành .dsl theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/).

B3: Mở DSDT lên và search `Embedded` để xác định các trường EC. Ở đây ta có 4 trường EC:

- ECOR
- SMBX
- SMB2
- NSBS

![](https://i.imgur.com/mHjuCbv.png)

![](https://i.imgur.com/dCyEepS.png)

![](https://i.imgur.com/MkH33ag.png)

![](https://i.imgur.com/yW0kwml.png)

B4: Ta tiến hành search các trường EC đã xác định ở trên

![](https://i.imgur.com/c4T4m4n.png)

![](https://i.imgur.com/nTeM727.png)

![](https://i.imgur.com/Nc97ACs.png)

![](https://i.imgur.com/l3IesQ7.png)

Ở đây ta có:

- Biến NSBS xuất hiện ở 1 field
- Biến SMB2 xuất hiện ở 2 field
- Biến SMBX xuất hiện ở 3 field
- Và biến ECOR xuất hiện ở 1 field

B5: Các bạn tiến hành nhìn vào các biến ở các field đã xác định ở trên. Biến nào trên 8 bit thì ghi lại.

![](https://i.imgur.com/VowFNDs.png)

**Lưu ý:**

- Đối với những trường 16 bit và 32 bit các bạn tiết hành search các trường đó nếu chỉ trả về 1 giá trị có nghĩa là trường đó không được sử dụng và có thể bỏ qua.

![](https://i.imgur.com/I8f6UAl.png)

- **Ở đây ta có trường B0TM có 16 bit**
  - Tuy nhiên khi search lại trường này tả chỉ thấy có 1 kết quả trả về. Tức trường này không được sử dụng và có thể bỏ qua.

Làm tương tự như vậy ta có:

```
DT2B,   16 
TAH0,   16,  
TAH1,   16,  
B0C3,   16, 
B0SN,   16, 
B1SN,   16 
BDAT,   256, 
BDA2,   256,  
```

#### P2: Patch các trường 16 bit và 32 bit

##### Trường 16 bit:

Để fix các trường 16 bit bạn sẽ tiến hành chia nhỏ các trường 16 bit thành 2 trường 8 bit

B1: Đặt tên cho 2 trường vừa tách. Tên mới không được trùng với các tên đã có trong DSDT (mẹo ở đây là bạn sẽ lấy tên của trường chính bỏ đi ký tự đầu tiên và thêm sau là các sô thứ tự. Cần tránh các tên bắt đầu bằn chữ số)

```
// sau khi làm xong ta được
variableOrig.            variable1     variable2
DT2B,   16,      ==>        T2B1,        T2B2
TAH0,   16,      ==>        AH01,        AH02
TAH1,   16,      ==>        AH11,        AH12
B0C3,   16,      ==>        B0CX,        B0CK
B0SN,   16,      ==>        B0SX,        B0SK
B1SN,   16,      ==>        B1SX,        B1SK
```

B2: Tiến hành viết patch để tách các trường thành các trường nhỏ 8 bit.

```
# cấu trúc chung:
into device label EC code_regex variableOrig,\s+16, replace_matched begin variable1,8,variable2,8, end;
# Note thay variable bằng tên trường các bạn đã xác định ở trên

# ví dụ
into device label EC code_regex DT2B,\s+16 replace_matched begin T2B1,8,T2B2,8, end;

# giái thích cú pháp:
  # into device label EC code_regex DT2B,\s+16: tức là tìm trong device EC đoạn code DT2B,   16
  # replace_matched begin T2B1,8,T2B2,8,: tức là thay thế đoạn code trên và thay thế chúng bằng T2B1,8,T2B2,8,

# sau khi làm xong ta được
into device label EC code_regex DT2B,\s+16 replace_matched begin T2B1,8,T2B2,8, end;
into device label EC code_regex TAH0,\s+16, replace_matched begin AH01,8,AH02,8, end;
into device label EC code_regex TAH1,\s+16, replace_matched begin AH11,8,AH12,8, end;
into device label EC code_regex B0C3,\s+16, replace_matched begin B0CX,8,B0CK,8, end;
into device label EC code_regex B0SN,\s+16, replace_matched begin B0SX,8,B0SK,8, end;
into device label EC code_regex B1SN,\s+16 replace_matched begin B1SX,8,B1SK,8, end;
```

B3: Tiến hành apply patch vừa viết theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvii-patch-dsdt-phan-2/). Sau khi apply sẽ được như hình:

![](https://imgur.com/UeO8FcP.png)

hình ảnh chỉ mang tính chất minh họa

B4: Ta sẽ tiến hành fix các lỗi. Do ở bước trên bạn đã tiên hành tách các trường 16 bit nên những method call qua trường này sẽ bị lỗi. Để fix lỗi đó các bạn sẽ tiến hành add method b1b2 vào:

```
into method label B1B2 remove_entry;
into definitionblock code_regex . insert
begin
Method (B1B2, 2, NotSerialized) { Return(Or(Arg0, ShiftLeft(Arg1, 8))) }\n
end;
```

B5: Nhấn compiler để tiến hành check lỗi.

B6: Các bạn sẽ ân vào vùng lỗi và tìm xem nó ở method nào.

![](https://i.imgur.com/NMHGJi2.png)

B7: Ta sẽ tiến hành phân tích lỗi và viết patch.

![](https://i.imgur.com/UcTCMTg.png)

Ở đây ta thấy nó báo lỗi trường `B1SN` khi nhấn vào thì sẽ hiện dòng code bị lỗi là `Store (B1SN, Local0)` ở method `BIFA` thì ta sẽ có cách dạng lỗi thường gặp:

- `Store (variableOrig, )`
  - Đối với dạng này ta sẽ có cấu trúc fix chung là:
    - `into method label <method> code_regex \(<variableOrig>, replaceall_matched begin (B1B2(<variable1>,<variable2>), end;`
  - Ví dụ:
    - `into method label BIFA code_regex \(B1SN, replaceall_matched begin (B1B2(B1SX,B1SK), end;`
- `Store ( ,variableOrig)`
  - Đối với dạn này ta sẽ có câu trúc fix chung là:
    - `into method label <method> code_regex \<variableOrig>) replaceall_matched begin B1B2(<variable1>,<variable2>)) end;`
  - Ví dụ:
    - `into method label SMBW code_regex \DT2B) replaceall_matched begin B1B2(T2B1,T2B2)) end;`

Ở trên là 2 dạng khá phổ biến mà bạn có thể gặp phải. Ngoài ra còn 1 dạng lỗi đặc biệt nữa như sau:

![](https://i.imgur.com/91jK5ZI.png)

Ta có thể thấy biến bị lỗi được gắn với 1 đường dẫn. Vì vậy để fix lỗi này ta sẽ tiến hành thay toàn bộ đường dẫn bằng b1b2. Tuy nhiên bạn phải tiến hành gắn các biến trong b1b2 với đường dẫn đã thay thế:

```
// before
Store (^^LPCB.EC.B0C3, Index (BIXT, 0x0A))
// after
Store (B1B2(^^LPCB.EC.B0CX, ^^LPCB.EC.B0CK), Index (BIXT, 0x0A))
```

- Cấu trúc chung:
  - `into method label<method> code_regex \<path variableOrig>, replaceall_matched begin B1B2(<path variable1>, <path variable2>), end;`
    - Xem các xác định path `variable` ở mục lưu ý
- Ví dụ:
  - `into method label _BIX code_regex \^\^LPCB.EC.B0C3, replaceall_matched begin B1B2(^\^LPCB.EC.B0CX, ^\^LPCB.EC.B0CK), end;`

**Chú ý:**

- **<method> có nghĩa là method chứa dòng code lỗi như ở trên sẽ là `_BIX`**
  - Vậy để xác định được method của dòng code lỗi ta sẽ ấn nút compiler để check lỗi và ấn vào lỗi cần fix nhìn vào gốc dưới bên trái để xác định được method cần dùng.

![](https://i.imgur.com/5KJWGIW.png)

- **Cách xác định `path variable1` và `path variable2`:**
  - `path variable Orig:_SB.PCI0.LPCB.EC.<variable Orig> ==>`
  - `path variable1:_SB.PCI0.LPCB.EC.<variable1>`
  - `path variable2:_SB.PCI0.LPCB.EC.<variable2>`
- Ví dụ:
  - `path variable Orig: _SB.PCI0.LPCB.EC.B0C3 ==>`
  - `path variable1:_SB.PCI0.LPCB.EC.B0CX`
  - `path variable2:_SB.PCI0.LPCB.EC.B0CK`

B8: Sau khi làm như trên ta sẽ được các patch:

```
into method label _BIX code_regex \^\^LPCB\.EC\.B0C3, replaceall_matched begin B1B2(^\^LPCB\.EC\.B0CX, ^\^LPCB\.EC\.B0CK), end;

into method label BIFA code_regex \(B1SN, replaceall_matched begin (B1B2(B1SX,B1SK), end;

into method label BIFA code_regex \(B0SN, replaceall_matched begin (B1B2(B0SX,B0SK), end;

into method label SMBR code_regex \(DT2B, replaceall_matched begin (B1B2(T2B1,T2B2), end;

into method label SMBW code_regex \DT2B\) replaceall_matched begin B1B2(T2B1,T2B2)) end;

into method label TACH code_regex \(TAH0, replaceall_matched begin (B1B2(AH01,AH02), end;

into method label TACH code_regex \(TAH1, replaceall_matched begin (B1B2(AH11,AH12), end;
```

Apply các patch theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvii-patch-dsdt-phan-2/).

##### Trường 32 bit

Ở đây do DSDT patch ở trên không có trường 32 bit nên ta sẽ dùng tạm DSDT khác để tiến hành patch trường 32 bit. Làm tương tự như P1 ta có các trường 32 bit sau:

```
BTY0,   32, 
BTY1,   32,
```

B1: Ta sẽ tiến hành tách các trường 32 bit thành 4 trường 8 bit và đặt tên cho 4 trường đó:

```
BTY0,   32, ==> TY01,TY02,TY03,TY04
BTY1,   32, ==> TY11,TY12,TY13,TY14
```

B2: Ta tiến hành viết patch trương tự như ở trường 16 bit. Nhưng thay vì thay bằng 2 trường 8 bit ta sẽ tiến hành thay bằng 4 trường 8 bit. Sau khi làm xong ta được:

```
into device label EC code_regex BTY0,\s+32 replace_matched begin TY01,8,TY02,8,TY03,8,TY04,8 end;

into device label EC code_regex BTY1,\s+32 replace_matched begin TY11,8,TY12,8,TY13,8,TY14,8 end;
```

Tương tự như trường 16 bit ta vẫn sẽ gặp error.

B3: Để khắc phục điều này ta sẽ tiến hành tương tự như trường 16 bit nhưng thay vì add method B1B2 ta sẽ add method B1B4:

```
into method label B1B4 remove_entry;
into definitionblock code_regex . insert
begin
Method (B1B4, 4, NotSerialized)\n
{\n
    Store(Arg3, Local0)\n
    Or(Arg2, ShiftLeft(Local0, 8), Local0)\n
    Or(Arg1, ShiftLeft(Local0, 8), Local0)\n
    Or(Arg0, ShiftLeft(Local0, 8), Local0)\n
    Return(Local0)\n
}\n
end;
```

B4: Ta sẽ tiến hành add các patch fix lỗi tương tự như trường 16 bit nhưng thay B1B2 thành B1B4. Sau khi làm xong ta sẽ được:

```
into method label GBTI code_regex \(BTY0, replaceall_matched begin (B1B4(TY01,TY02,TY03,TY04), end;

into method label GBTI code_regex \(BTY1, replaceall_matched begin (B1B4(TY11,TY12,TY13,TY14), end;
```

B5: Tiến hành apply các patch.

#### P3: Patch các trường lớn hơn 32 bit

Như đã xác định ở P1 ta có các trường lớn hơn 32 bit là:

```
BDAT,   256, 
BDA2,   256, 
```

B1: Ta sẽ tiến hành đổi tên các trường này:

```
variableOrig      variablerenamed
BDAT,   256,  ==>     DATX
BDA2,   256,  ==>     DA2X
```

B2: Tiến hành viết patch

- Cấu trúc chung:
  - `into device label EC code_regex (<variableOrig>,)\s+(56) replace_matched begin <variablerenamed>,%2,//%1%2 end;`
  - Giải thích cú pháp:
    - `into device label EC code_regex (<variableOrig>,)\s+(56)`: tìm trong device EC đoạn code là `BDAT, 256`
    - `replace_matched begin <variablerenamed>,%2,//%1%2 end;`: là thay đoạn tìm thấy bằng `DATX,256,//BDAT,256,`
- Ví dụ:
  - `into device label EC code_regex (BDAT,)\s+(256) replace_matched begin DATX,%2,//%1%2 end;`
  - `into device label EC code_regex (BDA2,)\s+(256) replace_matched begin DA2X,%2,//%1%2 end;`

B3: Apply patch vào DSDT.

B4: Ta tiến hành xác định offset ở các trường này. Có 2 trường hợp có thể xảy ra:

- Thông thường thì ở các field sẽ có ít nhất 1 offset được cho trước![](https://i.imgur.com/EoDAibZ.png)

```
// Ta sẽ có cách tính như sau
                Offset (0xDE), //convert hex to decimal ta được 222
                B1TM,   16, // 222 = 0xDE
                B1C1,   16, // 222 + (16/8) = 224 = 0xE0
                B1C2,   16, // 222 + (16/8) + (16/8) = 226 = 0xE2
                B1C3,   16, // 222 + (16/8) + (16/8) + 16/8) = 228 = 0xE4
```

- Như vậy ta có các offset sau:
  - B1TM: 0xDE
  - B1C1: 0xE0
  - B1C2: 0xE2
  - B1C3: 0xE4
- Tuy nhiên có những trường hợp trong field không cho sẵn offset như là:  
  ![](https://i.imgur.com/YsVWh4i.png)  
  ![](https://i.imgur.com/CX0WaqK.png)
  - Ta sẽ nhìn vào `OperationRegion (SMB2, EmbeddedControl, 0x40, 0x28)` và `OperationRegion (SMBX, EmbeddedControl, 0x18, 0x28)` ta có như sau:
    - `0x28` thì đang chỉ vị trị của Region này
    - `0x40` và `0x18` chính là offset lần của `SMB2` và `SMBX`

```
// Ta có như sau
                  //convert hex to decimal: 0x18 = 24
            OperationRegion (SMBX, EmbeddedControl, 0x18, 0x28)
            Field (SMBX, ByteAcc, NoLock, Preserve)
            {
              // 24 = 0x18
                PRTC,   8,
              // 24 + (8/8) = 25 = 0x19 
                SSTS,   5, 
              // 24 + (8/8) + (5/8) = 25.625 = 0x19.A
                    ,   1,
             // 24 + (8/8) + (5/8) + (1/8) = 25.75 = 0x19.C 
                ALFG,   1,
             // 24 + (8/8) + (5/8) + (1/8) + (1/8) = 25.875 = 0x19.E 
                CDFG,   1, 
       // 24 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) = 26 = 0x1A 
                ADDR,   8, 
       // 24 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) + (8/8) = 27 = 0x1B 
                CMDB,   8, 
// 24 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) + (8/8) + (8/8) = 28 = 0x1C 
                BDAT,   256, 
            }
               //convert hex to decimal: 0x40 = 64
            OperationRegion (SMB2, EmbeddedControl, 0x40, 0x28)
            Field (SMB2, ByteAcc, NoLock, Preserve)
            {
              // 64 = 0x40
                PRT2,   8, 
        // 64 + (8/8) = 65 = 0x41
                SST2,   5, 
   // 64 + (8/8) + (5/8) = 65.625 = 0x41.A
                    ,   1, 
 // 64 + (8/8) + (5/8) + (1/8) = 65.75 = 0x41.C
                ALF2,   1, 
// 64 + (8/8) + (5/8) + (1/8) + (1/8) = 65.875 = 0x41.E
                CDF2,   1, 
// 64 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) = 66 = 0x42
                ADD2,   8, 
// 64 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) + (8/8) = 67 = 0x43
                CMD2,   8, 
// 64 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) + (8/8) + (8/8) = 68 = 0x44
                BDA2,   256, 
            }
```

Vậy ta sẽ có các offset sau:

- BDAT: 0x1C
- BDA2: 0x44

B5: Vì ta đã rename ở bước 2 nên khi compiler sẽ có error. Để khắc phục ta sẽ tiến hành add method RECB (Read EC Buffer) và WECB (Write EC Buffer)

- Khi bạn check lỗi mà nhận được lỗi có dạng `Store(<variableOrig>, )`
  - thì nó sẽ thuộc method RECB
  - Ví dụ:
    - `Store (BDAT, )`
- Khi check lỗi mà có dạng là `Store( ,<variableOrig>)`
  - thì nó sẽ thuộc method WECB
  - Ví dụ:
    - `Store ( ,BDAT)`

```
# RECB
# utility methods to read/write buffers from/to EC
into method label RE1B parent_label EC remove_entry;
into method label RECB parent_label EC remove_entry;
into device label EC insert
begin
Method (RE1B, 1, NotSerialized)\n
{\n
    OperationRegion(ERAM, EmbeddedControl, Arg0, 1)\n
    Field(ERAM, ByteAcc, NoLock, Preserve) { BYTE, 8 }\n
    Return(BYTE)\n
}\n
Method (RECB, 2, Serialized)\n
// Arg0 - offset in bytes from zero-based EC\n
// Arg1 - size of buffer in bits\n
{\n
    ShiftRight(Add(Arg1,7), 3, Arg1)\n
    Name(TEMP, Buffer(Arg1) { })\n
    Add(Arg0, Arg1, Arg1)\n
    Store(0, Local0)\n
    While (LLess(Arg0, Arg1))\n
    {\n
        Store(RE1B(Arg0), Index(TEMP, Local0))\n
        Increment(Arg0)\n
        Increment(Local0)\n
    }\n
    Return(TEMP)\n
}\n
end;

#WECB
into method label WE1B parent_label EC remove_entry;
into method label WECB parent_label EC remove_entry;
into device label EC insert
begin
Method (WE1B, 2, NotSerialized)\n
{\n
    OperationRegion(ERAM, EmbeddedControl, Arg0, 1)\n
    Field(ERAM, ByteAcc, NoLock, Preserve) { BYTE, 8 }\n
    Store(Arg1, BYTE)\n
}\n
Method (WECB, 3, Serialized)\n
// Arg0 - offset in bytes from zero-based EC\n
// Arg1 - size of buffer in bits\n
// Arg2 - value to write\n
{\n
    ShiftRight(Add(Arg1,7), 3, Arg1)\n
    Name(TEMP, Buffer(Arg1) { })\n
    Store(Arg2, TEMP)\n
    Add(Arg0, Arg1, Arg1)\n
    Store(0, Local0)\n
    While (LLess(Arg0, Arg1))\n
    {\n
        WE1B(Arg0, DerefOf(Index(TEMP, Local0)))\n
        Increment(Arg0)\n
        Increment(Local0)\n
    }\n
}\n
end;
```

B6: Ta tiến hành viết patch. Tương tự như trường 16 bit và 32 bit việc đầu tiên ta cần làm là xác định đoạn code lỗi nằm ở method nào.

![](https://i.imgur.com/Bkr1BfD.png)

Như ảnh thì ta có thể thấy đoạn code lỗi nằm ở method `SMBR`. Tiến hành viết patch:

- WECB
  
  - Cú pháp chung:
    
    - `into method label <method> code_regex Store\s+((.*),\s+<variableOrig>) replaceall_matched begin WECB(<offset>,<size>,%1) end;`
  
  - Ví dụ:
    
    - `into method label SMBR code_regex Store\s+((.*),\s+BDAT) replaceall_matched begin WECB(0x1c,256,%1) end;`
    - Giải thích cú pháp:
      - `into method label SMBR code_regex Store\s+((.*),\s+BDAT)`: tức là sẽ tìm trong method `SMBR` đoạn code `Store (zero, BDAT)` (kí hiệu `.*` tức là đoạn code sẽ lấy giá trị ở trước trường `BDAT` trong đoạn code ở DSDT)
      - `replaceall_matched begin WECB(0x1c,256,%1) end;`: tức là tất cả đoạn code được tìm thấy ở trên sẽ được thay thế bằng `(0x1c,256,zero)` (kí hiệu `%1` có nghĩa là lấy phần tử T1 trong đoạn code được tìm thấy)

- RECB
  
  - Cú pháp chung:
    - `into method label <method> code_regex (variableOrig, replaceall_matched begin (RECB(<offset>,<size>), end;`
  - Ví dụ:
    - `into method label SMBR code_regex (BDAT, replaceall_matched begin (RECB(0x1c,256), end;`
    - Giải thích cú pháp:
      - `into method label SMBR code_regex (BDAT,`: tức là sẽ tìm trong method SMBR đoạn code `(BDAT,`
      - `replaceall_matched begin (RECB(0x1c,256), end;`: tức là sẽ thay thế tất cả các đoạn code được tìm thấy trong method SMBR bằng `(RECB(0x1c,256),`

```
// Patch tay
# RECB
  // before
Store (BDAT, Index (Local0, 0x02))
 // after
Store (RECB(0x1c,256), Index (Local0, 0x02))

# WECB
 // before
Store (Zero, BDAT)
 // after
WECB(0x1c,256,Zero)

// Trong đó 0x1c là offset 256 là dung lượng tính bằng bit của trường BDAT
```

**Chú ý:**

- **<offset>: là offset mà bạn đã xác định ở B4**
- **<method>: là method mà bạn đã xác định ở B6**

Sau khi làm xong ta sẽ có:

```
into method label SMBR code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label SMBR code_regex \(BDAT, replaceall_matched begin (RECB(0x1c,256), end;

into method label SMBW code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label ECSB code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label ECSB code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label ECSB code_regex \(BDAT, replaceall_matched begin (RECB(0x1c,256), end;

into method label ECSB code_regex Store\s+\((.*),\s+BDA2\) replaceall_matched begin WECB(0x44,256,%1) end;

into method label ECSB code_regex \(BDA2, replaceall_matched begin (RECB(0x44,256), end;
```

B7: Apply patch vào DSDT.

B8: Đối với 1 số dòng Asus thì bạn cần thêm patch  `Logic bug with charging/discharging status`

Vì 1 số laptop ASUS có thể gặp tình trạng khi sạc đầy nhưng hệ thống vẫn báo chưa đầy hoặc khi hết pin nhưng hệ thống vẫn báo chưa hết pin dẫn đến các lỗi như sập nguồn bất chợt ( lỗi nãy vẫn có thể xảy ra ở 1 só máy model khác ) cách fix là các bạn add đoạn sau vào:

```
into method label FBST code_regex If\s\(CHGS\s\(Zero\)\)[\s]+\{[\s]+Store\s\(0x02,\sLocal0\)[\s]+\}[\s]+Else[\s]+\{[\s]+Store\s\(One,\sLocal0\)[\s]+\} replaceall_matched begin
If (CHGS (Zero))\n
{\n
     Store (0x02, Local0)\n
}\n
Else\n
{\n
     Store (Zero, Local0)\n
}
end;
```

Sau khi làm xong tất cả các phần trên ta sẽ có đoạn patch sau:

```
#Rename field EC
DT2B,   16  ==>  T2B1,T2B2
TAH0,   16,  ==> AH01,AH02
TAH1,   16,  ==> AH11,AH12
B0C3,   16, ==> B0CX, B0CK
B0SN,   16, ==> B0SX,B0SK
B1SN,   16 ==> B1SX,B1SK
BDAT,   256, ==> DATX
BDA2,   256,  ==> DA2X

#16 bit convert 8 bit
into device label EC code_regex DT2B,\s+16 replace_matched begin T2B1,8,T2B2,8, end;
into device label EC code_regex TAH0,\s+16, replace_matched begin AH01,8,AH02,8, end;
into device label EC code_regex TAH1,\s+16, replace_matched begin AH11,8,AH12,8, end;
into device label EC code_regex B0C3,\s+16, replace_matched begin B0CX,8,B0CK,8, end;
into device label EC code_regex B0SN,\s+16, replace_matched begin B0SX,8,B0SK,8, end;
into device label EC code_regex B1SN,\s+16 replace_matched begin B1SX,8,B1SK,8, end;
#into b1b2 method
into method label B1B2 remove_entry;
into definitionblock code_regex . insert
begin
Method (B1B2, 2, NotSerialized) { Return(Or(Arg0, ShiftLeft(Arg1, 8))) }\n
end;
#fix 16 bit error
into method label _BIX code_regex \^\^LPCB\.EC\.B0C3, replaceall_matched begin B1B2(^\^LPCB\.EC\.B0CX, ^\^LPCB\.EC\.B0CK), end;

into method label BIFA code_regex \(B1SN, replaceall_matched begin (B1B2(B1SX,B1SK), end;

into method label BIFA code_regex \(B0SN, replaceall_matched begin (B1B2(B0SX,B0SK), end;

into method label SMBR code_regex \(DT2B, replaceall_matched begin (B1B2(T2B1,T2B2), end;

into method label SMBW code_regex \DT2B\) replaceall_matched begin B1B2(T2B1,T2B2)) end;

into method label TACH code_regex \(TAH0, replaceall_matched begin (B1B2(AH01,AH02), end;

into method label TACH code_regex \(TAH1, replaceall_matched begin (B1B2(AH11,AH12), end;

#Patch 32 bit

into device label EC code_regex (BDAT,)\s+(256) replace_matched begin DATX,%2,//%1%2 end;
into device label EC code_regex (BDA2,)\s+(256) replace_matched begin DA2X,%2,//%1%2 end;

#xác định offset

BDAT: 24 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) + (8/8) + (8/8) = 28 = 0x1c
BDA2: 64 + (8/8) + (5/8) + (1/8) + (1/8) + (1/8) + (8/8) + (8/8) = 68 = 0x44

#fix 32 bit error

into method label SMBR code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label SMBR code_regex \(BDAT, replaceall_matched begin (RECB(0x1c,256), end;

into method label SMBW code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label ECSB code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label ECSB code_regex Store\s+\((.*),\s+BDAT\) replaceall_matched begin WECB(0x1c,256,%1) end;

into method label ECSB code_regex \(BDAT, replaceall_matched begin (RECB(0x1c,256), end;

into method label ECSB code_regex Store\s+\((.*),\s+BDA2\) replaceall_matched begin WECB(0x44,256,%1) end;

into method label ECSB code_regex \(BDA2, replaceall_matched begin (RECB(0x44,256), end;

#32 bit field
BTY0,   32, ==> TY01,TY02,TY03,TY04
BTY1,   32, ==> TY11,TY12,TY13,TY14

#32 bit convert to 8 bit
into device label EC code_regex BTY0,\s+32 replace_matched begin TY01,8,TY02,8,TY03,8,TY04,8 end;
into device label EC code_regex BTY1,\s+32 replace_matched begin TY11,8,TY12,8,TY13,8,TY14,8 end;
```

**File cho các bạn tham khảo: [DSDT-Orig](https://github.com/king-dragon/EFI-asusx450c/blob/main/DSDT-orig.dsl), [DSDT-patched](https://github.com/king-dragon/EFI-asusx450c/blob/main/DSDT-patch.dsl)**

**Lưu ý: Bạn nên trích DSDT ra SSDT theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxviii-patch-dsdt-phan-3/). Vì nếu dùng DSDT thì khi update bios các patch sẽ bị mất tác dụng. Hoặc bạn có thể save file patched.txt lại để apply lại vào DSDT khi update BIOS.**

**Lưu ý 2: Khi các bạn không thể nào tìm thấy `EmbeddedControl` trong DSDT tức là pin của bạn không thuộc sự quản lý của EC Controler. Để khắc phục tình trạng này các bạn sẽ tiến hành update bios để cập nhật lại DSDT**

**Source tham khảo: [[Guide] How to patch DSDT for working battery status | tonymacx86.com](https://www.tonymacx86.com/threads/guide-how-to-patch-dsdt-for-working-battery-status.116102/) | [[Guide] Laptop Battery Indicator – The DSDT Patching Horror – Page 14 – Guides and Tutorials – Olarila](https://www.olarila.com/topic/5673-guide-laptop-battery-indicator-the-dsdt-patching-horror/) ( thank you )**
