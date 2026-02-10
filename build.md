# 使用electron-builder编译electron对应的版本

## electron/node对应的abi查询
> npm install --save-dev node-abi
```js
const nodeAbi = require('node-abi')
console.log( nodeAbi.getAbi('24.8.8', 'electron') );
console.log( nodeAbi.getAbi('20.15.0', 'node') );

console.log( nodeAbi.getTarget('116', 'node') );
console.log( nodeAbi.getTarget('116', 'electron') );
```

## 编译electron **https://github.com/octalmage/robotjs/wiki/Electron**
### 24.8.8(成功在mac上编译)
####
> 使用nodejs 14.17.6
> mv package.json package.json_def
> mv package.json_114_24.8.8 package.json
> npm install
> ./node_modules/.bin/electron-rebuild
> 在bin目录下


#### 具体修改
> npm install --save-dev electron-rebuild@3.2.9
> npm install electron@24.8.8
> npm install --save nan@2.17.0
> 添加执行脚本，貌似不需要
```json
{
    "scripts": {
        "rebuild": "npm rebuild --runtime=electron --target=24.8.8 --disturl=https://atom.io/download/atom-shell --abi=114"
    }
}
```
> ./node_modules/.bin/electron-rebuild

### 如何放到引入项目下
> 将编译出的*.node按目录格式（build/Release/*.node）放好，打包成tar.gz
> tar zcvf robotjs-v0.6.0-electron-v${abi}-${target}.tar.gz build

> win32_x64: tar zcvf robotjs-v0.6.0-electron-v114-win32-x64.tar.gz build
> win32_ia32: tar zcvf robotjs-v0.6.0-electron-v114-win32-ia32.tar.gz build
> darwin_x64: tar zcvf robotjs-v0.6.0-electron-v114-darwin-x64.tar.gz build
> linux_x64: tar zcvf robotjs-v0.6.0-electron-v114-linux-x64.tar.gz build

### windows下24.8.8
> 先安装visual studio, node14.x.x, python3.12
> 去掉package.json里面的"targetpractice": "0.0.7"，会依赖下载其他版本的electron
> 不要执行： npm install
> 执行： npm install --save-dev @electron/rebuild@3.6.0
> 会自动安装electron及其他模块
> 执行x64： .\node_modules\.bin\electron-rebuild.cmd -a x64
> 执行ia32： .\node_modules\.bin\electron-rebuild.cmd -a ia32

#### 问题1`python没找到distutils`
> pip install setuptools

### electron 25.4.0
> npm install --save-dev electron@25.4.0
> npm install