1. 模块顶层将reset_rtl_0信号拉高，设置ext_reset_in复位为0
        assign reset_rtl_0 = 1'b1;
    2. 编译完成后测试读写PL DDR测试通过
        root@7z100_11_1:~# devmem 0x60000000 32
        0xFFFFEBFB
        root@7z100_11_1:~# devmem 0x60010000 32
        0xFFFFFBFF
        root@7z100_11_1:~# devmem 0x60000000 32
        0xFFFFEBFB
        root@7z100_11_1:~# devmem 0x60000000 32 0x11223344
        root@7z100_11_1:~# devmem 0x60000000 32
        0x11223344
        root@7z100_11_1:~# devmem 0x70000000 32
        0xFEFFFFFF
        root@7z100_11_1:~# devmem 0x70000000 32 0x12345678
        root@7z100_11_1:~# devmem 0x70000000 32
        0x12345678
        root@7z100_11_1:~#
    3. NVMe 挂盘读写测试正常
        root@7z100_11_1:~# df -h
        Filesystem                Size      Used Available Use% Mounted on
        devtmpfs                435.9M      4.0K    435.9M   0% /dev
        tmpfs                   503.5M     84.0K    503.4M   0% /run
        tmpfs                   503.5M     44.0K    503.4M   0% /var/volatile
        /dev/mmcblk1p1            6.9G    336.4M      6.2G   5% /run/media/mmcblk1p1
        /dev/nvme0n1              1.8T     67.3M      1.7T   0% /mnt

        root@7z100_11_1:~# time dd if=/dev/urandom of=/mnt/1.dat bs=1M count=1024
            1024+0 records in
            1024+0 records out

            real    0m37.386s
            user    0m0.001s
            sys     0m37.327s
        
        reboot

        root@7z100_11_1:/run/media/nvme0n1# ls -alh
            total 1048600
            drwxr-xr-x    3 root     root        4.0K Mar  4 09:28 .
            drwxr-xr-x    4 root     root          80 Jan  1  1970 ..
            -rw-r--r--    1 root     root        1.0G Mar  4 09:28 1.dat
            drwx------    2 root     root       16.0K Mar  4 09:17 lost+found

