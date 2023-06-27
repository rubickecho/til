# HTTP DELETE 请求可以发送 body 数据吗？

> ***Anwser From: ChatGPT***

根据 HTTP/1.1 协议的规定，DELETE 请求不应该包含任何主题（body）数据。这意味着如果你尝试在 DELETE 请求中附加一个 body，那么这个请求可能会在某些浏览器或服务器端被忽略或抛出错误。

不过，实际上不同的浏览器和服务器的行为可能会有所不同。例如，一些浏览器和服务器可能会允许 DELETE 请求带有 body，而其他的却会禁止。

HTTP 协议的 RFC 7231 在第4.3.5节中有这样一段话：

> A payload within a DELETE request message has no defined semantics; sending a payload body on a DELETE request might cause some existing implementations to reject the request.

这段话的意思是“DELETE 请求中的主体（payload）没有定义的语义；在 DELETE 请求中发送主体可能会导致一些已有的实现拒绝该请求。”

所以，虽然一些浏览器或服务器可能允许带有 body 数据的 DELETE 请求，但是从 HTTP 协议规范的角度来看，我们不应该期望它们的可靠性。如果需要在 DELETE 请求中发送数据，可以考虑将数据放在 URL 参数中或者使用其他 HTTP 方法，例如 POST 或 PATCH。