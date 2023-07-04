# What is Playwright expect.poll

## Introduction

`expect.poll` is a method that allows you to wait for a condition to be met. It is similar to `expect.waitFor` but it allows you to specify a polling interval.

不设定固定的等待时间，而是设定一个轮询的时间间隔，直到满足条件为止。

## Usage

```javascript
await expect.poll(async () => {
  const response = await page.request.get('https://api.example.com');
  return response.status();
}, {
  // Probe, wait 1s, probe, wait 2s, probe, wait 10s, probe, wait 10s, probe, .... Defaults to [100, 250, 500, 1000].
  intervals: [1_000, 2_000, 10_000],
  timeout: 60_000
}).toBe(200);
```

