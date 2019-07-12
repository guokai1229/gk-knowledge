# linux下解压被分割的zip文件

由于网络不稳定以及文件上传比较慢问题，我们经常会把文件分割成多个文件，然后逐个上传到服务器上

```
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z01
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z02
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z03
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z04
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z05
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z06
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z07
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z08
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z09
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z10
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z11
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z12
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z13
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z14
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z15
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z16
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z17
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z18
-rw-rw-r-- 1 weblogic weblogic  50000000 02-06 16:18 weblogic-linux.z19
-rw-rw-r-- 1 weblogic weblogic  14979127 02-06 16:18 weblogic-linux.zip
```

就像上面一样，如果像windows一样用工具解压，就会报错

```
$ unzip weblogic-linux.zip

Archive:  weblogic-linux.zip
warning [weblogic-linux.zip]:  zipfile claims to be last disk of a multi-part archive;
  attempting to process anyway, assuming all parts have been concatenated
  together in order.  Expect "errors" and warnings...true multi-part support
  doesn't exist yet (coming soon).
caution: filename not matched:  weblogic-linux.zip
```

那正确的解压方法是怎么样的呢


1.合并文件

`#> cat weblogic-linux.* > weblogic-linux_all.zip`

2.尝试修复已损坏的压缩文件。

`#> zip -F weblogic-linux_all.zip`

3.正常解压

`#> unzip weblogic-linux_all.zip`