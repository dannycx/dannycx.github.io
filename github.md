# github上传

## 1.安装git
## 2.配置相关信息：
- git config --global user.name "name"
- git config --global user.email "email"
SSH KEY：（加密传输）
linux系统下：用户home目录，
- ls -al查看所有文件（.ssh默认为隐藏文件）
- cd .ssh进入目录，ls查看是否有id_rsa.pub文件，该文件中存储ssh key密钥
- 若没有通过该命令生成密钥：ssh-keygen -t rsa -C "email",一路回车即可
- 生成后将该密钥设置到github，点击github中的settings，然后打开SSH keys菜单， 点击Add SSH key新增密钥，将你的密钥拷贝过去，定义一个标题即可。
## 3.本地建立一个文件夹用来存放从github上clone下的项目
- git init：初始化本地仓库（可有可无）
- 克隆项目：git clone https://github.com/dannycx/knowledge.git（地址可去github复制）
- git add file：
- git commit -m "info" :提交代码
- 关联远程仓库：git remote add origin https://github.com/dannycx/knowledge.git
- 代码合并【注：pull=fetch+merge]：git pull --rebase origin master
- 上传代码：git push -u origin master。

# 上传完毕，可以去github查看上传结果，就是这么简单
