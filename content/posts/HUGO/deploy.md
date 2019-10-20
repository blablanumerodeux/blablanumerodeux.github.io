---
title: "HUGO"
date: 2019-08-17T20:53:32-04:00
draft: false
---

## HUGO


### create a new site

`hugo new site me`

### install theme
go here [minimal](https://themes.gohugo.io/minimal/)  and follow the instructions:
```
git submodule add https://github.com/calintat/minimal.git themes/minimal
git submodule init
git submodule update
git submodule update --remote themes/minimal
cp themes/minimal/exampleSite/config.toml 
```
in the file config.toml set all your params, and: 
```
baseurl = "http://examplesite" <- Change to https://yourgithubusername.github.io
```

### push on your github

On branch *master* you have the public folder (built version)  
And on the *hugo* branch you put the hugo project  

Checkout the hugo branch

`hugo serve` -> to test localy  
`hugo -d ../blablanumerodeux.github.io/` -> to build the static version in the folder blablanumerodeux.github.io

blablanumerodeux.github.io folder is used to push the public folder (wich contains the static version of the website) without the hugo files (we could have make 2 different repos and a submodule)


`git push origin master -u`
`git push origin hugo -u`


### some good themes: 
https://themes.gohugo.io/hugo-coder/
https://themes.gohugo.io/minimal/


### sources: 
https://dev.to/michalbryxi/simplify-pushing-to-git-32g6
https://dev.to/dgavlock/creating-a-hugo-site-on-github-pages-3cjo
https://themes.gohugo.io/minimal/
https://github.com/calintat/minimal/
https://gohugo.io/hosting-and-deployment/hosting-on-github/
https://gohugo.io/hosting-and-deployment/hosting-on-github/#use-a-custom-domain
https://themes.gohugo.io//theme/minimal/post/
  
[circleci git](https://support.circleci.com/hc/en-us/articles/360018860473-How-to-push-a-commit-back-to-the-same-repository-as-part-of-the-CircleCI-job)  
  
[circleci ssh key](https://circleci.com/docs/2.0/gh-bb-integration/#creating-a-github-deploy-key)  
  
[hugo docker image](https://hub.docker.com/r/jguyomard/hugo-builder/)


### inspiration 

https://tomlicha.github.io/
https://github.com/tomlicha/tomlicha.github.io


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwOTIzNDM4MDJdfQ==
-->