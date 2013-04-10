---
layout: post
title: how to  build a blog site by jekyll
category : git 
tagline: "caijinlin's blog"
tags : [git, github, jekyll]

---
{% include JB/setup %}

I built a blog powered by wordpress [caijinlin's blog](http://caijinlin.tk).

but since knowing about the git,

I learn to build a blog site with git. after four day's hard work ,

I am familar with working principle of git gradually. accroding to the 

tips,  do this step now. maybe there are some shortcoming. but I will 

continue  to make better. below is agbout the process that I built the 

blog site.[caijinlin's blog](https://caijinlin.github.com)

 

<!--more-->


### Examples

This website is created with Jekyll. [Other Jekyll websites](https://github.com/mojombo/jekyll/wiki/Sites).

### Tips(the framework)

https://github.com/mojombo/jekyll

https://github.com/plusjade/jekyll-bootstrap

https://github.com/imathis/octopress





###first steps.

Register [github](https://github.com)

[the reference ](http://www.worldhello.net/gotgithub/02-join-github/010-account-setup.html)



###second steps

Create the new Item  

by git bash:

$ git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
$ cd USERNAME.github.com
$ git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git
$ git push origin master

so you can go to "https://USERNAME.github.com".



###third steps

Modify the configuration file named _config.yml


#
title : Jekyll Bootstrap
tagline: Site Tagline
author :
  name : Name Lastname
  email : blah@email.test
  github : username
  twitter : username
  feedburner : feedname

# The production_url is only used when full-domain names are needed




###fouth steps

use themes

you can find some useful themes [there](https://github.com/jekyllbootstrap)




###fifth steps

Create index.md

layout: page

title: my  blog
 
<p>最新文章</p>
 
{% for post in site.posts %}
<li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>.</li>
{% endfor %}
 

 
###publish your article

you can write the article with Sublime Text2.

for example:

 
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
 
{% include JB/setup %}
 
 the content



then.

$ git add .
$ git commit -m "first post"
$ git push origin master


#######[some more details](http://jekyllbootstrap.com/)



 