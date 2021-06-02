### 如何动态注入vue环境变量
&emsp;在vue开发过程中我们虽然可以在环境变量文件`.env`中定义`VUE_APP_XXX`，对app注入所需要的常量。比如常见的请求地址。但是这个环境变量如果是多变的呢？  
&emsp;我们完全可以把全部的可能性都定义成环境变量：比如VUE_APP_(A-Z)。  
&emsp;我们其实也可以通过外部传递参数的方式来注入环境变量，下面开始具体介绍。  
- 方法总共2步：
  - 第一步，我们首先通过终端对npm script传递参数。  
  比如我们有如下的npm script  

    ```javascript
      "scripts": {
        "grunt": "grunt",
        "server": "node server.js"
      }
    ```  
    我们可以通过`npm run server -- --port=1337`的方式去向`node serve.js`这条指令传递参数，即结果为`node serve.js --port=1337`，传递了`port`为`1337`这个参数.
  - 第二步就是通过`webpack`的`definePlugin`来进行注入。在vue中的操作方式即在`vue.config.js`文件中进行。例如
    
    ```javascript
    const { argv } = require('yargs') 
    // https://www.npmjs.com/package/yargs 
    // 这里引入的yargs是一个方便获取命令行参数的包，具体见url
    ...
    ...
    ...
    module.exports = {
      chainWebpack: (config) => {
        const specifiedPort = argv.port //从argv中获取到上文中我们传入进来的port
        config.plugin('define').tap(options => {
          // 注入环境变量VUE_APP_API_PORT
          options[0]['process.env'].VUE_APP_API_PORT = `"${specifiedPort}"`
          return options
        })
      }
    }
    ```
> 参考自：
> - https://forum.vuejs.org/t/how-to-pass-command-line-parameters-to-vue/38701/13  
> - https://stackoverflow.com/questions/11580961/sending-command-line-arguments-to-npm-script