# Simple Cache

Simple cache plugin middleware caches responses on disk.

## Configuration

To configure this plugin you should add its configuration to the Traefik dynamic configuration as explained [here](https://docs.traefik.io/getting-started/configuration-overview/#the-dynamic-configuration).
The following snippet shows how to configure this plugin with the File provider in TOML and YAML: 

Static:

```toml
[experimental.plugins.cache]
  modulename = "github.com/traefik/plugin-simplecache"
  version = "v0.2.1"
```

Dynamic:

```toml
[http.middlewares]
  [http.middlewares.my-cache.plugin.cache]
    path = "/some/path/to/cache/dir"
```

```yaml
http:
  middlewares:
   my-cache:
      plugin:
        cache:
          path: /some/path/to/cache/dir
```

### Options

#### Path (`path`)

The base path that files will be created under. This must be a valid existing
filesystem path.

#### Max Expiry (`maxExpiry`)

*Default: 300*

The maximum number of seconds a response can be cached for. The 
actual cache time will always be lower or equal to this.

#### Cleanup (`cleanup`)

*Default: 600*

The number of seconds to wait between cache cleanup runs.

A value of -1 disables cleanup. It means stale records won't be cleaned up so if you
don't limit yourself to a fixed set of paths that is managed by this plugin you risk
filling up your cache filesystem. But it avoids issue [#21](https://github.com/traefik/plugin-simplecache/issues/21)
	
#### Add Status Header (`addStatusHeader`)

*Default: true*

This determines if the cache status header `Cache-Status` will be added to the
response headers. This header can have the value `hit`, `miss` or `error`.
