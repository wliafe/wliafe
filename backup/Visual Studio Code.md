# VScode 配置

## B站视频

[从零开始手把手教你配置属于你的VS Code](https://www.bilibili.com/video/BV1TT4y1g7aF?spm_id_from=333.999.0.0&vd_source=c7f0a8a1b453261561b18cd69cebd8b3)

## 资源

**mingw64：**[百度网盘链接](https://pan.baidu.com/s/1L8OdqC-4VIfRvU0_cWa4jw?pwd=4i7v)

**VS Code下载地址：**[VS Code官网](https://code.visualstudio.com/)

## mingw64

将资源中的mingw64下载后放到合适的位置，然后将文件路径加入到环境变量中

![环境变量图片](https://github.com/user-attachments/assets/3f23d11a-2dff-4686-b1ff-035da3b6f32f)

## VScode扩展下载

C/C++：运行C和C++的软件

![C/C++扩展图](https://github.com/user-attachments/assets/442f5666-d53a-47a3-8eb9-d1510ee16954)

Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code：汉化软件

![Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code扩展图](https://github.com/user-attachments/assets/6e389ffc-b040-413e-a39c-318b2b059acf)

Code Runner：代码运行软件

![Code Runner扩展图](https://github.com/user-attachments/assets/eb57ec5a-04a2-4eb9-bf18-82db9f51ebc7)

Git Graph：git版本控制查看软件

![Git Graph扩展图](https://github.com/user-attachments/assets/6de51f61-bc8e-403c-91ce-38e70c2d5b5c)

## Code Runner 配置

打开扩展设置

![5](https://github.com/user-attachments/assets/1e12a7e3-1338-499c-82c2-6fb2789983bd)

## C/C++ 配置

![6](https://github.com/user-attachments/assets/36ee4d88-7291-4a9d-97ac-9a926a38e367)

![7](https://github.com/user-attachments/assets/1623b70b-c47f-4f20-afce-b1bee020a90f)

![8](https://github.com/user-attachments/assets/8c0620e5-7717-46ab-8a82-46cbabf79b96)

![9](https://github.com/user-attachments/assets/bde10f96-b4a0-486c-a28d-16c10b8880fa)

![10](https://github.com/user-attachments/assets/efd71fc0-dddc-407e-bc95-1f1b091aadb3)

![11](https://github.com/user-attachments/assets/c8032e09-4c38-4be5-b35f-46cbb1a77be2)

# VScode 代码片段

用好了就是非常好用的快捷键

```json
"Print to console": {
    "prefix": "log",
 	  "body": [
 		  "console.log('$1');",
 		  "$2"
 	  ],
 	  "description": "Log output to console"
 }
```