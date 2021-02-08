# TypeScript

## 1. 安装

```shell
# 安装
npm install -g typescript
# 查看版本号
tsc -v
# 将ts编译为js
tsc .\01.ts
```

## 2. 配置vscode自动编译

```shell
# 生成 tsconfig.json 文件
tsc --init
# 修改 tsconfig.json
```

>-  ts的文件中直接书写js代码，那么在html文件中直接引入ts文件，在谷歌的浏览器中是可以直接使用的
>- ts文件中有了ts语法代码，那么就需要把这个ts文件编译成js文件，在html文件中引入js的文件来使用
>- ts文件中的函数中的形参，如果使用的了某个类型的进行修饰，那么最终在编辑js文件中是没有这个类型的
>- ts文件中的变量使用的是let进行修饰，编译的js文件中的修饰符就变成了 var 了

