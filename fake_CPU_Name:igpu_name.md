# Fake iGPU/ CPU Name

## Fake iGPU Name

B1: Mở config bằng ProperTree tới phần DeviceProperties. 

B2: Add dòng sau cùng phần với dòng deviec id:

`Model | string | name igpu` 

> name igpu là tên gpu các bạn muốn hiển thị 
> 
> như của mình, mình muốn nó hiện Intel HD Graphics 400 with AMD Radeon thì mình sẽ add như sau: `model | string | Intel HD Graphics 400 with AMD Radeon` 
> 
>  sau khi add các bạn sẽ được như hình: 

![](https://lh5.googleusercontent.com/C9_xZx8AGyiXZuJegyY9JCg06mkSCPq_kddX8k3K2dUGtvbZAjusA3jdrzyFYDdzj51zSofvHE5JjkAyvXi3x6p03eiLKCwvwngv3g_jbWa6fPg95uHEJ1rj3wUrReJ3yUHK6l9l=s0)

B3: Save lại. 

B4: Reboot và tận hưởng thành quả (sau khi làm xong các bạn sẽ được như hình).

![](https://lh4.googleusercontent.com/_4GvUv6DhJKRN-86kJxnSDBKrRfUkq9taP-c6FompyKzHY0gtk1vkJBCC91zYmRw0E1gVWSnUmEv4YtLsszs4fRa3d7uzsiqooD_fdnEoJoo1pL3nyyDC9RDQWbVHgcBgf2RCzlN=s0)

**Lưu ý: Cách này chỉ dành cho các bạn thích màu mè nó chỉ thay đổi Name GPU chứ không ảnh hưởng gì tới hiệu năng và độ phân giải ![🙂](https://s.w.org/images/core/emoji/14.0.0/svg/1f642.svg)**  **nên các bạn đừng nhầm lẫn nha**.

## Fake CPU Name:

> Lúc trước mình đã viết 1 bài fake cpu name bằng cách chỉnh file info.plist trong S/L/E nhưng các này đã khá cũ và nguy hiểm khi dùng trên bigsur+ bạn nào cần có thể xem [tại đây](https://everythingforhackintosher.wordpress.com/2021/09/10/fake-igpu-cpu-name/). Hôm nay mình sẽ hướng dẫn các bạn một cách đơn giản và hiệu quả hơn

B1: Tải kext [RestrictEvents](https://github.com/acidanthera/RestrictEvents)

B2: snapshot config

B3: Add các boot-arg

```
revcpu=1 revpatch=cpuname revcpuname=value

//thay value thành tên cpu mà bạn mong muốn
```

B4: chỉnh `ProcessorType` thành `1537`

B5: restart

> Như vậy là xong rồi

![](https://i.imgur.com/pXsKLOK.png)
