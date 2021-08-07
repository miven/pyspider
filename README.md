pyspider安装说明（踩坑）




环境 windows10 + anaconda + python3.7

pip install pyspider
pip uninstall wsgidav
pip install wsgidav==2.3.0
pip uninstall werkzeug
pip install werkzeug==0.16.1
pip uninstall Flask
pip install Flask==0.10

python3.7 因为async关键字的问题，要修改三个文件

/run.py
/webui/app.py
/fetcher/tornado_fetcher.py

将async 替换为 asyncs,注意区分大小写替换

下载 phantomjs 放到系统目录或者环境变量目录
https://phantomjs.org/download.html

然后pyspider all 启动成功





Sample Code 
-----------

```python
from pyspider.libs.base_handler import *


class Handler(BaseHandler):
    crawl_config = {
    }

    @every(minutes=24 * 60)
    def on_start(self):
        self.crawl('http://scrapy.org/', callback=self.index_page)

    @config(age=10 * 24 * 60 * 60)
    def index_page(self, response):
        for each in response.doc('a[href^="http"]').items():
            self.crawl(each.attr.href, callback=self.detail_page)

    def detail_page(self, response):
        return {
            "url": response.url,
            "title": response.doc('title').text(),
        }
```
