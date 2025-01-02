# StarUML导出图片去除水印

## 简介

StarUML版本为4.0

## 资源

资源为已经修改好的app.asar文件，可直接对原来文件进行替换。

[百度网盘资源](https://pan.baidu.com/s/1mmXueRtEDb7QNkYnidHK_A?pwd=yaob)


## 找到管理注册的文件

StarUML是用js写的，所以文件系统比较简单，在你安装的StarUML目录下resourse文件夹下面有一个app.asar的文件。

![文件目录](https://github.com/user-attachments/assets/4d5cf896-78f4-40f2-8e55-c8661585d4a6)

关于asar格式，其实这是一个代码的压缩包格式，里面存在着各种代码，只是为了不让里面的代码不直接暴露。这种格式就需要专门的asar工具包进行解压和打包。

## npm下载asar工具

利用Nodejs中的npm工具进行操作

如今是个开源的时代，你可以在npm中直接下载asar工具。

安装asar

```bash
npm install asar
```
## app.asar解压

然后你将原来的app.asar包复制出来一个，进行解压

解压命令为:

```bash
asar e app.asar app
```

## js文件修改

找到app/src/engine/licence-manager.js文件，使用任何编辑器打开都可以

![2](https://github.com/user-attachments/assets/0e7b7114-820f-424b-b145-1027c43e0f70)

将原来的validate()函数注释了然后增加一条上面

```js
  //增加新的函数
  validate() {
    return new Promise((resolve, reject) => {
      resolve({
        name: "DXkite",
        product: "DXkite product",
        licenseType: "DXkite Personal",
        quantity: "DXkite Quantity",
        timestamp: "1529049036",
      });
    })
  }
  //注释原来的函数
  // validate () {
  //   return new Promise((resolve, reject) => {
  //     try {
  //       // Local check
  //       var file = this.findLicense()
  //       if (!file) {
  //         reject('License key not found')
  //       } else {
  //         var data = fs.readFileSync(file, 'utf8')
  //         licenseInfo = JSON.parse(data)
  //         if (licenseInfo.product !== packageJSON.config.product_id) {
  //           app.toast.error(`License key is for old version (${licenseInfo.product})`)
  //           reject(`License key is not for ${packageJSON.config.product_id}`)
  //         } else {
  //           var base = SK + licenseInfo.name +
  //           SK + licenseInfo.product + '-' + licenseInfo.licenseType +
  //           SK + licenseInfo.quantity +
  //           SK + licenseInfo.timestamp + SK
  //           var _key = crypto.createHash('sha1').update(base).digest('hex').toUpperCase()
  //           if (_key !== licenseInfo.licenseKey) {
  //             reject('Invalid license key')
  //           } else {
  //             // Server check
  //             $.post(app.config.validation_url, {licenseKey: licenseInfo.licenseKey})
  //               .done(data => {
  //                 resolve(data)
  //               })
  //               .fail(err => {
  //                 if (err && err.status === 499) { /* License key not exists */
  //                   reject(err)
  //                 } else {
  //                   // If server is not available, assume that license key is valid
  //                   resolve(licenseInfo)
  //                 }
  //               })
  //           }
  //         }
  //       }
  //     } catch (err) {
  //       reject(err)
  //     }
  //   })
  // }
```

## 重新打包

修改完以后重新需要打包。

打包命令:

```bash
asar p app app.asar
```

然后将打包后的文件和原来的app.asar进行替换就可以了