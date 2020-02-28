# GitHub注意事项

## git地址
git@github.com:yiyu18048649256/demo.git

## git上传到hy_dev方法

1. git status (显示为红色)
2. git add -A(添加更改部分)
3. git status(显示为绿色)
4. git commit -m "XXXX" (XXXX为文字说明)
5. git pull origin master (拉取远端主分支master的内容)
6. git push origin lt_dev (推送本地代码到lt_dev，也就是你的分支)    

## 提醒
执行完上传到lt_dev成功之后，要给我说。我好进行合并。
每次在写之前 先执行git pull origin master(拉取远端主分支，因为这样可以避免我已经提交过的代码，你没有拉取，我们代码不同步，然后你上传的时候就出现出图，就要版本回退。)
拉取了远端主分支代码之后再开始写你本地的代码，然后再进行git上传到lt_dev。然后叫我给你合并。