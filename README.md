# codeandbugs.com

Static site created with [Hugo](https://gohugo.io/).

Visit [https://www.codeandbugs.com](https://www.codeandbugs.com).

## Theme

Initialized as follows:

```
git remote add -f theme https://github.com/drostamloo/hugo-theme-texify.git
git merge -s ours --no-commit --allow-unrelated-histories theme/master
git read-tree --prefix=themes/hugo-theme-texify -u theme/master
git commit -m "merge theme subtree into themes/hugo-theme-texify"
```

Update:

```
git remote add -f theme https://github.com/drostamloo/hugo-theme-texify.git
git pull -s subtree themes/hugo-theme-texify master
```
