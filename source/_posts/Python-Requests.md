---
title: Python-Requests用法简介
date: 2019-02-03
tags: [Python]
---

>Requests is an elegant and simple HTTP library for Python, built for human beings.

## 安装方法
使用命令 `pip install requests` 安装

## 使用方法
<!-- more -->
首先，导入Requests模块：
`import requests`  


1. **HTTP基本请求类型**  
```
r = requests.get('https://api.github.com/events')  
r = requests.post('http://httpbin.org/post'）
```
2. **传递URL参数**  
使用 `params` 关键字参数，以一个字符串字典提供这些参数，注意字典里值为 None 的键都不会被添加到 URL 的查询字符串里。  
```
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get("http://httpbin.org/get", params = payload)
```

3. **在POST请求中发送数据**   
传递一个字典给 data 参数，例如  
```
>>> payload = {'key1': 'value1', 'key2': 'value2'}
>>> r = requests.post("http://httpbin.org/post", data=payload)
```

4. **定制请求头**  
传递一个字典给 headers 参数，例如
```
>>> url = 'https://api.github.com/some/endpoint'
>>> headers = {'user-agent': 'my-app/0.0.1'}
>>> r = requests.get(url, headers=headers)
```
5. **读取响应内容**
```
>>> import requests
>>> r = requests.get('https://api.github.com/events')
```
- 用 `r.text` 读取服务器响应的内容
```
>>> r.text
u'[{"repository":{"open_issues":0,"url":"https://github.com/...
```
- 用 `r.encoding` 来获得**推测的**文本编码，并可以对其进行修改，改变 `r.text` 的解析方法
```
>>> r.encoding
'utf-8'
>>> r.encoding = 'ISO-8859-1'
```
- 用 `r.content` 读取二进制的响应内容。例如，可以用返回的二进制数据创建一张图片：
```
>>> from PIL import Image
>>> from io import BytesIO
>>> i = Image.open(BytesIO(r.content))
```
- 用 `r.status_code` 检测响应状态码，也可以使用内置的状态码查询对象 `requests.codes.ok` 
```
>>> r = requests.get('http://httpbin.org/get')
>>> r.status_code
200
>>> r.status_code == requests.codes.ok
True
```
- 用 `r.headers` 来获取响应头内容。
```
>>> r.headers
{
    'content-encoding': 'gzip',
    'transfer-encoding': 'chunked',
    'connection': 'close',
    'server': 'nginx/1.0.4',
    'x-runtime': '148ms',
    'etag': '"e1ca502697e5c9317743dc078f67693f"',
    'content-type': 'application/json'
} 
```
- 用 `r.json()` 获取经内置JSON解码器处理的JSON数据。该方法会返回一个JSON对象。当JSON解析失败时，r.json()会抛出一个异常。需要注意的是，成功调用 `r.json()` 并**不**意味着响应的成功。要检查请求是否成功，请使用 `r.raise_for_status()` 或者检查 `r.status_code` 是否和你的期望相同。
6. **POST一个多部分编码(Multipart-Encoded)的文件**  
注意，请使用二进制模式(binary mode)打开文件，防止 Requests 提供的 Content-Length header 发生错误。
```
>>> url = 'http://httpbin.org/post'
>>> files = {'file': open('report.xls', 'rb')}
>>> r = requests.post(url, files=files)
```
7. **自定义请求头部**  
伪装请求头部是采集时经常用的，我们可以用这个方法来隐藏：
```
>>> r = requests.get('http://www.zhidaow.com')
>>> print (r.request.headers['User-Agent'])
"python-requests/1.2.3 CPython/2.7.3 Windows/XP"
>>> headers = {'User-Agent': 'alexkh'}
>>> r = requests.get('http://www.zhidaow.com', headers = headers)
>>> print (r.request.headers['User-Agent'])  
"alexkh"
```
8. **超时**  
使用`timeout`参数使得对象在经过该参数所指定的秒数后停止响应，以此防止程序永远失去响应。  
*注意：`timeout` 仅对连接过程有效，与响应体的下载无关。*
```
>>> requests.get('http://github.com', timeout=0.001)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
requests.exceptions.Timeout: HTTPConnectionPool(host='github.com', port=80): Request timed out. (timeout=0.001)
```
9. **错误与异常**  
遇到网络问题（如：DNS 查询失败、拒绝连接等）时，Requests 会抛出一个 `ConnectionError` 异常。   
如果 HTTP 请求返回了不成功的状态码， `Response.raise_for_status()` 会抛出一个 `HTTPError` 异常。  
若请求超时，则抛出一个 `Timeout` 异常。  
若请求超过了设定的最大重定向次数，则会抛出一个 `TooManyRedirects` 异常。  
10. **代理**  
如果需要使用代理，你可以通过为任意请求方法提供 `proxies` 参数来配置单个请求：
```python
proxies = {
  "http": "http://10.10.1.10:3128",
  "https": "http://10.10.1.10:1080",
}
requests.get("http://example.org", proxies=proxies)
```
若你的代理需要使用HTTP Basic Auth，可以使用 http://user:password@host/ 语法：
```python
proxies = {
    "http": "http://user:pass@10.10.1.10:3128/",
}
```

## 其他链接
- [Requests官方中文文档](http://cn.python-requests.org/zh_CN/latest/index.html)

## 参考资料
1. <http://cn.python-requests.org/zh_CN/latest/index.html>
2. <https://www.cnblogs.com/lgh344902118/p/6780960.html>





