##Git常用命令

    git reflog 历史操作
    git reset --hard HEAD^ 回退到上次提交
    git reset --hard commit_id 历史版本间跳转
    git checkout -- filename 回滚到上次git commit / git add 的状态
    git push -u origin master

###fork管理
    1. 添加master的源地址，并把本地和源合并
    git remote add upstream git@gitlab.malong.com:malong/wlm-ts-api.git
    git remote -v
    git fetch upstream
    git merge upstream/master

###分支操作
    git checkout -b dev  创建并且换到分支 dev  -b
    = git branch dev; git checkout dev

    git branch 查看分支
    git branch <name> 创建分支
    git checkout <name> 切换分支
    git checkout -b <name> 创建并切换分支
    git branch -d <name> 删除分支

    git log --graph --pretty=oneline --abbrev-commit  查看分支合并图



###新建
    …or create a new repository on the command line

    echo "# git_test" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin git@github.com:sven9527/git_test.git
    git push -u origin master


    …or push an existing repository from the command line
    
    git remote add origin git@github.com:sven9527/git_test.git
    git push -u origin master


    …or import code from another repository

    You can initialize this repository with code from a Subversion, Mercurial, or TFS project.
