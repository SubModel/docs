# 暴露端口

有多种方法可以将 Pod 上的 port 暴露给外部世界。需要注意的是,对外暴露的 port 通常与容器上暴露的 port 并不相同。我们来看一个例子加以说明。

假设你想使用 uvicorn 在 Pod 上运行一个公开 API,命令如下:

```text
uvicorn main:app --host 0.0.0.0 --port 4000
```

该命令表示 uvicorn 会在所有网络接口的 4000 端口上监听。下面我们用两种不同的方法把这个 port 暴露到公网。

## 通过 SubModel 的 Proxy

在这种方式下,确保你想暴露的 port(此处为 4000)已在 Template 或 Pod 配置页面中设置。这里,我在 Pod 配置的 HTTP port 列表中添加了 4000。这一步也可以在你的 Template 定义中完成。

![Image](/assets/images/1386a3c-image-00ce91cf90cd62e743f1382d8d722b0a.png)

完成后,当你的服务器运行起来时,你应当能够通过 Pod 的 proxy 地址访问服务器。该地址按如下规则以程序化方式生成,其中 Pod ID 是你 Pod 的唯一 ID,本例中内部 port 为 4000:

```text
https://{POD_ID}-{INTERNAL_PORT}.tun.submodel.ai
```

请记住,这是暴露在公网上的。虽然你的 Pod ID 可以起到某种密码的作用,但它并不能替代真正的身份认证,后者应当在你的 API 层面实现。

### 关于 Proxy 行为的重要说明

当用户通过 SubModel proxy 访问其 Pod 时,连接会经过如下链路:

```text
User -> Cloudflare -> SubModel Loadbalancer -> Pod
```

务必注意以下行为:

- Cloudflare 对单个连接保持开启的时间限制为 100 秒。
- 如果你的服务在收到请求后 100 秒内没有响应,连接就会被关闭。
- 在这种情况下,用户会收到 `524` 错误码。

对于长时间运行的操作或响应可能超过 100 秒的服务而言,这个超时限制尤其重要。请在设计应用时把这一限制考虑在内,可以为较长的操作实现进度更新或分块响应。

## 通过 TCP 公网 IP

如果你的 Pod 支持公网 IP 地址,你也可以通过公网 TCP 暴露 API。在这种情况下,你需要把 port 添加到配置的 TCP 一侧。

![Image](/assets/images/49ebb9a-image-e69eedd0c7d53775006fdc73b0dcaabc.png)

这里唯一的区别是,你会得到一个外部 port 映射和一个公网 IP 地址来访问你的服务。例如,你的连接菜单可能是这样:

![Image](/assets/images/5e76c21-image-e7c348174fb6dd2ac5846743a696481a.png)

在这种情况下,你可以用如下的 ip:port 组合访问运行在 4000 上的服务:

```text
73.10.226.56:10027
```

请注意,使用 Community Cloud 时公网 IP 可能会发生变化,而使用 Secure Cloud 时则不会变化。如果 Pod 被重置,port 会发生变化。

## 请求对称的 port 映射

对某些应用来说,不对称的 port 映射并不理想。在上面的例子中,外部 port 10027 映射到内部 port 4000。如果你需要对称的 port 映射,可以在 TCP port 字段中填入大于 70000 的 port 来请求。

![Image](/assets/images/23c4178-image-4dc70800a254094973ca20d2132112f3.png)

当然,70000 并不是一个有效的 port 号,但这样做的作用是告诉 SubModel:你并不在意启动时实际的 port 号是多少,而是希望获得一个对称映射。你可以通过连接菜单查看实际的映射:

![Image](/assets/images/92e4f90-image-ce4e1e6f45d585a6ecc5f03583a23ab2.png)

在这个例子中,我请求了两个对称 port,最终得到的是 10030:10030 和 10031:10031。如果你需要在 Pod 中以程序方式访问它们,可以通过环境变量获取:

```text
SUBMODEL_TCP_PORT_70001=10031
SUBMODEL_TCP_PORT_70000=10030
```
