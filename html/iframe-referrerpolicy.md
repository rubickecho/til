# What is iframe referrerpolicy

## What?
`referrerpolicy` 是 `iframe` 属性，可以控制被嵌入的 `iframe` 时发送的 `referrer` 首部，以确保在获取资源时发送出最佳的 `referrer` 信息，提高安全性，防止跨域请求攻击，同时也更好的实现网站的统计和跟踪。
此外，`referrerpolicy` 还可以控制发送 `referrer` 信息时的深度，以及避免发送特定域名的 `referrer` 信息。

在 iframe 中，`Document.referrer` 会初始化为父窗口的`Window.location` 的 `href`。

## How?

* `no-referrer`: 整个 Referer 首部会被移除。访问来源信息不随着请求一起发送。
* `no-referrer-when-downgrade（默认值）`: 在没有指定任何策略的情况下用户代理的默认行为。在同等安全级别的情况下，引用页面的地址会被发送 (HTTPS->HTTPS)，但是在降级的情况下不会被发送 (HTTPS->HTTP)。
* `origin`: 在任何情况下，仅发送文件的源作为引用地址。例如 https://example.com/page.html 会将 https://example.com/ 作为引用地址。
* `origin-when-cross-origin`: 对于同源的请求，会发送完整的 URL 作为引用地址，但是对于非同源请求仅发送文件的源。
* `same-origin`: 对于同源的请求会发送引用地址，但是对于非同源请求则不发送引用地址信息。
* `strict-origin`: 在同等安全级别的情况下，发送文件的源作为引用地址 (HTTPS->HTTPS)，但是在降级的情况下不会发送 (HTTPS->HTTP)。
* `strict-origin-when-cross-origin`: 对于同源的请求，会发送完整的 URL 作为引用地址；在同等安全级别的情况下，发送文件的源作为引用地址 (HTTPS->HTTPS)；在降级的情况下不发送此首部 (HTTPS->HTTP)。
* `unsafe-url`: 无论是同源请求还是非同源请求，都发送完整的 URL（移除参数信息之后）作为引用地址。

## Examples

copy form [iframe.referrerpolicy](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referrer-Policy)

| Policy                                | Document                         | Navigation to                      | Referrer                      |
| ------------------------------------- | -------------------------------- | ---------------------------------- | ----------------------------- |
| **`no-referrer`**                     | `https://example.com/page.html`    | any domain or path                 | no referrer                   |
| **`no-referrer-when-downgrade`**      | `https://example.com/page.html`    | `https://example.com/otherpage.html` | `https://example.com/page.html` |
| **`no-referrer-when-downgrade`**      | `https://example.com/page.html`    | `https://mozilla.org`                | `https://example.com/page.html` |
| **`no-referrer-when-downgrade`**      | `https://example.com/page.html`    | **http**://example.org             | no referrer                   |
| **`origin`**                          | `https://example.com/page.html`    | any domain or path                 | `https://example.com/`          |
| **`origin-when-cross-origin`**        | `https://example.com/page.html`    | `https://example.com/otherpage.html` | `https://example.com/page.html` |
| **`origin-when-cross-origin`**        | `https://example.com/page.html`    | `https://mozilla.org`                | `https://example.com/`          |
| **`origin-when-cross-origin`**        | `https://example.com/page.html`    | **http**://example.com/page.html   | `https://example.com/`          |
| **`same-origin`**                     | `https://example.com/page.html`    | `https://example.com/otherpage.html` | `https://example.com/page.html` |
| **`same-origin`**                     | `https://example.com/page.html`    | `https://mozilla.org`                | no referrer                   |
| **`strict-origin`**                   | `https://example.com/page.html`    | `https://mozilla.org`                | `https://example.com/`          |
| **`strict-origin`**                   | `https://example.com/page.html`    | **http**://example.org             | no referrer                   |
| **`strict-origin`**                   | **http**://example.com/page.html | any domain or path                 | `http://example.com/`           |
| **`strict-origin-when-cross-origin`** | `https://example.com/page.html`    | `https://example.com/otherpage.html` | `https://example.com/page.html` |
| **`strict-origin-when-cross-origin`** | `https://example.com/page.html`    | `https://mozilla.org`                | `https://example.com/`          |
| **`strict-origin-when-cross-origin`** | `https://example.com/page.html`    | **http**://example.org             | no referrer                   |
| **`unsafe-url`**                      | `https://example.com/page.html`    | any domain or path                 | `https://example.com/page.html` |