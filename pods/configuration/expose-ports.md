# Expose Ports

There are several methods to expose ports on your pod to the external world. It's important to note that the publicly exposed port is typically NOT the same as the port exposed on your container. Let's explore an example to clarify this.

Suppose you want to run a public API on your pod using uvicorn with the following command:

```text
uvicorn main:app --host 0.0.0.0 --port 4000
```

This command means uvicorn will listen on all interfaces on port 4000. Now, let's expose this port to the public internet using two different methods.

## Through SubModel's Proxy

In this scenario, ensure the port you wish to expose (4000 in this case) is set in the [Template](https://www.SubModel.io/console/user/templates) or [Pod](https://www.SubModel.io/console/pods) configuration page. Here, I have added 4000 to the HTTP port list in my pod config. This can also be done in your template definition.

![Image](/assets/images/1386a3c-image-00ce91cf90cd62e743f1382d8d722b0a.png)

Once this is done and your server is running, you should be able to access your server using the pod's proxy address, which is formed programmatically as follows, where the pod ID is the unique ID of your pod, and the internal port in this case is 4000:

```text
https://{POD_ID}-{INTERNAL_PORT}.proxy.SubModel.net
```

Remember, this is exposed to the public internet. While your pod ID can act as a sort of password, it's not a substitute for real authentication, which should be implemented at your API level.

### Important Proxy Behavior Notes

When users access their pods via the SubModel proxy, the connection follows this chain:

```text
User -> Cloudflare -> SubModel Loadbalancer -> Pod
```

It's crucial to be aware of the following behavior:

- Cloudflare has a 100-second limit for a connection to remain open.
- If your service does not respond within 100 seconds of a request, the connection will be closed.
- In such cases, the user will receive a `524` error code.

This timeout limit is particularly important for long-running operations or services that might take more than 100 seconds to respond. Ensure your applications are designed with this limitation in mind, potentially implementing progress updates or chunked responses for longer operations.

## Through TCP Public IP

If your pod supports a public IP address, you can also expose your API over public TCP. In this case, you would add the port to the TCP side of the configuration.

![Image](/assets/images/49ebb9a-image-e69eedd0c7d53775006fdc73b0dcaabc.png)

The only difference here is that you will receive an external port mapping and a public IP address to access your service. For example, your connect menu may look something like this:

![Image](/assets/images/5e76c21-image-e7c348174fb6dd2ac5846743a696481a.png)

In this case, you would be hitting your service running on 4000 with the following ip:port combination:

```text
73.10.226.56:10027
```

Be aware that the public IP could potentially change when using Community Cloud, but should not change when using Secure Cloud. The port will change if your pod gets reset.

## Requesting a Symmetrical Port Mapping

For some applications, asymmetrical port mappings are not ideal. In the above case, we have external port 10027 mapping to internal port 4000. If you need to have a symmetrical port mapping, you can request them by putting in ports above 70000 in your TCP port field.

![Image](/assets/images/23c4178-image-4dc70800a254094973ca20d2132112f3.png)

Of course, 70000 isn't a valid port number, but what this does is it tells SubModel that you don't care what the actual port number is on launch, but to rather give you a symmetrical mapping. You can inspect the actual mapping via your connect menu:

![Image](/assets/images/92e4f90-image-ce4e1e6f45d585a6ecc5f03583a23ab2.png)

In this case, I have requested two symmetrical ports and they ended up being 10030:10030 and 10031:10031. If you need programmatic access to these in your pod, you can access them via environment variable:

```text
SUBMODEL_TCP_PORT_70001=10031
SUBMODEL_TCP_PORT_70000=10030
```