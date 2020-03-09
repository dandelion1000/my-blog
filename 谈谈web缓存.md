趁着5.1时光，将http缓存的知识再次补习了一番，并整理了一下笔记，

欢迎伙伴们阅读留言。



今天主要谈谈web缓存，将围绕以下几点来展开：

1.为什么要引入缓存的概念，意义是什么？

2.HTTP 标头中用于缓存的常用的指令有哪些，不同的值配置有何影响？

3.如何定义最佳的cache-control策略？

#### 1.缓存的意义

通过网络提取资源内容速度缓慢且开销巨大。较大的响应需要在客户端与服务端之间进行多次往返通信，往往可能还会遇到网络阻塞的情况，这会延迟浏览器获得和处理内容的时间，还会增加访问者的流量费用。因此缓存并重复利用之前获取的资源的能力成为**性能优化**的一个关键方面。

更多的利用缓存资源，可以节约带宽，降低服务器代价，提高网站的性能和响应速度。

#### 2..HTTP 标头中用于缓存的常用的指令

好在每个浏览器都自带了HTTP缓存实现功能，我们只需确保每个服务器响应都提供正确的 HTTP 标头指令，以指示浏览器何时可以缓存响应以及可以缓存多久。

并不是所有资源都是永久不变的：重要的是对一个资源的缓存应截止到其下一次发生改变，即不能缓存过期的资源。



![图片](https://coding-net-production-pp-ci.codehub.cn/3cf0e8f2-8214-4139-986a-017802105701.png)



当服务器返回响应时，会发出一组 HTTP 标头，用于描述响应的内容类型、长度、缓存指令、验证令牌等。 例如，在上图的交互中，服务器返回一个 1024 字节的响应，指示客户端将其缓存最多 120 秒，并提供一个验证令牌（“4a64......”），可在响应过期后用来检查资源是否被修改。

##### last-modified

服务器通过 `Last-Modified` 字段告知客户端，资源最后一次被修改的时间。

Last-Modified : Fri , 01 May 2019 18:53:33 GMT

客户端第二次请求此URL时，根据HTTP协议的规定，浏览器会向服务器传送If-Modified-Since报头，询问该时间之后文件是否有被修改过：

If-Modified-Since : Fri , 02 May 2019 18:53:33 GMT

如果服务器端的资源没有变化，则自动返回 HTTP 304（Not Changed.）状态码，内容为空，这样就节省了传输数据量。

[`Last-Modified`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified) 响应头可以作为一种弱校验器。说它弱是因它只能精确到一秒。由于精确度比ETag低，所以这是一个备用机制。最常见的应用场景是来更新没有特定 [`ETag`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/ETag) 标签的缓存实体。

##### ETag（验证令牌，实现**高效的资源更新检测**）

``问题场景``：假定在首次提取资源120秒之后，浏览器再次请求该资源。首先，浏览器会检测本地缓存并找到之前的响应。但该响应已经过期，浏览器无法使用，此时浏览器可直接发出新的请求并获取新的完整响应，但如果资源未发生变化，下载与缓存中已有的完全相同资源是低效且不合理的。

ETag旨在解决该问题。服务器生成并返回的随机令牌（指纹），在下一次请求中客户端自动在http请求头的 [`If-None-Match`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-None-Match) 提供ETag验证指纹。服务器根据当前资源核对令牌。 如果它未发生变化，服务器将返回“304 Not Modified”响应，告知浏览器缓存中的响应未发生变化。即可跳过下载，节约了时间和带宽。

![图片](https://coding-net-production-pp-ci.codehub.cn/c2219ef7-42e1-485a-a01b-763aebc29ff6.png)

​                                                 (图片来源：谷歌开发者)



![图片](https://coding-net-production-pp-ci.codehub.cn/b56ddd5b-072b-4c4e-897f-59b0f5b7cef2.png)

##### expires

```
Expires: Fri, 30 Oct 2019 14:19:41 GMT
```

表示缓存到期时间，在响应消息头中，设置这个字段之后，就可以告诉浏览器，在未过期之前不需要再次请求。是一个尽管这个头是有用的，但是它具有一些局限性。

如果在[`Cache-Control`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)响应头设置了 "max-age" 或者 "s-max-age" 指令，那么 `Expires` 头会被忽略。

由于是绝对时间，用户可能会将客户端本地的时间进行修改，而导致浏览器判断缓存失效，重新请求该资源。此外，即使不考虑自行修改，时差或者误差等因素也可能造成客户端与服务端的时间不一致，致使缓存失效。我们可以使用cache-control中的max-age来代替。

##### Cache-Control（缓存控制）

如果Cache-Control与expires同时存在，Cache-Control生效.

- 每个资源都可通过 Cache-Control HTTP 标头定义其缓存策略
- Cache-Control 指令控制谁（private（浏览器缓存）／public(中间缓存)）在什么条件（no-cache／no-store／must-revalidate）下可以缓存响应以及可以缓存多久（max-age）。

从性能优化的角度来说，最佳请求是无需与服务器通信的请求：您可以通过响应的本地副本消除所有网络延迟，以及避免数据传送的流量费用。 为实现此目的，HTTP 规范允许服务器返回 [Cache-Control 指令](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)，这些指令控制浏览器和其他中间缓存如何缓存各个响应以及缓存多久。

>  max-age(单位：秒)

指令指定从请求的时间开始，允许提取的响应被重用的最长的时间.

eg ：

```shell
Cache-Control: max-age=31536000 #表示浏览器或cdn可以缓存该资源一年都没问题，缓存内容可以在不询问服务器的情况下一直保持新鲜度在未过期之前。
```



> no-cache（强制确认缓存）

```
forces caches to submit the request to the origin server for validation before releasing a cached copy, every time. This is useful to assure that authentication is respected (in combination with public), or to maintain rigid freshness, without sacrificing all of the benefits of caching.
```

Cache-Control提供该值表示**每次**请求资源时**必须先与服务器确认**返回的响应是否发生了变化，然后再根据该响应处理后续请求。若资源未发生变化，返回304，则使用缓存资源副本。



> no-store（禁止进行缓存）

直接禁止浏览器以及所有中间缓存存储任何版本的返回响应资源。例如包含个人隐私数据或银行业务数据的响应。每次由客户端向服务器发起的请求都会下载完整的响应内容。

> private

该值表示响应只为单个用户缓存，只能应用于浏览器私有缓存中，不允许任何中间缓存对其进行缓存。例如，用户的浏览器可以缓存包含用户私人信息的html网页，但cdn却不能缓存。

> public

该指令值表示该响应可以被任何中间人（比如中间代理，cdn等）缓存。若Cache-Control指定了public，则一些通常不会被中间人缓存的页面（比如带有http验证信息（账号密码）的页面）将会被其缓存。

> must-revalidate

```
cache_docs解释：tells caches that they must obey any freshness information you give them about a representation. HTTP allows caches to serve stale representations under special conditions; by specifying this header, you’re telling the cache that you want it to strictly follow your rules.
```

告诉缓存他们必须遵守你给他们的关于表示的任何新鲜度信息。 HTTP允许缓存在特殊条件下提供陈旧的表示;通过指定此标头，您告诉缓存您希望它严格遵循您的规则。

> proxy-revalidate

 — similar to `must-revalidate`, except that it only applies to proxy caches.

例如：Cache-Control: max-age=3600, must-revalidate

#### 3.如何定义最佳的cache-control策略？

![图片](https://coding-net-production-pp-ci.codehub.cn/2483608c-8ae5-48db-b496-fcf828d2831e.png)

理想情况下，我们希望在客户端缓存尽可能多的响应，缓存尽可能长的时间，并且为每个响应提供验证令牌，已实现高效的重新验证。在实际项目中，我们确定哪些资源可以缓存，哪些是不可以缓存的，确保返回正确的cache-control和etag标头。

- 在资源“过期”之前，将一直使用本地缓存的响应。
- 您可以通过在网址中嵌入文件内容指纹，强制客户端更新到新版本的响应。
- 为获得最佳性能，每个应用都需要定义自己的缓存层次结构。

浏览器缓存响应后，缓存的版本将一直使用到过期（由 max-age 或 expires 决定），或一直使用到由于某种其他原因从缓存中删除，例如用户清除了浏览器缓存。 因此，构建网页时，不同的用户可能最终使用的是文件的不同版本；刚提取了资源的用户将使用新版本的响应，而缓存了早期（但仍有效）副本的用户将使用旧版本的响应。

如何做到**客户端缓存和快速更新**？

可以在资源内容发生变化时更改其网址，强制用户下载新响应。 通常情况下，可以通过在文件名中嵌入文件的指纹或版本号来实现—例如 style.**x234dff**.css。

可以组合使用 ETag、Cache-Control 和唯一网址来实现一举多得：较长的过期时间、控制可以缓存响应的位置以及随需更新。

在制定缓存策略时，参考如下技巧和方法：

- **使用一致的网址：**如果您在不同的网址上提供相同的内容，将会多次提取和存储这些内容。 提示：请注意，[网址区分大小写](http://www.w3.org/TR/WD-html40-970708/htmlweb.html)。
- **确保服务器提供验证令牌 (ETag)：**有了验证令牌，当服务器上的资源未发生变化时，就不需要传送相同的字节。
- **确定中间缓存可以缓存哪些资源：**对所有用户的响应完全相同的资源非常适合由 CDN 以及其他中间缓存进行缓存。
- **为每个资源确定最佳缓存周期：**不同的资源可能有不同的更新要求。 为每个资源审核并确定合适的 max-age。
- **确定最适合您的网站的缓存层次结构：**您可以通过为 HTML 文档组合使用包含内容指纹的资源网址和短时间或 no-cache 周期，来控制客户端获取更新的速度。
- **最大限度减少搅动：**某些资源的更新比其他资源频繁。 如果资源的特定部分（例如 JavaScript 函数或 CSS 样式集）会经常更新，可以考虑将其代码作为单独的文件提供。 这样一来，每次提取更新时，其余内容（例如变化不是很频繁的内容库代码）可以从缓存提取，从而最大限度减少下载的内容大小。



##### 扩展：

> 关于memory cache，disk cache

在**chrome浏览器**中的控制台Network中size栏通常会有三种状态,(1).from memory cache,(2).from disk cache,(3).资源本身的大小(如：1.5k)

![disk](https://heyingye.github.io/2018/04/16/%E5%BD%BB%E5%BA%95%E7%90%86%E8%A7%A3%E6%B5%8F%E8%A7%88%E5%99%A8%E7%9A%84%E7%BC%93%E5%AD%98%E6%9C%BA%E5%88%B6/img/disk.jpg)

from memory cache代表使用内存中的缓存当关闭该页面时，此资源就被内存释放掉了，再次重新打开相同页面时不会出现from memory cache，from disk cache则代表使用的是硬盘中的缓存，此资源不会随着该页面的关闭而释放掉，因为是存在硬盘当中的，下次打开仍会from disk cache,浏览器读取缓存的顺序为memory –> disk。

在浏览器中，浏览器会在js和图片等文件解析执行后直接存入内存缓存中，那么当刷新页面时只需直接从内存缓存中读取(from memory cache)；而css文件则会存入硬盘文件中，所以每次渲染页面都需要从硬盘读取缓存(from disk cache)。

 

参考文章：https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ

https://www.mnot.net/cache_docs/

