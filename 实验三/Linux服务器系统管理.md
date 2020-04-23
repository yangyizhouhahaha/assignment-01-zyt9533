# Linux服务器系统管理

## 实验目的

* 运用systemd进行开机自启动项管理

## 实验环境

* CentOS8
* asciinema

## 实验过程

* Systemd入门教程：命令篇

  [![asciicast](https://asciinema.org/a/323086.svg)](https://asciinema.org/a/323086)

* Systemd入门教程：实战篇

  [![asciicast](https://asciinema.org/a/323097.svg)](https://asciinema.org/a/323097)

## 自查清单

- 如何添加一个用户并使其具备sudo执行程序的权限？

  ```
   sudo adduser username
   sudo usermod -G sudo new
  ```

- 如何将一个用户添加到一个用户组？

  ```
  sudo usermode -aG groupname username
  ```

- 如何查看当前系统的分区表和文件系统详细信息？

  ```
   #查看当前系统的分区表（小于2T）
   sudo fdisk -l
   
   #查看文件系统详细信息
   df -a
  ```

- 如何实现开机自动挂载Virtualbox的共享目录分区？

  * 安装增强功能

    ```
    sudo apt install nfs-common
    sudo apt install cifs-utils
    sudo apt install virtualbox-guest-utils
    ```

  * 创建挂载文件夹 

    ```
    mkdir ~/shared
    ```

  * 挂载文件

    ```
    sudo mount -t vboxsf vbox_share ~/shared
    ```

  * 修改配置文件

    ```
     sudo vim /etc/fstab
     
     #配置内容
     vbshare ~/shared vboxsf defaults 0 0
    ```

  * 重启

    ```
    sudo reboot
    ```

- 基于LVM（逻辑分卷管理）的分区如何实现动态扩容和缩减容量？

  ```
  #查看硬盘分区情况
  sudo fdisk -1
  
  #扩容
  sudo lvextend -L +16M /dev/ubuntu-vg/root
  
  #缩容
  sudo lvreduce -L -16M /dev/ubuntu-vg/root
  ```

- 如何通过systemd设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本？

  * 修改networking.service

  ```
  #在service段添加ExecStartPost,ExecStopPost
  ExecStartPost=/bin/sh -c "echo up"
  ExecStopPost=/bin/sh -c "echo down"
  
  #重载
  systemctl daemon-reload
  ```

- 如何通过systemd设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现***杀不死***？

  ```
  #设置相关脚本配置文件中Restart = always
  
  #重新加载配置文件
  systemctl daemon-reload
  ```