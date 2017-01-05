changwentao.com
=================

My Website Repository

## important step
0.install node.js and git
 
1.$npm install -g hexo-cli

2.$<br>
npm install hexo-generator-index --save<br>
npm install hexo-generator-archive --save<br>
npm install hexo-generator-category --save<br>
npm install hexo-generator-tag --save<br>
npm install hexo-server --save<br>
npm install hexo-deployer-git --save<br>
npm install hexo-deployer-heroku --save<br>
npm install hexo-deployer-rsync --save<br>
npm install hexo-deployer-openshift --save<br>
npm install hexo-renderer-marked@0.2 --save<br>
npm install hexo-renderer-stylus@0.2 --save<br>
npm install hexo-generator-sitemap@1 --save<br>

3.change deploy type from github to git(/.config.yml)

4.Use eclipse generate the ssh-rsa type key,copy the public key to github.com and save the private&public key to ~/.ssh/

## tools
1.markdown eclipse plugin update site:
http://www.winterwell.com/software/updatesite/



## install Version-2
>*   excute 'hexo init' on an empty folder
>*   github clone rep. changwentao.github.io
>*   excute 'npm install hexo --save' on rep.
>*   copy 'package.json' and 'node_modules' into rep.