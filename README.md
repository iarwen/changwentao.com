---new
每次重新搭建环境都搞得各种npm包不成功 索性将
所有的npm包都上传，每次只要执行
```
npm install -g hexo 就好了
```
changwentao.com
=================

My Website Repository

# important step
0.install node.js and git
 
1.$npm install -g hexo-cli

2.$
* npm install hexo-generator-index --save
* npm install hexo-generator-archive --save
* npm install hexo-generator-category --save
* npm install hexo-generator-tag --save
* npm install hexo-server --save
* npm install hexo-deployer-git --save
* npm install hexo-deployer-heroku --save
* npm install hexo-deployer-rsync --save
* npm install hexo-deployer-openshift --save
* npm install hexo-renderer-marked@0.2 --save
* npm install hexo-renderer-stylus@0.2 --save
* npm install hexo-generator-sitemap@1 --save

3.change deploy type from github to git(/.config.yml)

4.Use eclipse generate the ssh-rsa type key,copy the public key to github.com and save the private&public key to ~/.ssh/

# tools
1.markdown eclipse plugin update site:
http://www.winterwell.com/software/updatesite/



# install Version-2
* excute 'hexo init' on an empty folder
* github clone rep. changwentao.github.io
* excute 'npm install hexo --save' on rep.
* copy 'package.json' and 'node_modules' into rep.