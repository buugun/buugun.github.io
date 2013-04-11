---
layout: post
category : git and github
tagline: "caijinlin's blog"
tags : [git]
---
{% include JB/setup %}


the two ways of Creating the respoitories

###one\(from remote to local)

<pre class="prettyprint linenums">
$ git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com 
$ cd USERNAME.github.com  
$ git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git 
$ git push origin master 
</pre>


###two\(from local to remote)
<pre class="prettyprint linenums">
mkdir prog_name \(创建项目目录)
cd prog_name \(进入项目目录)
git init touch README\(项目初始化)
git add README \(将文件添加到索引)
git commmit -m 'first commit' \(本地提交)
git remote add origin git@github.com:username/prog_name.git \(增加远程服务器代码库地址)
git push origin master  \(将本地代码提交到远程服务器)
</pre>