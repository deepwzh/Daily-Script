# 1. 简介
此脚本用于爬取网站上的软件评测师真题教程（https://www.educity.cn/rk/zhenti/test/），经过少量修改也可以用于其他网站
# 2. 工具及环境
- Chrome 浏览器
- Chrome浏览器 ChroPath插件
- IPython 
# 3. 步骤
## 1. 提取XPATH
打开Chrome浏览器
打开软件评测师的真题网站https://www.educity.cn/rk/zhenti/test/, 通过`ChroPath`插件验证我们输入的XPATH表达式 

根据分析界面元素，我们可以得出class属性为btn_blue且按钮文本是'PDF下载'的按钮的href属性值即为相对url

//a[@class='btn_blue' and text()='PDF下载']

上述XPATH表达式可以试下选中全部PDF下载按钮

## 2. Python运行xpath
### 1. 使用requests库获取网页内容
打开IPython运行环境（Python也可以，只是Python不带有代码提示)
'''python3
import requests
url = 'https://www.educity.cn/rk/zhenti/test/'
data = requests.get(url).text
'''
data为获取到的HTML源代码
### 2. 使用lxml库进行xpath匹配

'''python3
u = etree.HTML(data)
urls = ["http://www.educity.cn/" + url for url in u.xpath("//a[@class='btn_blue' and text()='PDF下载']/@href")]
'''
urls为获取的全部pdf的路径，`@href`语法获取`href`属性的值

## 3. Python下载文件
下列代码可以实现下载文件的功能
'''python3
import urllib
for url in urls:
    filename = url.split("/")[-1]
    urllib.request.urlretrieve(url, filename)
'''

注意IPython可以使用诸如`cd`, `mkdir`, `pwd`等Linux命令，注意切换到合适的目录再下载
