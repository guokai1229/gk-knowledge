# git设置自动提交

## 创建shell脚本

```
#!/bin/sh
cd /opt/BOCO/sxboco

ls_date=`date +%Y-%m-%d-%H:%M:%S`

git add -A
git commit -a -m "$ls_date--自动提交"
git push
```

## 设置定时任务

```
05 03 * * *     /opt/BOCO/shell/back_sxboco.sh > /opt/BOCO/shell/back_sxboco.log 2>&1
```