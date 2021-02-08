# Git

## 1.什么是Git?

- 版本控制软件

- 分布式版本控制软件

## 2. 命令

- git add：将本地文件增加到暂存区
- git commit：将暂存区的内容提交到本地仓库（本地分支，默认==master==分支）
- git push：将本地仓库的内容推送到远程仓库（远程分支）
- git pull：将远程仓库的内容拉取到本地仓库

## 3. 安装Git

- 安装时：Use git from git bash only..,其他默认下一步
- 配置path：E:\programs\Git\bin
- 配置Git：右键Git Bash
  - 配置用户名和邮箱
  
  - ```shell 
git config --global user.name "codehuan"
    git config --global user.email "1730272469@qq.com"
    ```
  
  - 查看C:\Users\ZhangHuan\.gitconfig

### 3.1 搭建Git仓库（远程仓库）：

0. 统一的托管网站（https://github.com）

1. 为了在本地和远程仓库之间进行免密钥登录，可以配置ssh

2. 配置ssh：先生成本地的ssh

3. ```java
   ssh-keygen -t rsa -C 1730272469@qq.com  一直回车
   ```

5. 将本地生成的id_sra_pub内容复制到远程的key中

   1. 首先登录Github账号

   2. 点击头像的Settings

   3. 然后点击SSH

   4. 点击New SSH title任意，在key中将生成的ssh复制到文本框中

   5. 测试连通性

      ```java
      ssh -T git@github.com
      ```

      - 如果本地和远程成功通信，则可以在/.ssh目录中发现know_hosts文件
      
- 如果失败：多尝试几次、检查回车符
  ```shell
  #本地项目-远程项目关联
git remote add origin git@github.com:zh1730272469/Study.git
  ```
  

## 4. 发布项目

#### 4.1 第一次发布项目

```shell
git add  
git commit -m "注释内容"
git push -u origin master
```

#### 4.2 第一次下载项目

```shell
 git clone git@github.com:zh1730272469/mygit.git
```

#### 4.3 提交（本地-远程）

```java
git add .
git commit -m "注释内容"
git push -u origin master （第一次提交需要 -u之后就不再需要）
```

#### 4.4 更新（远程-本地）

```shell
git pull
```

## 5. 多人开发（分支）

### 5.1 克隆项目到本地

```shell
git clone [仓库路径]	
```

### 5.2 创建分支

```shell
git branch [分支名称]
```

### 5.3 查看本地分支

```shell
git branch -a
```

### 5.4 切换分支

```shell
git checkout [分支名称]
```

### 5.5 将新分支推送到GitHub/Gitee

```shell
git push origin [branch name]
```

### 5.6 更新远程分支列表

```shell
git remote update origin --prune
```

### 5.7 删除分支

```shell
# 删除
git branch -d [分支名称]
# 删除远程分支
git push origin --deleye [分支名称]
```

