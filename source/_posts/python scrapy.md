title: python scrapy
id: 7
tags:
  - python
  - srcapy
categories:
  - Python
date: 2015-04-14 13:41:23

---

# [python网络爬虫入门](http://mp.weixin.qq.com/s?__biz=MzA4MjEyNTA5Mw==&amp;mid=206006839&amp;idx=1&amp;sn=568201291c68761da6cf873c1a7c5ef4&amp;scene=1&amp;key=2e5b2e802b7041cff91e825029c4e8dffd7e57e0850ce3f6ceddf95fa83fc9ef1a791f79976a57d5d12b17ba4373880d&amp;ascene=1&amp;uin=NTQ2NzM3NjAw&amp;devicetype=webwx&amp;version=70000001&amp;pass_ticket=JB%2FEpXYFNKiCkydl90TMnZcFgfueNnKbRfxBhEiBP5NJ5bt3KKdkREjW664WzO%2Fs "python网络爬虫入门")

## **学习要求**

1.  基本的爬虫工作原理；
2.  基本的http抓取工具，scrapy
3.  Bloom Filter: Bloom
4.  分布式爬虫：[python-rq](https://github.com/nvie/rq)
5.  rq和Scrapy的结合：[scrapy-redis](https://github.com/darkrho/scrapy-redis)
6.  后续处理，网页分析：[python-goose](https://github.com/grangier/python-goose)

### 爬虫的工作原理

以人民日报为例：http://www.renminribao.com

基本流程如下：伪代码
<pre>import Queue

initial_page = "http://www.renminribao.com"
url_open = Queue.Queue()
seen = set()
seen.insert(initial_page)
url_queue.put(initial_page)

while(True):
    if url_queue.size()&gt;0:
        current_url = url_queue.get()
        store(current_url)
        for next_url in extract_urls(current_url):
            if next_url not in seen:
                seen.put(next_url)
                url_queue.put(next_url)
            else:
                break</pre>

###  效率

N个网络链接，判重的复杂度N*log(N)，python的set实现是hash，太慢

通常的判重，Bloom Filter。它是一种hash的方法，但是它的特点是，它可以使用固定的内存（不随URL的数量而增长）以O(1)的效率判定URL是否已经在set中。[实例](http://billmill.org/bloomfilter-tutorial/)

###  集群化抓取

slave master模式，url_queue 放到master上，所有的slave都可以通过网络跟master通信，每当一个slave完成下载一个网页，就向master请求一个新的网页来抓取，每次slave新抓取到一个网页，就把这个网页上所有链接送到master的queue里去。同样，bloom filter也放到master上，但是master只发送确定没有被访问过的URL给slave。Bloom Filter放到master的内存里，而被访问过的URL放到master上的Redis里，这样保证所有操作都是O(1)，[LINSERT – Redis](http://redis.io/documentation)

在各个slave上装好scrapy，在master装好Redis和rq作为分布式队列。
<pre>#slave.py
current_url = request_from_master()
to_send = []
for next_url in extract_urls(current_url):
    to_send.append(next_url)
store(current_url)
send_to_master(to_send)

#master.py

distributed_queue = DistributedQueue()
bf = BloomFilter()

initial_pages = 'www.renminribao.com'

while(True):
    if request == 'GET'
        if distributed_queue.size()&gt;0:
            send(distributed_queue.get())
        else:
            break
    elif request == 'POST'
        bf.put(request.url)</pre>
已经有现成的例子：[<span class="author">arkrho</span><span class="path-divider">/</span>**scrapy-redis**](https://github.com/darkrho/scrapy-redis)

###  展望及后处理

1，有效地存储（数据库应该怎样安排）

2，有效的判重（网页内容判重）

3，有效的信息提取（抽取有用信息）

4，及时更新（预测网页多久更新一次）