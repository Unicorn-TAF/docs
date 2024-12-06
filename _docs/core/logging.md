---
title: Logging
category: Core
order: 1
---

`ULog` is a main framework logger that provides a simple way to log messages in your tests. It supports various logging levels such as _Trace_, _Debug_, _Info_, _Warning_, _Error_. You can also customize the logger to suit your needs.

By default build-in console logger is sused with _Debug_ level

## Logger customization

Set custom logger:

```csharp
ULog.SetLogger(new CustomLogger());
```

Set custom log level

```csharp
ULog.SetLevel(LogLevel.Trace);
```