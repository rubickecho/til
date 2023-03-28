# What is web document referer

Referer 请求头包含了当前请求页面的来源页面的地址，即表示当前页面是通过此来源页面里的链接进入的。服务端一般使用 Referer 请求头识别访问来源，可能会以此进行统计分析、日志记录以及缓存优化等。

在今天的工作中，有遇到一个问题，需要根据 Referer 请求头的值来判断用户的访问来源，然后根据不同的来源进行不同的处理，比如流量归因。
所以，我想要知道用户访问一个网站时，所有的操作方式以及它们的 Referer 值。


## Ask GPT-3
> 下面👇是 GPT-3.5 的回答

### 请帮我罗列出，用户访问一个网站（比如 www.example.com）时，所有的操作方式

以下是用户可能所有访问 `www.example.com` 的操作方式：

1. 在浏览器地址栏中输入 `www.example.com` 打开网站。

2. 在浏览器的书签中点击 `www.example.com` 的链接。

3. 在搜索引擎中搜索相关关键词并点击搜索结果中的 `www.example.com` 链接。

4. 从其他网站的链接跳转至 `www.example.com`。

5. 点击 `www.example.com` 网站内部链接或导航到其他页面。

6. 在社交媒体平台上点击包含 `www.example.com` 链接的帖子或广告。

7. 通过扫描包含 `www.example.com` 的二维码访问网站。

请注意，这只是可能的一些操作方式，用户还可以通过其他方式访问网站，具体取决于其个人使用习惯和目的。

### 分别给出这些操作方式，它的 referer 值

1. 在浏览器地址栏中输入 `www.example.com`，打开网站。此时 `Referer` 值为 `""`。

2. 在浏览器的书签中点击 `www.example.com` 的链接。此时如果点击链接前所在的页面是其他网站，`Referer` 值为该网站的 URL；如果点击链接前所在的页面是本地文件或网页，`Referer` 值为该文件或网页的 URL。

3. 在搜索引擎中搜索相关关键词并点击搜索结果中的 `www.example.com` 链接。此时 `Referer` 值为搜索引擎的 URL。

4. 从其他网站的链接跳转至 `www.example.com`。此时 `Referer` 值为来源网站的 URL。

5. 点击 `www.example.com` 网站内部链接或导航到其他页面。此时 `Referer` 值为上一个页面的 URL。

6. 在社交媒体平台上点击包含 `www.example.com` 链接的帖子或广告。此时 `Referer` 值为社交媒体平台的 URL。

7. 通过扫描包含 `www.example.com` 的二维码访问网站。此时 `Referer` 值为扫描二维码的应用程序或工具的 URL。

需要注意的是，有些浏览器或操作系统可能会限制或修改 `Referer` 值，因此在实际开发中需要特别注意。


## 最后结论
> 下面👇是我个人的理解。

需要处理几种情况：
1. `Referer` 值为 `""`，表示用户直接在浏览器地址栏中输入网址访问网站。
2. 有值时，需要根据值的不同进行不同的处理（比如判断是否是站外链接还是站内链接）。
3. 需要做兼容性处理，判断浏览器对 `Referer` 值的限制或修改（因为涉及到用户隐私，可能没有值）。

## 参考资料

- [MDN Referer](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer)