# KatanaProject.Owin.Compression #

Fork of https://katanaproject.codeplex.com/ Compression module from before they removed it.

[Brandon White](https://github.com/BrandonLWhite) was using this handy subcomponent from the Katana Project back before it became unmaintained and ultimately removed.  Brandon White has forked it [here](https://github.com/BrandonLWhite/Microsoft.Owin.Compression) and fixed a bug that was preventing it from working with the latest Microsoft.Owin (3.0.1.0 at this time).

## Getting Started ##

```csharp
app.UseStaticCompression(new StaticCompressionOptions());
```

## Basic Idea ##

This middleware would compress & cache the result if HTTP request header has `accept-encoding` with value of `gzip` or `deflate`.

It would check the `accept-encoding` header in HTTP request & choose the best-wanted format for compressing encoding.
If compressing necessary, it would check the `etag`, `request path`, `request query string`, `request method` as key from cache.
If the cache hit, it would return compressed result directly from cache.
If no, it would trigger the next pipeline, compress the result to cache, then return the result.

The cache is simply plat files under system temporary folder.

## Pitfalls ##

1. There is no quota control in current default implementation for cache, neither eviction policies.
1. There is no filters on request path, so compressing & caching always happen to even dynamic requests.
