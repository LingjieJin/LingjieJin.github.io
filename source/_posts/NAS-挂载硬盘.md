---
title: NAS-挂载硬盘
date: 2022-10-23
tags: [NAS, nas]
---

## 参考文章
ubuntu新硬盘挂载及格式化NTFS[https://codeantenna.com/a/XkEEPnJY3Z]

---
## 格式化新硬盘

#### 查看硬盘信息指令
查看硬盘具体信息，包括分区等
> sudo fdisk -l

查看有多少个硬盘
> sudo lshw -c disk 
```
一般会显示: 
disk:0（设备名为 /dev/sda） 
disk:1（设备名为 /dev/sdb）
```

查看所有的存储介质的UUID
> sudo blkid

#### 对硬盘进行分区
1. 使用fdisk分区
    > sudo fdisk /dev/sdb
    ```
    输入m查看帮助信息，一般要对照帮助进行操作，避免出错；

    输入n新建分区；

    输入分区号1，然后输入大小,默认是一个分区，全部的空间大小；

    然后查看要创建的分区表，这时还没有创建，按w保存退出后才成功。

    可以再次执行 sudo fdisk -l 查看是否创建。
    ```

2. Fdisk最大只能创建2T分区的盘，硬盘容量大于2t的时候需要用parted进行分区。
    > parted /dev/sd***

    sample output:
    ```
    GNU Parted 3.2
    使用 /dev/sdb
    欢迎使用 GNU Parted! 输入 'help'可获得命令列表.
    (parted) mklabel
    新的磁盘标签类型？ gpt
    警告: The existing disk label on /dev/sdb will be destroyed and all data on this disk will be lost. Do you want to continue?
    是/Yes/否/No? Yes
    (parted) mkpart
    分区名称？  []? Data01
    文件系统类型？  [ext2]? ntfs
    起始点？ 0%
    结束点？ 100%
    (parted) print
    Model: ATA WDC WD40EZAZ-00S (scsi)
    磁盘 /dev/sdb: 4001GB
    Sector size (logical/physical): 512B/4096B
    分区表：gpt
    Disk Flags:

    数字  开始：  End     大小    文件系统  Name    标志
    1    1049kB  4001GB  4001GB  ntfs      Data01

    (parted) quit
    信息: You may need to update /etc/fstab.

    ```

#### 格式化分区
快速格式化为NTFS
> sudo mkfs.ntfs -f /dev/sda1

格式化为ext4
> sudo mkfs.ext4 /dev/sdb
> sudo mkfs -t ext4 /dev/sdb1

格式化为fat32
> sudo mkfs.vfat -F 32 /dev/sdb 

#### 测试挂载
> sudo mount /dev/sd*** /your-dir

#### 修改/etc/fstab
为了能够开机自动挂载硬盘，需要修改/etc/fstab文件

```
// 查看硬盘的UUID
ls -l /dev/disk/by-uuid

```

#### 不需要重启生效/etc/fstab
```sudo mount -a```

#### TIPS
1. 为了防止设置错误，建议先使用mount挂载硬盘，否则可能导致进不去系统，进入control-D模式。

2. 通过hdparam查看硬盘序列号
    > sudo hdparm -i /dev/sda
    
    sample output:
    ```
    /dev/sdb:

    Model=WDC WD40EZAZ-00SF3B0, FwRev=80.00A80, SerialNo=WD-WX62D12LLNL2
    Config={ HardSect NotMFM HdSw>15uSec SpinMotCtl Fixed DTR>5Mbs FmtGapReq }
    RawCHS=16383/16/63, TrkSize=0, SectSize=0, ECCbytes=0
    BuffType=unknown, BuffSize=unknown, MaxMultSect=16, MultSect=16
    (maybe): CurCHS=16383/16/63, CurSects=16514064, LBA=yes, LBAsects=7814037168
    IORDY=on/off, tPIO={min:120,w/IORDY:120}, tDMA={min:120,rec:120}
    PIO modes:  pio0 pio3 pio4
    DMA modes:  mdma0 mdma1 mdma2
    UDMA modes: udma0 udma1 udma2 udma3 udma4 udma5 *udma6
    AdvancedPM=no WriteCache=enabled
    Drive conforms to: unknown:  ATA/ATAPI-1,2,3,4,5,6,7

    * signifies the current active mode
    ```

---
## 挂载旧硬盘
