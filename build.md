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

### electron 25.4.0 in mac
> npm install --save-dev electron@25.4.0
> npm install --save-dev @electron/rebuild@3.6.0
> ./node_modules/.bin/electron-rebuild

**arm的mac新机器出现的问题：**
```
pip install setuptools出现报错：
error: externally-managed-environment
```

使用brew安装***brew install python@3.9***,
安装到/usr/local/Cellar/python@3.9/3.9.25/bin下，

已经安装了其他版本了，直接使用
echo 'export PATH="/usr/local/Cellar/python@3.10/3.10.5/bin:$PATH"' >> ~/.zshrc
echo 'alias python=python3' >> ~/.zshrc
echo 'alias pip=pip3' >> ~/.zshrc

source ~/.zshrc

***参考安装配置步骤：***
```bash
# 2. 安装Homebrew版Python 3（推荐3.9.19，适配electron-rebuild）
brew install python@3.9

# 3. 配置zsh优先使用Homebrew Python（关键：避免指向系统Python）
echo 'export PATH="/usr/local/Cellar/python@3.9/3.9.25/bin:$PATH"' >> ~/.zshrc
echo 'alias python=python3' >> ~/.zshrc
echo 'alias pip=pip3' >> ~/.zshrc

# 4. 生效配置
source ~/.zshrc

# 5. 验证（必须输出Homebrew路径，而非/System/Library/...）
which python3 # 输出：/usr/local/Cellar/python@3.9/3.9.25/bin/python3
which pip3 # 输出：/usr/local/Cellar/python@3.9/3.9.25/bin/pip3

# 6. 现在安装依赖无限制
pip3 install setuptools distutils node-gyp
```

### electron 25.4.0 in windows
在vscode的终端执行：
1. 安装python3.7
2. npm install --save-dev electron@25.4.0
3. 执行： npm install --save-dev @electron/rebuild@3.6.0
4. 执行x64： .\node_modules\.bin\electron-rebuild.cmd -a x64
5. 执行ia32： .\node_modules\.bin\electron-rebuild.cmd -a ia32