### 如何自己生成及使用本地的npm包？
- 通过使用`npm pack`来生成`.tgz`的压缩包
>可以在`package.json`里指定`main`或者进入打包后的目录执行npm pack
- 得到.tgz的包后去需要使用的项目中`package.json`里对应的依赖下(`dependencies`或者`devDependencies`)中写入`"package的包名": "file:./xxxxx.tgz"`从而以通过`file`这个规则来引用，这样在执行npm install的时候就会从本地`.tgz`包里去安装。
> 参考自 https://stackoverflow.com/questions/55560791/build-and-use-npm-package-locally