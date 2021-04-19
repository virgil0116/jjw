# 前端问题

* `Error: Node Sass does not yet support your current environment: OS X Unsupported architecture (arm64) with Unsupported runtime (88)`
  
    系统中没有安装node sass 或者版本太低。解决方案：
    1. `npm install --save node-sass`
    
    2. `npm uninstall --save node-sass   先卸载`
    
       `npm cache clean -f    清除缓存`
    
       `npm install --save node-sass 		安装node-sass模块`


