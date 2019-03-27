# centos7安装chrome

## 安装google-chrome
```shell
vim /etc/yum.repos.d/google-chrome.repo

i

[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub

:wq
```

```shell
yum install google-chrome-stable --nogpgcheck

google-chrome --version
```
## 下载chromedriver
>从`https://sites.google.com/a/chromium.org/chromedriver/downloads`中找到对应版本的驱动
```shell
wget https://chromedriver.storage.googleapis.com/73.0.3683.68/chromedriver_linux64.zip

unzip chromedriver_linux64.zip
```
添加至系统路径
```shell
法一(当前会话有效)
export PATH=$PATH:/root/test/

法二(配置用户系统变量)
vim ~/.bash_profile
将 /root/test/
加入到 PATH=$PATH:$HOME/bin 一行之后（注意以冒号分隔）
source ~/.bash_profile

法三(所有用户)
vim /etc/profile
在文件末尾加上如下两行代码 
PATH=$PATH:/root/test/
export PATH
执行
source /etc/profile
```

## root用户启动
```shell
google-chrome --no-sandbox 'http://baidu.com'
google-chrome --no-sandbox --headless 'http://baidu.com'
```

## selenium无界面启动
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options


chrome_options = Options()
chrome_options.add_argument('--no-sandbox')
chrome_options.add_argument('--headless')
chrome_options.add_argument('--disable-gpu')
browser = webdriver.Chrome(chrome_options=chrome_options)

browser.quit()
```