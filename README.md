# test_sparse
testing sparse checkout

This is the best workaround if the repo is huge and you only need one subfolder.
You still clone the repo metadata but don’t download the entire working tree.

Steps (command line):
```
git clone --filter=blob:none --no-checkout https://github.com/client/their-repo.git  
cd their-repo  
git sparse-checkout init --cone  
git sparse-checkout set "R code"  
```   

This makes Git only download the R code directory, even though the repo exists locally.
