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

```
npm run ng build -- --prod --base-href "https://dakarinternational.github.io/DaKar-ngFront/"
```

or more important if you want to use your domain:    
```
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


## Sources

https://angular.io/guide/deployment
  
https://dev.to/apdharshi/deploying-your-angular-application-to-github-pages-4laf
  
https://help.github.com/en/github/working-with-github-pages/securing-your-github-pages-site-with-https
  

Other things to explore:  
  
https://www.codementor.io/@landonpatmore/how-to-setup-a-static-website-using-github-pages-and-cloudflare-with-your-own-domain-name-jb99nbuoe
  
https://hackernoon.com/set-up-ssl-on-github-pages-with-custom-domains-for-free-a576bdf51bc  
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMzg1MDMxNzVdfQ==
-->