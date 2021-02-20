title: 分析9600+家公司的简介以及产品说明，生成标签云
date: 2018/07/18 16:49:25
categories:
- 爬虫 结巴分词 标签云 Node.js Python
tags: [python,标签云]
---

最近工作有空闲，树莓派上跑了个Node服务器，爬了几天拉勾上的公司，爬取的网址类似下面的格式：
https://www.lagou.com/gongsi/200562.html ，先上分析结果：
![](https://static.oschina.net/uploads/space/2017/1026/161508_7vNm_584443.png)
<!--more-->
源码如下
```
var fetch_url = '/gongsi/${index}.html';

exports.runner = function fetchPage(index) {
    startRequest(index);
};


function startRequest(index) {
    CompanysModel.findOne({index: index}, '_id', function (err, _company) {
        if (err || _company) {
            console.log(index + '-------  got');
            incrAndReget(index);
            return;
        }
        handlerContent(index);
    });
}

function handlerContent(index) {
    //采用http模块向服务器发起一次get请求
    var url = fetch_url.replace('${index}', index);

    var options = {
        hostname: 'www.lagou.com',
        port: '443',
        path: url,
        method: 'GET',
        headers: {
            'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
            'Accept-Encoding': 'deflate, br',
            'Accept-Language': 'zh-CN,zh;q=0.8,en;q=0.6,zh-TW;q=0.4',
            'Connection': 'keep-alive',
            'Cookie': 'user_trace_token=20171019112945-24a9d09c-bcfe-47d7-8d27-3bf53bfdb347; LGUID=20171019112947-c1eabc85-b47d-11e7-9c7e-525400f775ce; JSESSIONID=ABAAABAACDBAAIAC9A9B32CD990D08ED4BAEFDDAB14C879; TG-TRACK-CODE=index_campus; _gid=GA1.2.705151711.1508383779; _ga=GA1.2.1181508252.1508383779; Hm_lvt_4233e74dff0ae5bd0a3d81c6ccf756e6=1508383779,1508383891,1508384000; Hm_lpvt_4233e74dff0ae5bd0a3d81c6ccf756e6=1508392375; LGSID=20171019125706-f472e429-b489-11e7-9ca5-525400f775ce; LGRID=20171019135302-c509ee47-b491-11e7-9cac-525400f775ce',
            'Host': 'www.lagou.com',
            'Origin': 'http://www.lagou.com',
            'Referer': 'http://www.lagou.com/',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.75 Safari/537.36'

        }
    }
    http.get(options, function (res) {
        console.log(url + '-------' + res.statusCode);
        if (res.statusCode == 303) {
            incrAndReget(index);
            return;
        }

        if (res.statusCode == 302) {
            setTimeout(function () {
                if (retryCount == RETRYTIMES) {
                    retryCount = 1;
                    incrAndReget(index);
                } else {
                    retryCount++;
                    handlerContent(index);
                }
            }, 1000 * 5);
            return;
        }
        var html = '';        //用来存储请求网页的整个html内容
        //监听data事件，每次取一块数据
        res.on('data', function (chunk) {
            html += chunk;
        });
        //监听end事件，如果整个网页内容的html都获取完毕，就执行回调函数
        res.on('end', function () {

            var $ = cheerio.load(html); //采用cheerio模块解析html

            var company = {
                index: index,
                //公司简称
                simpleName: $('a.hovertips').text().trim(),
                //公司名称
                name: $('a.hovertips').attr('title') ? $('a.hovertips').attr('title').trim() : '',
                //公司url
                urlLink: $('a.hovertips').attr('href') ? $('a.hovertips').attr('href').trim() : '',
                //公司简介
                companyDesc: $('span.company_content').text().trim(),
                //产品名
                productName: $('.url_valid').text().trim(),
                //产品类型
                productType: $('.product_details ul').text().trim(),
                //产品描述
                productDesc: $('.product_profile').text().trim(),
                //产品链接地址
                productLink: $('.url_valid').attr('href') ? $('.url_valid').attr('href').trim() : '',
                //管理者
                manager: $("#company_managers .item_manager_name").text().trim(),
                //管理者简介
                managerDesc: $("#company_managers .item_manager_content").text().trim(),
                //公司类型
                type: $($("#basic_container li")[0]).text().trim(),
                //融资进度
                process: $($("#basic_container li")[1]).text().trim(),
                //人数
                number: $($("#basic_container li")[2]).text().trim(),
                //坐标
                address: $($("#basic_container li")[3]).text().trim(),
                //评分
                score: $('#interview_container .comprehensive-review .score').text().trim(),
                //样本
                count: $('#interview_container .comprehensive-review .count').text().trim().replace("（ 来自 ", "").replace(" 份评价 ）", "")
            }
            var companysModel = new CompanysModel(company);
            companysModel.save();
        });
        incrAndReget(index);
    }).on('error', function (err) {
        console.log(err);
        setTimeout(function () {
            handlerContent(index);
        }, 1000 * 10);
    });
}

function incrAndReget(index) {
    index++;
    startRequest(index);
}
```

效率不是很高，可自行抓取或者优化抓取过程。抓取来的数据，放在了MongoDb中。

接下来使用结巴分词进行词频的抓取，整个python使用3.6的32位，需要安装的工具如下

pygame-1.9.3-cp36-cp36m-win32.whl

PyTagCloud

simplejson

为了显示中文，拷贝中文字体到PyTagCloud的路径Python36-32\Lib\site-packages\pytagcloud\fonts下，并修改fonts.json,增加
```
    {
		"name": "SimHei",
		"ttf": "simhei.ttf",
		"web": "none"
	},
```
接下来编写代码来分析公司介绍文件coms.txt，排除了一些不关心的词做stopword,加入了些自定义的词典，但词频依然很低。请自行进行调整：
```
import sys
import codecs
sys.path.append('../')

import jieba
import jieba.analyse
from optparse import OptionParser

USAGE = "usage:    python ana.py [file name] -k [top k] -w [with weight=1 or 0]"
jieba.load_userdict('dict.txt')
jieba.analyse.set_stop_words('stopwords.txt')
parser = OptionParser(USAGE)
parser.add_option("-k", dest="topK")
parser.add_option("-w", dest="withWeight")
opt, args = parser.parse_args()


if len(args) < 1:
    print(USAGE)
    sys.exit(1)

file_name = args[0]

if opt.topK is None:
    topK = 10
else:
    topK = int(opt.topK)

if opt.withWeight is None:
    withWeight = False
else:
    if int(opt.withWeight) is 1:
        withWeight = True
    else:
        withWeight = False

content = open(file_name, 'rb').read()

tags = jieba.analyse.extract_tags(content, topK=topK, withWeight=withWeight)
output = codecs.open('sort.txt', 'w','utf-8')
if withWeight is True:
    for tag in tags:
        output.write('%s%s%s%s'%(tag[0],':',tag[1],'\n'))

else:
    print(",".join(tags))
```

执行之后将输出结果到sort.txt

接下来生成词云，话不多说，直接上代码

```
# -*- coding: utf-8 -*-
import codecs
import random
from pytagcloud import create_tag_image, create_html_data, make_tags, \
    LAYOUT_HORIZONTAL, LAYOUTS
from pytagcloud.colors import COLOR_SCHEMES
from pytagcloud.lang.counter import get_tag_counts

wd = {}

fp=codecs.open(".\sort.txt", "r",'utf-8');

alllines=fp.readlines();

fp.close();

for eachline in alllines:
    line = eachline.split(':')
    wd[line[0]] =  int(float(line[1][:-11])*10000)
    print('%s%s'%(line[0],wd[line[0]]));


from operator import itemgetter
swd = sorted(wd.items(), key=itemgetter(1), reverse=True)
tags = make_tags(swd,minsize = 20, maxsize = 200,colors=random.choice(list(COLOR_SCHEMES.values())))
create_tag_image(tags, 'keyword_tag_cloud4.png', background=(34, 34, 34, 255),
size=(1200, 1000),layout=LAYOUT_HORIZONTAL,
fontname="SimHei")
```
输出结果keyword_tag_cloud4.png

ref:

http://www.cnblogs.com/Yiutto/p/5998262.html

已打包上传gitee,地址

https://gitee.com/changwenwen/CompanysAnalysis