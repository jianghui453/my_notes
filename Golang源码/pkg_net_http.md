# net/http 包

## 主要接口、类型

### type Handler interface

主要用于处理请求

```
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

### type ServeMux struct

一个 HTTP 数据选择器，这里的功能类似路由器，可以根据路径配置相应的 Handler。

```
type ServeMux struct {
	mu    sync.RWMutex
	m     map[string]muxEntry
	es    []muxEntry // slice of entries sorted from longest to shortest.
	hosts bool       // whether any patterns contain hostnames
}
```

### func HandleFunc

使用默人的复用器 DefaultServeMux 实现请求处理。

```
// HandleFunc registers the handler function for the given pattern
// in the DefaultServeMux.
// The documentation for ServeMux explains how patterns are matched.
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	DefaultServeMux.HandleFunc(pattern, handler)
}
```

## http 客户端

### 使用 http 包提供的函数

```
package main
import (
    "fmt"
    "io/ioutil"
    "net/http"
)

func main() {
    response, err := http.Get("http://www.baidu.com")
    if err != nil {
    // handle error
    }
    //程序在使用完回复后必须关闭回复的主体。
    defer response.Body.Close()

    body, _ := ioutil.ReadAll(response.Body)
    fmt.Println(string(body))
}
```

```
package main

import (
    "fmt"
    "io/ioutil"
    "net/http"
    "bytes"
)

func main() {
    body := "{\"action\":20}"
    res, err := http.Post("http://xxx.com", "application/json;charset=utf-8", bytes.NewBuffer([]byte(body)))
    if err != nil {
        fmt.Println("Fatal error ", err.Error())
    }

    defer res.Body.Close()

    content, err := ioutil.ReadAll(res.Body)
    if err != nil {
        fmt.Println("Fatal error ", err.Error())
    }

    fmt.Println(string(content))
}
```

### 使用 http.Client和 http.NewRequest 实现

```
package main
import (
    "fmt"
    "io/ioutil"
    "net/http"
    "net/url"
    "strings"
)

func main() {
    v := url.Values{}
    v.Set("username", "xxxx")
    v.Set("password", "xxxx")
    //利用指定的method,url以及可选的body返回一个新的请求.如果body参数实现了io.Closer接口，Request返回值的Body 字段会被设置为body，并会被Client类型的Do、Post和PostFOrm方法以及Transport.RoundTrip方法关闭。
    body := ioutil.NopCloser(strings.NewReader(v.Encode())) //把form数据编下码
    client := &http.Client{}//客户端,被Get,Head以及Post使用
    reqest, err := http.NewRequest("POST", "http://xxx.com/logindo", body)
    if err != nil {
        fmt.Println("Fatal error ", err.Error())
    }
    //给一个key设定为响应的value.
    reqest.Header.Set("Content-Type", "application/x-www-form-urlencoded;param=value") //必须设定该参数,POST参数才能正常提交

    resp, err := client.Do(reqest)//发送请求
    defer resp.Body.Close()//一定要关闭resp.Body
    content, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Println("Fatal error ", err.Error())
    }

    fmt.Println(string(content))
}
```

## http 服务端

```
package main

import (
    "net/http"
)

func SayHello(w http.ResponseWriter, req *http.Request) {
    w.Write([]byte("Hello"))
}

func main() {
    http.HandleFunc("/hello", SayHello)
    http.ListenAndServe(":8001", nil)

}
```

## 引用

1. [golang net/http包使用](https://studygolang.com/articles/5219)
1. [Go 语言net/http 包使用模式](https://segmentfault.com/a/1190000006812688)