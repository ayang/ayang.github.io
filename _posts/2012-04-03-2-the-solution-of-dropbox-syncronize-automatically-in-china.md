---
layout: post
title: 解决Dropbox大陆地区无法及时自动同步的问题
date: 2012-04-03 22:10:26.641000
tags: Dropbox, GFW
---

最近由于想要使用Dropbox的多人协作功能，就发现Dropbox不能自动同步其他机器上产生的文件变化，经过一番搜索，发现原来是GFW在作怪（GFW和GD的性质和用心我们心知肚明，就不在这里评价了）。月光博客发布了[解决Dropbox无法实时更新的问题](http://www.williamlong.info/archives/2585.html)分析了产生这个问题的原因并提出一个有效的解决方案。但是在使用时我发现，我找不到一个优良稳定的代理服务器，也没工夫去学习privoxy软件的配置和使用，而且我要将解决方案提供给我的合伙人，一个复杂的方案是不能接受的。经过一番研究，提出如下比较**简单**的办法：

分析
======

我发现Dropbox向notify8发出的请求很简单，回应也很简单，一共有两种：

	{"ret":"new"}
	和
	{"ret":"punt"}

分别表示云端有变化和无变化，然后客户端考虑去下载文件列表并同步。

经过一番痛苦的失败，我发现这个请求的其实是一个comet请求，服务器端并不马上回应，而是会挂起，如果有变化，则马上回应，如果一直没有变化，大约一分钟超时回应punt，然后客户端再连接服务器。在我分析Dropbox的过程中一直不解：为什么Dropbox的其他请求都是https，而只有这一个请求是http的。现在找到了答案：因为它是comet请求，长连接，而且连接频率非常高，如果使用https代价太大，而且影响效率。如果这个请求返回new，客户端就会使用https连接服务器端。

解决
======

由此提出一个完美的解决方案，不仅可以解决本机的问题，而且可以解决朋友的问题，只要让朋友修改hosts为我的ip地址：

* 修改hosts将notify8对应的ip地址改为本机
* 在本机建立一个http服务，代理notify8得到dropbox的返回值，再原封不动地返回给本机dropbox客户端

具体方法是使用tornado，进行一步http请求，这样只占用很少一部分系统资源。贴出代码。

代码
======
	#!/usr/bin/env python
	# -*- coding: utf-8 -*-

	import tornado.httpserver
	import tornado.ioloop
	import tornado.options
	import tornado.web
	from tornado.options import define, options
	from tornado import httpclient


	define("port", default=8888, help="run on the given port", type=int)


	class Application(tornado.web.Application):
	    def __init__(self):
	        handlers = [
	            (r"/subscribe", NotifyHandler),
	            (r"/.*", HomeHandler),
	        ]
	        settings = dict(
	            debug=True,
	        )
	        self.debug = True
	        tornado.web.Application.__init__(self, handlers, **settings)


	class HomeHandler(tornado.web.RequestHandler):
	    def get(self):
	        self.set_header("Content-Type", "text/plain")
	        self.write("Hello from Tornado!")


	class NotifyHandler(tornado.web.RequestHandler):
	    @tornado.web.asynchronous
	    def get(self):
	        self.set_header("Content-Type", "text/plain")
	        url = xxxxx #关键代码还是不贴出来了，人怕出名猪怕壮，要是大多数人会用了，估计这个方法死期不远矣!
	        http_client = httpclient.AsyncHTTPClient()
	        http_client.fetch(url, self.handle_response, request_timeout=100.0)

	    def handle_response(self, response):
	        if response.error:
	            print "Can not connect."
	            self.write("{\"ret\": \"new\"}")
	        else:
	            print "Connect Successfull."
	            self.write(response.body)
	        self.finish()


	def main():
	    tornado.options.parse_command_line()
	    http_server = tornado.httpserver.HTTPServer(Application())
	    http_server.listen(options.port)
	    tornado.ioloop.IOLoop.instance().start()

	if __name__ == "__main__":
	    main()


**版权声明：**原创作品，允许转载，转载请务必以超链接形式标明原始出处，否则将追究法律责任。GiveupTech.com