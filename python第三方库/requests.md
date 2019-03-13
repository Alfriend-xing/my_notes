# requests
---
```python
import requests
#请求
r = requests.get('https://api.github.com/events')
r = requests.post('https://httpbin.org/post', data = {'key':'value'})
r = requests.put('https://httpbin.org/put', data = {'key':'value'})
r = requests.delete('https://httpbin.org/delete')
r = requests.head('https://httpbin.org/get')
r = requests.options('https://httpbin.org/get')

#URL参数
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get('https://httpbin.org/get', params=payload)
print(r.url)
#https://httpbin.org/get?key2=value2&key1=value1

#Response 
r.text #自动解码来自服务器的内容
r.encoding #更改编码
r.content #对于非文本请求，以字节为单位访问响应正文
#从二进制数据创建图片
from PIL import Image
from io import BytesIO
i = Image.open(BytesIO(r.content))
r.json()#访问json数据,解析失败会报错

#定制Headers
url = 'https://api.github.com/some/endpoint'
headers = {'user-agent': 'my-app/0.0.1'}
r = requests.get(url, headers=headers)

#复杂的post
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.post("https://httpbin.org/post", data=payload)
#效果相同
payload_tuples = [('key1', 'value1'), ('key1', 'value2')]
r1 = requests.post('https://httpbin.org/post', data=payload_tuples)
payload_dict = {'key1': ['value1', 'value2']}
r2 = requests.post('https://httpbin.org/post', data=payload_dict)
#json请求
url = 'https://api.github.com/some/endpoint'
payload = {'some': 'data'}
r = requests.post(url, json=payload)
#会改变header中Content-Type为 application/json.

#上传文件，强烈建议以二进制模式打开文件
url = 'https://httpbin.org/post'
files = {'file': open('report.xls', 'rb')}
r = requests.post(url, files=files)
#以文件方式发送字符串
url = 'https://httpbin.org/post'
files = {'file': ('report.csv', 'some,data,to,send\nanother,row,to,send\n')}
r = requests.post(url, files=files)
#requests 不支持流模式

#状态码
r.status_code #200

#返回头
r.headers
r.headers['Content-Type']
r.headers.get('content-type')

#Cookies
url = 'http://example.com/some/cookie/setting/url'
r = requests.get(url)
r.cookies['example_cookie_name']
#发送cookies
url = 'https://httpbin.org/cookies'
cookies = dict(cookies_are='working')
r = requests.get(url, cookies=cookies)
#Cookies 与字典稍有区别
jar = requests.cookies.RequestsCookieJar()
jar.set('tasty_cookie', 'yum', domain='httpbin.org', path='/cookies')
jar.set('gross_cookie', 'blech', domain='httpbin.org', path='/elsewhere')
url = 'https://httpbin.org/cookies'
r = requests.get(url, cookies=jar)

#使用历史来跟踪重定向
r = requests.get('http://github.com/')
r.url #'https://github.com/'
r.status_code #200
r.history #[<Response [301]>]
#禁用重定向
r = requests.get('http://github.com/', allow_redirects=False)
r.status_code #301
r.history #[]

#超时，所有生产代码都应使用，否则可能导致程序无限期挂起
requests.get('https://github.com/', timeout=0.001)

#错误和异常，如DNS故障，拒绝连接等
#返回ConnectionError ,HTTPError ,Timeout ,TooManyRedirects
try:
    pass
except requests.exceptions.Timeout:
    pass

#Session 对象
#Session 对象允许跨请求保持参数和cookie；如果向同一主机发出多个请求，则底层TCP连接将被重用，这会导致显著的性能提高
#session对象拥有Requests 的全部方法
s = requests.Session()
s.get('https://httpbin.org/cookies/set/sessioncookie/123456789')
r = s.get('https://httpbin.org/cookies')
#提供默认数据
s = requests.Session()
s.auth = ('user', 'pass')
s.headers.update({'x-test': 'true'})
# both 'x-test' and 'x-test2' are sent
s.get('https://httpbin.org/headers', headers={'x-test2': 'true'})
#请求中的数据不会设为默认数据，不会被保存到下一个请求中
s = requests.Session()
r = s.get('https://httpbin.org/cookies', cookies={'from-my': 'browser'})
print(r.text)
# '{"cookies": {"from-my": "browser"}}'
r = s.get('https://httpbin.org/cookies')
print(r.text)
# '{"cookies": {}}'
#上下文
with requests.Session() as s:
    s.get('https://httpbin.org/cookies/set/sessioncookie/123456789')
#删除参数，将key对应的value设为None，程序就会自动忽略他

#请求和响应对象
r = requests.get('https://en.wikipedia.org/wiki/Monty_Python')
r.headers#响应头
r.request.headers#请求头

#SSL证书验证
requests.get('https://requestb.in')
#如果无法验证，抛出SSLError
#设置证书目录
requests.get('https://github.com', verify='/path/to/certfile')
#或
s = requests.Session()
s.verify = '/path/to/certfile'
#忽略验证SSL证书
requests.get('https://kennethreitz.org', verify=False)

#流媒体上传
#发送大型流或文件而无需将其读入内存
with open('massive-body', 'rb') as f:
    requests.post('http://some.url/streamed', data=f)
#建议以二进制模式打开文件

#代理
proxies = {
  'http': 'http://10.10.1.10:3128',
  'https': 'http://10.10.1.10:1080',
}
requests.get('http://example.org', proxies=proxies)
#proxies = {'http': 'http://user:pass@10.10.1.10:3128/'}

#SOCKS
#$ pip install requests[socks]
proxies = {
    'http': 'socks5://user:pass@host:port',
    'https': 'socks5://user:pass@host:port'
}

```




http://docs.python-requests.org/en/master/




