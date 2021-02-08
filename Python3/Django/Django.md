# Django入门

## 1. 安装

```shell
# 不输入版本号 则默认下载最新版本
pip install Django==1.9

# 如果下载失败 请使用镜像下载
pip install Django -i https://pypi.douban.com/simple

#python安装库一般会用到pip，但是更新时经常会碰到超时的情况，在这里介绍一种加快更新的方法：
#建议先更新pip工具：
pip3 install -i https://pypi.douban.com/simple --upgrade pip
#打开cmd，进入python环境，在python环境下执行命令
python
import django
django.get_version()
# 显示 django版本 则django 安装成功

```

### 2. pip命令下载速度慢

>1. windows下在个人用户目录下新建`pip`文件夹，再新建`pip.ini`
>
>2. 在`pip.ini`文件中配置源地址
>
>   ```shell
>   [global]
>   #index-url 后面的地址可以替换为其他镜像地址
>   index-url = https://pypi.tuna.tsinghua.edu.cn/simple
>    
>   [install]
>    
>   trusted-host=mirrors.aliyun.com
>   ```
>
>   