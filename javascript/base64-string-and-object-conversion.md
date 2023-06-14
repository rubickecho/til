# base64 字符串和对象互转

```js
/**
 * base64 字符串转对象
 * @param str {string} base64 字符串
 * @returns Object
 */
export const base64StrToObj = <T>(str: string): T => {
  const utf8Decoder = new TextDecoder();
  let obj;
  try {
    const base64Str = str.replace(/-/g, '+').replace(/_/g, '/');
    const buffer = new Uint8Array(
      atob(base64Str)
        .split('')
        .map((c) => c.charCodeAt(0))
    );
    const utf8Str = utf8Decoder.decode(buffer);
    obj = JSON.parse(utf8Str);
    if (typeof obj === 'string') {
      obj = JSON.parse(obj);
    }
  } catch (error) {
    return obj;
  }
  return obj;
};
```

```js
/**
 * base64 转对象，需要处理中文字符
 * @param obj {Object | string}
 * @returns string
 */
export const objToBase64Str = (obj: Object | string) => {
  let str = '';
  if (typeof obj === 'object') {
    str = JSON.stringify(obj);
  } else {
    str = obj.toString();
  }
  const utf8Encoder = new TextEncoder();
  let utf8Array;
  try {
    utf8Array = utf8Encoder.encode(JSON.stringify(str));
  } catch (e) {
    throw new Error('Failed to encode to UTF-8');
  }
  let base64Str;
  try {
    base64Str = btoa(String.fromCharCode(...new Uint8Array(utf8Array))); // 转成 base64
  } catch (e) {
    throw new Error('Failed to encode to base64');
  }
  return base64Str;
};
```