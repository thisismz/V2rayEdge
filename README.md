# V2ray Edge（Beta）

Deploy **V2ray** to Edge/Serverless Functions platform.

**v2ray-heroku project is dead due to heroku unfree. Here is the new project. **

> For international user, I write this readme in Chinese. But I understand English pretty well, if you has any issue, please open it in Github.

> The project is under development, basically available, there will be bugs. .
> **Please follow the prompts of github regularly and only synchronize to your own project. You only need to care about the prompts in the red box below, and do not click ** for other prompts.
> ![sync](./doc/sync.jpg)

> After the synchronization is complete, if you find that it is different, **please see the document**.

> This project is purely technical verification, exploring the latest web standard. No guarantees are given.

## Edge Tunnel server --- Deno deploy

Edge tunnel service uses [Deno deploy](https://deno.com/deploy).

### risk warning

`Deno deploy` adopts [fair use policy](https://deno.com/deploy/docs/fair-use-policy), translated into Chinese is `see conscience use`. Violations may result in banning.
According to my understanding, this project should violate the fair use policy. Please **use as appropriate**.

>I am very grateful to Deno deploy for taking the web standard seriously, supporting HTTP request & response streaming, and making edge tunnel possible.

### How to deploy the service

Please check the tutorial below。

[Deno deploy Install](./doc/edge-tunnel-deno.md)

## Edge Tunnel server --- Cloudflare Worker （stay tuned）

This needs to wait for Cloudflare to release the following technology.
https://blog.cloudflare.com/introducing-socket-workers/

## Client v2rayN configuration

> ⚠️ UDP packets cannot be forwarded due to edge platform limitations. Please change the DNS policy to "Asis" when configuring, otherwise the speed will be affected.

> [ DNS Popular science articles](https://tachyondevel.medium.com/%E6%BC%AB%E8%B0%88%E5%90%84%E7%A7%8D%E9%BB%91%E7%A7%91%E6%8A%80%E5%BC%8F-dns-%E6%8A%80%E6%9C%AF%E5%9C%A8%E4%BB%A3%E7%90%86%E7%8E%AF%E5%A2%83%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8-62c50e58cbd0)

https://github.com/2dust/v2rayN
Other people's configuration tutorial reference，https://v2raytech.com/v2rayn-config-tutorial/.
![v2rayN](./doc/v2rayn.jpg)

## VLESS websocket 客户端配置

> ⚠️ UDP packets cannot be forwarded due to edge platform limitations. Please change the DNS policy to "Asis" when configuring, otherwise the speed will be affected.

```json
"outbounds": [
        {
            "protocol": "vless",
            "settings": {
                "vnext": [
                    {
                        "address": "***.herokuapp.com", // edge app URL 或者 cloudflare worker url/ip
                        "port": 443,
                        "users": [
                            {
                                "id": "", // 填写你的 UUID
                                "encryption": "none"
                            }
                        ]
                    }
                ]
            },
            "streamSettings": {
                "network": "ws",
                "security": "tls",
                "tlsSettings": {
                    "serverName": "***.***.com" // edge app host 或者 cloudflare worker host
                }
              }
          }
    ]
```

## Establish cloudflare worker （optional）

```js
const targetHost = 'xxx.xxxx.dev'; //The hostname of your edge function
addEventListener('fetch', (event) => {
  let url = new URL(event.request.url);
  url.hostname = targetHost;
  let request = new Request(url, event.request);
  event.respondWith(fetch(request));
});
```

# FAQ

## does not support UDP

UDP packets cannot be forwarded due to edge platform limitations. So please set the DNS policy to `Asis`.

## VMESS is not supported

The VMESS protocol is too complicated, and all edge platforms support HTTPS, so there is no need for VMESS.
