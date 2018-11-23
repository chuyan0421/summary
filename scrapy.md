# Scrapy

## 多个spider对应不同的pipelines

给spider设置自己的custom_settings

## 下载的文件重命名
继承ImagesPipeline，重写file_path函数

## csv文件按指定的顺序输出
[参考](https://www.jianshu.com/p/fd6f7eba6abe)

## 访问动态页面
方式1：重写中间件，采用无界面浏览器selenium，禁用原本的middleware
方式2：selenium最好放置在spider中，如果存在下载图片任务，如果selenium放置在middleware中，下载图片也会经过selenium，每下载一个图片会打开一个浏览器，效率低。

## no module named ‘win32api’
```
$ pip install pypiwin32
```

## 访问<a></a>中的所有文字内容
当<a></a>中没有其他标签时，可以使用xpathData.xpath(‘./text()’).extract()提取

当<a></a>中还有其他标签，使用xpathData.xpath(‘string(.)’).extract()提取

可以考虑使用preceding，[参考](https://stackoverflow.com/questions/35811535/what-xpath-i-need-to-extract-the-text-inside-span-that-is-preceded-by-a-specific)

## 爬取天猫时提示登录
Selenium调用geckodriver启动火狐（或chrome），当遇到爬虫不好处理的情况时（登录或者输入验证码）采用人工输入的方式
```
driver = webdriver.Firefox(executable_path=r'C:\geckodriver.exe')
driver.get(request.url)
time.sleep(10)
driver.find_element_by_xpath('//*[@class="sn-container"]/p/a[1]').click()
print("please log in , and input message in search box ")
print("waiting...")
time.sleep(60)
```
启动火狐后，进入到登录界面，扫描二维码登录，在输入框中搜索需要检索的数据，再切换到需要的页面吗，爬虫开始工作。爬取完当前的页面，提示（人工）切换到下一个页面。

geckodriver是一个控制浏览器的工具
