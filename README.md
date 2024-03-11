# Edge tunnel

the original repositoy is [https://github.com/zizifn/edgetunnel](https://github.com/zizifn/edgetunnel), and I made some changes in it!

## Available branches and explain

| Branch Name   | Description                                                   |
| ------------- | ------------------------------------------------------------- |
| remote-socks5 | Branch for remote SOCKS5 proxy pool used implementation       |
| socks5        | Branch for SOCKS5 proxyIP implementation                      |
| vless         | Branch for outbound VLESS protocol implementation             |
| vless2        | Branch for alternative outbound VLESS protocol implementation |
| code1         | Branch for code1 feature development                          |
| code2         | Branch for code2 alternative feature development              |
| dns           | Branch for DNS alternative related development                |
| main          | Main branch for the project                                   |
| pages         | New version for deployment on Cloudflare Pages                |

## UUID Setting

Set UUID via repository secret keywrangler by github actions, this is for security reason, set UUID via github repository secret key and pass it to wrangler cli.

1. Its name must be a CF_TUN_UUID.
2. Its value must be a valid and lowercase uuid which is like  95392ce0-ca77-43d2-bd52-14cbdf35454b.

### UUID Setting Example

1. single uuid environment variable

   ```.environment
   UUID = "uuid here your want to set"
   ```

2. multiple uuid environment variable

   ```.environment
   UUID = "uuid1,uuid2,uuid3"
   ```

   note: uuid1, uuid2, uuid3 are separated by commas`,`.
   when you set multiple uuid, you can use `https://tun.<yourusername>.workers.dev` to get the clash config and vless:// link.

## Deploy in worker.dev

You can click the button below to deploy directly.

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/islongshiyu/tun)

## Subscribe vless:// link (Optional)

1. visit `https://tun.<yourusername>.workers.dev/uuid your set` to get the subscribe link.

2. visit `https://tun.<yourusername>.workers.dev/sub/uuid your set` to get the subscribe content with `uuid your set` path.

   note: `uuid your set` is the uuid you set in UUID enviroment or `wrangler.toml`, `_worker.js` file.
   when you set multiple uuid, you can use `https://tun.<yourusername>.workers.dev/sub/uuid1` to get the subscribe content with `uuid1` path.(only support first uuid in multiple uuid set)

3. visit `https://tun.<yourusername>.workers.dev/sub/uuid your set/?format=clash` to get the subscribe content with `uuid your set` path and `clash` format. content will return with base64 encode.

   note: `uuid your set` is the uuid you set in UUID enviroment or `wrangler.toml`, `_worker.js` file.
   when you set multiple uuid, you can will use `https://tun.<yourusername>.workers.dev/sub/uuid1/?format=clash` to get the subscribe content with `uuid1` path and `clash` format.(only support first uuid in multiple uuid set)

## Subscribe Cloudflare bestip(pure ip) link

1. visit `https://tun.<yourusername>.workers.dev/bestip/uuid your set` to get subscribe info.

2. cpoy subscribe url link `https://tun.<yourusername>.workers.dev/bestip/uuid your set` to any clients(clash/v2rayN/v2rayNG) you want to use.

## Multiple port support (Optional)

   <!-- let portArray_http = [80, 8080, 8880, 2052, 2086, 2095];
	let portArray_https = [443, 8443, 2053, 2096, 2087, 2083]; -->

For a list of Cloudflare supported ports, please refer to the [official documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/ports).

By default, the port is 80 and 443. If you want to add more ports, you can use the following ports:

```text
80, 8080, 8880, 2052, 2086, 2095, 443, 8443, 2053, 2096, 2087, 2083
http port: 80, 8080, 8880, 2052, 2086, 2095
https port: 443, 8443, 2053, 2096, 2087, 2083
```

if you deploy in cloudflare pages, https port is not supported. Simply add multiple ports node drictly use subscribe link, subscribe content will return all Cloudflare supported ports.

## ProxyIP (Optional)

1. When deploy in cloudflare pages, you can set proxyIP in `wrangler.toml` file. variable name is `PROXYIP`.

2. When deploy in worker.dev, you can set proxyIP in `_worker.js` file. variable name is `proxyIP`.

note: `proxyIP` is the ip or domain you want to set. this means that the proxyIP is used to route traffic through a proxy rather than directly to a website that is using Cloudflare's (CDN). if you don't set this variable, connection to the Cloudflare IP will be cancelled (or blocked)...

resons: Outbound TCP sockets to Cloudflare IP ranges are temporarily blocked, please refer to the [tcp-sockets documentation](https://developers.cloudflare.com/workers/runtime-apis/tcp-sockets/#considerations)
