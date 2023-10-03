# bug3_6.4.10
A kernel bug in pvr2_send_request_ex in drivers/media/usb/pvrusb2/pvrusb2-hdw.c
Due to a specific malformed usb descriptor file, the control endpoint of a usb device is not properly configured, resulting in the function pvr2_send_request_ex in drivers/media/usb/pvrusb2/pvrusb2-hdw.c being executed to probe the status of the control endpoint, issuing commands and attempting to fetch a response from the device for a limited amount of time until it times out.
![image](https://github.com/wanrenmi/bug3_6.4.10/assets/42407501/09c7fa7e-dff9-42e8-b8c7-d5bc4d23ebc0)
This phenomenon may not be called as a serious vulnerability, but it is at least a denial of service vulnerability that wastes host system resources. The following is the input usb descriptor file:
![image](https://github.com/wanrenmi/bug3_6.4.10/assets/42407501/9f396728-6bd7-41c3-9dfd-80d3ae80e62b)
Among them, the first 18 bytes are the device descriptor, and the latter is the config descriptor. Insert the above file as a real or simulated USB device into a host using the linux kernel (It is currently certain that kernel version <=6.4.10 will be affected by this bug, the latest kernel version has not been tested, but because there is no such part of code update, so there is a high probability that this vulnerability also exists). The result of the bug triggering situation is shown in the figure below:
![image](https://github.com/wanrenmi/bug3_6.4.10/assets/42407501/6a91ca07-6228-4a51-bcd2-1f276622c850)

