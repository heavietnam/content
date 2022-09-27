# Disable dgpu Laptop

> Đối với các laptop có DGPU không support thì chúng ta cần phải disable chúng đi để tránh những hậu quả như panic, chóp màn, wake,… Một cách rất đơn giản để làm điều dó chính là thêm boot-arg `-wegnoegpu`. nhưng nó cũng có 1 vấn đề đó chính là DGPU vẫn sẽ nhận được điệ năng tức là nó vẫn đang âm thầm làm cho máy tinh chúng ta mau hết pin. Để khắc phục điều đó chúng ta có 2 phương pháp

## Optimus Method

Các làm này thực sự rất đơn giản. Những gì chúng ta cần làm là call tới method `.off` để disable đi GPU

B1: tải [SSDT-dGPU-Off.dsl](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/decompiled/SSDT-dGPU-Off.dsl.zip)

B2: Boot vào windows hoặc winpe truy cập device manager theo đường dẫn

```
Device Manager -> Display Adapters -> dGPU -> Properties -> Details > BIOS device name
```

chúng ta sẽ có thể thấy dược ACPI-path của DGPU. một số ACPI path phổ biến là

- Nvidia dGPU: `\_SB.PCI0.PEG0.PEGP`
- AMD dGPU: `\_SB.PCI0.PEGP.DGFX`

![](https://heavietnam.ga/wp-content/uploads/2022/06/image-2.png)

B3: chỉnh sửa SSDT các bạn sẽ cần đổi ACPI path của SSDT thành ACPI path vừa xác định ở trên. Cụ thể những phần cần chỉnh sửa là

```
External(_SB.PCI0.PEG0.PEGP._OFF, MethodObj)

If (CondRefOf(\_SB.PCI0.PEG0.PEGP._OFF)) { \_SB.PCI0.PEG0.PEGP._OFF() }
```

![](https://i.imgur.com/znCkaTB.png)

B4: biên dịch SSDT thnahf file aml theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)

B5: Bỏ SSDT vào EFI --> OC --> ACPI (hoặc EFI --> Clover --> ACPI --> patched | snapshot nếu là opencore)

## Bumblebee Method

Tuy nhiên trong 1 số trường hợp DGPU không thể bị disbale bởi cách call qua method `.off` đó là lý do cách Bumblebee method này ra đời. Cụ thể các này sẽ đưa dgpu tiến vào trạng thái D3. Một trạng thái mà DGPU sẽ tiêu tốn ít năng lượng nhất

B1: Tải [SSDT-NoHybGfx.dsl](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/decompiled/SSDT-NoHybGfx.dsl.zip)

B2: Boot vào windows hoặc winpe truy cập device manager theo đường dẫn

```
Device Manager -> Display Adapters -> dGPU -> Properties -> Details > BIOS device name
```

chúng ta sẽ có thể thấy dược ACPI-path của DGPU. một số ACPI path phổ biến là

- Nvidia dGPU: `\_SB.PCI0.PEG0.PEGP`
- AMD dGPU: `\_SB.PCI0.PEGP.DGFX`

![](https://heavietnam.ga/wp-content/uploads/2022/06/image-2.png)

B3: Ta cần tiến hành đổi ACPI path trong SSDT thành ACPI path vừa xác định được. Cụ thể những phần cần đổi là

```
External (_SB_.PCI0.PEG0.PEGP._DSM, MethodObj)    // dGPU ACPI Path
External (_SB_.PCI0.PEG0.PEGP._PS3, MethodObj)    // dGPU ACPI Path

If ((CondRefOf (\_SB.PCI0.PEG0.PEGP._DSM) && CondRefOf (\_SB.PCI0.PEG0.PEGP._PS3)))

// Card Off Request
 \_SB.PCI0.PEG0.PEGP._DSM (ToUUID ("a486d8f8-0bda-471b-a72b-6042a6b5bee0"), 0x0100, 0x1A, Buffer (0x04)

 // Card Off
\_SB.PCI0.PEG0.PEGP._PS3 ()
```

![](https://i.imgur.com/GWvuM5o.png)

B4: biên dịch SSDT thành file aml theo hướng dẫn [tại đây](https://heavietnam.ga/2021/09/29/xxvi-patch-dsdt-phan-1/)

B5: Bỏ SSDT vào EFI --> OC --> ACPI (hoặc EFI --> Clover --> ACPI --> patched | snapshot nếu là opencore)

**Source tham khảo: [Disabling laptop dGPUs (SSDT-dGPU-Off/NoHybGfx) | Getting Started With ACPI (dortania.github.io)](https://dortania.github.io/Getting-Started-With-ACPI/Laptops/laptop-disable.html#bumblebee-method)**
