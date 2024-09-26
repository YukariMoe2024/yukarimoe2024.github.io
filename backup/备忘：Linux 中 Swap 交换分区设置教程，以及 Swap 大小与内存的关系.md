# 备忘：Linux 中 Swap 交换分区设置教程，以及 Swap 大小与内存的关系  

## Swap 分区是什么

Swap（Swap 分区、Swap 内存），中文名是交换分区，类似于 Windows 中的虚拟内存，就是当内存不足的时候，把一部分硬盘空间虚拟成内存使用，从而解决内存容量不足的情况。  

因此，Swap 分区的作用就是牺牲硬盘，增加内存，解决内存不够用或者爆满的问题。  

## Swap 大小选择  

Swap 合理的大小是与服务器的物理内存有关的,以下是个人推荐 Swap 大小：  

1. 内存 ≤ 4g：Swap 至少 4G  
2. 内存 4~16G：Swap 至少 8G  
3. 内存 16G~64G：Swap 至少 16G  
4. 内存 64G~256G：Swap 至少 32G  

## Swap 分区设置教程  

1. 查看 Linux 当前内存使用情况：  

```Bash
free -m
```

如果是增加 swap 分区，则先把当前所有分区都关闭了：  

```Bash
swapoff -a
```

2. 创建要作为 Swap 分区文件（其中 `/var/swapfile` 是文件位置，`bs*count` 是文件大下，例如以下命令就会创建一个 4G 的文件）：  

```Bash
dd if=/dev/zero of=/var/swapfile bs=1M count=4096
```

3. 建立 Swap 文件系统  

```Bash
mkswap /var/swapfile
```

4. 启用 Swap 分区：  

```Bash
swapon /var/swapfile
```

5. 查看是否创建成功  

```Bash
free -m
```

如果有 Swap 就说明创建成功了。  

6. 设置开机自动启用 Swap  

打开`/etc/fstab`，加入以下内容：  

```fstab
/var/swapfile swap swap defaults 0 0
```