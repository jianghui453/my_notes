# artifact

使用artifact指示Nomad获取远程资源。Nomad使用go-getter库下载artifacts。

```
job "docs" {
  group "example" {
    task "server" {
      artifact {
        source      = "https://example.com/file.tar.gz"
        destination = "local/some-directory"
        options {
          checksum = "md5:df6a4178aec9fbdc1d6d7e3634d1bc33"
        }
      }
    }
  }
}
```
