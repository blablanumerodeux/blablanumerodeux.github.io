---
title: "Host your angular app on github"
date: 2020-03-31T20:53:32-04:00
draft: false
---

# Host your angular app on github
  

## CNAME record
  
On your domain provider, create a new CNAME record such as:   
 `dakar.project.lambla.eu.-> dakarinternational.github.io.` 
  
In your src folder create a CNAME file and write your custom domain in it.    
then, inside angular.json file add this line:       

```json
"assets": [  
"src/favicon.ico",  
"src/assets",  
"src/CNAME" // This is the change you need to make.  
],
```
  
https://github.com/tschaub/gh-pages/issues/236
  


## Angular build
  
```bash
npm run ng build -- --prod --base-href "https://dakarinternational.github.io/DaKar-ngFront/"
```

or more important if you want to use your domain:     
```bash  
npm run ng build -- --prod --base-href "https://dakar.project.lambla.eu/"
```  
  
otherwise you may have:    

`Mixed Content: The page at ‘<your_gpages_remote_url>’ was loaded over HTTPS, but requested an insecure stylesheet ‘<url_provided_with_http_prefix_while_deploying>/styles.acb808cb000123f5c6ec.css'. This request has been blocked; the content must be served over HTTPS.`

  
## Angular CLI deploy 
  
```bash
npx ngh --dir=dist/DaKar-ngFront  
or  
npx angular-cli-ghpages --dir=dist/homeless-front
```

https://medium.com/tech-insights/how-to-deploy-angular-apps-to-github-pages-gh-pages-896c4e10f9b4
  
## Github pages config
  

https://github.com/DaKarInternational/DaKar-ngFront/settings
    
in Settings -> Options -> Github Pages :    
put your custom domain such as :  *dakar.project.lambla.eu*    
enforce https.    
done    
  


## Github Actions setup


```
1.  First login to [https://github.com](https://github.com)
2.  Go to [https://github.com/setting/profile](https://github.com/setting/profile)
3.  Now click on to the `Developer settings`
4.  Click on `Personal access tokens` and generate a token
5. 1.  Go to your repository `Settings`
6.  Click on `Secrets`
7.  Click on `Add new secret`
8.  Put `ACCESS_TOKEN` for name and `your github token that you had copy` as value. Now click on `Add secret` button. A secret with name `ACCESS_TOKEN` is saved in your repository.
9. in your repository's `Action` tab, set up workflow using these code

main.yaml
```

```yml

```yaml
name: CI

on: [push]

jobs:
  build:

    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@v1
    - name: Use Node.js 8.15.1
      uses: actions/setup-node@v1
      with:
        node-version: 8.15.1
    - name: Build and Deploy
      uses: JamesIves/github-pages-deploy-action@releases/v2
      env:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        BASE_BRANCH: develop
        BRANCH: gh-pages
        FOLDER: dist/homeless-front
        BUILD_SCRIPT: npm install && npm run ng build -- --prod --base-href "https://homeless.project.lambla.eu/"
```





https://uxworks.org/how-to-deploy-angular-app-on-github-pages-using-github-actions


## Sources
  
https://angular.io/guide/deployment
    
https://dev.to/apdharshi/deploying-your-angular-application-to-github-pages-4laf
    
https://help.github.com/en/github/working-with-github-pages/securing-your-github-pages-site-with-https
  
  
Other things to explore:    
    
https://www.codementor.io/@landonpatmore/how-to-setup-a-static-website-using-github-pages-and-cloudflare-with-your-own-domain-name-jb99nbuoe
    
https://hackernoon.com/set-up-ssl-on-github-pages-with-custom-domains-for-free-a576bdf51bc    


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwOTYzMzg1MjMsLTIwODM3MTQ5MTUsLT
IwMzg1MDMxNzVdfQ==
-->