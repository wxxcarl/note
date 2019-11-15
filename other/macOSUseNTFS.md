# 如何打开Mac OSX原生的读写NTFS功能

- 终端 `diskutil list`
- 找到U盘的Volume Name， 比如`BOOTCAMP`
- 更新 /etc/fstab文件  `sudo nano /etc/fstab`
- 写入 `LABEL=BOOTCAMP none ntfs rw,auto,nobrowse`

    ##### BOOTCAMP换成自己的卷名。如果你的名字里面有空格键，就需要用\040代替空格键，如：`BOOT/040CAMP`
    ##### Ntfs rw表示把这个分区挂载为可读写的ntfs格式，最后nobrowse非常重要，因为这个代表了在finder里不显示这个分区，这个选项非常重要，如果不打开的话挂载是不会成功的

-  Ctrl + X之后按Y保存

- 创建快捷方式 `sudo ln -s /Volumes/BOOTCAMP ~/Desktop/My Disk` 或者文件夹 `sudo ln -s /Volumes ~/Desktop/My Disks`


<style>
    .page-header {
        display: none;
    }
</style>