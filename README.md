# test_sparse
testing sparse checkout

This is the best workaround if the repo is huge and you only need one subfolder.
You still clone the repo metadata but don’t download the entire working tree.

Up-to-date method (starting from scratch, without an existing project):

### 1. Create folder (or folder/subfolder) in Github and add a README.md in it
  * In this case, `R-scripts/test-package` (`test-package` will be the name of the R project)

### 2. Create a folder on your laptop where the repo will live
  * In this case, `C:\Data\R_test\test-sparse-checkout`

### 3. In the terminal, go to `test-sparse-checkout`
  * For instance, open the terminal in RStudio and navigate there by `cd test-sparse-checkout`

### 4. Run code below (requires Git version >= 2.25)  
```
git clone --filter=blob:none --sparse https://github.com/DagHjermann/test_sparse.git
cd test_sparse
```
* sparse automatically enables sparse‑checkout mode  
* filter=blob:none avoids downloading file contents (partial clone)  
* No need for sparse-checkout init (as in older workflows)  
* Fully supported in Git 2.25+ and recommended in [2026 sparse‑checkout guides](https://oneuptime.com/blog/post/2026-01-24-git-sparse-checkout/view).
  
### 5. Run
```
git sparse-checkout set "R-scripts/test-package"
```

### 6. Then in RStudio:  
* File → New Project → Existing Directory  
* Point to the "lowest" folder created, i.e. `C:\Data\R_test\test-sparse-checkout\test_sparse\R-scripts\test-package`  
* RStudio will create the .Rproj file (`test-package.Rproj`) inside `test-package`
* RStudio also creates a .gitignore file *at root level of the git folder*, i.e. in `C:\Data\R_test\test-sparse-checkout\test_sparse`  

### 7. Move the .gitignore file to `R-scripts\test-package`  
* NOTE: if the got repo already contained a gitignore file, it may have been overwritten by RStudio! You may want to check this with `git log --test_sparse/.gitignore`  
* If this is not a problem, just move the file - this means it will only apply to your R project
* If this *is* a problem, restore the gitignore. Then make a new empty .gitignore in test-package and add
```
.Rproj.user/
.Rhistory
.RData
.Ruserdata
```

### 8. Make your first commit and push  
* In RStudio, go to Git tab  
* Tick off the file `test-package.Rproj` and `.gitignore`  
*  Commit with git message "Initiate R project" for instance
*  Push by clicking push button

### 9. Test it with an R file:
* Create a new R file (File > New File)
* Write something in it (code or whatever)
* Save as testfile1.R  
 
### 10. Test it with a Quarto file (File > New File)  
* There is automatically content, but in order for the results to be shown, you must add `execute: keep-md:true` to the header, so the header looks like this:
```
---
title: "test sparse git"
format: html
editor: visual
execute:
  keep-md: true
---
```
* Also add a plot by adding an R chunk with the following code:
```
x <- 4 + rnorm(200)
y <- 10 + 0.5*x + rnorm(n=200,sd=0.2)
plot(x, y)
```
* Save as testfile2.qmd
* Click render - this creates two more files (with extensions html and html.md) and a folder `testfile2_files` (containing both javascript files and your figure)   
* Open .gitignore and add the following four lines, and save it. This means these types of file will not be added to git or github. This is something we typuically want, as HTML files are usually not so useful on github (they are big and cannot be "diff'ed").  
```
*.html
*.js
*.css
*.woff
```
* If you now update the Git pane, there should now be *two* 'testfile2' files to commit: testfile2.qmd`and `testfile2.html.md`, plus `.gitignore`. Tick of all off them.
* Also tick off the folder `testfile2_files`, which automatically ticks off just one file: `testfile2_files/figure-html/unnamed-chunk-3-1.png`
* Commit with some git message, and then push

### 11. Now check your github repo online
* Try to open `testfile2.html.md`. THis shold now contain both code and results, including your figure.

## Old version (first suggestion from Copilot)  
```
git clone --filter=blob:none --no-checkout https://github.com/client/their-repo.git  
cd their-repo  
git sparse-checkout init --cone  
git sparse-checkout set "R code"  
```   

This makes Git only download the R code directory, even though the repo exists locally.

