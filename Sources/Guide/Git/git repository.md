## 空仓库: 向一个空仓库中条件文件

在网页端新建好一个空仓库后，接下来在待上传的目录下输入：

``` shell
echo "Message......" >> ./README.md
git init
git add . && git commit -m "first commit" 
# 也可以使用git commit -a -m "firestorm commit"， 直接上传，不必add .
git branch -M main
git remote add origin git@xxx.com......
git push -u origin main
```

## 已在本地存在的仓库

``` shell
git remote add origin git@github.com:xxx.git
git branch -M main
git push -u origin main
```
