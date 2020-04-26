### 安装并配置samba
#### 安装Samba
```
sudo apt-get install samba    
sudo apt-get install samba-common-bin
```
#### 配置Samba
1、编辑配置文件
````
sudo nano /etc/samba/smb.conf
````
2、修改配置文件
找到Share Definitions节[homes]部分：将`read only = yes` 行改为`read only = no`
在文件末尾加入如下内容：
```
[MyShare] 
#MyShare为访问链接的目录

comment = My Public Storage  
# 共享文件夹说明 

path = /Samba 
# 共享文件夹目录

browseable = yes  
# 可被其他人看到资源名称（非内容）  

writable = yes  
# 可写  

create mask = 0777 
# 新建文件的权限为 777 

directory mask = 0777 
#新建目录的权限为 775 

guest ok = yes 
# guest访问，无需密码
```
3、添加Pi用户
```
sudo smbpasswd -a pi
```
#### 启动服务
```
sudo /etc/init.d/smbd restart
sudo /etc/init.d/nmbd restart
```

### 创建共享文件目录
```
cd /
sudo mkdir  Samba
sudo chmod -R 777 Samba
```

### 配置PC端的来宾访问
运行组策略编辑器`gpedit.msc`
```
计算机配置 - 管理模板 - 网络 - Lanman 工作站 - 启用不安全的来宾登录
```
或者修改注册表
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters] 
"AllowInsecureGuestAuth"=dword:1
```
最后运行 `\\IP地址\MyShare` 即可访问。
